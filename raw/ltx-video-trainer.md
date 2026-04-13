# LTX Video Trainer - Fine-Tuning Toolkit

## Overview

The LTX ecosystem includes two training/fine-tuning tools:

1. **LTX-Video-Trainer** - Community trainer for the original LTX-Video (LTXV) model
2. **ltx-trainer** (part of LTX-2 repo) - Official trainer for the LTX-2 model

## LTX-Video-Trainer (Community)

**GitHub:** https://github.com/Lightricks/LTX-Video-Trainer
**License:** Apache 2.0
**Stars:** ~424 | **Forks:** ~58
**Language:** Python (100%)
**Commits:** 85 on main branch

### Purpose
Community-developed toolkit for training and fine-tuning Lightricks' LTX-Video (LTXV) model. Enables LoRA training, full fine-tuning, and video-to-video transformation workflows on custom datasets.

### Supported Models
- LTX-Video 2B (original)
- LTXV 13B (added May 6, 2025)

### Training Types

**Standard LoRA Training:**
- Custom style LoRAs (e.g., Cakeify LoRA -- transforms objects to appear cake-like, Squish LoRA -- playful squishing effects)
- Effect LoRAs for specialized effects and transformations

**IC-LoRA (In-Context LoRA) Control Adapters** (added July/August 2025):
- Depth map control -- generate videos guided by depth maps
- Human pose skeleton control -- generate videos from pose skeletons
- Canny edge map control -- generate videos from edge maps

**Full Fine-Tuning:**
- Complete model weight updates on custom datasets

### Repository Structure
```
docs/              - Comprehensive guides (7 total)
configs/           - Training configurations
src/ltxv_trainer/  - Core implementation
scripts/           - Utility tools
templates/         - Training templates
```

### Documentation
Seven detailed guides covering:
- Quick start setup
- Dataset preparation
- Training modes
- Configuration reference
- Utility scripts
- Troubleshooting
- IC-LoRA specific guide

### Community Participation
- Gradio web UI available for graphical interface
- Example datasets hosted on HuggingFace
- Discord server for real-time assistance
- GitHub issues for bug reports and feature requests

### Authors
Matan Ben Yosef, Naomi Ken Korem, and Tavi Halperin (2025)

## LTX-2 Trainer (Official)

**Location:** https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer
**README:** https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md

### Purpose
Official training infrastructure within the LTX-2 monorepo for training and fine-tuning the LTX-2 audio-video generation model.

### Capabilities
- LoRA training on LTX-2 models
- Full fine-tuning (base dev model ltx-2-19b-dev is fully trainable in bf16)
- IC-LoRA training (video-to-video transformations on custom datasets)
- Training can be completed in less than an hour for motion, style, or likeness adaptation

### Pre-trained LoRA Models Available
- Camera control LoRA
- Pose guidance LoRA
- Depth control IC-LoRA
- Edge detection IC-LoRA
- Detailer IC-LoRA (https://huggingface.co/Lightricks/LTX-2-19b-IC-LoRA-Detailer)

## Sources

- https://github.com/Lightricks/LTX-Video-Trainer
- https://github.com/Lightricks/LTX-Video-Trainer/blob/main/docs/quick-start.md
- https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer
- https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
- https://huggingface.co/Lightricks/LTX-2-19b-IC-LoRA-Detailer
