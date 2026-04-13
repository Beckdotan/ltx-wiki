---
title: LTX-2 Overview
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-version-history-and-releases.md
  - raw/ltx2-architecture-deep-dive.md
  - raw/ltx2-capabilities-and-specifications.md
tags:
  - ltx-2
  - lightricks
  - video-generation
  - audio-video
  - foundation-model
---

# LTX-2 Overview

LTX-2 is the second generation of [[lightricks]]' video generation model family, representing a complete architectural overhaul from [[ltx-video-to-ltx-2-evolution|LTX Video (0.9.x)]]. The model was renamed from "LTXV" to "LTX-2" to reflect the magnitude of changes.

## What It Is

LTX-2 is a **19-billion parameter** asymmetric dual-stream [[ltx-2-architecture|Diffusion Transformer (DiT)]] that generates synchronized video and audio in a single inference pass. It is the first production-ready open-source model combining truly open audio and video generation.

- **Video stream:** 14B parameters for spatiotemporal video latents
- **Audio stream:** 5B parameters for 1D temporal audio latents
- **Transformer depth:** 48 layers

## Core Capabilities

- Native 4K resolution (3840x2160) at up to 50 FPS
- Up to 20 seconds of continuous video with synchronized stereo audio
- Multiple [[ltx-2-capabilities|generation modes]]: text-to-video, image-to-video, audio-to-video, video extension, retake, multi-keyframe conditioning, interpolation
- Multilingual prompt support via Gemma 3-12B text encoder

## Model Versions

| Version | Parameters | Release |
|---------|-----------|---------|
| LTX-2 | 19B | January 2026 |
| [[ltx-2.3-model|LTX-2.3]] | 22B | March 2026 |

There is no LTX-2.1 or LTX-2.2. Versioning jumped directly from LTX-2 to LTX-2.3. See [[ltx-2-version-history]] for the full timeline.

## Licensing

- Code: Apache 2.0
- Model weights: Free for companies under $10M annual revenue; above that threshold requires contacting Lightricks

## Key Links

- **GitHub:** https://github.com/Lightricks/LTX-2
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2
- **ArXiv paper:** 2601.03233
- **API:** See [[ltx-2-api-and-pricing]]

## Related Pages

- [[ltx-2-architecture]] -- Dual-stream DiT, text encoder, VAEs, bimodal CFG
- [[ltx-2-capabilities]] -- Resolution, FPS, generation modes, specifications
- [[ltx-2-model-variants]] -- Weight checkpoints, distilled models, quantizations
- [[ltx-2-benchmarks]] -- Performance benchmarks and leaderboard rankings
- [[ltx-2-lora-training]] -- LoRA, IC-LoRA, and fine-tuning
- [[ltx-2-community-reception]] -- Reviews, praise, and concerns
- [[ltx-2-nvidia-optimization]] -- NVFP4/NVFP8 acceleration
- [[ltx-desktop]] -- Local desktop application
- [[ltx-2-huggingface-ecosystem]] -- HuggingFace repos, spaces, diffusers
- [[ltx-video-to-ltx-2-evolution]] -- Migration from LTX Video 0.9.x
