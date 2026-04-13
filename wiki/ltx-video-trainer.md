---
title: LTX Video Trainer
type: tool
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-trainer.md
  - raw/training-fine-tuning.md
  - raw/community-project-lora-training-tools.md
tags:
  - trainer
  - tool
  - lora
  - fine-tuning
  - open-source
---
# LTX Video Trainer

The LTX ecosystem includes two official training tools for [[lora-training]] and fine-tuning. Both are open source and developed by [[lightricks]].

## LTX-Video-Trainer (Community Trainer)

- **Repository:** github.com/Lightricks/LTX-Video-Trainer
- **License:** Apache 2.0
- **Stars:** ~424 | **Forks:** ~58
- **Language:** Python (100%)
- **Authors:** Matan Ben Yosef, Naomi Ken Korem, Tavi Halperin (2025)

### Supported Models

- LTX-Video 2B (original)
- LTXV 13B (added May 2025)

### Training Types

- **Standard LoRA training** -- custom styles, characters, effects
- **[[ic-lora|IC-LoRA]] training** (added July/August 2025) -- depth map, human pose skeleton, canny edge map control
- **Full fine-tuning** -- complete model weight updates

### Installation

```bash
git clone https://github.com/Lightricks/LTX-Video-Trainer.git
cd LTX-Video-Trainer
pip install -e .
```

### Repository Structure

```
docs/              -- Comprehensive guides (7 total)
configs/           -- Training configurations
src/ltxv_trainer/  -- Core implementation
scripts/           -- Utility tools
templates/         -- Training templates
```

### Documentation

Seven detailed guides covering quick start, dataset preparation, training modes, configuration reference, utility scripts, troubleshooting, and IC-LoRA.

### Example LoRA Models

- **Cakeify LoRA** -- cake-texture transformation effects
- **Squish LoRA** -- playful deformation effects

## LTX-2 Trainer (Official)

- **Location:** github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer
- Part of the LTX-2 monorepo alongside ltx-core and ltx-pipelines

### Capabilities

- **LoRA training** on LTX-2 and LTX-2.3 models
- **Full fine-tuning** (base dev model ltx-2-19b-dev is fully trainable in bf16)
- **[[ic-lora|IC-LoRA]] training** -- video-to-video transformations on custom datasets
- **Audio-video joint training** support
- Training can be completed in **less than an hour** for motion, style, or likeness adaptation

### Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate

# Or install the trainer package directly
pip install -e packages/ltx-trainer
```

### Pre-trained LoRA Models

| Model | Type |
|-------|------|
| Camera control LoRA | Standard |
| Pose guidance LoRA | Standard |
| Depth control IC-LoRA | IC-LoRA |
| Edge detection IC-LoRA | IC-LoRA |
| Detailer IC-LoRA | IC-LoRA |

## Community Training Tools

### ai-toolkit (Ostris)

Third-party training toolkit supporting LTX Video LoRA training alongside other video models.

- **Repository:** github.com/ostris/ai-toolkit
- See [[training-hyperparameters]] for configuration example

### eisneim -- LTX LoRA Training (i2v & t2v)

Community-created training code supporting both image-to-video and text-to-video LoRA training.

- **Repository:** github.com/eisneim/ltx_lora_training_i2v_t2v
- MIT license
- Produced the bullet_time LoRA as a proof of concept

### HuggingFace finetrainers

- Added T2V LoRA fine-tuning support for LTX Video in December 2024
- Test model: finetrainers/dummy-ltxvideo on HuggingFace
- Scalable and memory-optimized training

### Gradio Web UI

A Gradio web UI is available with the LTX-Video-Trainer for graphical training interface.

## Community Support

- Discord server for real-time assistance
- GitHub issues for bug reports and feature requests
- Example datasets hosted on HuggingFace (see [[training-dataset-preparation]])

## References

- [[lora-training]]
- [[training-hyperparameters]]
- [[training-dataset-preparation]]
- [[ic-lora]]
- [[third-party-training-services]]
