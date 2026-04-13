---
title: Video DiT Transformer
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/architecture-video-dit-transformer.md
  - raw/architecture-video-dit-vae.md
  - raw/ltx-video-architecture-paper-details.md
  - raw/ltx-video-architecture-technical.md
  - raw/ltx-video-transformer-dit-architecture.md
tags:
  - transformer
  - dit
  - diffusion
  - attention
  - ltx-video
---

# Video DiT Transformer

The Video DiT (Diffusion Transformer) is the denoising backbone of the [[ltx-video-architecture-overview|LTX-Video architecture]]. It is built upon the PixArt-alpha architecture, which extends the DiT framework for open text conditioning rather than ImageNet class labels.

## Specifications

| Parameter | Value |
|-----------|-------|
| Hidden Dimension | 2048 |
| Attention Blocks | 28 |
| FFN Dimension Factor | 4x |
| Total Parameters | ~1.9B (original), 13B (LTXV-13B) |
| Attention Type | Self-attention + Cross-attention |
| Normalization | RMSNorm (replacing LayerNorm) |
| Timestep Conditioning | AdaLN (Adaptive Layer Normalization) |

## 3D Transformer Block

Each block performs the following operations in sequence:

1. **Self-attention** with [[rope-positional-encoding|RoPE]] and QK normalization
2. **Cross-attention** with text embeddings (see [[text-conditioning]])
3. **Feed-forward network** (4x hidden dim)
4. **AdaLN modulation** for timestep and conditioning

## Full Spatiotemporal Self-Attention

A critical design choice: the transformer applies full spatiotemporal self-attention across all video tokens. This is computationally quadratic in token count, making it feasible only because of the extreme [[latent-space-compression]] (1:8192 pixels-to-tokens ratio). Without the aggressive VAE compression, full attention over all spatial and temporal positions would be prohibitively expensive.

## QK Normalization

- RMSNorm applied to queries and keys before dot-product attention
- Prevents extremely large attention logit values
- Avoids near-zero entropy attention weights (where attention collapses to focus on one token)
- RMSNorm found to outperform LayerNorm for this purpose
- Based on findings from ScalingViT and LargeDiT

## RMSNorm

RMSNorm replaces LayerNorm throughout the entire architecture. It is both more computationally efficient and empirically better performing in this context.

## Scaling to 13B (LTXV-13B)

The 13B variant introduced **multiscale rendering** -- a layered generation process:

1. **Coarse pass**: Draft at lower detail to capture coarse motion
2. **Progressive refinement**: Progressively add structure, lighting, and micro-motion
3. **Built-in upscaling**: For 1080p output, first render at 960x540 then upscale 2x

This approach yields render times up to 30x faster than comparable-sized models and supports videos up to 60 seconds.

## Dual-Stream Architecture (LTX-2)

LTX-2 extended the transformer to an asymmetric dual-stream design:

- **Video stream**: 14B parameters, processes 3D spatiotemporal latents
- **Audio stream**: 5B parameters, processes 1D temporal latents
- **48 transformer layers** (up from 28)
- Shared depth but different widths

Each dual-stream block contains:
1. Self-attention within the same modality
2. Text cross-attention for prompt conditioning
3. Audio-visual cross-attention for inter-modal exchange
4. Feed-forward network

Cross-modal attention is bidirectional: video queries attend to audio keys/values and vice versa, using temporal 1D RoPE for alignment.
