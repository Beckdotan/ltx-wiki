---
title: LTX Video Architecture
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-version-changelog-improvements.md
  - raw/ltx-video-capabilities-resolution-fps.md
  - raw/dev-model-versions-and-weights.md
  - raw/model-ltx-video-versions-huggingface.md
tags:
  - ltx-video
  - architecture
  - dit
  - vae
  - technical
---
# LTX Video Architecture

This page documents the core technical architecture of [[ltx-video-overview|LTX-Video]], which remained fundamentally consistent across all versions (v0.9.0 through v0.9.8).

## Architecture Overview

LTX-Video is built on a [[dit-architecture]] (Diffusion Transformer) architecture, derived from Pixart-alpha. It operates in a compressed latent space and uses a novel Video-VAE for encoding and decoding.

## Core Components

### Diffusion Transformer (DiT)

The main generative backbone of LTX-Video. It operates on latent representations of video frames.

- **2B variant:** 1.9B transformer parameters (v0.9.0 through v0.9.6)
- **13B variant:** Introduced in v0.9.7, representing a 6.5x scale-up
- Uses spatial and temporal attention mechanisms
- Supports variable resolution and frame counts

### Video-VAE

The [[ltx-video-vae]] is a key architectural innovation of LTX-Video.

- **Compression ratio:** 1:192 (32x32x8 spatiotemporal downscaling)
- **Spatial compression:** 32x (height and width each compressed by 32)
- **Temporal compression:** 8x (every 8 frames compressed to 1 latent frame)
- **Latent channels:** 128
- **Denoising decoder:** The VAE performs the final diffusion step in pixel space, combining decoding with denoising
- **Training losses:** Reconstruction GAN loss + Video DWT loss (for high-frequency detail preservation)
- **Causal structure:** Used for image-to-video conditioning (conditioning image encoded as first frame)

Key decoder parameters:
- `decode_timestep` controls denoising decoder behavior (e.g., 0.03)
- `decode_noise_scale` controls noise injection (e.g., 0.025)

### Text Encoder

- **Model:** T5-XXL
- **Conditioning:** Cross-attention between text embeddings and DiT layers
- **Language:** English only
- **Note:** Replaced by Gemma 3 12B in [[ltx-2]]

### Positional Encoding

- **Type:** RoPE (Rotary Position Embeddings) with normalized fractional coordinates
- **Temporal:** RoPE temporal embedding incorporates FPS for natural motion
- **Benefit:** Supports variable resolution and frame counts without retraining

### Normalization

- QK normalization with RMSNorm
- Per-token timestep conditioning for image-to-video mode

## Training Framework

- **Paradigm:** Rectified-flow training with velocity prediction
- **Multi-resolution training:** Model trained on multiple resolutions simultaneously
- **Distillation:** Knowledge distillation used from v0.9.6 onward to create fast inference variants

## Resolution and Frame Constraints

These constraints stem directly from the architecture:

- **Resolution divisibility:** 32 (due to VAE spatial compression ratio)
- **Frame divisibility:** 8n+1 pattern (due to VAE temporal compression ratio of 8, plus 1 for causal structure)
- **Maximum frames:** 257
- **FPS range:** 24-30

## Multiscale Rendering (v0.9.7+)

A breakthrough rendering approach introduced with the 13B model:

1. Draft at lower detail first for coarse motion planning
2. Progressively add structure, lighting, and micro-motion
3. For 1080p: render at 960x540, then upscale 2x via [[ltx-video-spatial-upscaler]]
4. Result: 30x faster than comparable-sized models

## Quantization

### FP8 (v0.9.7+)

Official FP8 quantized models provide approximately 50% memory reduction versus BF16 with minimal quality degradation. Recommended for GPUs with 12-16 GB VRAM.

### Community Quantizations

16+ community quantizations available including GGUF, AWQ, and other formats.

## Inference Pipelines

### Original API (v0.9.0 to v0.9.5)

- `LTXPipeline` -- Text-to-video
- `LTXImageToVideoPipeline` -- Image-to-video

### Condition API (v0.9.7+)

- `LTXConditionPipeline` -- Unified pipeline for T2V, I2V, V2V, and multi-condition generation
- `LTXLatentUpsamplePipeline` -- Spatial and temporal upscaling
- `LTXVideoCondition` -- Condition specification object

## System Requirements

- **Python:** >= 3.10.5
- **PyTorch:** >= 2.1.2
- **CUDA:** >= 12.2
- **Precision:** bfloat16 (default), fp8 (quantized variants)

## Architecture Evolution into LTX-2

When LTX-Video evolved into [[ltx-2]], the following architectural changes were made:

- Added 5B audio stream alongside the video stream
- Replaced T5-XXL with Gemma 3 12B text encoder
- Added bidirectional cross-attention between audio and video
- Added thinking tokens for better prompt adherence
- Added modality-aware CFG

See [[ltx-video-to-ltx-2]] for the full evolution story.

## References

- [[ltx-video-overview]] -- Model family overview
- [[ltx-video-code-structure]] -- Repository layout and code organization
- [[ltx-video-vae]] -- Video-VAE details
- [[ltx-video-capabilities]] -- Resolution, FPS, and generation modes
- [[ltx-video-model-variants]] -- All model variants including FP8
- [[ltx-video-to-ltx-2]] -- Architecture evolution to LTX-2
