---
title: LTX Video Schedulers Reference
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/schedulers/flow_match_euler_discrete
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
tags:
  - scheduler
  - flow-matching
  - inference
  - timesteps
  - python
---

# LTX Video Schedulers Reference

LTX Video uses flow-matching based schedulers for the denoising process. Two schedulers are available depending on the use case.

## FlowMatchEulerDiscreteScheduler (Default)

The default scheduler for all LTX Video pipelines, based on flow-matching sampling introduced in Stable Diffusion 3.

### Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `num_train_timesteps` | int | 1000 | Number of diffusion training steps |
| `shift` | float | 1.0 | Shift value for timestep schedule |
| `use_dynamic_shifting` | bool | False | Resolution-dependent timestep shifting |
| `base_shift` | float | 0.5 | Stabilizes generation (higher = more consistent) |
| `max_shift` | float | 1.15 | Max variation (higher = more varied/stylized) |
| `base_image_seq_len` | int | 256 | Base image sequence length |
| `max_image_seq_len` | int | 4096 | Maximum image sequence length |
| `invert_sigmas` | bool | False | Whether to invert the sigmas |
| `shift_terminal` | float | None | End value of shifted schedule (LTX-specific) |
| `use_karras_sigmas` | bool | False | Karras sigma schedule |
| `use_exponential_sigmas` | bool | False | Exponential sigma schedule |
| `use_beta_sigmas` | bool | False | Beta sigma schedule |
| `time_shift_type` | str | "exponential" | Type of dynamic shifting |
| `stochastic_sampling` | bool | False | Enable stochastic sampling |

### Key Methods

#### set_timesteps

```python
scheduler.set_timesteps(
    num_inference_steps=50,
    device="cuda",
    sigmas=None,
    mu=None,
    timesteps=None
)
```

#### step

```python
output = scheduler.step(
    model_output=noise_pred,
    timestep=t,
    sample=latent_sample,
    s_churn=0.0,
    s_tmin=0.0,
    s_tmax=float("inf"),
    s_noise=1.0,
    generator=None,
    return_dict=True
)
prev_sample = output.prev_sample
```

#### scale_noise (Forward Process)

```python
noisy = scheduler.scale_noise(
    sample=clean_latent,
    timestep=t,
    noise=noise
)
```

#### LTX-Specific Methods

- `set_shift(shift)` -- update the shift value
- `stretch_shift_to_terminal(t)` -- stretch and shift timestep schedule to terminate at `shift_terminal`
- `time_shift(mu, sigma, t)` -- apply time shifting to sigmas

### Usage

```python
from diffusers import FlowMatchEulerDiscreteScheduler, LTXPipeline

# Default usage (auto-loaded with pipeline)
pipeline = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
# pipeline.scheduler is already FlowMatchEulerDiscreteScheduler

# Custom configuration
scheduler = FlowMatchEulerDiscreteScheduler(
    num_train_timesteps=1000,
    shift=1.0,
)
pipeline.scheduler = scheduler
```

## LTXEulerAncestralRFScheduler

An alternative scheduler implementing Rectified Flow (RF) sampling with ancestral noise injection. Designed for ComfyUI parity with the [[ltx-long-multi-prompt-pipeline|LTXI2VLongMultiPromptPipeline]].

```python
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(
    pipe.scheduler.config
)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)
```

## Custom Timesteps for Distilled Models

Distilled models use specific timestep schedules instead of uniform spacing:

```python
# For base generation (before upscaling)
base_timesteps = [1000, 993, 987, 981, 975, 909, 725, 0.03]

# For upscaling refinement
upsample_timesteps = [1000, 909, 725, 421, 0]

video = pipeline(
    prompt="...",
    timesteps=base_timesteps,
    guidance_scale=1.0,  # Required for distilled models
).frames[0]
```

See [[two-stage-inference-pattern]] for the full distilled workflow.

## Scheduler Selection Guide

| Scheduler | Use Case | Typical Steps |
|-----------|----------|---------------|
| `FlowMatchEulerDiscreteScheduler` | Standard generation | 30-50 |
| `FlowMatchEulerDiscreteScheduler` + custom timesteps | Distilled models | 4-10 |
| `LTXEulerAncestralRFScheduler` | ComfyUI parity, long videos | 4-10 |

## Guidance Scale Recommendations

| Model Type | guidance_scale | guidance_rescale | Notes |
|-----------|---------------|-----------------|-------|
| Base (non-distilled) | 3.0 - 7.0 | 0.0 - 0.7 | Higher = more prompt adherence |
| Guidance-distilled | 1.0 | 0.7 | Must be 1.0 |
| With upscaling refinement | Same as base | 0.7 | Keep consistent |

## Timestep-Aware VAE Decode Settings

For LTX Video v0.9.1+, the [[ltxv-video-vae|VAE]] performs timestep-aware decoding:

| Parameter | Recommended | Description |
|-----------|-------------|-------------|
| `decode_timestep` | 0.05 | When to decode |
| `decode_noise_scale` | 0.025 | Noise interpolation |
| `image_cond_noise_scale` | 0.025 | For image conditioning |

## See Also

- [[python-schedulers]] -- workflow-focused scheduler configuration guide
- [[diffusers-pipeline-overview]] -- pipeline classes that use these schedulers
- [[ltxv-video-vae]] -- timestep-aware decoding
- [[two-stage-inference-pattern]] -- distilled model workflow with custom timesteps
- [[ltxv-model-variants]] -- distilled vs full model differences
