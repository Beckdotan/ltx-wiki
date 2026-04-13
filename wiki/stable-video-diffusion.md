---
title: Stable Video Diffusion
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-other-notable.md
  - raw/related-work-and-comparisons.md
tags:
  - competitor
  - stability-ai
  - video-generation
  - open-source
  - legacy
---

# Stable Video Diffusion

Stable Video Diffusion (SVD) is Stability AI's entry into video generation, building on the Stable Diffusion image generation ecosystem. It uses a latent diffusion architecture with temporal layers for image-to-video generation. The [[related-work-and-comparisons|LTX-Video paper]] cites SVD as an influential video diffusion model with a different approach to image conditioning.

## Key Facts

- **Developer:** Stability AI
- **Architecture:** Latent diffusion with temporal layers
- **Primary mode:** Image-to-video only (no text-to-video)
- **Resolution:** 576x1024
- **Duration:** Approximately 4 seconds (14-25 frames)
- **License:** Stability AI Community License
- **arXiv:** 2311.15127

## Strengths

- Strong image-to-video quality for short clips
- Leverages the Stable Diffusion ecosystem
- Good motion quality within its limited scope

## Weaknesses

- **Image-to-video only** -- no text-to-video capability
- **Short duration** -- approximately 4 seconds
- **Lower resolution** -- 576x1024
- **Stability AI's uncertain corporate future** impacts ongoing model support
- **Surpassed** by newer dedicated video models across the board

## Architectural Influence

SVD's approach to image conditioning influenced later models. The [[related-work-and-comparisons|LTX-Video paper]] references SVD as using a U-Net based approach, a different paradigm from the [[diffusion-transformer|DiT]]-based architecture that [[ltx-video-overview|LTX-Video]] adopted.

## See Also

- [[animatediff]]
- [[open-source-video-generation-landscape]]
- [[video-generation-architectures]]
