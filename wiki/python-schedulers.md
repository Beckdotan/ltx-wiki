---
title: Python Scheduler Configuration
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/main/api/schedulers/flow_match_euler_discrete
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
tags:
  - python
  - scheduler
  - diffusers
  - timesteps
  - distilled
---

# Python Scheduler Configuration

LTX-Video uses noise scheduling algorithms to control the denoising process. The scheduler can be swapped and configured to match different model variants and quality requirements.

## Default: FlowMatchEulerDiscreteScheduler

The default scheduler for all LTX-Video models in [[diffusers-integration]]:

```python
from diffusers import FlowMatchEulerDiscreteScheduler

scheduler = FlowMatchEulerDiscreteScheduler(
    num_train_timesteps=1000,
    shift=1.0,
    use_dynamic_shifting=True,   # Resolution-dependent shifting
    base_shift=0.5,
    max_shift=1.15,
    time_shift_type="exponential",
)

pipe.scheduler = scheduler
```

Basic usage (accept defaults):

```python
from diffusers import FlowMatchEulerDiscreteScheduler

scheduler = FlowMatchEulerDiscreteScheduler()
pipeline.scheduler = scheduler
```

## LTXEulerAncestralRFScheduler (ComfyUI Parity)

Use this scheduler when you need results matching ComfyUI outputs, particularly with the [[python-long-video-generation]] pipeline:

```python
from diffusers import LTXEulerAncestralRFScheduler

pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
```

## Custom Timesteps for Distilled Models

Distilled models use specific timestep schedules rather than a fixed step count for optimal results. This is a key part of [[python-performance-speed]] optimization:

```python
# Base model generation timesteps (distilled, 8 steps)
base_timesteps = [1000, 993, 987, 981, 975, 909, 725, 0.03]

# Upscaling refinement timesteps (distilled, 5 steps)
upsample_timesteps = [1000, 909, 725, 421, 0]

video = pipeline(
    prompt="...",
    timesteps=base_timesteps,
    guidance_scale=1.0,  # Must be 1.0 for distilled models
    output_type="latent",
).frames
```

For the upscaling stage in [[python-two-stage-pipeline]]:

```python
video = pipe(
    prompt=prompt,
    timesteps=[1000, 909, 725, 421, 0],
    denoise_strength=0.999,
    guidance_scale=1.0,
    latents=upscaled_latents,
).frames[0]
```

## Gradient Estimation for Fewer Steps

The [[sdk-ltx-2]] provides a gradient estimation utility that reduces inference from 40 to 20-30 steps while maintaining quality:

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

## See Also

- [[python-performance-speed]] -- Distilled models and step reduction
- [[python-guidance-parameters]] -- CFG and guidance rescale settings
- [[python-two-stage-pipeline]] -- Two-stage pipeline with separate timestep schedules
