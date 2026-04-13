---
title: Python Conditioning Types
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
tags:
  - python
  - conditioning
  - image-to-video
  - video-to-video
  - diffusers
---

# Python Conditioning Types

LTX-Video supports three distinct conditioning mechanisms via `LTXConditionPipeline` (v0.9.5+), enabling image-to-video, video-to-video, and multi-keyframe generation through the [[diffusers-integration]] library.

## Conditioning Mechanisms

### 1. Replacing Latents (Strongest Control)

Directly substitutes latent values at specific frames. Provides the strongest frame-level control.

```python
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
).to("cuda")

condition = LTXVideoCondition(image=image, frame_index=0, strength=1.0)

video = pipe(
    conditions=[condition],
    prompt="...",
    num_frames=161,
    num_inference_steps=40,
).frames[0]
```

### 2. Guiding Latents (Smoother Interpolation)

Additively conditions on images for smoother interpolation between keyframes:

```python
condition1 = LTXVideoCondition(image=start_image, frame_index=0)
condition2 = LTXVideoCondition(image=end_image, frame_index=160)

video = pipe(
    conditions=[condition1, condition2],
    prompt="A smooth transition from day to night",
    num_frames=161,
).frames[0]
```

### 3. Video Conditioning (Vid2Vid)

Full reference video guidance for video-to-video transformations:

```python
from diffusers.utils import load_video

input_video = load_video("path/to/reference.mp4")
condition = LTXVideoCondition(video=input_video, frame_index=0)

video = pipe(
    conditions=[condition],
    prompt="Transform to anime style...",
    denoise_strength=0.6,
    num_frames=161,
).frames[0]
```

## Multi-Condition Generation

Multiple conditions can be placed at arbitrary frame positions:

```python
condition1 = LTXVideoCondition(image=image, frame_index=0)
condition2 = LTXVideoCondition(video=video_clip, frame_index=80)
condition3 = LTXVideoCondition(image=end_image, frame_index=160)

video = pipe(
    conditions=[condition1, condition2, condition3],
    prompt="A smooth transition between scenes",
    negative_prompt="worst quality, blurry, jittery",
    width=768, height=512, num_frames=161,
    num_inference_steps=40,
).frames[0]
```

## Conditioning Strength Control

```python
# Strong conditioning (preserves input closely)
condition = LTXVideoCondition(image=image, frame_index=0, strength=1.0)

# Weak conditioning (more creative freedom)
condition = LTXVideoCondition(image=image, frame_index=0, strength=0.5)
```

## Image Conditioning Noise Scale

The `image_cond_noise_scale` parameter adds timestep-dependent noise to conditioning latents, helping with motion continuity when conditioned on a single frame:

```python
video = pipe(
    conditions=[condition],
    prompt="...",
    image_cond_noise_scale=0.15,  # Default; use 0.0 for 0.9.7+ with upscaler
    decode_timestep=0.05,
    decode_noise_scale=0.025,
).frames[0]
```

Lower values keep output closer to the conditioning image.

## Denoise Strength for Vid2Vid

Controls how much the generation deviates from the input:

- **Low denoise (0.3)** -- subtle changes (style transfer)
- **High denoise (0.8)** -- major changes (creative reinterpretation)

```python
video = pipeline(
    conditions=[condition],
    prompt="Oil painting style scene",
    denoise_strength=0.3,
    num_inference_steps=30,
).frames[0]
```

## See Also

- [[python-two-stage-pipeline]] -- Two-stage generation with latent upscaling
- [[python-long-video-generation]] -- Long video with temporal tiling
- [[python-guidance-parameters]] -- Guidance scale and rescale settings
- [[python-integration-patterns]] -- Combining with SDXL/FLUX for first-frame generation
