---
title: Python Memory Optimization
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/optimization/memory
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://deepwiki.com/Lightricks/LTX-Video/4.2-performance-optimization
  - https://github.com/nalexand/LTX-2-OPTIMIZED
  - https://apatero.com/blog/ltx-2-8gb-vram-optimization-complete-guide-2025
tags:
  - python
  - performance
  - memory
  - vram
  - fp8
  - quantization
  - offloading
---

# Python Memory Optimization

Memory optimization techniques for running LTX-Video on limited VRAM. For speed-focused optimizations, see [[python-performance-speed]].

## FP8 Layerwise Casting (~10GB VRAM)

Store weights in FP8 format, compute in BFloat16. The most effective memory-saving technique:

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

### Fine-Grained Layerwise Casting

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

### FP8 with LTX-2 Pipelines

```python
from ltx_core.quantization import QuantizationPolicy

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    quantization=QuantizationPolicy.fp8_cast(),  # ~40% VRAM reduction
    # ...
)
```

CLI usage:

```bash
PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True python -m ltx_pipelines.ti2vid_two_stages \
    --quantization fp8-cast \
    --checkpoint-path /path/to/checkpoint.safetensors \
    --prompt "A beautiful sunset" \
    --output-path output.mp4
```

## Group Offloading

Moves groups of layers between CPU and GPU. Better memory savings than model offloading, faster than sequential CPU offloading.

### Block-Level Offloading

```python
from diffusers.hooks import apply_group_offloading

onload_device = torch.device("cuda")
offload_device = torch.device("cpu")

pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)

apply_group_offloading(
    pipeline.text_encoder,
    onload_device=onload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)
```

### Leaf-Level Offloading with CUDA Streams (Fastest)

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
```

### Low CPU Memory Usage

```python
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    use_stream=True,
    low_cpu_mem_usage=True  # Creates pinned tensors on-the-fly
)
```

### Offloading to Disk

For systems with limited system RAM:

```python
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    offload_to_disk_path="path/to/disk"
)
```

## Model CPU Offloading

Moves entire models between CPU and GPU. Less aggressive but faster than sequential offloading:

```python
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
# Don't call .to("cuda") -- offloading handles device placement
pipeline.enable_model_cpu_offload()
```

## Sequential CPU Offloading (Maximum Memory Savings)

Moves individual submodules between devices. Slowest but uses minimum memory:

```python
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
# Don't call .to("cuda")
pipeline.enable_sequential_cpu_offload()
```

## VAE Tiling

Splits the VAE input into overlapping tiles to reduce peak memory during decoding. Critical for high resolutions:

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

Disable with `pipeline.vae.disable_tiling()`.

## VAE Slicing

Splits batch processing into single items (useful for multi-video batches):

```python
pipeline.enable_vae_slicing()
```

## GGUF Quantized Models

For the lowest VRAM usage, load GGUF quantized checkpoints:

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

Also available: `unsloth/LTX-2.3-GGUF`

## Sharded Checkpoints

Load large models in shards to reduce peak memory during loading:

```python
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

## Multi-GPU Setup

```python
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

## Low VRAM (~10GB) Complete Recipe

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading

# 1. FP8 weights
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video", subfolder="transformer", torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn, compute_dtype=torch.bfloat16
)

# 2. Build pipeline
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", transformer=transformer, torch_dtype=torch.bfloat16
)

# 3. Group offloading with streams
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

# 4. VAE tiling
pipeline.vae.enable_tiling()
```

## Resolution and Frame Count Impact

### Safe Mode Limits (8GB VRAM, FP8)

| Resolution | Max Frames (txt2vid) | Approx Time |
|-----------|---------------------|-------------|
| 1280x704 | ~257 frames | ~11 seconds |
| 1920x1088 | ~121 frames | ~5 seconds |
| 3840x2176 | ~25 frames | Longer |

### General Guidelines

- Resolution must be divisible by 32
- Frame count must follow 8n+1 pattern (9, 17, 25, ..., 161, 257, 361)
- Lower resolution + upscale is often faster than direct high-res generation
- The [[python-two-stage-pipeline]] is recommended for production use

## See Also

- [[python-performance-speed]] -- Speed optimizations (torch.compile, distilled models)
- [[python-two-stage-pipeline]] -- Low-res generate + latent upscale pattern
- [[python-lora-loading]] -- LoRA on quantized models
