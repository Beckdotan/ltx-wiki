---
title: VAE Loss Functions
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/architecture-video-dit-vae.md
  - raw/architecture-video-vae-design.md
  - raw/ltx-video-vae-deep-dive.md
  - raw/ltx-video-vae-design-and-innovations.md
  - raw/ltx-video-architecture-paper-details.md
tags:
  - loss-functions
  - vae
  - gan
  - training
  - ltx-video
---

# VAE Loss Functions

The [[video-vae]] is trained with a combination of four loss functions, two of which are novel contributions. These losses are essential for maintaining reconstruction quality at the extreme 1:192 [[latent-space-compression]] ratio.

## Complete Loss Configuration

1. **Pixel Reconstruction (MSE)** -- standard
2. **Video DWT Loss (L1)** -- novel
3. **Perceptual Loss (LPIPS)** -- standard
4. **Reconstruction GAN (rGAN)** -- novel

## Reconstruction GAN (rGAN)

The most significant novel loss function. Traditional GAN discriminators see either real OR fake samples in isolation, making the task unnecessarily hard -- for example, a blurry patch could be depth-of-field or a reconstruction artifact.

### How rGAN Works

- The discriminator receives BOTH the original and reconstructed samples, concatenated together
- Its task: determine which is the original and which is the reconstruction
- This is a relative comparison rather than an absolute real/fake classification

### Benefits

- **Simplified discriminator task**: Relative comparison is much easier than absolute classification
- **Improved GAN stability**: The paired input provides clearer training signal
- **Dual-purpose loss**: Serves as both a distribution-matching loss AND a robust reconstruction loss
- **Better balance**: Achieves improved fidelity and perceptual quality simultaneously
- **Patch-GAN friendly**: Especially helps Patch-GAN discriminators that have limited spatial context
- **Critical at high compression**: Traditional GAN loss was insufficient at the 1:192 compression level

## Video DWT Loss

A spatio-temporal frequency-domain loss that addresses the known weakness of pixel-space L1/L2 losses for high-frequency detail reconstruction.

### How It Works

- Applies 3D Discrete Wavelet Transform to both input and reconstructed videos
- Produces 8 wavelet coefficients capturing multi-scale frequency information
- Computes L1 distance between the transform coefficients of original and reconstructed videos
- Ensures that high-frequency spatial and temporal details are preserved during training

### Why It Matters

Standard pixel reconstruction losses (MSE, L1) tend to produce blurry results because they penalize all errors equally. The DWT loss explicitly measures preservation of frequency content at multiple scales, providing a stronger training signal for detail preservation.

## Uniform Log-Variance

A regularization technique for the VAE's KL divergence loss, addressing a problem specific to wide latent spaces.

### The Problem

With 128 latent channels (vs. the typical 16), the standard KL loss causes uneven channel utilization. Some channels get "sacrificed" -- their means shrink to zero and variances approach one -- while the model concentrates information in the remaining channels. This wastes latent capacity.

### The Solution

A single predicted log-variance is shared across ALL 128 channels, rather than predicting per-channel variance. This uniformly distributes the KL loss effect across all channels, ensuring the model utilizes the full capacity of the latent space.

## Perceptual Loss (LPIPS)

Standard Learned Perceptual Image Patch Similarity loss. Measures perceptual similarity using features from a pre-trained neural network, encouraging reconstructions that look perceptually correct to human observers even if individual pixels differ.

## Pixel Reconstruction (MSE)

Standard mean squared error between input and reconstructed frames. Provides the foundational pixel-level training signal that the other losses refine.
