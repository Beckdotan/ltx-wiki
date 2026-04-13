---
title: Python Installation and Setup
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://github.com/Lightricks/LTX-Video
  - https://github.com/Lightricks/LTX-2
  - https://pypi.org/project/ltx-video/
  - https://deepwiki.com/Lightricks/LTX-Video/1.1-installation-and-setup
tags:
  - installation
  - setup
  - python
  - diffusers
  - prerequisites
---

# Python Installation and Setup

This page covers all methods for installing the LTX Video Python tooling, including [[diffusers-pipeline-overview|HuggingFace Diffusers]], the official [[ltx-native-pytorch-api|LTX-Video repository]], and the [[ltx-native-pytorch-api|LTX-2 monorepo]].

## System Requirements

### LTX-Video (v1)

| Requirement | Minimum Version |
|-------------|-----------------|
| Python | 3.10.5+ |
| CUDA | 12.2+ |
| PyTorch | >= 2.1.2 |
| macOS MPS | PyTorch 2.3.0 or >= 2.6 |

### LTX-2 / LTX-2.3

| Requirement | Minimum Version |
|-------------|-----------------|
| Python | >= 3.12 |
| CUDA | > 12.7 |
| PyTorch | ~= 2.7 |

### FP8 Quantized Models

FP8 requires CUDA Toolkit 12.8+ and an Ada Lovelace architecture GPU (RTX 40xx series) or newer.

## VRAM Requirements

| Model / Mode | Minimum VRAM | Notes |
|--------------|-------------|-------|
| LTX-Video 2B (FP8 + offloading) | ~6-8 GB | See [[ltxv-memory-optimization]] |
| LTX-Video 2B (BF16 + offloading) | ~10 GB | Group offloading |
| LTX-Video 2B (BF16, no offloading) | ~24 GB | Full precision |
| LTX-Video 13B (FP8) | ~12-16 GB | With quantization |
| LTX-Video 13B (BF16) | ~24-30 GB | Full precision |
| LTX-2 (FP8, optimized) | ~8 GB | 480p, limited frames |
| LTX-2 (BF16) | ~24+ GB | Full precision |

See [[ltxv-model-variants]] for a full breakdown of models by VRAM budget.

## Installation Methods

### Method 1: HuggingFace Diffusers (Recommended for Quick Start)

```bash
# Install PyTorch with CUDA support
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install diffusers from main branch (recommended for latest LTX features)
pip install -U git+https://github.com/huggingface/diffusers

# Or install stable release
pip install diffusers[torch] transformers accelerate
```

For the full set of optional dependencies:

```bash
pip install -U peft                   # LoRA support
pip install imageio imageio-ffmpeg    # Video export
pip install -U gguf                   # GGUF quantized model support
pip install xformers                  # Memory-efficient attention (optional)
pip install torchao                   # TorchAO quantization (optional)
```

### Method 2: LTX-Video Repository

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference]
```

### Method 3: LTX-2 Monorepo

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate
```

Or with pip:

```bash
pip install -e packages/ltx-pipelines
```

### Method 4: PyPI Package

```bash
pip install ltx-video
```

The PyPI package (`ltx-video`) was released July 2025 and provides the core LTX-Video model package.

## FP8 Kernel Installation (Optional)

For a significant performance boost on Ada architecture GPUs:

```bash
git clone https://github.com/Lightricks/LTX-Video-Q8-Kernels.git
cd LTX-Video-Q8-Kernels
pip install -e .
```

## Verification

```python
import torch
import diffusers

print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
print(f"CUDA version: {torch.version.cuda}")
print(f"Diffusers version: {diffusers.__version__}")

from diffusers import LTXPipeline, LTXImageToVideoPipeline
print("LTX Video pipelines imported successfully!")

try:
    from diffusers import LTXConditionPipeline
    print("LTXConditionPipeline available")
except ImportError:
    print("LTXConditionPipeline not available - update diffusers")
```

## Model Download

Models are downloaded automatically on first use via `from_pretrained()`. To pre-download:

```bash
pip install huggingface_hub
huggingface-cli download Lightricks/LTX-Video
huggingface-cli download Lightricks/LTX-Video-0.9.8-dev
huggingface-cli download Lightricks/ltxv-spatial-upscaler-0.9.7
```

See [[ltxv-model-variants]] for the full list of available model identifiers.

## Key Dependencies

| Package | Purpose | Minimum Version |
|---------|---------|----------------|
| `diffusers` | Pipeline framework | 0.30+ (latest recommended) |
| `transformers` | T5 text encoder | 4.30+ |
| `accelerate` | Device management, offloading | 0.20+ |
| `torch` | Deep learning framework | 2.0+ |
| `safetensors` | Efficient model loading | 0.3+ |
| `imageio` + `imageio-ffmpeg` | Video export | 2.9+ / 0.4+ |
| `peft` | LoRA adapter support | 0.6+ (optional) |

## Environment Variables

```bash
# Recommended for memory management with FP8
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
```

## Troubleshooting

- **CUDA Out of Memory:** Use [[ltxv-memory-optimization|memory optimization techniques]] -- FP8 casting, group offloading, VAE tiling, or reduce resolution/frame count
- **Import errors for new pipelines:** Install diffusers from source: `pip install -U git+https://github.com/huggingface/diffusers`
- **PyTorch version mismatch:** Ensure PyTorch matches your CUDA version
- **Resolution errors:** Width and height must be divisible by 32
- **Frame count errors:** Must follow the `8n + 1` pattern (e.g., 9, 17, 25, ..., 161, 257)
- **macOS MPS issues:** Use PyTorch 2.3.0 or >= 2.6 specifically

## See Also

- [[diffusers-pipeline-overview]] -- overview of all available pipeline classes
- [[ltxv-model-variants]] -- which model to choose
- [[ltxv-memory-optimization]] -- reducing VRAM usage
- [[ltx-native-pytorch-api]] -- alternative to Diffusers
