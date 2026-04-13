---
title: HunyuanVideo
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-hunyuan-video.md
  - raw/open-source-comparison.md
  - raw/related-work-and-comparisons.md
tags:
  - competitor
  - tencent
  - video-generation
  - open-source
  - dit
---

# HunyuanVideo

HunyuanVideo is Tencent's open-source video foundation model that achieves performance comparable to or exceeding leading closed-source models. The family includes the original 13B parameter model and the more efficient HunyuanVideo-1.5 with 8.3B parameters. It is particularly strong at multi-person scene generation.

## Key Facts

- **Developer:** Tencent
- **Parameters:** 13B (original), 8.3B (v1.5)
- **Architecture:** [[diffusion-transformer|DiT]] with SSTA attention (v1.5)
- **Max resolution:** 1080p (via two-stage upscaling in v1.5)
- **FPS:** 16-24 fps
- **Duration:** 2-10 seconds
- **License:** Tencent Hunyuan Community License (original), open source (v1.5)

## Version History

### HunyuanVideo (Original, 2024)
- 13 billion parameters
- Outperformed Runway Gen-3, Luma 1.6, and top Chinese video models in professional human evaluation
- Full open source on GitHub and Hugging Face

### HunyuanVideo-1.5 (November 2025)
- 8.3 billion parameters (lightweight variant)
- State-of-the-art visual quality and motion coherence
- Designed for consumer-grade GPU inference
- Two-stage pipeline: DiT for initial generation + Super-Resolution Network for upscaling

### HunyuanVideo-Avatar (May 2025)
- Audio-driven human animation model built on HunyuanVideo foundation

### HunyuanCustom (May 2025)
- Multimodal-driven architecture for customized video generation

### Training Code Release (December 2025)
- Full training pipeline with FSDP, context parallel, gradient checkpointing
- LoRA tuning scripts released

## Architecture Innovations (v1.5)

- **SSTA (Selective and Sliding Tile Attention):** Prunes redundant spatiotemporal KV blocks, achieving 1.87x end-to-end speedup over FlashAttention-3 for 10-second 720p synthesis
- **Unified DiT:** Single transformer supports both text-to-video and image-to-video
- **Progressive Pre-training:** Starts at 256p/16fps, advances through 480p and 720p at 24fps
- **Step Distillation:** Reduces inference steps while maintaining quality
- **3D Causal VAE:** 16x spatial, 4x temporal compression

## Strengths

- **Exceptional multi-person scene quality** -- cinematic clarity in complex compositions
- **Consumer GPU accessible** -- v1.5 runs on a single RTX 4090 with approximately 14 GB VRAM
- **75% faster generation** on RTX 4090 with SSTA attention (videos within 75 seconds)
- **Professional-grade motion coherence** rivaling closed-source models
- **Complete training pipeline** released for community fine-tuning
- **Bilingual text understanding** (Chinese and English) with glyph-aware encoding

## Weaknesses

- **No native audio** -- video only
- **Complex setup** -- two-stage pipeline adds workflow complexity
- **Lower native resolution** -- generation is 480p-720p; 1080p requires upscaling stage
- **License ambiguity** -- Tencent Hunyuan Community License for original model is more restrictive than Apache 2.0
- **Slower than LTX** for equivalent quality output

## Comparison to LTX

HunyuanVideo excels at multi-person scenes and cinematic quality within consumer GPU constraints. [[ltx-2-overview|LTX-2]] surpasses it with native 4K, synchronized audio, faster generation, and a broader product ecosystem. HunyuanVideo's VAE uses standard 8x8x4 compression with 16 channels, compared to [[ltx-video-overview|LTX-Video]]'s more aggressive 32x32x8 compression with 128 channels.

## See Also

- [[open-source-video-generation-landscape]]
- [[wan-video]]
- [[video-generation-architectures]]
