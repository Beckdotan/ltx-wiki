---
title: LoRA Ecosystem
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-lora-ecosystem.md
  - raw/community-project-camera-control-loras.md
  - raw/community-project-ic-lora-controls.md
  - raw/community-project-lora-training-tools.md
  - raw/social-lora-training-community-ecosystem.md
tags:
  - lora
  - ic-lora
  - finetune
  - training
  - community
  - lightricks
---

# LoRA Ecosystem

The LTX model family has a rich ecosystem of LoRA (Low-Rank Adaptation) adapters, spanning official releases by [[lightricks]] and community-created variants. This page covers the full inventory and training tools.

## Official LoRAs by Lightricks

### Style and Effect LoRAs (LTX-Video)

| LoRA | Base Model | Purpose | Likes |
|------|-----------|---------|-------|
| LTX-Video-Squish-LoRA | v0.9.5 | Squish/deformation effects | 15 |
| LTX-Video-Cakeify-LoRA | v0.9.5 | Object-to-cake transformation | 19 |

- **Squish LoRA** trigger: `SQUISH two hands squeezing a squeezable object that is shaped like [your object]`
- **Cakeify LoRA** trigger: `CAKEIFY a person using a knife to cut a cake shaped like [your object]`
- Both include public training datasets and ComfyUI workflows

### IC-LoRA Control Adapters

See [[ic-lora]] for the underlying technology.

| Model | Base | Downloads | Likes | Purpose |
|-------|------|-----------|-------|---------|
| ICLoRA-detailer-13b-0.9.8 | LTXV 13B | 29,812/mo | 29 | Detail/texture enhancement |
| ICLoRA-canny-13b-0.9.7 | LTXV 13B | 405/mo | 13 | Edge-based control |
| ICLoRA-depth-13b-0.9.7 | LTXV 13B | 362/mo | 13 | Depth-based control |
| LTX-2-19b-IC-LoRA-Detailer | LTX-2 | -- | 62 | Detail refinement for LTX-2 |
| LTX-2.3-22b-IC-LoRA-Union-Control | LTX-2.3 | -- | 43 | Canny + Depth + Pose |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | LTX-2.3 | -- | 32 | Sparse point trajectory guidance |

### Camera Control LoRAs

See [[camera-control-loras]] for detailed information on camera movement controls (dolly, jib, static) and the motion track system.

## Community-Created LoRAs

| Creator | LoRA Name | Purpose | Likes |
|---------|-----------|---------|-------|
| Burgstall | amgery-lora-for-ltxv-13b-0.9.7 | Angry expressions | 15 |
| Burgstall | nhl-smile-lora-for-ltxv-13b-0.9.7 | Smile effects | 1 |
| Burgstall | sorpresa-lora-for-ltxv-13b | Surprise expressions | 0 |
| Burgstall | headbanger-lora-for-ltxv-13b-0.9.7 | Head movement | 0 |
| eisneim | LTX-video_lora_bullet_time | Bullet time effect | 0 |
| svjack | ltx_video_anime_landscape_early_lora | Anime landscapes | 1 |
| svjack | ltx_video_pixel_early_lora | Pixel art style | 0 |
| Pierre-Jean | ltx-video-trajectory-lora | Camera trajectory | 0 |
| Sameric934 | ltx2-video-lora | General video | 0 |
| xuaxu | ltxvideo_lora_mine | Custom LoRA | 0 |

Burgstall is the most prolific community LoRA creator, focusing on facial expression and head movement effects.

## LoRA Training Tools

### Official Trainers

- **LTX-Video Trainer:** https://github.com/Lightricks/ltx-video-trainer
- **LTX-2 Trainer:** https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- Training time: Under 1 hour for motion, style, or likeness (sound+appearance) LoRAs

### Community Training

- **eisneim** published a community training repository: https://github.com/eisneim/ltx_lora_training_i2v_t2v
- Community experimentation confirmed LoRA fine-tuning works on FP8 models (Discussion #92)

## See Also

- [[ic-lora]] -- In-Context LoRA technology details
- [[camera-control-loras]] -- Camera movement control LoRAs
- [[community-models-finetunes]] -- Broader ecosystem of community models
- [[ltx-video-overview]] -- LTX Video base model
- [[ltx-2-overview]] -- LTX-2 base model
- [[lora-training]] -- LoRA training guide
- [[lora-community-ecosystem]] -- Community training practices and discoveries
- [[ltx-video-trainer]] -- Official training tools
- [[third-party-training-services]] -- Cloud-based training options
