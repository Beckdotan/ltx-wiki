---
title: LTX-2 Training and Fine-Tuning
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-ltx2-training.md
tags:
  - ltx2
  - training
  - fine-tuning
  - lora
  - ic-lora
---

# LTX-2 Training and Fine-Tuning

The LTX-2 Trainer enables researchers and advanced users to fine-tune [[ltx2-open-source-overview|LTX-2]] models on custom datasets.

## Repository

The trainer is part of the LTX-2 monorepo:

- Location: `packages/ltx-trainer/` in the LTX-2 repository
- GitHub: https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer

## Training Approaches

| Method | Description | Use Case |
|--------|-------------|----------|
| **LoRA Adaptation** | Lightweight adapter training | Style, motion, and character customization |
| **Full Fine-tuning** | Complete model weight updates | Domain-specific models |
| **IC-LoRA** | Control adapter training | Video-to-video conditioning (depth, pose, canny) |

For using trained IC-LoRA adapters in inference, see [[ltx2-ic-lora-guide]].

## Prerequisites

| Requirement | Specification |
|-------------|--------------|
| Model Checkpoint | Local `.safetensors` file (LTX-2.3) |
| Text Encoder | Local Gemma model directory |
| Operating System | Linux with CUDA |
| CUDA Version | 13+ recommended |
| GPU | NVIDIA H100 with 80GB+ VRAM |

These requirements are significantly higher than inference requirements. See [[ltx2-system-requirements]] for the full comparison.

## Documentation Contents

The trainer repository contains guides for:

1. Getting started quickly
2. Preparing video datasets
3. Explaining the three training methods (LoRA, full, IC-LoRA)
4. All configuration parameters
5. Common problem troubleshooting
6. Utility tools

## See Also

- [[ltx2-ic-lora-guide]]
- [[ltx2-open-source-overview]]
- [[ltx2-system-requirements]]
- [[ltx2-comfyui-integration]]
