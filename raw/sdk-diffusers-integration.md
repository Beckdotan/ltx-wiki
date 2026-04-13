# Hugging Face Diffusers Integration for LTX Video

## Sources
- https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
- https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
- https://huggingface.co/docs/diffusers/main/api/pipelines/ltx2
- https://github.com/huggingface/diffusers/blob/main/docs/source/en/api/pipelines/ltx_video.md
- https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/ltx/pipeline_ltx_condition.py

---

## Overview

LTX Video has first-class integration with the Hugging Face Diffusers library, the de facto standard Python library for diffusion model inference. Both LTX-Video (v1) and LTX-2 have dedicated pipeline classes.

The main architectural feature of LTX-Video is its Video-VAE, which has a higher pixel-to-latent compression ratio (1:192) enabling more efficient video data processing and faster generation speed. The Video-VAE decoder performs the latent-to-pixel conversion AND the last denoising step.

---

## Installation

```bash
# Install latest diffusers (required for LTX-Video 0.9.8+ support)
pip install -U git+https://github.com/huggingface/diffusers

# Or from PyPI
pip install diffusers>=0.32.0

# Additional dependencies
pip install torch>=2.1.2 transformers accelerate
```

---

## LTX-Video (v1) Pipeline Classes

### LTXPipeline

Basic text-to-video pipeline. Requires ~10GB VRAM with memory optimizations.

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")

video = pipeline(
    prompt="A woman with brown hair smiles...",
    negative_prompt="worst quality, inconsistent motion, blurry",
    width=768,
    height=512,
    num_frames=161,
    decode_timestep=0.03,
    decode_noise_scale=0.025,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

### LTXConditionPipeline

Conditional pipeline supporting image and video conditioning.

```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", torch_dtype=torch.bfloat16
)
```

**Key parameters:**
- `conditions` - List of `LTXVideoCondition` objects
- `prompt` / `negative_prompt` - Text prompts
- `width` / `height` - Output dimensions (divisible by 32)
- `num_frames` - Number of output frames (divisible by 8, +1)
- `num_inference_steps` - Denoising steps (default: 30)
- `denoise_strength` - Denoising strength (0.0-1.0, for img2img/vid2vid)
- `decode_timestep` - Timestep for VAE decoding (0.05 for v0.9.1+)
- `image_cond_noise_scale` - Conditioning noise scale (0.025 for v0.9.1+)
- `guidance_scale` - CFG scale (1.0 for distilled, 5.0 for full models)
- `guidance_rescale` - Guidance rescale factor (0.7 recommended)
- `generator` - PyTorch generator for reproducibility
- `output_type` - "pil" for PIL images, "latent" for latent tensors

### LTXLatentUpsamplePipeline

2x spatial upscaler working in latent space.

```python
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8",
    vae=pipe.vae,
    torch_dtype=torch.bfloat16
)
```

### LTXVideoCondition

Defines conditioning for video generation.

```python
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

condition = LTXVideoCondition(
    video=video,       # video frames (list of PIL or tensor)
    frame_index=0      # which frame index to condition on
)
```

### LTXImageToVideoPipeline

Dedicated image-to-video pipeline.

---

## LTX-2 Pipeline Classes

### LTX2Pipeline

Main pipeline for LTX-2 audio-video generation.

```python
from diffusers.pipelines.ltx2 import LTX2Pipeline

pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2", torch_dtype=torch.bfloat16
)
pipe.enable_sequential_cpu_offload(device="cuda:0")

video_latent, audio_latent = pipe(
    prompt="A beautiful sunset",
    negative_prompt="shaky, glitchy, low quality",
    width=768,
    height=512,
    num_frames=121,
    frame_rate=24.0,
    num_inference_steps=40,
    guidance_scale=4.0,
    output_type="latent",
    return_dict=False,
)
```

### LTX2LatentUpsamplePipeline

Upsampler for LTX-2 latents.

```python
from diffusers.pipelines.ltx2 import LTX2LatentUpsamplePipeline
from diffusers.pipelines.ltx2.latent_upsampler import LTX2LatentUpsamplerModel

latent_upsampler = LTX2LatentUpsamplerModel.from_pretrained(
    "Lightricks/LTX-2",
    subfolder="latent_upsampler",
    torch_dtype=torch.bfloat16,
)
upsample_pipe = LTX2LatentUpsamplePipeline(
    vae=pipe.vae, latent_upsampler=latent_upsampler
)
```

---

## Model Identifiers on Hugging Face Hub

| Model ID | Description |
|----------|-------------|
| `Lightricks/LTX-Video` | Base v1 model hub (all variants) |
| `Lightricks/LTX-Video-0.9.8-dev` | 13B full quality dev model |
| `Lightricks/LTX-Video-0.9.8-distilled` | 13B distilled for speed |
| `Lightricks/ltxv-spatial-upscaler-0.9.8` | v1 spatial upscaler |
| `Lightricks/LTX-2` | LTX-2 19B base model |
| `Lightricks/LTX-2.3` | LTX-2.3 22B latest model |
| `Lightricks/LTX-2.3-fp8` | LTX-2.3 quantized variant |

---

## Memory Optimization Techniques

### FP8 Layerwise Weight Casting (~10GB VRAM)

```python
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading

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

# Group offloading
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
```

### torch.compile Optimization

```python
pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)
```

### CPU Offloading

```python
pipe.enable_model_cpu_offload()
# or
pipe.enable_sequential_cpu_offload()
```

### VAE Tiling (for large resolutions)

```python
pipe.vae.enable_tiling()
```

---

## Two-Stage Inference Pattern (Recommended)

1. **Stage 1:** Generate at 2/3 of target resolution (`output_type="latent"`)
2. **Stage 2:** Upscale 2x using latent upsampler
3. **Stage 3:** Denoise upscaled video (`denoise_strength=0.4`, `num_inference_steps=10`)
4. **Stage 4:** Downscale to final target resolution

---

## Recommended Settings

| Setting | Guidance-Distilled | Full Model |
|---------|-------------------|------------|
| `guidance_scale` | 1.0 | 5.0 |
| `decode_timestep` (v0.9.1+) | 0.05 | 0.05 |
| `image_cond_noise_scale` (v0.9.1+) | 0.025 | 0.025 |
| `dtype` | torch.bfloat16 | torch.bfloat16 |

---

## Utility Functions

```python
from diffusers.utils import (
    export_to_video,    # Export PIL frames to MP4
    load_image,         # Load image from path or URL
    load_video,         # Load video as list of PIL frames
)
```

---

## Documentation Links

- **LTX-Video Diffusers:** https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
- **LTX-2 Diffusers:** https://huggingface.co/docs/diffusers/main/api/pipelines/ltx2
- **Memory Optimization:** https://huggingface.co/docs/diffusers/main/optimization/memory
- **Pipeline Source:** https://github.com/huggingface/diffusers/tree/main/src/diffusers/pipelines/ltx
