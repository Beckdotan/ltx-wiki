---
title: Python Long Video Generation
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - python
  - long-video
  - temporal-tiling
  - multi-prompt
  - diffusers
---

# Python Long Video Generation

For videos exceeding the model's native frame limit, `LTXI2VLongMultiPromptPipeline` provides temporal tiling with multi-prompt support.

## Basic Usage

```python
import torch
from diffusers import LTXI2VLongMultiPromptPipeline, LTXEulerAncestralRFScheduler
from diffusers.utils import export_to_video, load_image

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

cond_image = load_image("first_frame.png")

out = pipe(
    prompt="A person walks through a city | They enter a park | They sit on a bench and read",
    cond_image=cond_image,
    cond_strength=0.5,
    num_frames=257,
    height=512,
    width=704,
    temporal_tile_size=80,           # Window size in frames
    temporal_overlap=24,              # Overlap between windows
    temporal_overlap_cond_strength=0.5,  # Cross-window conditioning
    adain_factor=0.25,               # Cross-window consistency
    num_inference_steps=8,
    guidance_scale=1.0,
    seed=42,
    output_type="pil",
)

export_to_video(out.frames[0], "long_video.mp4", fps=25)
```

## Multi-Prompt with Pipe Separator

Use `|` to separate prompts for different temporal segments:

```python
prompt = "A cat sleeping peacefully | The cat wakes up and stretches | The cat walks to its food bowl"
```

## Fine-Grained Prompt Segments

For precise control over which prompt applies to which temporal windows:

```python
out = pipe(
    prompt_segments=[
        {"start_window": 0, "end_window": 2, "text": "A cat sleeping peacefully"},
        {"start_window": 2, "end_window": 4, "text": "The cat wakes up and stretches"},
        {"start_window": 4, "end_window": 6, "text": "The cat walks to its food bowl"},
    ],
    num_frames=257,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    output_type="pil",
)
```

## Key Parameters

| Parameter | Description | Typical Value |
|-----------|-------------|---------------|
| `temporal_tile_size` | Window size in frames | 80 |
| `temporal_overlap` | Overlap between temporal windows | 24 |
| `temporal_overlap_cond_strength` | Cross-window conditioning strength | 0.5 |
| `adain_factor` | Cross-window style consistency | 0.25 |
| `cond_strength` | First-frame conditioning strength | 0.5 |

## Scheduler Requirement

This pipeline uses `LTXEulerAncestralRFScheduler` for ComfyUI parity. See [[python-schedulers]] for details.

## See Also

- [[python-conditioning]] -- Image and video conditioning
- [[python-schedulers]] -- LTXEulerAncestralRFScheduler for ComfyUI parity
- [[python-two-stage-pipeline]] -- Two-stage generation for higher resolution
