# LTX-2 in HuggingFace Diffusers: Pipeline Usage and Python Integration

## Overview

LTX-2 is supported in the HuggingFace Diffusers library for text-to-video, image-to-video, and conditioned generation. The Diffusers integration provides a higher-level API compared to the native PyTorch API (ltx-pipelines), but the native API from the LTX-2 repository exposes more pipeline variants and advanced configuration.

## Documentation

- **Diffusers docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx2
- **Latest (main):** https://huggingface.co/docs/diffusers/main/api/pipelines/ltx2
- **Native PyTorch API docs:** https://docs.ltx.video/open-source-model/integration-tools/pytorch-api

## Available Pipeline Classes

| Class | Purpose |
|-------|---------|
| `LTX2Pipeline` | Main text-to-video generation |
| `LTX2LatentUpsamplePipeline` | Latent space 2x upsampling (stage 2) |
| `LTX2LatentUpsamplerModel` | Upsampler model component |
| `AutoencoderKLLTX2Audio` | Audio VAE encoder/decoder |
| `FlowMatchEulerDiscreteScheduler` | Recommended scheduler |

## Two-Stage Generation Pipeline (Recommended)

The recommended approach for production quality uses two stages:

### Stage 1: Generate Base Video
- Generates video at target resolution using diffusion sampling with CFG
- Produces coherent low-noise video sequence

### Stage 2: Upsample and Refine
- Upsamples Stage 1 output by 2x
- Refines details using distilled LoRA
- Improves fidelity and visual quality

### Code Example
```python
import torch
from diffusers import FlowMatchEulerDiscreteScheduler
from diffusers.pipelines.ltx2 import LTX2Pipeline, LTX2LatentUpsamplePipeline
from diffusers.pipelines.ltx2.latent_upsampler import LTX2LatentUpsamplerModel
from diffusers.pipelines.ltx2.export_utils import encode_video

# Load pipeline
pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2",
    torch_dtype=torch.bfloat16
)
pipe.enable_sequential_cpu_offload()

# Stage 1: Generate
prompt = "A beautiful sunset over the ocean with waves gently rolling"
video = pipe(
    prompt=prompt,
    width=768,
    height=512,
    num_inference_steps=30,
).videos[0]

# Stage 2: Upsample (using latent upsampler)
# ... load upsampler and refine
```

## Native PyTorch API (ltx-pipelines)

The LTX-2 repository provides more pipeline variants than Diffusers:

### Available Pipelines
| Pipeline | Description |
|----------|-------------|
| TI2VidTwoStagesPipeline | Production-quality with 2x upsampling (recommended) |
| TI2VidTwoStagesHQPipeline | Higher quality using res_2s sampler |
| TI2VidOneStagePipeline | Rapid prototyping |
| DistilledPipeline | Fastest inference with 8 predefined sigmas |
| ICLoraPipeline | Video-to-video transformations |
| KeyframeInterpolationPipeline | Inter-frame generation |
| A2VidPipelineTwoStage | Audio-conditioned generation |

### Installation
```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
pip install -e packages/ltx-core
pip install -e packages/ltx-pipelines
```

### System Requirements
- Python 3.10+
- CUDA 12.7+
- PyTorch ~2.7
- NVIDIA GPU with 32GB+ VRAM (recommended)

## Condition Pipeline

The Diffusers condition pipeline enables flexible video generation with various conditioning inputs:
- Source: `diffusers/src/diffusers/pipelines/ltx/pipeline_ltx_condition.py`
- Supports both single-stage and two-stage approaches

## Memory Optimization

### CPU Offloading
```python
pipe.enable_sequential_cpu_offload()  # Recommended for consumer GPUs
```

### FP8 Loading
```python
# Use FP8 variant for lower VRAM
pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2",
    variant="fp8",
    torch_dtype=torch.float8_e4m3fn
)
```

## Comparison: Diffusers vs Native API

| Feature | Diffusers | Native (ltx-pipelines) |
|---------|-----------|----------------------|
| Ease of use | Higher (standard HF patterns) | Lower (custom API) |
| Pipeline variants | Core (text2vid, upsample) | All 7+ variants |
| IC-LoRA support | In progress | Full support |
| Audio-to-video | In progress | Full support |
| Memory management | HF standard offloading | Custom optimizations |
| Integration | Works with HF ecosystem | Standalone |

## Known Issues and Tracking

- **Issue #12925:** Distilled checkpoint support in Diffusers
- **Issue #12926:** LTX-2 condition pipeline support
- Image-to-video stability issues documented in HF discussions

## Community Diffusers Conversions

- **CalamitousFelicitousness/LTX-2.3-distilled-Diffusers** -- Community conversion of LTX-2.3 distilled model to Diffusers format

## Sources

- [Diffusers LTX-2 Documentation](https://huggingface.co/docs/diffusers/api/pipelines/ltx2)
- [LTX-2 HuggingFace Model Card](https://huggingface.co/Lightricks/LTX-2)
- [LTX-2 GitHub -- ltx-pipelines README](https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md)
- [LTX-2 Native PyTorch API Docs](https://docs.ltx.video/open-source-model/integration-tools/pytorch-api)
- [Diffusers Releases](https://github.com/huggingface/diffusers/releases)
