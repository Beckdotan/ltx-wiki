# LTX-2 LoRA Training, IC-LoRA, and Fine-Tuning

## Overview

The LTX-2 repository includes an official training framework (ltx-trainer) for LoRA and IC-LoRA fine-tuning. The trainer supports three distinct training modes and is designed for both individual developers and production teams.

## Repository Structure

```
LTX-2/packages/ltx-trainer/
├── README.md
├── docs/
│   └── training-modes.md
└── ... (training scripts and configs)
```

## Three LoRA Training Modes

### 1. Standard LoRA (Style/Effect/Motion)
- Fine-tunes the model by adding small, trainable adapter layers while keeping base model frozen
- Requires significantly less memory and compute than full fine-tuning
- Use cases: Custom art styles, specific motion patterns, branded aesthetics

### 2. AV LoRA (Audio-Video Paired)
- Trains on paired audio + visual data
- Preserves audio-video synchronization during fine-tuning
- Use cases: Specific sound design styles, branded audio-visual content

### 3. IC-LoRA (Image-Conditioned)
- Enables conditioning video generation on reference video frames at inference time
- Allows fine-grained video-to-video control on top of text-to-video base model
- Three control modes:
  - **Canny** -- Edge preservation
  - **Depth** -- Camera and spatial geometry
  - **Pose** -- Human motion transfer

## Official IC-LoRA Adapters

### LTX-2 (19B)
- **LTX-2-19b-IC-LoRA-Union-Control** -- Union control supporting depth, pose, and edges automatically
  - Trained to work on a small grid
  - Automatically chooses the right condition given provided input
  - Released in end-of-January 2026 update

### LTX-2.3 (22B)
- **LTX-2.3-22b-IC-LoRA-Union-Control** -- Union control for 2.3 architecture
- **LTX-2.3-22b-IC-LoRA-Motion-Track-Control** -- Motion tracking control

## Training Requirements

### Hardware
- Officially targets NVIDIA H100 GPUs with 80GB+ VRAM
- Lower VRAM setups can work with gradient checkpointing and reduced resolutions

### Resolution Constraints
- Width and height must be divisible by 32
- Frame count must follow the **8n+1 rule** (1, 9, 17, 25 frames, etc.)

### Configuration
- Reference configurations provided in the repository
- Multi-GPU support included
- Evaluation tools bundled for assessing fine-tuned models

## LoRA Stacking

- You can stack up to **3 LoRA adapters simultaneously**
- Enables blending custom aesthetics with structural control
- Example: Style LoRA + Motion LoRA + IC-LoRA depth control

## Cloud Training Options

### fal.ai
- LTX-2 Video Trainer endpoint available
- https://fal.ai/models/fal-ai/ltx2-video-trainer
- Cloud-based training without local GPU requirements

### WaveSpeedAI
- LTX-2 19b IC-LoRA Trainer
- https://wavespeed.ai

## LoRA Compatibility Warning

**CRITICAL:** LTX-2 LoRAs are NOT compatible with LTX-2.3. The architecture changes in LTX-2.3 (new VAE, expanded text connector, parameter count) break backward compatibility. LoRAs must be retrained for each major version.

### Migration from LTX-2 to LTX-2.3
- Detailed migration guide: https://wavespeed.ai/blog/posts/ltx-2-to-ltx-2-3-upgrade-guide-2026/
- All custom LoRAs need retraining
- Training data can be reused but configs need updating

## ComfyUI IC-LoRA Usage

### LTXVAddVideoICLoRAGuideAdvanced Node
- Granular IC-LoRA strength control
- Global strength adjustment
- Optional spatial/spatiotemporal masking

### Workflow
1. Load base model (dev or distilled)
2. Apply IC-LoRA adapter
3. Provide conditioning input (depth map, edge map, or pose)
4. Generate with multimodal guidance

## Documentation

- **Official training docs:** https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
- **LoRA usage guide:** https://docs.ltx.video/open-source-model/usage-guides/lo-ra
- **IC-LoRA guide:** https://ltx.io/model/model-blog/how-to-use-ic-lora-in-ltx-2
- **Training modes:** https://huggingface.co/spaces/Lightricks/ltx-2/blob/f48dcae23478df95ffcb35015f4ea923d274e342/packages/ltx-trainer/docs/training-modes.md

## Sources

- [LTX-2 GitHub -- ltx-trainer README](https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md)
- [LTX-2 Training Documentation](https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training)
- [LTX-2 LoRA Usage Guide](https://docs.ltx.video/open-source-model/usage-guides/lo-ra)
- [LTX-2.3 LoRA Training Guide -- WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/)
- [Medium -- Train Your Own LoRA for LTX2](https://medium.com/@alexbuzunov_81436/train-your-own-lora-for-ltx2-with-ltx-trainer-standard-av-and-ic-setups-a0c15572ea83)
- [LTX-2 to LTX-2.3 Upgrade Guide -- WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-to-ltx-2-3-upgrade-guide-2026/)
- [End-of-January LTX-2 Drop](https://ltx.io/model/model-blog/end-of-january-ltx-2-drop)
- [fal.ai LTX-2 Video Trainer](https://fal.ai/models/fal-ai/ltx2-video-trainer)
