---
title: Asymmetric Diffusion Transformer (AsymmDiT)
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-mochi.md
tags:
  - architecture
  - diffusion-transformer
  - video-generation
---

# Asymmetric Diffusion Transformer (AsymmDiT)

The Asymmetric Diffusion Transformer (AsymmDiT) is a novel architecture introduced by Genmo in their [[mochi|Mochi 1]] video generation model. It represents an alternative approach to multimodal diffusion within the broader family of [[dit-architecture|Diffusion Transformer]] architectures.

## Core Design

AsymmDiT dedicates **4x more parameters to visual processing** than to text encoding. This asymmetry is its defining characteristic -- the architecture recognizes that video generation is fundamentally a visual reasoning task and allocates neural network capacity accordingly.

## Key Features

- **Multi-modal self-attention** with separate MLP layers for each modality (text and video)
- **Efficient prompt processing** alongside compressed video tokens
- **Streamlined text processing** that frees capacity for visual reasoning

## Relationship to Other Architectures

Within the [[video-generation-architectures|taxonomy of video generation approaches]], AsymmDiT sits alongside:

- **DiT + MM-DiT** (used by SD3, FLUX.1, [[cogvideo|CogVideoX]]) -- symmetric multimodal attention
- **DiT + Cross-attention** (used by PixArt-alpha, [[ltx-video-overview|LTX-Video]]) -- separate cross-attention for text conditioning
- **Asymmetric dual-stream** (used by [[ltx-2-overview|LTX-2]]) -- separate streams for video and audio with asymmetric parameter allocation

LTX-2's asymmetric dual-stream approach shares the principle of allocating more parameters to the primary modality (14B for video vs 5B for audio), suggesting that asymmetric designs are a recurring pattern in effective multimodal generation.

## See Also

- [[mochi]]
- [[dit-architecture]]
- [[video-generation-architectures]]
