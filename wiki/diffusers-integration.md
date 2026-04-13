---
title: Diffusers Integration
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx2
  - https://huggingface.co/Lightricks/LTX-Video
  - https://huggingface.co/Lightricks/LTX-2
tags:
  - python
  - diffusers
  - huggingface
  - installation
  - pipelines
---

# Diffusers Integration

LTX-Video is fully integrated into the HuggingFace Diffusers Python library, providing a familiar API for programmatic video generation. This is the primary interface for most Python users alongside the native [[ltx-2-pipeline-api]].

## Documentation

- **LTX-Video (LTXV) Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
- **LTX-2 Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx2

## Installation

```bash
pip install -U git+https://github.com/huggingface/diffusers
```

### Requirements

- Python >= 3.10.5
- PyTorch >= 2.1.2
- CUDA 12.2+ (for LTXV) or CUDA >12.7 (for LTX-2)

## Available Pipelines

### LTX-Video (Original LTXV)

| Pipeline | Purpose |
|----------|---------|
| `LTXPipeline` | Text-to-video generation |
| `LTXImageToVideoPipeline` | Image-to-video generation |
| `LTXConditionPipeline` | Conditioned generation (v0.9.5+) |
| `LTXLatentUpsamplePipeline` | Spatial upscaling in latent space |
| `LTXI2VLongMultiPromptPipeline` | Long video with temporal tiling |

### LTX-2

| Pipeline | Purpose |
|----------|---------|
| `LTX2Pipeline` | Unified text/image/audio-to-video pipeline |

## Quick Start Examples

### Text-to-Video

```python
import torch
from diffusers import LTXPipeline

pipe = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")
```

### Conditioned Generation (v0.9.5+)

```python
from diffusers import LTXConditionPipeline

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", torch_dtype=torch.bfloat16
).to("cuda")
```

### LTX-2

```python
from diffusers.pipelines.ltx2 import LTX2Pipeline

pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2", torch_dtype=torch.bfloat16
).to("cuda")
```

## Key Features

- [[python-lora-loading]] -- LoRA weight loading and adapter management
- Single-file checkpoint loading via `from_single_file()`
- Standard export utilities (`export_to_video`)
- Compatible with all Diffusers optimization features:
  - [[python-performance-memory]] -- attention slicing, CPU offloading, VAE tiling
  - [[python-performance-speed]] -- torch.compile, fused QKV

## Scheduler

All LTX pipelines use `FlowMatchEulerDiscreteScheduler` by default. See [[python-schedulers]] for alternatives.

## Community Conversions

- **a-r-r-o-w/LTX-Video-diffusers** -- early community conversion for Diffusers compatibility

## LTX-2.3 Status

As of the research date, LTX-2.3 Diffusers support is listed as "coming soon" on the HuggingFace model card. Use the [[sdk-ltx-2]] native API for LTX-2.3.

## See Also

- [[sdk-ltx-video]] -- Original LTX-Video repository
- [[sdk-ltx-2]] -- LTX-2 monorepo with native pipelines
- [[ltx-2-pipeline-api]] -- LTX-2 native pipeline API
- [[python-conditioning]] -- Conditioning types and usage
