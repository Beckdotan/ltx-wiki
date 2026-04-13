# LTX Video - Schedulers Reference

> **Source:** https://huggingface.co/docs/diffusers/api/schedulers/flow_match_euler_discrete
> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video

## FlowMatchEulerDiscreteScheduler (Default)

The default scheduler for LTX Video, based on flow-matching sampling introduced in Stable Diffusion 3.

### Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `num_train_timesteps` | `int` | `1000` | Number of diffusion training steps |
| `shift` | `float` | `1.0` | Shift value for timestep schedule |
| `use_dynamic_shifting` | `bool` | `False` | Resolution-dependent timestep shifting |
| `base_shift` | `float` | `0.5` | Stabilizes generation (higher = more consistent) |
| `max_shift` | `float` | `1.15` | Max variation allowed (higher = more varied) |
| `base_image_seq_len` | `int` | `256` | Base image sequence length |
| `max_image_seq_len` | `int` | `4096` | Maximum image sequence length |
| `invert_sigmas` | `bool` | `False` | Whether to invert the sigmas |
| `shift_terminal` | `float` | `None` | End value of shifted timestep schedule |
| `use_karras_sigmas` | `bool` | `False` | Use Karras sigmas for step sizes |
| `use_exponential_sigmas` | `bool` | `False` | Use exponential sigmas |
| `use_beta_sigmas` | `bool` | `False` | Use beta sigmas |
| `time_shift_type` | `str` | `"exponential"` | Type of dynamic shifting: "exponential" or "linear" |
| `stochastic_sampling` | `bool` | `False` | Enable stochastic sampling |

### Key Methods

#### set_timesteps

```python
scheduler.set_timesteps(
    num_inference_steps=50,
    device="cuda",
    sigmas=None,        # Optional custom sigmas
    mu=None,            # Resolution-dependent shifting
    timesteps=None      # Optional custom timesteps
)
```

#### step

```python
# Predict sample from previous timestep (reverse SDE)
output = scheduler.step(
    model_output=noise_pred,    # Model's predicted noise
    timestep=t,                 # Current timestep
    sample=latent_sample,       # Current noisy latent
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
# Add noise to sample (for training or conditioning)
noisy = scheduler.scale_noise(
    sample=clean_latent,
    timestep=t,
    noise=noise
)
```

#### stretch_shift_to_terminal

References the LTX-Video scheduler implementation:

```python
# Stretches and shifts timestep schedule to terminate at shift_terminal
adjusted_t = scheduler.stretch_shift_to_terminal(timesteps)
```

### Usage with LTX Video

```python
from diffusers import FlowMatchEulerDiscreteScheduler, LTXPipeline

# Default usage (scheduler is auto-loaded with pipeline)
pipeline = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
# pipeline.scheduler is already FlowMatchEulerDiscreteScheduler

# Custom configuration
scheduler = FlowMatchEulerDiscreteScheduler(
    num_train_timesteps=1000,
    shift=1.0,
)
pipeline.scheduler = scheduler
```

### Custom Timesteps for Distilled Models

Distilled models use specific timestep schedules instead of the default uniform schedule:

```python
# For base generation (before upscaling)
base_timesteps = [1000, 993, 987, 981, 975, 909, 725, 0.03]

# For upscaling refinement
upsample_timesteps = [1000, 909, 725, 421, 0]

video = pipeline(
    prompt="...",
    timesteps=base_timesteps,
    guidance_scale=1.0,  # Required for distilled models
    # ...
).frames[0]
```

## LTXEulerAncestralRFScheduler

An alternative scheduler designed for ComfyUI parity with the `LTXI2VLongMultiPromptPipeline`.

### Usage

```python
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)

# Swap scheduler for ComfyUI parity
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(
    pipe.scheduler.config
)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)
```

This scheduler implements RF (Rectified Flow) sampling with ancestral noise injection, providing results that match the ComfyUI LTX Video node output.

## Scheduler Selection Guide

| Scheduler | Use Case | Steps | Speed |
|-----------|----------|-------|-------|
| `FlowMatchEulerDiscreteScheduler` | Standard generation | 30-50 | Normal |
| `FlowMatchEulerDiscreteScheduler` + custom timesteps | Distilled models | 4-10 | Fast |
| `LTXEulerAncestralRFScheduler` | ComfyUI parity, long videos | 4-10 | Fast |

## Guidance Scale Recommendations

| Model Type | guidance_scale | guidance_rescale | Notes |
|-----------|---------------|-----------------|-------|
| Base (non-distilled) | 3.0 - 7.0 | 0.0 - 0.7 | Higher = more prompt adherence |
| Guidance-distilled | 1.0 | 0.7 | Must be 1.0 |
| With upscaling refinement | Same as base | 0.7 | Keep consistent |

## Timestep-Aware VAE Decode Settings

For LTX Video v0.9.1 and above, the VAE performs timestep-aware decoding:

| Parameter | Recommended Value | Description |
|-----------|-------------------|-------------|
| `decode_timestep` | `0.05` | When to decode (closer to 0 = cleaner) |
| `decode_noise_scale` | `0.025` | Noise interpolation at decode time |
| `image_cond_noise_scale` | `0.025` | For image conditioning |

These values are used in the pipeline call:

```python
video = pipeline(
    prompt="...",
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.025,
    # ...
).frames[0]
```
