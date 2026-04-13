---
title: SDK - LTX-2 (Monorepo)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/LTX-2
  - https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
  - https://docs.ltx.video/open-source-model/getting-started/quick-start
tags:
  - sdk
  - ltx-2
  - python
  - installation
  - monorepo
---

# SDK - LTX-2 (Monorepo)

LTX-2 is the official Python inference and LoRA trainer package for the LTX-2 audio-video generative model. The repository is a monorepo containing three packages. For the original LTX-Video repo (up to 0.9.8), see [[sdk-ltx-video]].

- **Repository:** https://github.com/Lightricks/LTX-2
- **Stars:** 5.8k | **Forks:** 891
- **Language:** 100% Python

## Monorepo Package Structure

```
LTX-2/
  packages/
    ltx-core/       # Core model implementation, inference stack, utilities
    ltx-pipelines/  # High-level pipeline implementations
    ltx-trainer/    # Training and fine-tuning tools
```

### ltx-core

The foundational package containing building blocks:

- **Schedulers** -- noise scheduling algorithms
- **Guiders** -- guidance mechanisms (CFG, STG, multimodal)
- **Noisers** -- noise injection utilities
- **Patchifiers** -- token patchification for the transformer
- **Quantization** -- FP8 quantization policies
- **Loader** -- model loading and LoRA management

### ltx-pipelines

High-level pipeline implementations. See [[ltx-2-pipeline-api]] for full details.

### ltx-trainer

Training tools for LoRA, full fine-tuning, and IC-LoRA. Training a motion, style, or likeness LoRA takes under 1 hour in many settings.

## Installation

### Requirements

- Python >= 3.12 (for LTX-2.3) or >= 3.10 (for LTX-2)
- CUDA > 12.7
- PyTorch ~= 2.7

### Setup

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate
```

For attention optimization:

```bash
uv sync --extra xformers
# or Flash Attention 3 for Hopper GPUs
```

## Required Model Downloads

From https://huggingface.co/Lightricks/LTX-2.3:

**Core Models:**
- `ltx-2.3-22b-dev.safetensors`
- `ltx-2.3-22b-distilled.safetensors`

**Upscalers:**
- `ltx-2.3-spatial-upscaler-x2-1.1.safetensors`
- `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors`
- `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`

**Additional:**
- Distilled LoRA
- Gemma 3 text encoder
- Control LoRAs (camera, pose, motion tracking)

## Dimension Requirements

- Width and height: divisible by 32
- Frame count: pattern `8n + 1` (valid: 1, 9, 17, 25, 33, ..., 97, 121, 161, 257)

## Quick Start Example

```python
from ltx_pipelines.distilled_pipeline import DistilledPipeline

pipeline = DistilledPipeline.from_config("path/to/config.yaml")

output = pipeline(
    prompt="A golden retriever running through a sunlit meadow...",
    width=768,
    height=512,
    num_frames=97,
    fps=24.0,
    seed=42,
)
```

## FP8 Quantization

```python
from ltx_core.quantization.policy import QuantizationPolicy

# FP8 Cast (broad GPU compatibility)
pipeline = DistilledPipeline.from_config(
    "path/to/config.yaml",
    quantization=QuantizationPolicy.fp8_cast(),
)

# FP8 Scaled MM (Hopper GPUs only)
pipeline = DistilledPipeline.from_config(
    "path/to/config.yaml",
    quantization=QuantizationPolicy.fp8_scaled_mm(),
)
```

## Compatibility Notes

- LTX-2 LoRAs do NOT transfer to LTX-2.3 (VAE redesign + text connector changes)
- LTX-2.3 Diffusers support is listed as "coming soon" as of research date
- Distilled models require `guidance_scale=1.0`

## Additional Resources

- **Website:** https://ltx.io
- **Model Hub:** https://huggingface.co/Lightricks
- **Paper:** https://arxiv.org/abs/2601.03233
- **Community:** Discord (ltxplatform)
- **Demo:** https://console.ltx.video/playground/

## See Also

- [[ltx-2-pipeline-api]] -- All available LTX-2 pipeline classes
- [[sdk-ltx-video]] -- Original LTX-Video repository
- [[diffusers-integration]] -- HuggingFace Diffusers integration
- [[python-lora-loading]] -- LoRA loading in both Diffusers and LTX-2 APIs
