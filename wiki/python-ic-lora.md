---
title: Python IC-LoRA (Identity-Consistent LoRA)
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/open-source-model/usage-guides/lo-ra
  - https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Motion-Track-Control
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
tags:
  - python
  - ic-lora
  - video-control
  - depth
  - pose
  - canny
  - motion-tracking
---

# Python IC-LoRA (Identity-Consistent LoRA)

IC-LoRA enables conditioning video generation on reference video frames at inference time, allowing fine-grained video-to-video control. Unlike standard [[python-lora-loading]], IC-LoRA models are designed for structural control rather than style transfer.

## IC-LoRA Types

Four official control types from Lightricks:

- **Depth Map Control** -- conditions on depth maps extracted from reference video
- **Human Pose Control** -- conditions on pose skeletons
- **Canny Edge Control** -- conditions on edge detection maps
- **Motion Track Control** -- conditions on motion tracking data

## Usage with LTX-2 Pipelines

```python
from ltx_pipelines.ic_lora import ICLoraPipeline

pipeline = ICLoraPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    ic_lora_path="/path/to/ic_lora.safetensors",
)

pipeline(
    prompt="A person dancing in the rain",
    reference_video="path/to/reference.mp4",
    output_path="output.mp4",
    seed=42,
    height=512,
    width=768,
    num_frames=121,
)
```

## Strength Guidelines

IC-LoRA control models are designed for full strength (1.0). Unlike standard LoRAs:

- Always set IC-LoRA to strength 1.0
- Do not mix multiple IC-LoRA control types simultaneously
- Standard effect LoRAs can be combined alongside a single IC-LoRA

## ComfyUI Nodes

IC-LoRA is also available via ComfyUI custom nodes:

- `LTXICLoRALoaderModelOnly` -- loads the IC-LoRA model
- `LTXAddVideoICLoRAGuide` -- applies IC-LoRA guidance to the generation

## See Also

- [[python-lora-loading]] -- Standard LoRA loading and management
- [[ltx-2-pipeline-api]] -- LTX-2 pipeline architecture including ICLoraPipeline
- [[sdk-ltx-2]] -- LTX-2 monorepo structure
