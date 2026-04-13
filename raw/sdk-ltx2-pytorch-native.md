# LTX-2 Native PyTorch API (ltx-pipelines)

**Source:** https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
**Fetched:** 2026-04-13

---

## Overview

The LTX-2 repository provides a native PyTorch API separate from Hugging Face Diffusers. This is the most full-featured way to use LTX-2 locally, exposing all pipeline variants and advanced configuration options.

**Repository:** https://github.com/Lightricks/LTX-2

---

## Repository Structure

The LTX-2 monorepo contains three packages:

| Package | Description |
|---------|-------------|
| `ltx-core` | Model architecture, schedulers, guiders, noisers, and patchifiers |
| `ltx-pipelines` | High-level inference pipelines for all generation modes |
| `ltx-trainer` | Fine-tuning capabilities for LoRA variants |

---

## System Requirements

- Python 3.10+
- CUDA 12.7+
- PyTorch ~2.7
- NVIDIA GPU with 32GB+ VRAM (recommended)

---

## Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate
```

---

## Pipeline Classes

Eight specialized pipeline implementations are available:

| Pipeline | Purpose |
|----------|---------|
| `TI2VidTwoStagesPipeline` | Text/image-to-video with dual-stage upscaling |
| `TI2VidTwoStagesRes2sPipeline` | Two-stage with res_2s second-order sampler |
| `TI2VidOneStagePipeline` | Single-stage generation without upscaling |
| `DistilledPipeline` | Fast execution using distilled checkpoint |
| `ICLoraPipeline` | Video-to-video with depth, pose, canny edge controls |
| `A2VidPipelineTwoStage` | Audio-conditioned generation |
| `RetakePipeline` | Selective temporal region regeneration |
| `KeyframeInterpolationPipeline` | Smooth transitions between keyframes |

---

## Usage Example: Distilled Pipeline

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

---

## Guidance Parameters (MultiModalGuiderParams)

| Parameter | Range | Purpose |
|-----------|-------|---------|
| `cfg_scale` | 2.0-5.0 | Classifier-Free Guidance strength |
| `stg_scale` | 0.5-1.5 | Spatio-Temporal coherence control |
| `stg_blocks` | e.g., [29] | Transformer blocks for perturbation |
| `rescale_scale` | ~0.7 | Prevents over-saturation |
| `modality_scale` | 1.0-3.0 | Audio-visual synchronization |

---

## Quantization Options

### FP8 Cast (Broad Compatibility)
```python
from ltx_core.quantization.policy import QuantizationPolicy

pipeline = DistilledPipeline.from_config(
    "path/to/config.yaml",
    quantization=QuantizationPolicy.fp8_cast(),
)
```

### FP8 Scaled MM (Hopper GPUs Only - H100, etc.)
```python
pipeline = DistilledPipeline.from_config(
    "path/to/config.yaml",
    quantization=QuantizationPolicy.fp8_scaled_mm(),
)
```

---

## Sampling Recommendations

| Model Type | Steps | CFG Scale |
|-----------|-------|-----------|
| Distilled | 4-8 | 1.0 recommended |
| Full | 20-50 | 2.0-5.0 recommended |

---

## Dimension Constraints

- **Width/Height:** Must be divisible by 32
- **Frame Count:** Must follow 8n + 1 pattern
  - Valid: 1, 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 121, 161, 257...

---

## HuggingFace Diffusers Alternative

For simpler usage with fewer advanced features:

```python
from diffusers import LTX2Pipeline
import torch

pipeline = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2",
    torch_dtype=torch.bfloat16,
)
pipeline.to("cuda")
result = pipeline(prompt="...", width=768, height=512, num_frames=97)
```

**Note:** The Diffusers version may not expose all features available in the native `ltx-pipelines` package.

---

## Related Links

- **PyTorch API Docs:** https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
- **GitHub Repository:** https://github.com/Lightricks/LTX-2
- **Model Weights:** https://huggingface.co/Lightricks/LTX-2
