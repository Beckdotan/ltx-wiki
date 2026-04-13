---
title: Python Callback Functions
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - python
  - callbacks
  - diffusers
  - progress
  - debugging
---

# Python Callback Functions

LTX-Video pipelines in [[diffusers-integration]] support `callback_on_step_end` for monitoring progress, inspecting intermediate states, and implementing custom logic during the denoising process.

## Progress Monitoring

```python
def progress_callback(pipeline, step, timestep, callback_kwargs):
    total_steps = pipeline._num_inference_steps
    print(f"Step {step}/{total_steps}, timestep: {timestep:.2f}")
    return callback_kwargs

video = pipeline(
    prompt="...",
    num_inference_steps=50,
    callback_on_step_end=progress_callback,
).frames[0]
```

## Intermediate Latent Inspection

Capture latent states at each denoising step for analysis or visualization:

```python
intermediates = []

def save_intermediates(pipeline, step, timestep, callback_kwargs):
    latents = callback_kwargs["latents"]
    intermediates.append(latents.clone())
    return callback_kwargs

video = pipeline(
    prompt="...",
    num_inference_steps=50,
    callback_on_step_end=save_intermediates,
    callback_on_step_end_tensor_inputs=["latents"],
).frames[0]

print(f"Captured {len(intermediates)} intermediate latent states")
```

The `callback_on_step_end_tensor_inputs` parameter specifies which tensors should be available in `callback_kwargs`.

## Early Stopping

```python
def early_stop_callback(pipeline, step, timestep, callback_kwargs):
    if step >= 30:  # Stop after 30 steps
        raise StopIteration()
    return callback_kwargs
```

## Latent Shape Logging

```python
def callback_fn(pipe, step, timestep, callback_kwargs):
    latents = callback_kwargs["latents"]
    print(f"Step {step}, timestep {timestep}, latent shape: {latents.shape}")
    return callback_kwargs

video = pipe(
    prompt=prompt,
    num_inference_steps=50,
    callback_on_step_end=callback_fn,
    callback_on_step_end_tensor_inputs=["latents"],
).frames[0]
```

## Dynamic LoRA Scale Scheduling

Callbacks can be used to adjust [[python-lora-loading]] strength during sampling:

```python
import torch

num_inference_steps = 30
lora_steps = 20
lora_scales = torch.linspace(1.5, 0.7, lora_steps).tolist()
lora_scales += [0.2] * (num_inference_steps - lora_steps + 1)

pipeline.set_adapters("my_lora", lora_scales[0])

def callback(pipeline, step, timestep, callback_kwargs):
    pipeline.set_adapters("my_lora", lora_scales[step + 1])
    return callback_kwargs

video = pipeline(
    prompt="...",
    num_inference_steps=num_inference_steps,
    callback_on_step_end=callback,
).frames[0]
```

## See Also

- [[python-lora-loading]] -- LoRA scale scheduling via callbacks
- [[python-batch-generation]] -- Progress monitoring for batch runs
- [[diffusers-integration]] -- Diffusers pipeline API overview
