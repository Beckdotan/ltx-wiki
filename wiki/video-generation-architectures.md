---
title: Video Generation Architectures
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/related-work-and-comparisons.md
  - raw/competitor-model-other-notable.md
  - raw/open-source-comparison.md
tags:
  - architecture
  - video-generation
  - taxonomy
  - dit
---

# Video Generation Architectures

A taxonomy of the architectural approaches used by open-source video generation models, synthesized from the [[related-work-and-comparisons|LTX-Video and LTX-2 papers]] and the broader [[open-source-video-generation-landscape]].

## Video Generation Architecture Families

| Approach | Examples | Notes |
|----------|---------|-------|
| **DiT + MM-DiT** | SD3, FLUX.1, [[cogvideo\|CogVideoX]] | Symmetric multimodal attention |
| **DiT + Cross-attention** | PixArt-alpha, [[ltx-video-overview\|LTX-Video]] | LTX-Video's choice; found to outperform MM-DiT |
| **[[asymmetric-diffusion-transformer\|AsymmDiT]]** | [[mochi\|Mochi 1]] | 4x more parameters for visual processing |
| **U-Net based** | [[stable-video-diffusion\|SVD]], earlier models | Older paradigm, not used by leading 2025-2026 models |
| **Plugin/motion injection** | [[animatediff\|AnimateDiff]] | Adds motion to existing image models; largely obsolete |
| **MoE diffusion** | [[wan-video\|Wan 2.2]] | Mixture of Experts for efficient scaling |

## Audio-Visual Generation Approaches

A critical architectural decision for models generating both video and audio.

| Approach | Examples | Trade-offs |
|----------|---------|------------|
| **Sequential V2A** | MMAudio, CAFA, FoleyCrafter | Sub-optimal -- misses joint distribution |
| **Sequential A2V** | DiffFoley | Sub-optimal -- audio drives video |
| **Symmetric dual-stream** | Ovi, BridgeDiT | High computational overhead |
| **Asymmetric dual-stream** | [[ltx-2-overview\|LTX-2]] | Efficient and high quality; 14B video + 5B audio |
| **Proprietary joint** | Veo 3 | Closed-source |

LTX-2 is the first production-ready open-source model using the asymmetric dual-stream approach, which captures the joint audio-video distribution while keeping computational overhead manageable.

## VAE Compression Strategies

How models compress raw video into latent space for efficient generation.

| Strategy | Spatial | Temporal | Channels | Model |
|----------|---------|----------|----------|-------|
| Standard + patchifier | 8x8 | 4-8x | 16 | Most models ([[hunyuan-video]], [[cogvideo\|CogVideoX]]) |
| High compression, no patchifier | 32x32 | 8x | 128 | [[ltx-video-overview\|LTX-Video]] |
| Causal compression to 96x | 8x8 | 6x | 12 | [[mochi\|Mochi 1]] |
| 3D Causal VAE (2% of original) | 8x8 | 4x | -- | [[cogvideo\|CogVideoX]] |

LTX-Video's 32x32x8 with 128 channels is the most aggressive compression strategy, inspired by DC-VAE research showing that text-to-image transformers work better with higher spatial compression.

## Key Design Influences on LTX

Several models and papers directly influenced LTX-Video's architectural decisions:

- **PixArt-alpha** -- Direct base architecture; extends DiT with cross-attention for text conditioning
- **DC-VAE** -- Demonstrated higher spatial compression works better; inspired LTX-Video's 128-channel approach
- **SimpleDiffusion** -- Showed higher resolutions need more noising for proper SNR; influenced resolution-dependent timestep scheduling
- **StyleGAN** -- Multi-layer noise injection adopted for LTX-Video's VAE decoder
- **Gemma 3** (12B) -- Replaced T5-XXL as text encoder in LTX-2, enabling multilingual support
- **HiFi-GAN** -- Audio vocoder architecture adapted for LTX-2's stereo audio reconstruction

## Convergence Trends (2026)

1. **DiT dominance** -- Most leading models now use Diffusion Transformer variants
2. **Audio-video unification** -- LTX-2 leads; others expected to follow
3. **Consumer GPU accessibility** -- VRAM requirements dropping to 8-16 GB
4. **MoE adoption** -- [[wan-video|Wan 2.2]]'s approach gaining traction
5. **Native 4K** -- LTX-2 first to achieve it; others targeting similar
6. **LoRA/fine-tuning** as a standard feature across all major models

## See Also

- [[open-source-video-generation-landscape]]
- [[ltx-video-overview]]
- [[ltx-2-overview]]
