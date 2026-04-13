---
title: LoRA Training and Fine-Tuning
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/use-cases-lora-training-finetuning.md
tags:
  - lora
  - training
  - fine-tuning
  - ic-lora
  - customization
  - use-cases
---

# LoRA Training and Fine-Tuning

LoRA (Low-Rank Adaptation) training is one of the most active use cases in the [[ltx-video-overview|LTX-Video]] ecosystem, with both official and community-created training tools enabling custom video effects, styles, and controls.

## Official Training Tools

### LTX-Video Trainer

- **Repository:** https://github.com/Lightricks/ltx-video-trainer
- Supports training LoRAs for LTX-Video 0.9.x models
- Used to create official Squish and Cakeify LoRAs

### LTX-2 Trainer

- **Repository:** https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- Part of the LTX-2 monorepo
- **Training time:** Under 1 hour for motion, style, or likeness LoRAs
- Supports training on audio+video joint data

### IC-LoRA (In-Context LoRA)

An advanced technique enabling video-to-video control, used for:

- Detailer (enhancing fine details)
- Canny edge control
- Depth control
- Union control
- Motion tracking

Paper: "AVControl: Efficient Framework for Training Audio-Visual Controls" (arXiv: 2603.24793). Uses a reference downscale factor for efficient conditioning with smaller reference inputs.

## Community Training Projects

### eisneim - LTX LoRA Training

- **Repository:** https://github.com/eisneim/ltx_lora_training_i2v_t2v
- Community-created training code for both image-to-video and text-to-video LoRAs
- Released the "bullet_time" LoRA as a demonstration

### Community-Trained LoRAs

| Creator | LoRA | Training Focus |
|---------|------|----------------|
| Burgstall | amgery-lora | Facial expressions |
| Burgstall | nhl-smile-lora | Smile animation |
| Burgstall | headbanger-lora | Head movement |
| svjack | anime_landscape | Anime-style landscapes |
| svjack | pixel_early | Pixel art style |
| Pierre-Jean | trajectory-lora | Camera trajectories |

## Types of Custom LoRAs

1. **Motion LoRAs:** Camera movements (dolly, jib, static), bullet time, headbanging
2. **Style LoRAs:** Anime, pixel art, artistic styles
3. **Effect LoRAs:** Squish, cakeify, custom visual effects
4. **Control LoRAs:** Canny edge, depth, pose, motion tracking
5. **Likeness LoRAs:** Character-specific appearance and sound
6. **Detailer LoRAs:** Enhancing fine details in generated video

## Training on Quantized Models

- LoRA fine-tuning works on FP8 models (confirmed in Discussion #92)
- LTX-2.0 trained LoRAs transfer well to LTX-2.3 (often better than on 2.0)
- Inference requires reduced LoRA strength when using 2.0 LoRAs on 2.3

## Public Training Datasets

Lightricks released public datasets for reproducible LoRA training:

- **Squish-Dataset:** https://huggingface.co/datasets/Lightricks/Squish-Dataset
- **Cakeify-Dataset:** https://huggingface.co/datasets/Lightricks/Cakeify-Dataset
- **Canny-Control-Dataset:** https://huggingface.co/datasets/Lightricks/Canny-Control-Dataset

## External Training Guides

- WaveSpeedAI: "LTX-2.3 LoRA Training Guide: Style, Motion & IC-LoRA Control" -- https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/
- Oxen.ai: "How to Train a LTX-2 Character LoRA with Oxen.ai" -- https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/

## See Also

- [[use-cases-overview]] -- Full use case landscape
- [[hardware-requirements]] -- GPU requirements for training
- [[installation-quickstart]] -- Getting the training tools set up
- [[tutorials-and-community-guides]] -- External tutorials
