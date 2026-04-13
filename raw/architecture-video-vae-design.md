# LTX-Video: Video-VAE Architecture Deep Dive

## Source
- Extracted from: LTX-Video paper (arXiv:2501.00103), Sections 2.1, 2.1.1-2.1.6
- Additional context from: LTX-2 paper (arXiv:2601.03233), Section 3.3

## Overview

The Video-VAE is the core innovation of LTX-Video, achieving a 1:192 compression ratio that enables real-time video generation. The design philosophy is to relocate the patchifying operation from the transformer's input to the VAE's input, and to share the denoising objective between the transformer and the VAE decoder.

## Compression Comparison

| Model | Spatial | Temporal | Channels | Total Compression | Patchifier | Pixels-to-Tokens |
|-------|---------|----------|----------|-------------------|-----------|-------------------|
| CogVideoX | 8x8 | 4 | 16 | 1:48 | 2x2x1 | 1:1024 |
| MovieGen | 8x8 | 8 | 16 | 1:96 | 2x2x1 | 1:2048 |
| PyramidFlow | 8x8 | 8 | 16 | 1:96 | 2x2x1 | 1:2048 |
| Open-Sora Plan | 8x8 | 4 | 16 | 1:48 | 2x2x1 | 1:1024 |
| HunyuanVideo | 8x8 | 4 | 16 | 1:48 | 2x2x1 | 1:1024 |
| **LTX-Video** | **32x32** | **8** | **128** | **1:192** | **None** | **1:8192** |

## Encoder Architecture (Causal Encoder)

- **3D Causal Convolutions** throughout
- Applies 32x32x8 spatiotemporal compression
- First frame encoded as separate latent frame (causality for image compatibility)
- Tested both causal and non-causal designs:
  - Non-causal: easier to train for better reconstruction
  - Causal: enables simultaneous image+video training and first-frame conditioned generation
- Also tested separable convolutions (2D spatial + 1D temporal) -- found 3D convolutions slightly better

## Decoder Architecture (Denoising Decoder)

- Standard coarse-to-fine latent-to-pixel decoding architecture
- **Diffusion timestep conditioning** via adaptive normalization layers (AdaLN)
- **Multi-layer noise injection** (following StyleGAN): noise injected at several decoder layers with learned per-channel noise level
- Trained to decode at varying noise levels in range [0, 0.2]

## Shared Diffusion Objective

### Problem
In latent diffusion, the limited number of denoising iterations means residual uncertainty persists. At high compression rates (like 1:192), this manifests as out-of-distribution inputs to the decoder, causing artifacts especially in high-frequency regions.

### Solution
The VAE decoder is trained to map noisy latents directly to clean pixels:
- x_0 = D(z_t_i, t_i)
- The decoder cannot be applied iteratively (different input/output dimensionality)
- But it executes the final denoising step in pixel space -- something the latent-space transformer cannot do
- The decoder is trained with pixel-space losses, not constrained by latent expressiveness

### During Inference
- Transformer performs N-1 denoising steps in latent space
- Final step: decoder converts noisy latents to clean pixels while performing last denoising
- This is particularly impactful for high-motion videos where compression artifacts are worst

## Novel Loss Functions

### Reconstruction GAN (rGAN)
- **Problem:** Traditional GAN discriminator sees either real OR fake, making the task unnecessarily hard (e.g., is a blurry patch depth-of-field or reconstruction artifact?)
- **Solution:** Discriminator sees BOTH original and reconstructed samples concatenated, determines which is which
- **Benefits:**
  - Significantly simplified discriminator task
  - Improved GAN stability
  - Serves as both distribution-matching loss AND robust reconstruction loss
  - Better balance of fidelity and perceptual quality

### Video DWT Loss
- Spatio-temporal Discrete Wavelet Transform loss
- Computes 8 3D DWT transforms for both input and reconstructed videos
- L1 distance between transforms as loss
- Addresses insufficiency of L1/L2 pixel loss for high-frequency detail reconstruction

### Uniform Log-Variance
- **Problem:** With wide latent space (128 channels), standard KL loss causes uneven utilization -- some channels "sacrificed" to satisfy KL (means shrink to zero, variances to one)
- **Solution:** Single predicted log-variance shared across ALL channels
- Uniformly distributes KL loss effect across channels

### Multi-Layer Noise Injection
- Following StyleGAN, noise injected at several decoder layers
- Noise level learned per-channel
- Enables more diverse high-frequency detail generation
- Provides additional stochasticity beyond latent noise

## Final Loss Configuration
1. Pixel reconstruction (MSE)
2. Video-DWT (L1)
3. Perceptual (LPIPS)
4. Reconstruction-GAN

## Latent Space Analysis
- PCA analysis over 128 video samples shows:
  - As training progresses, VAE learns to utilize available channels better
  - Redundancy reduces -- components contribute more evenly to variance
  - Auto-correlation matrices: high off-diagonal values early in training, near-zero at completion
  - Naive patchification of latents (as done by other models) does NOT reduce redundancy

## Audio VAE (LTX-2 Extension)

### Design
- Separate causal audio VAE for audio latent space
- Input: stereo mel-spectrograms at 16 kHz
- Two-channel mel-spectrogram concatenated along channel dimension
- Each latent token: ~1/25 seconds of audio, 128-dimensional

### Vocoder
- Modified HiFi-GAN V1 architecture
- Joint stereo synthesis and upsampling (16 kHz input -> 24 kHz output)
- Doubled channels relative to original HiFi-GAN V1 for stereo modeling
