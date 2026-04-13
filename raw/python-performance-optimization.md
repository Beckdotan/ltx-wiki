# LTX Video - Performance Optimization Guide

> **Source:** https://huggingface.co/docs/diffusers/optimization/fp16
> **Source:** https://huggingface.co/docs/diffusers/optimization/memory
> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video

## Overview

LTX Video generation can be optimized for both speed and memory. This guide covers all available optimization techniques, from basic mixed precision to advanced compilation and quantization strategies.

## Speed Optimizations

### 1. torch.compile

Compiling the transformer model provides the largest speed improvement. The first call is slow (compilation), but subsequent calls are significantly faster.

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

# Enable compiler optimizations
torch._inductor.config.conv_1x1_as_mm = True
torch._inductor.config.coordinate_descent_tuning = True
torch._inductor.config.epilogue_fusion = False
torch._inductor.config.coordinate_descent_check_all_directions = True

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)

# Compile the transformer (most compute-intensive component)
pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)

prompt = "A woman with long brown hair and light skin smiles at another woman"
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=768,
    height=512,
    num_frames=161,
    decode_timestep=0.03,
    decode_noise_scale=0.025,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

#### Regional Compilation (Faster Compile Time)

Compile only the repeated transformer blocks instead of the full model (8-10x faster compilation):

```python
# Compile only repeated transformer layers
pipeline.transformer.compile_repeated_blocks(fullgraph=True)
```

#### Dynamic Shape Compilation

Avoid recompilation when generating at different resolutions:

```python
torch.fx.experimental._config.use_duck_shape = False
pipeline.transformer = torch.compile(
    pipeline.transformer, fullgraph=True, dynamic=True
)
```

### 2. Use Distilled Models

Distilled models require 4-10 inference steps instead of 50, providing 5-12x speedup:

```python
import torch
from diffusers import LTXConditionPipeline

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")

# Custom timesteps optimized for distilled model
video = pipeline(
    prompt="A cinematic scene of a sunset over the ocean",
    negative_prompt="worst quality, blurry",
    width=512,
    height=320,
    num_frames=161,
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],
    guidance_scale=1.0,  # Must be 1.0 for distilled
    output_type="pil",
).frames[0]
```

### 3. Mixed Precision (BFloat16)

Always use `torch.bfloat16` for LTX Video:

```python
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")
```

### 4. TF32 for Ampere GPUs

Enable TensorFloat-32 for faster matrix operations on NVIDIA Ampere+ GPUs:

```python
import torch
torch.backends.cuda.matmul.allow_tf32 = True
```

### 5. Scaled Dot Product Attention (SDPA)

Automatically enabled with PyTorch 2.0+. No code changes needed. You can also explicitly use xFormers:

```python
# Optional: force xFormers if desired
pipeline.enable_xformers_memory_efficient_attention()
```

### 6. Fused QKV Projections

Combine Q, K, V projection matrices for larger, faster matrix multiplications:

```python
pipeline.fuse_qkv_projections()
```

## Memory Optimizations

### 1. FP8 Layerwise Casting (~10GB VRAM)

Store weights in FP8 format, compute in BFloat16. This is the most effective memory-saving technique:

```python
import torch
from diffusers import LTXPipeline, AutoModel

transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn,
    compute_dtype=torch.bfloat16
)

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    transformer=transformer,
    torch_dtype=torch.bfloat16
)
```

#### Fine-Grained Layerwise Casting

```python
from diffusers.hooks import apply_layerwise_casting

apply_layerwise_casting(
    transformer,
    storage_dtype=torch.float8_e4m3fn,
    compute_dtype=torch.bfloat16,
    skip_modules_classes=["norm"],  # Skip normalization layers
    non_blocking=True,
)
```

### 2. Group Offloading

Moves groups of layers between CPU and GPU. Better memory savings than model offloading, faster than CPU offloading.

#### Block-Level Offloading

```python
from diffusers.hooks import apply_group_offloading

onload_device = torch.device("cuda")
offload_device = torch.device("cpu")

# For Diffusers models (ModelMixin)
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)

# For other models (torch.nn.Module)
apply_group_offloading(
    pipeline.text_encoder,
    onload_device=onload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)
```

#### Leaf-Level Offloading with CUDA Streams (Fastest)

```python
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    use_stream=True,
    record_stream=True  # Slightly more memory, faster
)
apply_group_offloading(
    pipeline.vae,
    onload_device=onload_device,
    offload_type="leaf_level"
)
apply_group_offloading(
    pipeline.text_encoder,
    onload_device=onload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)
```

#### Low CPU Memory Usage

```python
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    use_stream=True,
    low_cpu_mem_usage=True  # Creates pinned tensors on-the-fly
)
```

#### Offloading to Disk

For systems with limited system RAM:

```python
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    offload_to_disk_path="path/to/disk"
)
```

### 3. Model CPU Offloading

Moves entire models (not individual layers) between CPU and GPU:

```python
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
# Don't call .to("cuda") -- offloading handles device placement
pipeline.enable_model_cpu_offload()
```

### 4. Sequential CPU Offloading (Maximum Memory Savings)

Moves submodules between devices. Very slow but uses minimum memory:

```python
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
# Don't call .to("cuda")
pipeline.enable_sequential_cpu_offload()
```

### 5. VAE Tiling

Splits the VAE input into overlapping tiles to reduce peak memory during decoding:

```python
pipeline.vae.enable_tiling()

# Optional: configure tile parameters
pipeline.vae.enable_tiling(
    tile_sample_min_height=256,
    tile_sample_min_width=256,
    tile_sample_stride_height=192,
    tile_sample_stride_width=192
)
```

### 6. VAE Slicing

Splits batch processing into single items (useful for multi-video batches):

```python
pipeline.enable_vae_slicing()
```

### 7. GGUF Quantization

Load quantized models for dramatically reduced memory:

```python
import torch
from diffusers import LTXPipeline, AutoModel, GGUFQuantizationConfig

transformer = AutoModel.from_single_file(
    "https://huggingface.co/city96/LTX-Video-gguf/blob/main/ltx-video-2b-v0.9-Q3_K_S.gguf",
    quantization_config=GGUFQuantizationConfig(compute_dtype=torch.bfloat16),
    torch_dtype=torch.bfloat16
)
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    transformer=transformer,
    torch_dtype=torch.bfloat16
)
```

### 8. Sharded Checkpoints

Load large models in shards to reduce peak memory during loading:

```python
from diffusers import AutoModel

# Save as sharded checkpoint
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", subfolder="transformer"
)
transformer.save_pretrained("ltx-transformer-sharded", max_shard_size="5GB")

# Load sharded checkpoint (lower peak memory)
transformer = AutoModel.from_pretrained(
    "ltx-transformer-sharded", torch_dtype=torch.bfloat16
)
```

## Combined Optimization Strategy

### Low VRAM (~10GB) -- Maximum Memory Savings

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading

# FP8 weights
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video", subfolder="transformer", torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn, compute_dtype=torch.bfloat16
)

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", transformer=transformer, torch_dtype=torch.bfloat16
)

# Group offloading with streams
onload_device = torch.device("cuda")
offload_device = torch.device("cpu")
pipeline.transformer.enable_group_offload(
    onload_device=onload_device, offload_device=offload_device,
    offload_type="leaf_level", use_stream=True
)
apply_group_offloading(pipeline.text_encoder, onload_device=onload_device,
    offload_type="block_level", num_blocks_per_group=2)
apply_group_offloading(pipeline.vae, onload_device=onload_device,
    offload_type="leaf_level")

# VAE tiling for large outputs
pipeline.vae.enable_tiling()
```

### High VRAM (~24GB+) -- Maximum Speed

```python
import torch
from diffusers import LTXPipeline

torch._inductor.config.conv_1x1_as_mm = True
torch._inductor.config.coordinate_descent_tuning = True
torch._inductor.config.epilogue_fusion = False
torch.backends.cuda.matmul.allow_tf32 = True

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

# Compile transformer
pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)

# Fuse QKV projections
pipeline.fuse_qkv_projections()
```

### Multi-GPU Setup

```python
from diffusers import LTXPipeline

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16,
    device_map="balanced"  # Distribute across GPUs
)

# Or with memory limits
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16,
    device_map="balanced",
    max_memory={0: "12GB", 1: "12GB"}
)
```

## Recommended Settings by Model

| Model | VRAM | Recommended Optimizations |
|-------|------|--------------------------|
| 2B base | 8-10 GB | FP8 + group offloading + VAE tiling |
| 2B base | 24 GB | BFloat16 + torch.compile |
| 2B distilled | 8-10 GB | FP8 + group offloading + 8 steps |
| 13B dev | 24 GB | FP8 + group offloading + VAE tiling |
| 13B dev | 40 GB | BFloat16 + torch.compile |
| 13B distilled | 24 GB | FP8 + group offloading + 8 steps |
