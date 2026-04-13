# Use Case: LoRA Training and Fine-tuning

**Source:** https://huggingface.co/Lightricks/LTX-Video-ICLoRA-detailer-13b-0.9.8, https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-2.3/discussions/4

---

## Overview

LoRA (Low-Rank Adaptation) training is one of the most active use cases in the LTX Video ecosystem, with both official and community-created training tools enabling custom video effects, styles, and controls.

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

### IC-LoRA (In-Context LoRA) Training
- Advanced technique enabling video-to-video control
- Used for detailer, canny control, depth control, union control, and motion tracking
- Paper: "AVControl: Efficient Framework for Training Audio-Visual Controls" (arXiv: 2603.24793)
- Reference downscale factor allows efficient conditioning with smaller reference inputs

## Community Training Projects

### eisneim - LTX LoRA Training (i2v/t2v)
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

## Training on Quantized Models

Community discussion confirms:
- LoRA fine-tuning works on fp8 models (Discussion #92)
- LTX-2.0 trained LoRAs transfer well to LTX-2.3 (often better than on 2.0)
- Inference requires reduced LoRA strength when using 2.0 LoRAs on 2.3

## Public Training Datasets

Lightricks released public datasets for reproducible LoRA training:
- **Squish-Dataset:** https://huggingface.co/datasets/Lightricks/Squish-Dataset
- **Cakeify-Dataset:** https://huggingface.co/datasets/Lightricks/Cakeify-Dataset
- **Canny-Control-Dataset:** https://huggingface.co/datasets/Lightricks/Canny-Control-Dataset

## Types of Custom LoRAs Being Trained

1. **Motion LoRAs:** Camera movements (dolly, jib, static), bullet time, headbanging
2. **Style LoRAs:** Anime, pixel art, artistic styles
3. **Effect LoRAs:** Squish, cakeify, custom visual effects
4. **Control LoRAs:** Canny edge, depth, pose, motion tracking
5. **Likeness LoRAs:** Character-specific appearance and sound
6. **Detailer LoRAs:** Enhancing fine details in generated video
