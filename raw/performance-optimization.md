# LTX Video Performance Optimization

> **Sources:**
> - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
> - https://deepwiki.com/Lightricks/LTX-Video/4.2-performance-optimization
> - https://github.com/nalexand/LTX-2-OPTIMIZED
> - https://apatero.com/blog/ltx-2-8gb-vram-optimization-complete-guide-2025
> - https://github.com/Lightricks/ComfyUI-LTXVideo/issues/421
> - https://huggingface.co/Lightricks/LTX-Video/discussions/6
> - https://huggingface.co/Lightricks/LTX-Video/discussions/18
> - https://github.com/Lightricks/LTX-Video-Q8-Kernels

---

## torch.compile

`torch.compile` optimizes the transformer execution graph. The first call is slow (compilation), but subsequent calls are significantly faster.

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)

# Apply torch.compile to the transformer
pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)

prompt = "A woman with long brown hair and light skin smiles..."
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

**Notes:**
- torch.compile shows small gains for longer runs; for short 8-16 frame tests, compile time can outweigh savings
- Use `mode="max-autotune"` for best performance with `fullgraph=True`
- `torch.channels_last` memory format can improve GPU utilization
- Best combined with other optimizations (FP8, caching)

---

## Mixed Precision (FP8, FP16, BF16)

### FP8 Layerwise Weight-Casting (~10GB VRAM)

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading
from diffusers.utils import export_to_video

# Load transformer with FP8 storage, BF16 compute
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

### FP8 Quantization with LTX-2 Pipelines

```python
from ltx_core.quantization import QuantizationPolicy
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
    quantization=QuantizationPolicy.fp8_cast(),
)
```

**CLI with FP8:**

```bash
PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True python -m ltx_pipelines.ti2vid_two_stages \
    --quantization fp8-cast \
    --checkpoint-path /path/to/checkpoint.safetensors \
    --prompt "A beautiful sunset" \
    --output-path output.mp4
```

### Precision Comparison

| Precision | VRAM Usage | Quality | Speed | GPU Support |
|-----------|-----------|---------|-------|-------------|
| BF16 | Highest | Best | Baseline | Ampere+ (RTX 30xx+) |
| FP16 | High | Very Good | Slightly faster | All modern GPUs |
| FP8 (e4m3fn) | ~30% less | Good | Faster | Ada Lovelace+ (RTX 40xx+) |
| GGUF (Q3-Q8) | Lowest | Variable | Variable | All modern GPUs |

**Recommendations:**
- FP16 works best on RTX 4070 class GPUs
- BF16 also works but uses slightly more memory
- FP8 makes 8GB VRAM viable for 480p resolution
- Use `torch.bfloat16` as the recommended dtype for transformer, VAE, and text encoder

---

## Memory Optimization

### Group Offloading (CPU/GPU Swapping)

```python
from diffusers.hooks import apply_group_offloading

onload_device = torch.device("cuda")
offload_device = torch.device("cpu")

# Transformer: leaf-level offloading with streaming
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    use_stream=True
)

# Text encoder: block-level offloading
apply_group_offloading(
    pipeline.text_encoder,
    onload_device=onload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)

# VAE: leaf-level offloading
apply_group_offloading(
    pipeline.vae,
    onload_device=onload_device,
    offload_type="leaf_level"
)
```

### VAE Tiling (Reduce Memory During Decode)

```python
pipeline.vae.enable_tiling()
```

Splits the input tensor into tiles for decode/encode, saving significant memory for large resolutions. Can be disabled with `pipeline.vae.disable_tiling()`.

### VAE Slicing

```python
pipeline.vae.enable_slicing()
```

Splits the input tensor into slices for batch processing. Useful for larger batch sizes.

### Sequential CPU Offloading

```python
pipeline.enable_sequential_cpu_offload()
```

Moves model components to GPU only when needed, one at a time. Maximum memory savings but slower.

### Model CPU Offloading

```python
pipeline.enable_model_cpu_offload()
```

Keeps entire models on CPU and moves them to GPU for forward pass. Less aggressive than sequential offloading but faster.

---

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

Unsloth also provides GGUF variants: `unsloth/LTX-2.3-GGUF`

---

## Attention Backends

### Native PyTorch SDPA (Recommended)

PyTorch's native `scaled_dot_product_attention` is generally the most stable:

```python
# This is the default; no extra configuration needed
# If you need to explicitly set it:
from diffusers.models.attention_processor import AttnProcessor2_0
pipeline.transformer.set_attn_processor(AttnProcessor2_0())
```

### xFormers

```bash
pip install xformers
```

```python
pipeline.enable_xformers_memory_efficient_attention()
```

### Flash Attention 3

For Hopper architecture GPUs (H100):

```bash
pip install flash-attn --no-build-isolation
```

### Sage Attention

Community-requested for faster computation and reduced memory:

```python
# Install sage-attention package
# Provides 2-4x inference speedup potential
```

**Note:** If your setup exposes a toggle for `scaled_dot_product_attention` (PyTorch native) vs xFormers, try native first on recent PyTorch as it is reportedly more stable on Windows.

---

## FP8 Kernels (Ada Architecture)

Custom FP8 kernels optimized for LTX-Video on Ada Lovelace GPUs:

```bash
git clone https://github.com/Lightricks/LTX-Video-Q8-Kernels.git
cd LTX-Video-Q8-Kernels
pip install -e .
```

**Requirements:** CUDA Toolkit 12.8+, Ada architecture GPU (RTX 40xx+)

---

## Distilled Models for Fast Inference

Distilled models use fewer steps for comparable quality:

```python
# Distilled model: 4-10 inference steps
video = pipeline(
    prompt=prompt,
    guidance_scale=1.0,  # Must be 1.0 for distilled
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],
    num_frames=161,
).frames[0]
```

### DistilledPipeline (LTX-2)

Uses only 8 predefined sigmas (8 steps stage 1, 4 steps stage 2):

```python
from ltx_pipelines.distilled import DistilledPipeline

pipeline = DistilledPipeline(
    checkpoint_path="/path/to/ltx-2.3-22b-distilled.safetensors",
    gemma_root="/path/to/gemma",
)
```

---

## Gradient Estimation (Reduce Steps)

Reduce inference from 40 to 20-30 steps while maintaining quality:

```python
from ltx_pipelines.utils import gradient_estimating_euler_denoising_loop

def denoising_loop(sigmas, video_state, audio_state, stepper):
    return gradient_estimating_euler_denoising_loop(
        sigmas=sigmas,
        video_state=video_state,
        audio_state=audio_state,
        stepper=stepper,
        denoise_fn=your_denoise_function,
        ge_gamma=2.0,
    )
```

---

## Caching

Diffusers supports caching of intermediate outputs to speed up inference:

```python
# Refer to diffusers caching documentation
# https://huggingface.co/docs/diffusers/main/optimization/cache
```

---

## Resolution and Frame Count Impact on Performance

### Safe Mode Limits (8GB VRAM, FP8)

| Resolution | Max Frames (txt2vid) | Approx Generation Time |
|-----------|---------------------|----------------------|
| 1280x704 | ~257 frames | ~11 seconds |
| 1920x1088 | ~121 frames | ~5 seconds |
| 3840x2176 | ~25 frames | Longer |

### General Guidelines

- Resolution must be divisible by 32
- Frame count must follow 8n+1 pattern (9, 17, 25, ..., 161, 257, 361)
- Lower resolution + upscale is often faster than direct high-res generation
- The two-stage pipeline (generate low-res + upscale) is recommended for production use

---

## Optimization Combination Example (Maximum Performance)

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading
from diffusers.utils import export_to_video

# 1. FP8 weight casting
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn,
    compute_dtype=torch.bfloat16
)

# 2. Build pipeline
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    transformer=transformer,
    torch_dtype=torch.bfloat16
)

# 3. Group offloading
onload_device = torch.device("cuda")
offload_device = torch.device("cpu")
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    use_stream=True
)
apply_group_offloading(pipeline.text_encoder, onload_device=onload_device, offload_type="block_level", num_blocks_per_group=2)
apply_group_offloading(pipeline.vae, onload_device=onload_device, offload_type="leaf_level")

# 4. Enable VAE tiling
pipeline.vae.enable_tiling()

# 5. torch.compile (optional, slow first run)
# pipeline.transformer = torch.compile(pipeline.transformer, mode="max-autotune", fullgraph=True)

# 6. Generate
video = pipeline(
    prompt="...",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=768,
    height=512,
    num_frames=161,
    decode_timestep=0.03,
    decode_noise_scale=0.025,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

---

## Environment Variables

```bash
# Recommended for FP8 memory management
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True

# Disable CUDA launch blocking for better throughput
export CUDA_LAUNCH_BLOCKING=0
```
