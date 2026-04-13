---
title: LTX-2.3 Model
type: technical
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-2.3-model-details.md
  - raw/ltx2-version-history-and-releases.md
  - raw/ltx2-architecture-deep-dive.md
tags:
  - ltx-2
  - ltx-2.3
  - model
  - foundation-model
---

# LTX-2.3 Model

LTX-2.3 is the latest version of the [[ltx-2-overview|LTX-2]] audio-visual foundation model family, released in March 2026. It scales to **22 billion parameters** (up from 19B in LTX-2) and brings improvements across visual quality, audio fidelity, prompt understanding, and portrait support.

## Key Improvements over LTX-2

- **Sharper details:** Pores, fine baby hairs, eye corners survive motion better; improved denim, linen, brushed steel textures
- **Stronger motion:** Up to 48 FPS for smoother motion without soap-opera look
- **Cleaner audio:** Training set filtered for silence/noise/artifacts; new vocoder
- **Native portrait:** Up to 1080x1920, trained on vertical data (not cropped from landscape)
- **Better prompt understanding:** 4x larger text connector with gated attention architecture
- **Text rendering improvements**
- **Redesigned VAE:** Sharper fine details, more realistic textures, cleaner edges, less "beauty filter" effect
- **Temporal upscaler:** New latent-space frame rate doubling (in addition to existing spatial upscalers)

## Model Tiers

| Tier | Max Duration | Key Traits |
|------|-------------|------------|
| LTX-2.3 Fast | 20 seconds | 2x faster inference, 1/10 compute cost |
| LTX-2.3 Pro | 10 seconds | Best quality, supports all endpoints (audio-to-video, retake, extend) |

## Model Checkpoints

| Checkpoint | Purpose |
|-----------|---------|
| `ltx-2.3-22b-dev` | Full model (bf16, ~42GB), flexible, trainable |
| `ltx-2.3-22b-distilled` | 8-step distilled (CFG=1), faster inference |
| `ltx-2.3-22b-distilled-lora-384` | LoRA for distilled/full models |
| `ltx-2.3-spatial-upscaler-x2-1.1` | 2x spatial upscaling |
| `ltx-2.3-spatial-upscaler-x1.5-1.0` | 1.5x spatial upscaling |
| `ltx-2.3-temporal-upscaler-x2-1.0` | 2x temporal upscaling (higher FPS) |

## IC-LoRA Control Models

| Model | Controls |
|-------|----------|
| LTX-2.3-22b-IC-LoRA-Union-Control | Canny + Depth + Pose |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | Motion tracking |

See [[ltx-2-lora-training]] for LoRA training and usage details.

## Technical Constraints

- Width and height must be divisible by 32
- Frame count must follow the 8n+1 rule (1, 9, 17, 25 frames, etc.)
- Works best under 720x1280 resolution for base generation; use [[ltx-2-model-variants|spatial upscalers]] for higher resolutions

## Requirements

- Python >= 3.12
- CUDA > 12.7
- PyTorch ~= 2.7
- Supports bfloat16 and quantized formats (fp8, fp4)

## LoRA Compatibility Warning

LTX-2 LoRAs are **NOT compatible** with LTX-2.3 due to architecture changes (new VAE, expanded text connector, parameter count). LoRAs must be retrained. See [[ltx-2-lora-training]] for migration guidance.

## Architectural Continuity

LTX-2.3 maintains the same [[ltx-2-architecture|architectural principles]] as LTX-2: asymmetric dual-stream DiT, bidirectional cross-attention, modality-aware CFG. The improvements come from additional training, scaling to 22B parameters, and component redesigns (VAE, text connector, vocoder).

## Download Statistics

- 1.66M monthly downloads on HuggingFace (as of April 2026)

## Links

- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2.3
- **Product page:** https://ltx.io/model/ltx-2-3

## Related Pages

- [[ltx-2-overview]] -- Model family overview
- [[ltx-2-version-history]] -- Release timeline
- [[ltx-2-architecture]] -- Shared architectural foundation
- [[ltx-2.3-technical]] -- Detailed technical improvements (VAE, vocoder, text connector)
- [[ltx-2-3-huggingface]] -- HuggingFace model card
- [[ltx-desktop]] -- Desktop app released alongside LTX-2.3
- [[ltx-2-benchmarks]] -- Performance comparisons
