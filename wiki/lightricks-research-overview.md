---
title: Lightricks Research Overview
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/lightricks-research-overview.md
  - raw/related-lightricks-papers.md
  - raw/research-papers-ltx-video.md
tags:
  - lightricks
  - research
  - overview
  - team
  - open-source
---

# Lightricks Research Overview

**Company:** Lightricks
**Founded:** January 2013, Jerusalem, Israel
**Employees:** ~600
**Research team on HuggingFace:** 54 members
**Contact:** ltx-video@lightricks.com / ltx-2@lightricks.com

Lightricks is a technology company focused on generative AI, computer vision, audio/speech, and diffusion models for content editing and creation. Known products include Facetune (Apple's most downloaded paid app 2017), Videoleap (Apple App of the Year 2017), and LTX Studio.

## Published Research Papers

### Core Video Generation

| Paper | arXiv | Date | Summary |
|-------|-------|------|---------|
| [[paper-ltx-video|LTX-Video]] | 2501.00103 | Dec 2024 | Foundational video generation with 1:192 compression VAE |
| [[paper-ltx-2|LTX-2]] | 2601.03233 | Jan 2026 | First open-source joint audio-visual model (19B) |
| [[ltx-2.3-technical|LTX-2.3]] | -- | Mar 2026 | Rebuilt VAE, 4x text connector, 22B parameters |

### Control and Conditioning

| Paper | arXiv | Date | Summary |
|-------|-------|------|---------|
| [[paper-avcontrol|AVControl]] | 2603.24793 | Mar 2026 | Modular LoRA-based control framework (13 modalities) |
| [[paper-cafa|CAFA]] | 2504.06778 | Apr 2025 | Controllable video-to-audio foley generation |

### Collaborative Research

| Paper | arXiv | Date | Collaborators |
|-------|-------|------|---------------|
| [[paper-just-dub-it|JUST-DUB-IT]] | 2601.22143 | Jan 2026 | Tel Aviv University |
| [[paper-id-lora|ID-LoRA]] | 2603.10256 | Mar 2026 | Tel Aviv University |

### Earlier Research (Pre-LTX)

- **Endless Loops: Detecting and Animating Periodic Patterns in Still Images** (SIGGRAPH 2021) -- early motion generation from static imagery
- **Temporally Stable Video Segmentation Without Video Annotations** (WACV 2022) -- temporal consistency theme that carries through to LTX-Video

## Research Timeline

| Year | Milestone |
|------|-----------|
| 2013 | Lightricks founded; Facetune launched |
| 2015 | Ofir Bibi joins as Senior Researcher |
| 2017 | Videoleap launched |
| 2021 | Endless Loops paper (SIGGRAPH) |
| 2022 | Temporally Stable Video Segmentation (WACV) |
| Nov 2024 | [[paper-ltx-video|LTX-Video]] 2B open-source release |
| Dec 2024 | LTX-Video paper on arXiv |
| Apr 2025 | [[paper-cafa|CAFA]] paper |
| Mar 2025 | LTX-Video v0.9.5 |
| May 2025 | LTXV-13B with multiscale rendering |
| Jul 2025 | 60-second video generation barrier broken |
| Jan 2026 | [[paper-ltx-2|LTX-2]] (19B) open-sourced |
| Jan 2026 | [[paper-just-dub-it|JUST-DUB-IT]] paper |
| Mar 2026 | [[ltx-2.3-technical|LTX-2.3]] (22B) released |
| Mar 2026 | [[paper-avcontrol|AVControl]] and [[paper-id-lora|ID-LoRA]] papers |

## Key Research Themes

### 1. High-Compression Latent Spaces
Video-VAE with 1:192 compression ratio (32x32x8), 128-channel latent space. Novel approach of relocating the patchifier from transformer to VAE. Shared denoising objective between transformer and VAE decoder. Compact audio VAE with 128-dimensional latent tokens.

### 2. Efficient Transformer Architectures
DiT-based architectures scaling from 2B to 22B parameters. Asymmetric dual-stream design for multimodal generation. RoPE with normalized fractional coordinates. Cross-attention for text conditioning (found to outperform MM-DiT).

### 3. Audio-Visual Synthesis
Evolution from silent video ([[paper-ltx-video|LTX-Video]]) to joint A/V ([[paper-ltx-2|LTX-2]]). Bidirectional cross-attention between modalities. Modality-aware classifier-free guidance. Temporal synchronization via 1D temporal RoPE in cross-attention.

### 4. Modular Control
IC-LoRA (In-Context LoRA) paradigm via [[paper-avcontrol|AVControl]]. Parallel canvas conditioning. Per-modality LoRA adapters on frozen backbone. 13 control modalities trained independently. Data and compute efficient (few hundred to few thousand steps per modality).

### 5. Real-Time Generation
Faster-than-real-time generation (core claim of LTX-Video). ~18x faster than comparable models (LTX-2 vs Wan 2.2). Distillation for further speedup (8 steps, CFG=1). Multi-scale, multi-tile inference for high-resolution output.

## Key Technical Innovations

1. **Reconstruction GAN (rGAN)** -- novel discriminator training approach ([[paper-ltx-video|LTX-Video]])
2. **Video DWT Loss** -- 3D Discrete Wavelet Transform for high-frequency detail preservation ([[paper-ltx-video|LTX-Video]])
3. **Shared Diffusion Objective** -- VAE decoder performs final denoising step in pixel space ([[paper-ltx-video|LTX-Video]])
4. **Per-Token Timestep** -- different diffusion timestep per token for conditioning ([[paper-ltx-video|LTX-Video]])
5. **Thinking Tokens** -- learnable tokens in text connector for prompt understanding ([[paper-ltx-2|LTX-2]])
6. **Modality-Aware CFG** -- separate text and cross-modal guidance scales ([[paper-ltx-2|LTX-2]])
7. **Parallel Canvas** -- reference signal as additional attention tokens for LoRA control ([[paper-avcontrol|AVControl]])

## Key Research Team Members

- **Yoav HaCohen** -- Generative AI Research Team Lead; PhD from Hebrew University; leads [[paper-ltx-video|LTX-Video]] and [[paper-ltx-2|LTX-2]]
- **Ofir Bibi** -- VP of Research; manages 40+ researchers; appears on all major papers
- **Benny Brazowski** -- Core team (LTX-Video, LTX-2)
- **Nisan Chiprut** -- Core team (LTX-Video, LTX-2)
- **Tavi Halperin** -- Controls/audio research ([[paper-cafa|CAFA]], [[paper-avcontrol|AVControl]], [[paper-just-dub-it|JUST-DUB-IT]])
- **Naomi Ken Korem** -- Controls research ([[paper-avcontrol|AVControl]], [[paper-just-dub-it|JUST-DUB-IT]])
- **Eitan Richardson** -- Core team (LTX-Video, LTX-2)
- **Victor Kulikov** -- Core team (LTX-Video, LTX-2)
- **Roi Benita** -- Audio research ([[paper-cafa|CAFA]], LTX-2)

## Open-Source Releases

### Models
- LTX-Video (all versions through v0.9.8)
- LTX-2 (19B, all variants including FP8, FP4, distilled)
- LTX-2.3 (22B, all variants)
- All IC-LoRA control models (Union Control, Detailer, Motion Track, etc.)
- Spatial and temporal upscalers

### Code
- [github.com/Lightricks/LTX-Video](https://github.com/Lightricks/LTX-Video)
- [github.com/Lightricks/LTX-2](https://github.com/Lightricks/LTX-2)
- [github.com/Lightricks/ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo)

### Open-Source Philosophy
Lightricks made LTX-Video open-source to encourage further innovation in the AI industry. All LTX model weights, code, and training infrastructure are publicly available under open-source licensing (LTX-2 under CC BY 4.0).

See [[ltx-video-citations]] for community adoption metrics and papers citing LTX-Video.
