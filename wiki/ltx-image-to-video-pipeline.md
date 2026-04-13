---
title: LTXImageToVideoPipeline
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - diffusers
  - pipeline
  - image-to-video
  - python
  - code-example
---

# LTXImageToVideoPipeline

`LTXImageToVideoPipeline` is the dedicated Diffusers pipeline for generating video from a single input image. It works with the original 2B model (`Lightricks/LTX-Video`).

For v0.9.5+ models, the more flexible [[ltx-condition-pipeline|LTXConditionPipeline]] is recommended, as it supports multi-condition generation and video-to-video editing in addition to image-to-video.

## Basic Usage

```python
import torch
from diffusers import LTXImageToVideoPipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXImageToVideoPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

image = load_image(
    "https://huggingface.co/datasets/a-r-r-o-w/tiny-meme-dataset-captioned/resolve/main/images/8.png"
)
prompt = "A young girl stands calmly in the foreground, looking directly at the camera, as a house fire rages in the background. Flames engulf the structure, with smoke billowing into the air."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

video = pipe(
    image=image,
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

## Parameters

This pipeline accepts all standard [[diffusers-pipeline-overview|shared parameters]] plus:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `image` | None | Input PIL image or tensor |

## When to Use This vs LTXConditionPipeline

| Feature | LTXImageToVideoPipeline | [[ltx-condition-pipeline|LTXConditionPipeline]] |
|---------|------------------------|----------------------------------------------|
| Single image conditioning | Yes | Yes |
| Multi-condition (image + video) | No | Yes |
| Video-to-video editing | No | Yes |
| Frame-level targeting | No | Yes |
| Supported models | 2B original | 0.9.5+ (all) |

## See Also

- [[ltx-condition-pipeline]] -- recommended multi-condition pipeline for 0.9.5+
- [[ltx-pipeline-text-to-video]] -- text-only generation
- [[diffusers-pipeline-overview]] -- all pipeline classes
