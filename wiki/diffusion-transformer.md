---
title: Diffusion Transformer (DiT)
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-090-initial-release.md
  - raw/ltx-video-early-versions-090-091-095.md
tags:
  - ltx-video
  - dit
  - architecture
  - transformer
  - diffusion
---
# Diffusion Transformer (DiT)

The Diffusion Transformer (DiT) is the architectural paradigm used by LTX-Video, replacing the U-Net backbone traditionally used in diffusion models with a transformer-based architecture. LTX-Video was claimed to be the first DiT-based video generation model capable of real-time generation.

## LTX-Video's DiT Configuration

| Property | Value |
|----------|-------|
| Parameters | ~1.9B (2B models) |
| Blocks | 28 |
| Hidden dimension | 2048 |
| Text encoder | T5-XXL |
| Training | Rectified-flow with velocity prediction |

## How DiT Differs from U-Net Diffusion

Traditional diffusion models (e.g., Stable Diffusion 1.x/2.x) use a U-Net architecture with skip connections for the denoising network. DiT replaces this with a transformer that processes the noisy latent as a sequence of patches (tokens), applying self-attention across all patches.

Advantages of the transformer approach for video:
- **Scalability:** Transformers scale more predictably with parameters
- **Temporal modeling:** Self-attention naturally handles temporal relationships between frames
- **Unified architecture:** The same architecture handles spatial and temporal dimensions

## The Patchification Innovation

In standard DiT models, the input latent is divided into patches (patchification) within the transformer. LTX-Video moves this step into the [[video-vae]], achieving a 1:192 compression ratio before the latent even reaches the transformer. This design choice is central to achieving real-time performance with only ~2B parameters.

## Scale Comparison

LTX-Video's DiT approach achieves competitive quality with far fewer parameters than rival models:

| Model | Parameters | Architecture |
|-------|-----------|--------------|
| [[ltx-video-090|LTX-Video 0.9.0]] | ~2B | DiT |
| CogVideoX | 2B / 5B | DiT |
| HunyuanVideo | 13B | DiT |
| MovieGen (Meta) | 30B | Transformer-based |

The efficiency comes primarily from the [[video-vae]]'s extreme compression rather than from architectural novelty in the transformer itself.

## Related Pages

- [[video-dit-transformer]] -- detailed technical deep dive into the Video DiT transformer internals

## References

- [arXiv paper: LTX-Video: Realtime Video Latent Diffusion](https://arxiv.org/abs/2501.00103)
- See [[ltx-video-090]] for full architecture details
- See [[video-vae]] for the compression innovation
