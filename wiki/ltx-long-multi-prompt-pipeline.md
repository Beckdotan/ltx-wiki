---
title: LTXI2VLongMultiPromptPipeline
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - diffusers
  - pipeline
  - long-video
  - multi-prompt
  - python
  - code-example
---

# LTXI2VLongMultiPromptPipeline

`LTXI2VLongMultiPromptPipeline` generates long-duration videos using temporal sliding windows and per-window prompt scheduling. It supports up to 361 frames and smooth scene transitions.

This pipeline is designed for use with the v0.9.8 distilled 13B model and uses the [[ltxv-schedulers|LTXEulerAncestralRFScheduler]] for ComfyUI parity.

## Key Features

- Temporal sliding-window sampling (no spatial H/W sharding)
- Autoregressive fusion across windows
- Multi-prompt segmentation per window with smooth transitions
- First-frame hard conditioning via per-token mask for image-to-video
- VRAM control via temporal windowing and VAE tiled decoding

## Basic Text-to-Video (Multi-Prompt)

Use `|` to separate prompts for different temporal windows:

```python
import torch
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

out = pipe(
    prompt="a chimpanzee walks | a chimpanzee eats",
    num_frames=161,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    output_type="pil",
    return_dict=True,
)
frames = out.frames[0]
```

## Image-to-Video with Scene Transitions

Condition on a first-frame image while using multi-prompt for scene changes:

```python
from diffusers.utils import load_image, export_to_video

cond_image = load_image("first_frame.png")

out = pipe(
    prompt="a person walks through a forest | the person arrives at a clearing | sunset over the clearing",
    cond_image=cond_image,
    cond_strength=0.5,
    num_frames=257,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    num_inference_steps=8,
    guidance_scale=1.0,
    seed=42,
    output_type="pil",
    return_dict=True,
)
export_to_video(out.frames[0], "long_video.mp4", fps=25)
```

## Precise Prompt Segments

For fine-grained control, use `prompt_segments` instead of pipe-separated prompts:

```python
out = pipe(
    prompt_segments=[
        {"start_window": 0, "end_window": 2, "text": "a chimpanzee walks through a forest"},
        {"start_window": 2, "end_window": 4, "text": "a chimpanzee picks up a banana and eats it"},
    ],
    num_frames=321,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    output_type="pil",
)
```

## Latent Output for Later Decoding

```python
out_latent = pipe(
    prompt="a chimpanzee walking",
    output_type="latent",
    return_dict=True
).frames
frames = pipe.vae_decode_tiled(out_latent, output_type="pil")[0]
```

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `temporal_tile_size` | 80 | Window size in decoded frames |
| `temporal_overlap` | 24 | Overlap between windows |
| `temporal_overlap_cond_strength` | 0.5 | Strength for cross-window injection |
| `adain_factor` | 0.25 | Cross-window consistency normalization |
| `cond_image` | None | First-frame conditioning image |
| `cond_strength` | 0.5 | First-frame conditioning strength |
| `prompt_segments` | None | Per-window prompt mapping `[{"start_window", "end_window", "text"}]` |

## See Also

- [[diffusers-pipeline-overview]] -- all pipeline classes
- [[ltxv-schedulers]] -- LTXEulerAncestralRFScheduler details
- [[ltx-condition-pipeline]] -- alternative for shorter multi-condition videos
- [[ltxv-model-variants]] -- model selection
