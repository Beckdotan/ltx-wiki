# LTX-2 Training / Fine-Tuning Guide

**Source:** https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
**Fetched:** 2026-04-13

---

## Overview

The LTX-2 Trainer enables researchers and advanced users to fine-tune LTX-2 models on custom datasets. Three training approaches are supported.

---

## Training Approaches

| Method | Description | Use Case |
|--------|-------------|----------|
| **LoRA Adaptation** | Lightweight adapter training | Style, motion, and character customization |
| **Full Fine-tuning** | Complete model weight updates | Domain-specific models |
| **IC-LoRA** | Control adapter training | Video-to-video conditioning (depth, pose, canny) |

---

## Prerequisites

| Requirement | Specification |
|-------------|--------------|
| Model Checkpoint | Local `.safetensors` file (LTX-2.3) |
| Text Encoder | Local Gemma model directory |
| Operating System | Linux with CUDA |
| CUDA Version | 13+ recommended |
| GPU | NVIDIA H100 with 80GB+ VRAM |

---

## Repository

The LTX-2 Trainer is part of the LTX-2 monorepo:
- **Location:** `packages/ltx-trainer/` in the LTX-2 repository
- **GitHub:** https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer

---

## Documentation Contents

The trainer GitHub repository contains guides for:
1. Getting started quickly
2. Preparing video datasets
3. Explaining the three training methods (LoRA, full, IC-LoRA)
4. All configuration parameters
5. Common problem troubleshooting
6. Utility tools

---

## Related Links

- **Training Docs:** https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
- **GitHub Trainer:** https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer
- **LoRA Usage Guide:** https://docs.ltx.video/open-source-model/usage-guides/lo-ra
- **IC-LoRA Usage Guide:** https://docs.ltx.video/open-source-model/usage-guides/ic-lo-ra
