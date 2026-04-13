---
title: "LTX-2.3: Technical Improvements"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-ltx-2.3-technical.md
tags:
  - ltx-2
  - ltx-2.3
  - vae
  - audio-visual
  - lightricks
---

# LTX-2.3: Technical Improvements

**Developer:** [[lightricks-research-overview|Lightricks]]
**Release Date:** March 5, 2026
**Parameters:** 22B
**Model Page:** [ltx.io](https://ltx.io/model/ltx-2-3) | [HuggingFace](https://huggingface.co/Lightricks/LTX-2.3)
**Note:** No separate arXiv paper; builds on the [[paper-ltx-2|LTX-2 paper]] (arXiv:2601.03233)

## Summary

LTX-2.3 is a 22-billion-parameter open-source video model generating native 4K at up to 50 FPS with synchronized audio in a single pass. It represents a significant upgrade over [[paper-ltx-2|LTX-2]] with four major architectural improvements: a rebuilt VAE, a gated attention text connector, an improved HiFi-GAN vocoder, and a refined latent space.

## Key Technical Improvements Over LTX-2

### 1. Rebuilt Video VAE

- Redesigned VAE producing sharper fine details, more realistic textures, and cleaner edges
- PSNR improvement of approximately 2.5 dB over predecessor
- Fine textures like hair, facial features, text, and edges better preserved through the full generation pipeline
- Sharper fabric, cleaner hair, and stable chrome reflections during camera moves

### 2. Gated Attention Text Connector

- 4x larger text connector than LTX-2
- Complex prompts with multiple subjects, spatial relationships, and stylistic instructions now resolve accurately
- Gated attention mechanism improves prompt adherence
- Still uses Gemma 3 12B as text encoder

### 3. Improved HiFi-GAN Vocoder

- Cleaner audio generation with stereo output at 24 kHz
- Upgraded vocoder produces cleaner, more realistic audio
- Improved foley and ambient sound quality

### 4. Refined Latent Space

- Updated VAE trained on higher-quality data
- Fine textures, hair, text, and edge detail better preserved
- Reduced artifacts in high-motion scenes

## Architecture Specifications

| Component | Specification |
|-----------|--------------|
| Total parameters | 22B |
| Video stream | ~14B parameters |
| Audio stream | ~5B parameters |
| Text connector | 4x larger than LTX-2 |
| Architecture type | Dual-stream asymmetric DiT |
| Text encoder | Gemma 3 12B |
| Context window | 1,000 tokens |
| Max resolution | 4K |
| Max frame rate | 50 fps |
| Max duration | 20 seconds |
| Audio output | Stereo 24 kHz |

## Core Architecture (Inherited from LTX-2)

- Dual-stream asymmetric diffusion transformer
- Bidirectional audio-video cross-attention with temporal RoPE
- Cross-modality AdaLN conditioning
- Modality-aware classifier-free guidance (modality-CFG)

Built on a Diffusion Transformer (DiT) foundation that combines iterative refinement of diffusion models with sequence modeling power of transformers. The DiT architecture scales more efficiently with compute and data than traditional diffusion U-Nets, enabling processing of longer video sequences at higher resolutions.

## Relationship to Other Work

LTX-2.3 is the latest model in the lineage from [[paper-ltx-video|LTX-Video]] through [[paper-ltx-2|LTX-2]]. The [[paper-avcontrol|AVControl]] framework and its downstream applications ([[paper-just-dub-it|JUST-DUB-IT]], [[paper-id-lora|ID-LoRA]]) are built on the LTX-2 backbone which LTX-2.3 extends.
