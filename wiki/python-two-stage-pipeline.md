---
title: Python Two-Stage Pipeline (Latent Upscaling)
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
tags:
  - python
  - two-stage
  - upscaling
  - latent
  - production
---

# Python Two-Stage Pipeline (Latent Upscaling)

The recommended production pattern for LTX-Video: generate at reduced resolution, upscale in latent space, then refine. This approach yields better quality/speed tradeoffs than direct high-resolution generation.

## Diffusers Two-Stage Pattern

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video

# Load both pipelines sharing the VAE
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8",
    vae=pipe.vae,  # Share VAE to save memory
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")
pipe_upsample.to("cuda")
pipe.vae.enable_tiling()

# Stage 1: Generate at reduced resolution
latents = pipe(
    prompt="A dramatic ocean scene",
    width=512, height=320, num_frames=97,
    num_inference_steps=30,
    output_type="latent",
    generator=torch.Generator().manual_seed(42),
).frames

# Stage 2: Upscale latents (2x)
upscaled = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    tone_map_compression_ratio=0.6,
    output_type="latent",
).frames

# Stage 3: Refine upscaled video
video = pipe(
    prompt="A dramatic ocean scene",
    width=1024, height=640, num_frames=97,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled,
    output_type="pil",
    generator=torch.Generator().manual_seed(42),
).frames[0]

export_to_video(video, "output_upscaled.mp4", fps=24)
```

## LTX-2 Native Two-Stage Pipeline

The [[sdk-ltx-2]] provides `TI2VidTwoStagesPipeline` which handles two-stage generation internally:

```python
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
)

pipeline(
    prompt="A dramatic ocean scene",
    output_path="output.mp4",
    seed=42,
    height=512, width=768, num_frames=121,
    num_inference_steps=40,
)
```

Default step counts: 40 steps for stage 1, 8 steps for stage 2.

## Higher Quality Variant

`TI2VidTwoStagesHQPipeline` uses a res_2s second-order sampler for better quality:

```python
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesHQPipeline
```

## Available Upscalers (LTX-2.3)

| Upscaler | Factor | File |
|----------|--------|------|
| Spatial 2x | 2x resolution | `ltx-2.3-spatial-upscaler-x2-1.1.safetensors` |
| Spatial 1.5x | 1.5x resolution | `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` |
| Temporal 2x | 2x frame count | `ltx-2.3-temporal-upscaler-x2-1.0.safetensors` |

## Distilled Model Timesteps

When using distilled models in two-stage mode, use specific [[python-schedulers]] timestep schedules:

- **Stage 1 (generation):** `[1000, 993, 987, 981, 975, 909, 725, 0.03]`
- **Stage 2 (refinement):** `[1000, 909, 725, 421, 0]`

Both stages require `guidance_scale=1.0` for distilled models.

## See Also

- [[python-conditioning]] -- Conditioning types for the generation stage
- [[python-performance-memory]] -- Memory optimization for two-pipeline setup
- [[ltx-2-pipeline-api]] -- All available LTX-2 pipeline variants
- [[python-long-video-generation]] -- Temporal tiling for videos beyond native frame limits
