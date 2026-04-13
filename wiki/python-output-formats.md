---
title: Python Output Formats and Resolution Handling
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - python
  - output
  - resolution
  - frames
  - vae
---

# Python Output Formats and Resolution Handling

Reference for output types, resolution constraints, and frame count requirements when working with LTX-Video in Python.

## Output Types

### PIL Images (Default)

```python
result = pipeline(prompt="...", output_type="pil").frames[0]
# result is a list of PIL.Image.Image

for i, frame in enumerate(result):
    frame.save(f"frame_{i:04d}.png")
```

### NumPy Arrays

```python
result = pipeline(prompt="...", output_type="np").frames
# shape: (batch, frames, height, width, channels)
```

### PyTorch Tensors

```python
result = pipeline(prompt="...", output_type="pt").frames
# shape: (batch, frames, channels, height, width)
```

### Latent Output (for Further Processing)

Used in [[python-two-stage-pipeline]] for passing to the upsampler:

```python
latents = pipeline(prompt="...", output_type="latent").frames
# torch.Tensor in normalized latent space
```

## Resolution Requirements

### VAE-Compatible Dimensions

Resolution must be divisible by the VAE spatial compression ratio (32):

```python
def round_to_nearest_resolution_acceptable_by_vae(pipeline, height, width):
    height = height - (height % pipeline.vae_spatial_compression_ratio)
    width = width - (width % pipeline.vae_spatial_compression_ratio)
    return height, width

height, width = round_to_nearest_resolution_acceptable_by_vae(pipeline, 720, 1280)
```

### Frame Count Requirements

Frame count must follow the 8n+1 pattern:

```python
def get_valid_frame_count(desired_frames):
    """Return the nearest valid frame count (8n+1 pattern)."""
    n = round((desired_frames - 1) / 8)
    return 8 * n + 1

# Valid: 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 121, 161, 257, 361
frames = get_valid_frame_count(100)  # Returns 97
```

## Prompt Enhancement

LTX-Video supports automatic prompt enhancement to improve generation quality:

```python
from ltx_video.inference import infer, InferenceConfig

infer(
    InferenceConfig(
        pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
        prompt="A sunset",
        enhance_prompt=True,  # Automatically enriches the prompt
        output_path="output.mp4",
    )
)
```

### Prompting Guidelines

Effective prompts should be approximately 200 words with detailed, chronological descriptions covering:

1. Primary action statement
2. Specific movement details
3. Character/object appearance descriptions
4. Environmental context
5. Camera angles and movements
6. Lighting and color specifications
7. Scene changes or events

## See Also

- [[python-two-stage-pipeline]] -- Using latent output for upscaling
- [[python-performance-memory]] -- Resolution impact on VRAM usage
- [[python-long-video-generation]] -- Generating beyond native frame limits
