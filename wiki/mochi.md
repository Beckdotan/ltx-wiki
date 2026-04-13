---
title: Mochi 1
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-mochi.md
  - raw/open-source-comparison.md
tags:
  - competitor
  - genmo
  - video-generation
  - open-source
  - asymmetric-diffusion-transformer
---

# Mochi 1

Mochi 1 is Genmo's 10 billion parameter open-source video generation model, built on a novel [[asymmetric-diffusion-transformer]] (AsymmDiT) architecture. Released as an open research preview, Mochi 1 focuses on realistic motion dynamics and strong prompt fidelity, with particular strength in physics simulation. While landmark at release, it has been significantly outpaced by newer models as of 2026.

## Key Facts

- **Developer:** Genmo
- **Parameters:** 10 billion
- **Architecture:** [[asymmetric-diffusion-transformer]] (AsymmDiT)
- **Resolution:** 480p (base); HD version planned but timeline uncertain
- **FPS:** 30 fps
- **Duration:** Up to 5.4 seconds
- **VRAM:** 42 GB optimal; approximately 20 GB with community optimizations
- **License:** Apache 2.0

## Architecture

The [[asymmetric-diffusion-transformer]] dedicates 4x more parameters to visual processing than text encoding. It uses multi-modal self-attention with separate MLP layers for each modality, efficiently processing text prompts alongside compressed video tokens.

### Video VAE
- Causally compresses videos to 96x smaller size
- 8x8 spatial compression, 6x temporal compression
- 12-channel latent space
- Open-sourced alongside the model

## Strengths

- **Exceptional motion realism** -- smooth 30 fps output with realistic physics
- **Strong prompt fidelity** -- high alignment between text and generated video
- **Physics simulation quality** -- fluid dynamics, fur/hair, cloth simulation
- **Consistent human motion** -- fluid and natural action sequences
- **Apache 2.0 license** for commercial use
- **Originally required 4x H100** -- community optimized down to a single RTX 4090

## Weaknesses

- **480p only** -- lowest resolution among major competitors
- **Short duration** -- limited to 5.4 seconds
- **Photorealistic only** -- does not perform well with animated or stylized content
- **No image-to-video** -- text-to-video only
- **No native audio**
- **Development pace unclear** -- Mochi 1 HD timeline not confirmed as of early 2026
- **Surpassed** by [[wan-video]], [[hunyuan-video]], and [[ltx-2-overview|LTX-2]]

## Comparison to LTX

LTX-2 surpasses Mochi in every practical dimension: native 4K vs 480p, up to 50 FPS vs 30 FPS, 20 seconds vs 5.4 seconds duration, native synchronized audio, image-to-video support, and a far more complete production ecosystem. Mochi's main historical contribution is the [[asymmetric-diffusion-transformer]] architecture and its demonstration of physics simulation quality.

## See Also

- [[asymmetric-diffusion-transformer]]
- [[open-source-video-generation-landscape]]
