---
title: Video-VAE
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
  - vae
  - video-compression
  - autoencoder
  - ltx-video
---

# Video-VAE

The Video-VAE is the core component enabling LTX-Video's real-time video generation. It achieves an industry-leading 1:192 compression ratio by relocating the patchifying operation from the transformer input into the VAE itself, and by sharing the denoising objective between the transformer and the VAE decoder.

See [[ltx-video-architecture-overview]] for how this fits into the full system.

## Design Philosophy

Unlike competing approaches (CogVideoX, PyramidFlow, MovieGen, HunyuanVideo) where a 2x2x1 patchifier sits between the VAE output and transformer input, LTX-Video uses a 1x1x1 input patchifier (effectively none). All spatial and temporal compression is handled within the VAE itself. This is the [[latent-space-compression]] strategy that enables the extreme compression ratio.

## Encoder (Causal Encoder)

- Uses **3D causal convolutions** throughout
- Applies 32x32x8 spatiotemporal compression (32x spatial, 8x temporal)
- First frame encoded as a separate latent frame (causal boundary)
- Outputs 128-channel latent representation

### Design Decisions

- **Causal vs. non-causal**: Non-causal is easier to train for better reconstruction, but causal enables simultaneous image+video training and first-frame conditioned generation. Causal was chosen.
- **3D vs. separable convolutions**: Tested separable convolutions (2D spatial + 1D temporal) but found full 3D convolutions slightly better in quality.
- **Causal property**: Encoding a single image produces valid latent representations, enabling the same model to handle both image and video inputs natively.

## Decoder (Denoising Decoder)

The decoder is LTX-Video's most novel component, performing dual duty. See [[denoising-decoder]] for the full deep dive.

1. **Latent-to-pixel conversion** -- standard VAE decoder function
2. **Final denoising step** -- executes the last diffusion step directly in pixel space

Key features:
- Diffusion-timestep conditioning via adaptive normalization layers (AdaLN)
- Multi-layer noise injection (StyleGAN-inspired) with learned per-channel noise levels
- Trained with noise levels in range [0, 0.2]

## Why 128 Latent Channels?

Traditional video VAEs use 16 latent channels. LTX-Video uses 128 (an 8x increase) because:

- The extreme 32x32 spatial compression means each latent pixel must encode far more information
- 16 channels would be insufficient to represent the visual complexity of a 32x32 pixel patch
- The 128 channels keep the latent space information-rich enough to reconstruct high-quality video
- More channels means larger latents, but the extreme spatial compression more than compensates

### Uniform Log-Variance

With 128 channels, standard KL loss causes uneven utilization -- some channels get "sacrificed" (means shrink to zero, variances approach one). The solution is a single predicted log-variance shared across all channels, uniformly distributing the KL loss effect. See [[vae-loss-functions]] for details.

## Latent Space Quality

PCA analysis over 128 video samples demonstrates:
- As training progresses, the VAE learns to utilize all available channels
- Inter-channel correlation approaches near-zero by training completion
- Early training shows high off-diagonal auto-correlation values; end of training shows near-zero
- Naive patchification of latents (as done by other models) does NOT reduce this redundancy

## Loss Functions

The VAE is trained with four loss components: pixel reconstruction (MSE), Video DWT (L1), perceptual (LPIPS), and Reconstruction GAN. See [[vae-loss-functions]] for full details.

## Practical Constraints

- **Resolution**: Must be divisible by 32 (due to 32x spatial compression)
- **Frame count**: Must be divisible by 8, plus 1 for the causal first frame (valid counts: 9, 17, 25, 33, 65, 97, 121, 161, 257)
- **Maximum frames**: 257 recommended
- **Tiling**: VAE supports spatial tiling for memory efficiency (`pipe.vae.enable_tiling()`)

## Audio VAE (LTX-2)

LTX-2 introduced a separate causal audio VAE:
- Input: stereo mel-spectrograms at 16 kHz
- Each latent token represents ~1/25 seconds of audio, 128-dimensional
- Vocoder: Modified HiFi-GAN V1 with joint stereo synthesis and upsampling (16 kHz to 24 kHz)
