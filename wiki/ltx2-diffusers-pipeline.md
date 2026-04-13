---
title: LTX-2 Diffusers and PyTorch API
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-diffusers-pipeline-usage.md
tags:
  - ltx2
  - diffusers
  - huggingface
  - pytorch
  - python
  - api
---

# LTX-2 Diffusers and PyTorch API

[[ltx2-open-source-overview|LTX-2]] is available through two Python integration paths: the HuggingFace Diffusers library (higher-level API) and the native PyTorch API (`ltx-pipelines`) which exposes more pipeline variants and advanced configuration.

## Documentation

- Diffusers docs: https://huggingface.co/docs/diffusers/api/pipelines/ltx2
- Native PyTorch API docs: https://docs.ltx.video/open-source-model/integration-tools/pytorch-api

## HuggingFace Diffusers

### Available Pipeline Classes

| Class | Purpose |
|-------|---------|
| `LTX2Pipeline` | Main text-to-video generation |
| `LTX2LatentUpsamplePipeline` | Latent space 2x upsampling (stage 2) |
| `LTX2LatentUpsamplerModel` | Upsampler model component |
| `AutoencoderKLLTX2Audio` | Audio VAE encoder/decoder |
| `FlowMatchEulerDiscreteScheduler` | Recommended scheduler |

### Basic Usage

```python
import torch
from diffusers import FlowMatchEulerDiscreteScheduler
from diffusers.pipelines.ltx2 import LTX2Pipeline, LTX2LatentUpsamplePipeline
from diffusers.pipelines.ltx2.latent_upsampler import LTX2LatentUpsamplerModel

pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2",
    torch_dtype=torch.bfloat16
)
pipe.enable_sequential_cpu_offload()

prompt = "A beautiful sunset over the ocean with waves gently rolling"
video = pipe(
    prompt=prompt,
    width=768,
    height=512,
    num_inference_steps=30,
).videos[0]
```

### Memory Optimization

**CPU Offloading:**
```python
pipe.enable_sequential_cpu_offload()  # Recommended for consumer GPUs
```

**FP8 Loading:**
```python
pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2",
    variant="fp8",
    torch_dtype=torch.float8_e4m3fn
)
```

### Condition Pipeline

The Diffusers condition pipeline enables flexible video generation with various conditioning inputs, supporting both single-stage and two-stage approaches.

## Native PyTorch API (ltx-pipelines)

The LTX-2 repository provides more pipeline variants than the Diffusers integration.

### Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
pip install -e packages/ltx-core
pip install -e packages/ltx-pipelines
```

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

## Two-Stage Generation Pipeline (Recommended)

Both Diffusers and the native API support the recommended two-stage approach:

1. **Stage 1: Generate Base Video** -- Produces coherent video at target resolution using diffusion sampling with CFG
2. **Stage 2: Upsample and Refine** -- Upsamples by 2x, refines details using distilled LoRA for improved fidelity

## Diffusers vs Native API Comparison

| Feature | Diffusers | Native (ltx-pipelines) |
|---------|-----------|----------------------|
| Ease of use | Higher (standard HF patterns) | Lower (custom API) |
| Pipeline variants | Core (text2vid, upsample) | All 7+ variants |
| IC-LoRA support | In progress | Full support |
| Audio-to-video | In progress | Full support |
| Memory management | HF standard offloading | Custom optimizations |
| Integration | Works with HF ecosystem | Standalone |

## System Requirements

- Python 3.10+
- CUDA 12.7+
- PyTorch ~2.7
- NVIDIA GPU with 32GB+ VRAM (recommended)

See [[ltx2-system-requirements]] for full details.

## Known Issues

- **Issue #12925:** Distilled checkpoint support in Diffusers
- **Issue #12926:** LTX-2 condition pipeline support
- Image-to-video stability issues documented in HF discussions

## Community Conversions

- **CalamitousFelicitousness/LTX-2.3-distilled-Diffusers** -- Community conversion of LTX-2.3 distilled model to Diffusers format

## See Also

- [[ltx2-open-source-overview]]
- [[ltx2-comfyui-integration]]
- [[ltx2-text-to-video-guide]]
- [[ltx2-image-to-video-guide]]
- [[ltx2-system-requirements]]
