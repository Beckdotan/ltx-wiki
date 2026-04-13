# Official Python SDK: LTX-2 (GitHub)

## Sources
- https://github.com/Lightricks/LTX-2
- https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
- https://docs.ltx.video/open-source-model/getting-started/quick-start

---

## Overview

LTX-2 is the official Python inference and LoRA trainer package for the LTX-2 audio-video generative model. The repository is a monorepo containing three packages for model implementation, inference pipelines, and training tools.

- **Repository:** https://github.com/Lightricks/LTX-2
- **Stars:** 5.8k
- **Forks:** 891
- **Language:** 100% Python

---

## Monorepo Package Structure

| Package | Purpose |
|---------|---------|
| **ltx-core** | Model architecture, schedulers, guiders, noisers, and patchifiers |
| **ltx-pipelines** | High-level inference pipelines for text-to-video, image-to-video, IC-LoRA workflows |
| **ltx-trainer** | LoRA and full fine-tuning tools |

---

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

---

## Available Inference Pipelines

| Class | Purpose | Steps |
|-------|---------|-------|
| `TI2VidTwoStagesPipeline` | Production text/image-to-video with 2x upsampling (recommended) | 40 stage 1, 8 stage 2 |
| `TI2VidTwoStagesHQPipeline` | Two-stage with res_2s sampler (fewer steps, better quality) | Variable |
| `TI2VidOneStagePipeline` | Single-stage for quick prototyping | 20-50 |
| `DistilledPipeline` | Fastest inference (8 predefined sigmas) | 8 stage 1, 4 stage 2 |
| `ICLoraPipeline` | Video/image-to-video transformations | Variable |
| `KeyframeInterpolationPipeline` | Keyframe interpolation | Variable |
| `A2VidPipelineTwoStage` | Audio-conditioned generation | Variable |
| `RetakePipeline` | Region-specific video regeneration | Variable |

---

## Core Parameters

### Dimension Requirements
- Width and height: divisible by 32
- Frame count: pattern `8n + 1` (valid: 1, 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 121, etc.)

### Guidance Parameters (MultiModalGuiderParams)

| Parameter | Range | Function |
|-----------|-------|----------|
| `cfg_scale` | 2.0-5.0 | Classifier-Free Guidance; higher = more prompt adherence |
| `stg_scale` | 0.5-1.5 | Spatio-Temporal Guidance for temporal coherence |
| `stg_blocks` | e.g. `[29]` | Transformer blocks to perturb for STG |
| `rescale_scale` | ~0.7 | Rescales guided prediction to prevent over-saturation |
| `modality_scale` | 1.0-3.0 | Audio-visual sync strength |

### Sampling Recommendations

| Model Type | Steps | CFG Scale |
|-----------|-------|-----------|
| Distilled | 4-8 | 1.0 |
| Full | 20-50 | 2.0-5.0 |
| Both | -- | 3.0-3.5 recommended |

---

## Code Examples

### Text-to-Video with Distilled Pipeline

```python
import torch
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

### HuggingFace Diffusers Integration

```python
from diffusers import LTX2Pipeline
import torch

pipeline = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2",
    torch_dtype=torch.bfloat16,
)
pipeline.to("cuda")

result = pipeline(
    prompt="A golden retriever running through a sunlit meadow...",
    width=768,
    height=512,
    num_frames=97,
)
```

---

## Memory Optimization

### FP8 Quantization (~40% VRAM reduction)

```python
from ltx_core.quantization.policy import QuantizationPolicy

# FP8 Cast (broad compatibility)
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

Set environment variable for memory management:
```bash
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
```

### Performance Tips
- Use `DistilledPipeline` for fastest inference (8 steps stage 1, 4 steps stage 2)
- Enable FP8 quantization: `--quantization fp8-cast` or `fp8-scaled-mm` (Hopper GPUs)
- Gradient estimation reduces steps from 40 to 20-30 while maintaining quality
- Disable memory cleanup between stages if VRAM permits
- Choose `TI2VidOneStagePipeline` when high resolution is not required

---

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

---

## Training (ltx-trainer)

### Capabilities
- LoRA adapter training
- Full model fine-tuning
- IC-LoRA (Image Control LoRA) development
- Custom control mechanism training

### Training Time
- Motion, style, or likeness LoRAs: under 1 hour in many settings

See: https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md

---

## Additional Resources

- **Website:** https://ltx.io
- **Model Hub:** https://huggingface.co/Lightricks
- **Paper:** https://arxiv.org/abs/2601.03233
- **Community:** Discord (ltxplatform)
- **Demo:** https://console.ltx.video/playground/
