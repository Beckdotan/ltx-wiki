# LTX Video -- Hugging Face Diffusers Integration

## Overview
LTX Video is fully integrated into the Hugging Face Diffusers Python library, providing a familiar API for developers to generate videos programmatically.

## Documentation
- **LTX-Video (LTXV) Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
- **LTX-2 Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx2
- **Source Code:** https://github.com/huggingface/diffusers/blob/main/docs/source/en/api/pipelines/ltx_video.md

## Available Pipelines

### LTX-Video (Original LTXV)
- `LTXPipeline` -- text-to-video generation
- `LTXImageToVideoPipeline` -- image-to-video generation
- `LTXConditionPipeline` -- conditioned generation (v0.9.8+)
- `LTXLatentUpsamplePipeline` -- spatial upscaling in latent space

### LTX-2
- `LTX2Pipeline` -- unified text/image/audio-to-video pipeline
- Uses `FlowMatchEulerDiscreteScheduler`

## Installation
```bash
pip install -U git+https://github.com/huggingface/diffusers
```

## Usage Examples

### Text-to-Video (LTXV)
```python
import torch
from diffusers import LTXPipeline

pipe = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipe.to("cuda")
```

### Image-to-Video (LTXV)
```python
from diffusers import LTXImageToVideoPipeline

pipe = LTXImageToVideoPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipe.to("cuda")
```

### Conditioned Generation (v0.9.8+)
```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")
```

### LTX-2
```python
from diffusers import FlowMatchEulerDiscreteScheduler
from diffusers.pipelines.ltx2 import LTX2Pipeline

pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2", torch_dtype=torch.bfloat16
)
```

## Features
- LoRA weight loading and adapter management
- Single-file checkpoint loading via `from_single_file()`
- Standard Diffusers export utilities (`export_to_video`)
- Compatible with Diffusers optimization features (attention slicing, CPU offloading, etc.)

## Requirements
- Python >= 3.10.5
- PyTorch >= 2.1.2
- CUDA 12.2+ (for LTXV) or CUDA >12.7 (for LTX-2)

## Community Diffusers Conversions
- **a-r-r-o-w/LTX-Video-diffusers** -- early community conversion for Diffusers compatibility
  - URL: https://huggingface.co/a-r-r-o-w/LTX-Video-diffusers

## Note on LTX-2.3
As of the research date, LTX-2.3 Diffusers support is listed as "coming soon" on the Hugging Face model card.

## Sources
- https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
- https://huggingface.co/docs/diffusers/api/pipelines/ltx2
- https://huggingface.co/Lightricks/LTX-Video
- https://huggingface.co/Lightricks/LTX-2
