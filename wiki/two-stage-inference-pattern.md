---
title: Two-Stage Inference Pattern
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - inference
  - upscaling
  - workflow
  - pattern
  - python
  - code-example
---

# Two-Stage Inference Pattern

The two-stage inference pattern is the recommended workflow for high-quality LTX Video generation starting with v0.9.7. It generates video at a reduced resolution first, upscales 2x in latent space, then optionally refines the upscaled result.

## Workflow Overview

1. **Stage 1:** Generate at 2/3 of target resolution (`output_type="latent"`)
2. **Stage 2:** Upscale 2x using the [[ltx-latent-upsample-pipeline|LTXLatentUpsamplePipeline]]
3. **Stage 3:** Denoise the upscaled video (`denoise_strength=0.4`, `num_inference_steps=10`) -- optional but recommended
4. **Stage 4:** Downscale to final target resolution

## Full Model Example (0.9.7 Dev)

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipeline_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae,
    torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipeline_upsample.to("cuda")
pipeline.vae.enable_tiling()

def round_to_nearest_resolution_acceptable_by_vae(height, width):
    height = height - (height % pipeline.vae_temporal_compression_ratio)
    width = width - (width % pipeline.vae_temporal_compression_ratio)
    return height, width

prompt = "A cinematic mountain landscape with clouds..."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 768, 1152
downscale_factor = 2 / 3
num_frames = 161

# Stage 1: Generate at reduced resolution
downscaled_height = int(expected_height * downscale_factor)
downscaled_width = int(expected_width * downscale_factor)
downscaled_height, downscaled_width = round_to_nearest_resolution_acceptable_by_vae(
    downscaled_height, downscaled_width
)

latents = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=num_frames,
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=5.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Stage 2: Upscale latents (2x)
upscaled_height, upscaled_width = downscaled_height * 2, downscaled_width * 2
upscaled_latents = pipeline_upsample(
    latents=latents,
    output_type="latent"
).frames

# Stage 3: Refine upscaled latents
video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=num_frames,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled_latents,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=5.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

# Stage 4: Downscale to target resolution
video = [frame.resize((expected_width, expected_height)) for frame in video]
export_to_video(video, "output.mp4", fps=24)
```

## Distilled Model Example (Fast, 4-10 Steps)

Distilled models use custom timestep schedules and `guidance_scale=1.0`:

```python
pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-distilled", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae, torch_dtype=torch.bfloat16
)

# Stage 1: Custom distilled timesteps
latents = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=161,
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=1.0,       # Must be 1.0 for distilled
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Stage 2: Upscale with AdaIN
upscaled_latents = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    output_type="latent"
).frames

# Stage 3: Refine with upscaling timesteps
video = pipeline(
    prompt=prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=161,
    denoise_strength=0.999,
    timesteps=[1000, 909, 725, 421, 0],
    latents=upscaled_latents,
    guidance_scale=1.0,
    output_type="pil",
).frames[0]
```

## v0.9.8 Long Video with Tone Mapping

The 0.9.8 models add `tone_map_compression_ratio` for improved upscaling quality and support up to 361 frames:

```python
from diffusers.pipelines.ltx.modeling_latent_upsampler import LTXLatentUpsamplerModel

upsampler = LTXLatentUpsamplerModel.from_pretrained(
    "a-r-r-o-w/LTX-0.9.8-Latent-Upsampler", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline(
    vae=pipeline.vae, latent_upsampler=upsampler
).to(torch.bfloat16)

# Upscale with tone mapping
upscaled_latents = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    tone_map_compression_ratio=0.6,  # New in 0.9.8
    output_type="latent"
).frames
```

## Key Settings Summary

| Setting | Full Model | Distilled Model |
|---------|-----------|----------------|
| Stage 1 steps | 30 | Custom timesteps (8 values) |
| Stage 3 steps | 10 | Custom timesteps (5 values) |
| `guidance_scale` | 5.0 | 1.0 |
| `denoise_strength` (Stage 3) | 0.4 | 0.999 |
| `adain_factor` | 0.0 | 1.0 |
| `tone_map_compression_ratio` | 0.0 | 0.6 (0.9.8 only) |
| `downscale_factor` | 2/3 | 2/3 |

## See Also

- [[ltx-latent-upsample-pipeline]] -- the upscaler pipeline details
- [[ltx-condition-pipeline]] -- the main generation pipeline
- [[ltxv-schedulers]] -- custom timestep schedules for distilled models
- [[ltxv-model-variants]] -- choosing between dev and distilled
- [[ltxv-memory-optimization]] -- VAE tiling for upscaled resolutions
