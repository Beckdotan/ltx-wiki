# LTX Video Architecture: Video-DiT, VAE Design, Tokenization, and Temporal Compression

## Overview

LTX-Video's architecture is distinguished by its holistic integration of the Video-VAE and denoising transformer. Rather than treating video compression and denoising as independent stages, LTX-Video optimizes their interaction within a shared compressed latent space. This document consolidates architecture details across the LTX model family.

## Video-VAE Architecture

### Core Design Philosophy

The key innovation is relocating the patchifying operation from the transformer's input to the VAE's encoder input. In most competing approaches (CogVideoX, PyramidFlow, MovieGen), a 2x2x1 patchifier sits between the VAE output and the transformer input. LTX-Video instead uses a 1x1x1 input patchifier (effectively none), pushing all spatial/temporal compression into the VAE itself. This enables higher total compression without separate tokenization overhead.

### Compression Specifications

| Metric | Value |
|--------|-------|
| Spatial Compression | 32x32 pixels per token |
| Temporal Compression | 8 frames per token |
| Total Compression Ratio | 1:192 |
| Latent Channels | 128 |
| Pixels-to-Tokens Ratio | 1:8192 |

For comparison:
- CogVideoX: 1:48 compression, 16 latent channels
- PyramidFlow: 1:96 compression, 16 latent channels
- MovieGen: 1:96 compression, 16 latent channels

### Encoder Architecture

- **Type:** Causal Encoder with 3D Causal Convolutions
- **Spatial downscaling:** 32x
- **Temporal downscaling:** 8x (except first frame, which is encoded as a separate latent frame)
- **Output channels:** 128 latent dimensions

The causal design ensures that encoding of each frame depends only on the current and previous frames, not future frames, which is critical for maintaining temporal consistency and supporting streaming/progressive generation.

### Decoder Architecture (Denoising Decoder)

The decoder is one of LTX-Video's most novel components. It performs dual functions:

1. **Latent-to-pixel conversion:** Standard VAE decoding
2. **Final denoising step:** Executes the last step of the diffusion process directly in pixel space

**Key components:**
- Diffusion-timestep conditioning: The decoder is conditioned on the diffusion timestep, trained on noise levels [0, 0.2]
- Multi-layer noise injection: Noise injected at several layers with learned per-channel noise levels
- This enables generation of diverse high-frequency details that would otherwise be lost at the 1:192 compression ratio

**Benefit:** Preserves the ability to generate fine details without the runtime cost of a separate upsampling module. Particularly effective for high-motion videos where compression artifacts are most visible.

### Uniform Log-Variance

A single predicted log-variance is shared across all channels (rather than per-channel variance). This ensures the model utilizes the full capacity of the 128-channel latent space, avoiding degenerate solutions where some channels collapse.

### VAE Loss Functions

1. **Pixel Reconstruction (MSE):** Standard mean squared error between input and reconstructed frames
2. **Video DWT Loss (L1):** 3D Discrete Wavelet Transform applied to both input and reconstructed videos, producing 8 wavelet coefficients. L1 distance between these coefficients ensures high-frequency spatial and temporal details are preserved.
3. **Perceptual Loss (LPIPS):** Learned perceptual image patch similarity for perceptual quality
4. **Reconstruction-GAN (rGAN):** Novel adversarial loss where the discriminator sees both original and reconstructed samples concatenated and decides which is which. This relative comparison simplifies GAN training and reduces artifacts compared to traditional adversarial approaches.

### Latent Space Properties

PCA analysis of the latent space shows that inter-channel correlation approaches near-zero by training completion, indicating the VAE learns to efficiently utilize all 128 channels without redundancy.

## Video Transformer (Video-DiT)

### Architecture Specifications

| Parameter | Value |
|-----------|-------|
| Hidden Dimension | 2048 |
| Attention Blocks | 28 |
| FFN Dimension Factor | 4x |
| Total Parameters | 1.9B (original), 13B (LTXV-13B) |
| Attention Type | Self-attention + Cross-attention |

### Positional Encoding: RoPE with Fractional Coordinates

LTX-Video replaces traditional absolute positional embeddings with Rotary Positional Embeddings (RoPE) enhanced with normalized fractional coordinates:

- **Frequency spacing:** Exponentially increasing frequencies (outperforms inverse-exponential in ablations)
- **Coordinate system:** Computed in pixels and seconds relative to predefined max resolution and duration
- **FPS-aware:** Incorporates original FPS into temporal embedding for natural motion generation
- **Multi-resolution support:** Fractional coordinates enable training and inference at arbitrary resolutions

### QK Normalization

- RMSNorm applied to queries and keys before dot product attention
- Prevents extremely large values in attention logits
- RMSNorm found superior to LayerNorm through experimentation

### Text Conditioning

- **Encoder:** T5-XXL (LTX-Video) / Gemma 3 12B (LTX-2, LTX-2.3)
- **Method:** Cross-attention with learnable projection layer
- **Evolution:** LTX-2.3 introduced a 4x larger gated attention text connector for better prompt adherence

## Tokenization Pipeline

### LTX-Video (Original)

```
Input Video (H x W x T pixels)
    |
    v
Video-VAE Encoder (3D Causal Conv)
    |
    v
Latent Tokens (H/32 x W/32 x T/8 x 128 channels)
    |
    v
1x1x1 Patchifier (identity — no additional patchification)
    |
    v
Transformer Input
```

### Competing Approaches

```
Input Video (H x W x T pixels)
    |
    v
Video-VAE Encoder
    |
    v
Latent Tokens (lower compression, e.g., 1:48)
    |
    v
2x2x1 Patchifier (additional spatial compression at transformer input)
    |
    v
Transformer Input
```

## Temporal Compression

Video VAEs add temporal compression on top of spatial compression, ensuring neighboring frames have nearby latent vectors. This is what makes motion smooth and coherent rather than frame-by-frame noise.

### First Frame Handling

The first frame is encoded as a separate latent frame (not compressed with the 8x temporal factor). This design choice supports:
- Image-to-video generation (conditioning on a single frame)
- Cleaner temporal boundary at video start
- Independent quality control for the initial frame

### Image-to-Video Conditioning

LTX-Video extends Open-Sora's approach with per-token timestep conditioning:
- Different timestep and corresponding noising level per token
- During training: first frame tokens occasionally receive small random timestep values
- During inference: conditioning tokens get low t_x values; remaining tokens get t=1

## Evolution Across Model Versions

### LTX-Video (2B, Nov 2024)
- 28 transformer blocks, 2048 hidden dim
- T5-XXL text encoder
- 1:192 VAE compression
- Full spatiotemporal self-attention

### LTXV-13B (May 2025)
- Scaled to 13B parameters
- Introduced multiscale rendering: progressive detail generation starting from coarse motion on a low-resolution grid, then progressively filling tiles with finer detail
- 30x faster than comparable-scale models
- Supports up to 60-second videos (from 0.9.8)

### LTX-2 (19B, Jan 2026)
- Asymmetric dual-stream: 14B video + 5B audio
- 48 transformer layers
- Gemma 3 12B text encoder
- 3D RoPE (video) + 1D RoPE (audio)
- Bidirectional audio-visual cross-attention

### LTX-2.3 (22B, Mar 2026)
- Rebuilt VAE with +2.5 dB PSNR
- 4x larger gated attention text connector
- Improved HiFi-GAN vocoder
- Refined latent space

## Sources

- https://arxiv.org/abs/2501.00103
- https://arxiv.org/abs/2601.03233
- https://ltx.io/model/model-blog/what-is-vae-video-generation
- https://github.com/Lightricks/LTX-Video
- https://github.com/Lightricks/LTX-2
