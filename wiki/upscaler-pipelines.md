---
title: Upscaler Pipelines
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-upscaler-pipelines.md
tags:
  - upscaler
  - spatial-upscaler
  - temporal-upscaler
  - latent-space
  - pipeline
  - ltx-video
---

# Upscaler Pipelines

[[ltx-video-overview|LTX-Video]] includes dedicated upscaler models that operate in latent space to enhance video resolution and frame count. The recommended workflow is a multi-step pipeline: generate at lower resolution, upscale in latent space, then denoise for final quality.

## Available Upscalers

| Model | HuggingFace ID | Purpose |
|-------|----------------|---------|
| Spatial Upscaler | `Lightricks/ltxv-spatial-upscaler-0.9.8` | 2x spatial resolution increase |
| Temporal Upscaler | `Lightricks/ltxv-temporal-upscaler-0.9.8` | Frame interpolation / temporal upscaling |

## Why Use the Multi-Step Pipeline?

| Approach | Quality | Speed | VRAM |
|----------|---------|-------|------|
| Direct generation at full resolution | Good | Slow | High |
| Generate low-res + upscale + denoise | Best | Moderate | Lower peak |
| Generate low-res only | Lower | Fast | Low |

The multi-step approach produces the best quality because:

1. Base generation at lower resolution is faster and uses less VRAM
2. The latent upscaler adds spatial detail efficiently
3. A denoising pass at full resolution cleans up upscaling artifacts
4. Final resize ensures exact target dimensions

## Spatial Upscaler

The spatial upscaler doubles the resolution of generated video in latent space.

### Loading

```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8",
    vae=pipe.vae,  # Share VAE between pipelines
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")
pipe_upsample.to("cuda")
```

### Full Workflow

```python
# Step 1: Generate at 2/3 target resolution
downscale_factor = 2 / 3
target_h, target_w = 480, 832
ds_h = int(target_h * downscale_factor)
ds_w = int(target_w * downscale_factor)

# Round to VAE-compatible dimensions
ds_h = ds_h - (ds_h % pipe.vae_spatial_compression_ratio)
ds_w = ds_w - (ds_w % pipe.vae_spatial_compression_ratio)

latents = pipe(
    conditions=[condition],
    prompt=prompt,
    width=ds_w,
    height=ds_h,
    num_frames=96,
    num_inference_steps=30,
    output_type="latent",  # Output latents, not pixels
).frames

# Step 2: Upscale latents (2x spatial)
upscaled_latents = pipe_upsample(
    latents=latents,
    output_type="latent",
).frames

# Step 3: Denoise upscaled latents
final_video = pipe(
    conditions=[condition],
    prompt=prompt,
    width=ds_h * 2,
    height=ds_w * 2,
    num_frames=96,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled_latents,
    decode_timestep=0.05,
    image_cond_noise_scale=0.025,
    output_type="pil",
).frames[0]

# Step 4: Resize to exact target resolution
final_video = [f.resize((target_w, target_h)) for f in final_video]
export_to_video(final_video, "output.mp4", fps=24)
```

## Temporal Upscaler

The temporal upscaler doubles the number of frames via latent-space frame interpolation:

```python
pipe_temporal = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-temporal-upscaler-0.9.8",
    vae=pipe.vae,
    torch_dtype=torch.bfloat16
).to("cuda")

upscaled_temporal = pipe_temporal(
    latents=latents,
    output_type="latent",
).frames
```

## Combining Spatial and Temporal Upscaling

For maximum quality, apply both upscalers in sequence:

```python
# 1. Generate base video at low resolution
latents = pipe(..., output_type="latent").frames

# 2. Temporal upscale (double frames)
latents = pipe_temporal(latents=latents, output_type="latent").frames

# 3. Spatial upscale (double resolution)
latents = pipe_spatial(latents=latents, output_type="latent").frames

# 4. Final denoise pass
video = pipe(..., latents=latents, denoise_strength=0.4, output_type="pil").frames[0]
```

## Key Parameters for Upscale + Denoise

| Parameter | Recommended Value | Description |
|-----------|------------------|-------------|
| `denoise_strength` | 0.3-0.5 | How much to re-denoise (lower = more faithful to upscaled result) |
| `num_inference_steps` | 8-15 | Fewer steps needed for refinement |
| `decode_timestep` | 0.05 | Controls VAE decoding timing |
| `image_cond_noise_scale` | 0.025 | Noise added to conditioning image during refinement |

## See Also

- [[installation-quickstart]] -- Getting started with LTX-Video
- [[hardware-requirements]] -- VRAM and GPU guidance
- [[prompt-engineering]] -- Writing effective prompts
