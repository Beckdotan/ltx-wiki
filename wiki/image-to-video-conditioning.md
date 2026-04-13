---
title: Image-to-Video Conditioning
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
  - image-to-video
  - conditioning
  - inference
  - ltx-video
---

# Image-to-Video Conditioning

LTX-Video supports image-to-video (I2V) generation through a per-token timestep mechanism that requires no additional parameters or special tokens. This approach extends Open-Sora's method.

## Mechanism

The core idea is that different tokens in the sequence can have different diffusion timesteps:

- **Conditioning tokens** (from the input image): Assigned a small timestep value (e.g., t_c = 0), meaning they are treated as nearly clean
- **Generation tokens** (to be synthesized): Assigned timestep t = 1, meaning they start as pure noise

## How It Works

### Training

During training, the model occasionally sets the first-frame token timestep to a small random value with corresponding noise. The model quickly learns to use these near-clean tokens as conditioning signals for generating the remaining frames.

### Inference Pipeline

1. The conditioning image is encoded by the causal [[video-vae]] encoder with a temporal dimension of 1 (leveraging the causal first-frame encoding)
2. The image latent is concatenated with random noise latents for the remaining frames
3. The combined tokens are flattened to form the initial token set
4. Per-token timesteps are assigned: t_c (small value) for conditioning tokens, t=1 for generation tokens
5. Standard denoising loop runs with per-token noise levels

## Why This Design

- **No additional parameters**: The same [[video-dit-transformer]] architecture handles both text-to-video and image-to-video without modification
- **No special tokens**: No need for [IMG] tokens or separate conditioning pathways
- **Causal VAE compatibility**: The causal design of the [[video-vae]] naturally encodes the first frame as a separate latent frame, making it straightforward to use as a conditioning signal
- **Clean temporal boundary**: The separate first-frame encoding provides independent quality control

## Multi-Condition Support (v0.9.7+)

Later model versions extended this mechanism to support conditioning on multiple images or videos at specified frame positions, using adjustable conditioning strength per condition (default: 1.0).

## Performance

In human evaluation on 1,000 image-prompt pairs with 20 evaluators, LTX-Video achieved a 91% win rate for I2V, compared to 47% for CogVideoX 2B, 35% for PyramidFlow, and 20% for Open-Sora Plan.
