---
title: LTX-2 System Requirements
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-ltx2-system-requirements.md
  - raw/dev-ltx2-comfyui-quickstart.md
  - raw/ltx2-diffusers-pipeline-usage.md
tags:
  - ltx2
  - system-requirements
  - hardware
  - gpu
---

# LTX-2 System Requirements

Hardware, software, and storage requirements for running [[ltx2-open-source-overview|LTX-2 / LTX-2.3]] locally.

## GPU Requirements

| Tier | Specification |
|------|---------------|
| Minimum | NVIDIA GPU with 32GB+ VRAM |
| Recommended | NVIDIA A100 (80GB) or H100 |

## Memory and Storage

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| System RAM | 32GB | 64GB+ |
| Storage | 100GB free space | 200GB+ SSD |

## Software Requirements

| Software | Minimum Version |
|----------|----------------|
| CUDA | 11.8 or higher (12.1+ recommended) |
| Python | 3.10 or higher |
| PyTorch | ~2.7 |

## ComfyUI-Specific Requirements

- ComfyUI installed and working
- CUDA GPU with 32GB+ VRAM
- 100GB+ free disk space
- Python 3.10+

See [[ltx2-comfyui-integration]] for full setup instructions.

## PyTorch API Requirements

- Python 3.10+
- CUDA 12.7+
- PyTorch ~2.7
- NVIDIA GPU with 32GB+ VRAM (recommended)

See [[ltx2-diffusers-pipeline]] for pipeline setup.

## LTX Desktop Requirements

The standalone desktop application has lower hardware requirements:

- NVIDIA GPU with 16GB+ VRAM (Windows/Linux)
- 16GB system RAM minimum (32GB recommended)
- 160GB free disk space
- macOS users: API key required for cloud-based operation (no local GPU generation)

## Training Requirements

Training and fine-tuning have higher requirements than inference:

| Requirement | Specification |
|-------------|--------------|
| GPU | NVIDIA H100 with 80GB+ VRAM |
| OS | Linux with CUDA |
| CUDA | 13+ recommended |

See [[ltx2-training]] for full training details.

## Model Size Comparison

| Specification | LTX-Video 0.9.8 | LTX-2.3 |
|---------------|-----------------|---------|
| Model Size | 2B - 13B | ~20B |
| Min VRAM (Full) | 16GB (13B) | 32GB |
| Min VRAM (Quantized) | 6GB (2B FP8) | TBD |
| CUDA Minimum | 12.2 | 11.8 |
| Python | 3.10.5+ | 3.10+ |
| PyTorch | 2.1.2+ | ~2.7 |

## See Also

- [[ltx2-open-source-overview]]
- [[ltx2-comfyui-integration]]
- [[ltx2-diffusers-pipeline]]
- [[ltx2-training]]
