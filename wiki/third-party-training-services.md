---
title: Third-Party Training Services
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/third-party-lora-training-services.md
  - raw/social-lora-training-community-ecosystem.md
  - raw/training-fine-tuning.md
tags:
  - training
  - cloud
  - services
  - lora
  - wavespeed
  - fal-ai
  - oxen-ai
---
# Third-Party Training Services

Several cloud platforms offer managed [[lora-training]] for [[ltx-video-overview|LTX-Video]] models, removing the need to manage GPU infrastructure.

## WaveSpeedAI

Cloud-based training service built on the LTX-2 foundation model.

### Services

| Service | Description |
|---------|-------------|
| LTX-2 19B Video LoRA Trainer | Train LoRA adapters from video clips capturing motion patterns, visual styles, and character appearances |
| LTX-2 IC-LoRA Trainer | Custom video-to-video transformation training with advanced control adapters |
| LTX-2.3 Text-to-Video LoRA | Custom style LoRAs for text-to-video generation |
| LTX-2.3 Image-to-Video LoRA | Custom style LoRAs for image-to-video animation |

### Pricing

Training costs scale linearly:

| Steps | Cost |
|-------|------|
| 100 | $0.35 |
| 500 | $1.75 |
| 1,000 | $3.50 |
| 2,000 | $7.00 |

### Features

- No cold starts; training jobs begin immediately
- Video-first training (video clips, not static images)
- Motion-aware learning for temporal coherence
- Customizable parameters (steps, learning rate, LoRA rank)
- Audio-video sync learning during training
- Published LTX-2 to LTX-2.3 migration guide

### Use Cases

- Motion style transfer (dance, action sequences)
- Character animation with consistent movement
- Brand video production
- Artistic animation style replication
- Visual effects and transitions

## fal.ai

Cloud GPU infrastructure for LTX-2 training.

- LTX-2 Video Trainer user guide available
- Enables developers to create custom video generation models without managing GPU infrastructure
- URL: fal.ai/learn/devs/ltx-2-video-trainer-user-guide

## Oxen.ai

Managed platform for training LTX-2 Character LoRAs with automated GPU orchestration.

### Process

1. Upload training data
2. Oxen.ai orchestrates GPUs and training
3. Download trained LoRA weights

Emphasizes accessibility for non-ML-engineers: "Model training shouldn't be intimidating."

- Tutorial: "How to Train a LTX-2 Character LoRA with Oxen.ai"

## RunComfy

Browser-based LoRA training for LTX models.

- Upload dataset, configure parameters, download LoRA when complete
- Integrated with [[comfyui-ltx-integration-overview|ComfyUI]] workflows
- No GPU management required

## RunPod

Cloud GPU rental with complete guides for training video LoRAs on RunPod infrastructure.

- Provides raw GPU access rather than a managed training service
- Users run the [[ltx-video-trainer]] themselves on rented hardware

## References

- [[lora-training]]
- [[ltx-video-trainer]]
- [[lora-community-ecosystem]]
