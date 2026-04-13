---
title: LTX-2 LoRA Training and IC-LoRA
type: technical
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-lora-training-and-iclora.md
tags:
  - ltx-2
  - lora
  - ic-lora
  - fine-tuning
  - training
---

# LTX-2 LoRA Training and IC-LoRA

The [[ltx-2-overview|LTX-2]] repository includes an official training framework (`ltx-trainer`) for LoRA and IC-LoRA fine-tuning, supporting three distinct training modes.

## Repository Structure

```
LTX-2/packages/ltx-trainer/
├── README.md
├── docs/
│   └── training-modes.md
└── ... (training scripts and configs)
```

## Three Training Modes

### 1. Standard LoRA (Style/Effect/Motion)

Fine-tunes the model by adding small, trainable adapter layers while keeping the base model frozen. Requires significantly less memory and compute than full fine-tuning. Use cases: custom art styles, specific motion patterns, branded aesthetics.

### 2. AV LoRA (Audio-Video Paired)

Trains on paired audio + visual data, preserving audio-video synchronization during fine-tuning. Use cases: specific sound design styles, branded audio-visual content.

### 3. IC-LoRA (Image-Conditioned)

Enables conditioning video generation on reference video frames at inference time for fine-grained video-to-video control. Three control modes:

- **Canny** -- Edge preservation
- **Depth** -- Camera and spatial geometry
- **Pose** -- Human motion transfer

## Official IC-LoRA Adapters

### LTX-2 (19B)

- **LTX-2-19b-IC-LoRA-Union-Control** -- Union control supporting depth, pose, and edges automatically. Released in end-of-January 2026 update.

### [[ltx-2.3-model|LTX-2.3]] (22B)

- **LTX-2.3-22b-IC-LoRA-Union-Control** -- Union control for 2.3 architecture
- **LTX-2.3-22b-IC-LoRA-Motion-Track-Control** -- Motion tracking control

## Training Requirements

- **Hardware:** Officially targets NVIDIA H100 GPUs with 80GB+ VRAM. Lower VRAM setups work with gradient checkpointing and reduced resolutions.
- **Resolution constraints:** Width and height must be divisible by 32
- **Frame count:** Must follow the 8n+1 rule (1, 9, 17, 25 frames, etc.)
- **Training time:** Less than 1 hour for motion, style, or likeness tasks

## LoRA Stacking

You can stack up to **3 LoRA adapters simultaneously**, enabling blending of custom aesthetics with structural control. Example: Style LoRA + Motion LoRA + IC-LoRA depth control.

## ComfyUI IC-LoRA Usage

The **LTXVAddVideoICLoRAGuideAdvanced** node provides granular IC-LoRA strength control, global strength adjustment, and optional spatial/spatiotemporal masking.

Workflow:
1. Load base model (dev or distilled)
2. Apply IC-LoRA adapter
3. Provide conditioning input (depth map, edge map, or pose)
4. Generate with multimodal guidance

## Cloud Training Options

| Provider | URL |
|----------|-----|
| fal.ai | https://fal.ai/models/fal-ai/ltx2-video-trainer |
| WaveSpeedAI | https://wavespeed.ai |

## LoRA Compatibility Warning

**LTX-2 LoRAs are NOT compatible with LTX-2.3.** Architecture changes (new VAE, expanded text connector, parameter count) break backward compatibility. All custom LoRAs need retraining; training data can be reused but configs need updating.

Migration guide: https://wavespeed.ai/blog/posts/ltx-2-to-ltx-2-3-upgrade-guide-2026/

## Documentation

- Official training docs: https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
- LoRA usage guide: https://docs.ltx.video/open-source-model/usage-guides/lo-ra
- IC-LoRA guide: https://ltx.io/model/model-blog/how-to-use-ic-lora-in-ltx-2

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2-model-variants]] -- Base model variants for fine-tuning
- [[ltx-2-huggingface-ecosystem]] -- Where to find adapters
- [[ltx-2.3-model]] -- LTX-2.3 compatibility notes
