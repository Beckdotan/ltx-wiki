---
title: LTX-Video Architecture Overview
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/architecture-video-dit-transformer.md
  - raw/architecture-video-dit-vae.md
  - raw/ltx-video-architecture-paper-details.md
  - raw/ltx-video-architecture-technical.md
tags:
  - ltx-video
  - architecture
  - diffusion-transformer
  - video-generation
---

# LTX-Video Architecture Overview

LTX-Video is a transformer-based latent diffusion model for video generation built on the Diffusion Transformer (DiT) framework. Its defining principle is the holistic integration of the [[video-vae]] and the [[video-dit-transformer]] as unified components rather than independent modules, optimizing their interaction within a shared compressed latent space.

## Core Components

The architecture consists of two tightly coupled subsystems:

1. **[[video-vae]]** -- A causal Video Variational Autoencoder that compresses video into a highly compact latent space at a 1:192 ratio. The [[latent-space-compression]] strategy relocates patchification into the VAE itself, eliminating the need for a separate patchifier at the transformer input.

2. **[[video-dit-transformer]]** -- A ~2B parameter transformer (scaled to 13B in later versions) built on PixArt-alpha/DiT, using full spatiotemporal self-attention over the compressed tokens. Enhanced with [[rope-positional-encoding]], QK normalization, and [[text-conditioning]] via cross-attention.

## Key Innovations

- **[[denoising-decoder]]**: The VAE decoder performs dual duty -- converting latents to pixels and executing the final diffusion denoising step directly in pixel space.
- **[[latent-space-compression]]**: A 1:8192 pixels-to-tokens ratio (4x more compressed than competing models), enabled by pushing all compression into the VAE.
- **[[vae-loss-functions]]**: Novel training losses including the Reconstruction GAN (rGAN) and Video DWT loss that maintain quality at extreme compression ratios.
- **[[image-to-video-conditioning]]**: Per-token timestep mechanism requiring no additional parameters.
- **[[rectified-flow-training]]**: Velocity prediction with log-normal timestep scheduling and multi-resolution training.

## Model Specifications

| Parameter | LTX-Video (2B) | LTXV-13B | LTX-2 (19B) | LTX-2.3 (22B) |
|-----------|----------------|----------|--------------|----------------|
| Transformer blocks | 28 | 28 | 48 | 48 |
| Hidden dimension | 2048 | 2048 | -- | -- |
| Text encoder | T5-XXL | T5-XXL | Gemma 3 12B | Gemma 3 12B |
| VAE compression | 1:192 | 1:192 | 1:192 | Rebuilt (+2.5 dB) |
| Audio support | No | No | Yes (5B stream) | Yes (improved) |

## Performance

The original 2B model generates 5 seconds of 24 fps video at 768x512 resolution in 2 seconds on an Nvidia H100 GPU -- faster than real-time playback. The 13B model introduced multiscale rendering for progressive detail generation, achieving up to 30x faster inference than comparable-scale models.

## Competitive Comparison

| Metric | LTX-Video | CogVideoX | PyramidFlow | HunyuanVideo | MovieGen |
|--------|-----------|-----------|-------------|--------------|----------|
| Base size | 1.9B | 2B/5B | 2B | 13B | 30B |
| VAE compression | 32x32x8, 128ch | 8x8x4, 16ch | 8x8x8, 16ch | 8x8x4, 16ch | 8x8x8, 16ch |
| Patchifier | None (1x1x1) | 2x2x1 | 2x2x1 | 2x2x1 | 2x2x1 |
| Pixels-to-tokens | 1:8192 | 1:1024 | 1:2048 | 1:1024 | 1:2048 |

## Evolution

- **LTX-Video (Nov 2024)**: Original 2B model with 1:192 VAE and T5-XXL text encoder.
- **LTXV-13B (May 2025)**: Scaled transformer with multiscale rendering; supports up to 60-second videos.
- **LTX-2 (Jan 2026)**: Asymmetric dual-stream architecture (14B video + 5B audio) with Gemma 3 12B text encoder and bidirectional audio-visual cross-attention.
- **LTX-2.3 (Mar 2026)**: Rebuilt VAE with +2.5 dB PSNR, 4x larger gated attention text connector, improved vocoder.
