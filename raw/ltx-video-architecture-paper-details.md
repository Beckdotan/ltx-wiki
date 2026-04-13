# LTX-Video: Architecture and Technical Details (from Research Paper)

**Source:** https://arxiv.org/abs/2501.00103
**Title:** "LTX-Video: Realtime Video Latent Diffusion"
**Authors:** Yoav HaCohen, Nisan Chiprut, Benny Brazowski, Daniel Shalem, Dudu Moshe, Eitan Richardson, Eran Levin, Guy Shiran, Nir Zabari, Ori Gordon, Poriya Panet, Sapir Weissbuch, Victor Kulikov, Yaki Bitterman, Zeev Melumian, Ofir Bibi (Lightricks)

---

## Overview

LTX-Video is a transformer-based latent diffusion model that adopts a holistic approach to video generation by seamlessly integrating the responsibilities of the Video-VAE and the denoising transformer. Unlike existing methods, which treat these components as independent, LTX-Video optimizes their interaction for improved efficiency and quality.

The model is the first DiT-based (Diffusion Transformer) video generation model capable of generating high-quality videos in real-time. It produces 5 seconds of 24 fps video at 768x512 resolution in just 2 seconds on an Nvidia H100 GPU.

## Core Architecture

### Video-VAE

The Video-VAE is the critical component enabling LTX-Video's speed. Key design choices:

- **Compression ratio:** 1:192 total compression
- **Spatiotemporal downscaling:** 32x32x8 pixels per token (32x spatial, 8x temporal)
- **Latent channels:** 128 channels (compared to typical 16 channels in other models)
- **Pixels-to-tokens ratio:** 1:8192 (4x the typical ratio of 1:2048)
- **No patchifier needed:** The patchifying operation is relocated from the transformer's input to the VAE's input

For comparison, other models (CogVideoX, MovieGen, PyramidFlow, Open-Sora Plan, HunyuanVideo) use VAEs with 8x8x4 or 8x8x8 spatiotemporal downscaling with 16 channels, resulting in 1:48 or 1:96 compression. They then use a 2x2x1 patchifier to achieve 1:1024 or 1:2048 pixels-to-tokens ratios.

#### VAE Encoder
- **Type:** Causal Encoder using 3D Causal Convolutions
- **Compression:** 32x32x8 (except the first frame, which is encoded as a separate latent frame)
- **Convolution type:** Full 3D convolutions (tested separable 2D spatial + 1D temporal but found 3D slightly better)
- **Causal design:** Enables simultaneous training on images and videos, and first-frame conditioned video generation

#### VAE Decoder (Denoising Decoder)
- **Novel design:** The decoder is trained as a diffusion model that maps noisy latents to clean pixels
- **Diffusion-timestep conditioning:** Uses adaptive normalization layers
- **Multi-layer noise injection:** Following StyleGAN, noise is injected at several decoder layers for diverse high-frequency details
- **Noise range:** Trained with noise levels in [0, 0.2]
- **Shared diffusion objective:** The decoder performs the last denoising step in conjunction with converting latents to pixels, producing clean output directly in pixel space

#### Uniform Log-Variance
With 128 latent channels, standard KL loss caused some channels to be "sacrificed" (means shrink to zero, variances approach one). Solution: a single predicted log-variance shared across all channels, uniformly distributing the KL loss effect.

### Novel Loss Functions

#### Reconstruction GAN (rGAN)
A novel adaptation of traditional GAN training for reconstruction tasks:
- The discriminator sees BOTH the input and reconstructed samples (concatenated) for each iteration
- Its goal: determine which is original and which is reconstructed
- This relative comparison simplifies the discriminator's task and improves guidance
- Greatly enhances GAN stability and performance

#### Video DWT Loss
Spatio-temporal Discrete Wavelet Transform (DWT) loss:
- Computes 8 3D DWT transforms for both input and reconstructed videos
- Uses L1 distance as the loss
- Addresses insufficiency of L1/L2 pixel loss for high-frequency detail reconstruction

#### Final VAE Loss Combination
- Pixel reconstruction (MSE)
- Video-DWT (L1)
- Perceptual (LPIPS)
- Reconstruction-GAN

### Video Transformer (DiT)

Built upon Pixart-alpha architecture, which extends the DiT framework:

#### Key Modifications
1. **RoPE (Rotary Positional Embeddings):** Replaces traditional absolute positional embeddings
   - Uses normalized fractional coordinates (spatial in pixels, temporal in seconds, relative to max resolution/duration)
   - Exponential frequency spacing (not inverse-exponential, which performed worse)
   - Incorporating original FPS into temporal embedding produces more natural motion

2. **QK Normalization:** RMSNorm applied to queries and keys before dot product attention
   - Prevents extremely large attention logit values
   - Avoids near-zero entropy attention weights
   - RMSNorm outperformed LayerNorm

3. **RMSNorm:** Replaces LayerNorm throughout

4. **Full spatiotemporal self-attention:** Enabled by the highly compressed latent space

### Text Conditioning

- **Text encoder:** T5-XXL (pre-trained)
- **Conditioning method:** Cross-attention (tested MM-DiT but found cross-attention better)
- **Learnable projection layer** on input text embeddings

### Image Conditioning (I2V)

- **Method:** Timestep-based conditioning (no additional parameters or special tokens needed)
- **Training:** Occasionally set timestep of first-frame tokens to a small random value with corresponding noise
- **Inference:** Conditioning image encoded by causal VAE with temporal dimension of 1, concatenated with random noise latents
- **Per-token timesteps:** Conditioning tokens get small timestep (e.g., t_c=0), other tokens get t=1

### Rectified-Flow Training

- **Velocity prediction:** Predicts v = epsilon - z_0 (following SD3)
- **Timestep scheduling:** Log-normal distribution shifted toward higher-noise regions depending on token count (following SimpleDiffusion finding about SNR at higher resolutions)
- **Multi-resolution training:** Simultaneously trained on multiple resolution/duration combinations
- **Stochastic token dropping:** 0-20% rate to fix token counts without complex packing/padding
- **Image training:** Included alongside video training as one of the resolution-duration combinations

## Model Size

- **Original (paper) model:** < 2B parameters (before distillation)
- **Optimizer:** ADAM-W
- **Fine-tuning:** Post-pre-training fine-tuning on high-aesthetic video subset

## Performance

- **Speed:** 5 seconds of 24 fps video at 768x512 in 2 seconds on Nvidia H100
- **Faster-than-real-time:** Generates videos faster than playback speed
- **Evaluation:** Human survey with 20 participants, 1000 T2V prompts, 1000 I2V pairs
- **Comparison:** Outperformed Open-Sora Plan, CogVideoX (2B), and PyramidFlow in both T2V and I2V tasks

## Limitations (from Paper)

- Sensitivity to prompt formulation
- Limited to short videos (up to ~10 seconds)
- Domain-specific generalization not extensively tested
- Not designed for factual information generation
- May amplify societal biases
