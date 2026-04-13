# LTX-Video: Realtime Video Latent Diffusion

## Metadata

- **Title:** LTX-Video: Realtime Video Latent Diffusion
- **Authors:** Yoav HaCohen, Nisan Chiprut, Benny Brazowski, Daniel Shalem, Dudu Moshe, Eitan Richardson, Eran Levin, Guy Shiran, Nir Zabari, Ori Gordon, Poriya Panet, Sapir Weissbuch, Victor Kulikov, Yaki Bitterman, Zeev Melumian, Ofir Bibi
- **Affiliation:** Lightricks
- **Date:** December 30, 2024
- **arXiv ID:** 2501.00103
- **URL:** https://arxiv.org/abs/2501.00103
- **Field:** Computer Vision and Pattern Recognition (cs.CV)
- **Code:** https://github.com/Lightricks/LTX-Video

## Abstract

LTX-Video is a transformer-based latent diffusion model that adopts a holistic approach to video generation by seamlessly integrating the responsibilities of the Video-VAE and the denoising transformer. Unlike existing methods, which treat these components as independent, LTX-Video optimizes their interaction for improved efficiency and quality. At its core is a carefully designed Video-VAE that achieves a high compression ratio of 1:192, with spatiotemporal downscaling of 32x32x8 pixels per token, enabled by relocating the patchifying operation from the transformer's input to the VAE's input. Operating in this highly compressed latent space enables the transformer to efficiently perform full spatiotemporal self-attention, which is essential for generating high-resolution videos with temporal consistency. The model supports diverse use cases, including text-to-video and image-to-video generation, with both capabilities trained simultaneously. It achieves faster-than-real-time generation, producing 5 seconds of 24 fps video at 768x512 resolution in just 2 seconds on an Nvidia H100 GPU, outperforming all existing models of similar scale. Source code and pre-trained models are publicly available.

## Key Contributions

1. **Holistic VAE-Transformer Integration:** First model to deeply integrate Video-VAE and denoising transformer responsibilities rather than treating them as independent components.

2. **Ultra-High Compression Video-VAE:** Achieves 1:192 compression ratio (32x32x8 spatiotemporal downscaling) by relocating the patchifying operation from the transformer input to the VAE encoder input, with 128 latent channels (vs. 16 in competing models).

3. **Denoising VAE Decoder:** The decoder performs dual functions — latent-to-pixel conversion and the final denoising step — producing the clean result directly in pixel space without a separate upsampling module.

4. **Reconstruction GAN (rGAN):** Novel adversarial training approach where the discriminator sees both original and reconstructed versions concatenated and decides which is which, improving stability over traditional GAN training.

5. **Faster-Than-Real-Time Generation:** Produces 5 seconds of 24 fps video at 768x512 in ~2 seconds on H100 GPU.

## Architecture Details

### Video-VAE

| Component | Specification |
|-----------|--------------|
| Compression Ratio | 1:192 |
| Spatial Downscaling | 32x32 |
| Temporal Downscaling | 8 |
| Latent Channels | 128 |
| Pixels-to-Tokens Ratio | 1:8192 |
| Encoder Type | Causal Encoder with 3D Causal Convolutions |
| First Frame Handling | Encoded as a separate latent frame |

**Decoder Design:**
- Diffusion-timestep conditioning with multi-layer noise injection
- Trained on noise levels [0, 0.2] from the diffusion schedule
- Multi-layer noise injection: noise injected at several decoder layers, with learned per-channel noise levels, enabling diverse high-frequency detail generation
- Uniform log-variance: single predicted log-variance shared across all channels to utilize the full latent space capacity

**VAE Loss Functions:**
- Pixel reconstruction loss (MSE)
- Video DWT loss (L1): 3D Discrete Wavelet Transform applied to both input and reconstructed videos, producing 8 wavelet coefficients for high-frequency detail preservation
- Perceptual loss (LPIPS)
- Reconstruction-GAN loss (rGAN)

### Video Transformer (DiT)

| Component | Specification |
|-----------|--------------|
| Hidden Dimension | 2048 |
| Number of Blocks | 28 |
| FFN Dimension Factor | 4 |
| Total Parameters | 1.9B |
| Attention Type | Self + Cross-attention |
| Text Encoder | T5-XXL |
| Positional Encoding | RoPE with normalized fractional coordinates |

**RoPE Positional Embeddings:**
- Uses exponentially increasing frequencies (superior to inverse-exponential spacing)
- Coordinates computed in pixels and seconds relative to predefined max resolution and duration
- Incorporates original FPS into temporal embedding for natural motion generation
- Normalized fractional coordinates improve spatial and temporal coherence

**QK Normalization:**
- RMSNorm applied to queries and keys before dot product attention
- Determined superior to LayerNorm through experimentation

**Text Conditioning:**
- Cross-attention architecture with learnable projection layer on T5-XXL input embeddings

## Training Methodology

### Data Pipeline
- Publicly available data and licensed material
- Aesthetic filtering using Siamese Network trained on manually-tagged image pairs
- Motion filtering to remove videos with insignificant motion
- Aspect ratio standardization by cropping black bars
- Automatic captioning across entire training set
- CDN-driven approach for aesthetic evaluation

### Training Procedure
- **Rectified Flow:** Model predicts velocity v = epsilon - z_0 instead of noise, providing more balanced training difficulty across timesteps
- **Timestep Scheduling:** Scheduler biased towards higher-noise regions depending on token count; sampling from log-normal distribution with percentiles 0.5 and 99.9 clamping
- **Multi-Resolution Training:** Simultaneous training on multiple resolution-duration combinations, with token count normalization using stochastic token dropping (0%-20%)
- **Image Training:** Images included as one resolution-duration variant
- **Optimizer:** AdamW with subsequent fine-tuning on high-aesthetic data subset

### Image-to-Video Conditioning
- Extends Open-Sora's approach using diffusion timestep as conditioning indicator
- Different timestep and noising level per token
- Training: Occasionally set first frame tokens to small random timestep values
- Inference: Conditioning tokens receive low t_x values; remaining tokens receive t=1

## Benchmark Results

### Human Evaluation (1,000 prompts, 20 participants)

| Model | Text-to-Video Win Rate | Image-to-Video Win Rate |
|-------|----------------------|------------------------|
| **LTX-Video** | **85%** | **91%** |
| PyramidFlow | 51% | 35% |
| CogVideoX 2B | 38% | 47% |
| Open-Sora Plan | 20% | 20% |

### Speed Performance
- 121 frames at 768x512 pixels, 20 diffusion steps on H100 GPU: ~2 seconds
- Faster-than-real-time generation for 5 seconds of 24 fps video

### VBench Score
- VBench total score: ~71.93 (baseline; later model versions improved significantly)

### Comparative Architecture Analysis

| Aspect | LTX-Video | CogVideoX | PyramidFlow | MovieGen |
|--------|-----------|-----------|-------------|----------|
| Model Size | 1.9B | 2B/5B | 2B | 30B |
| VAE Compression | 1:192 | 1:48 | 1:96 | 1:96 |
| Latent Channels | 128 | 16 | 16 | 16 |
| Input Patchifier | 1x1x1 | 2x2x1 | 2x2x1 | 2x2x1 |
| Attention Blocks | Self + Cross | Self-only | Self-only | Self + Cross |

## Ablation Studies

1. **Reconstruction GAN vs. Traditional GAN:** At high compression (1:192), standard GAN+L2 produces visible artifacts; rGAN significantly reduces these.
2. **RoPE Frequency Spacing:** Exponential spacing yields consistently lower training loss than inverse-exponential spacing.
3. **Denoising Decoder:** Internal user study showed strong preference for VAE decoder denoising (t=0.05) over standard latent-only (t=0.0), particularly in high-motion videos.
4. **Latent Space Redundancy:** PCA analysis shows VAE learns to reduce inter-channel correlation during training, reaching near-zero at completion.

## Limitations

- Focuses on generating short videos of up to ten seconds
- Performance varies significantly based on quality and clarity of textual prompts
- Domain-specific applications (multi-view synthesis, fine-grained editing) remain untested

## Model Version History

| Version | Date | Parameters | Notes |
|---------|------|-----------|-------|
| 0.9 (original) | Nov 2024 | 2B | Initial open-source release |
| 0.9.1 | Dec 2024 | 2B | Improvements |
| 0.9.5 | Mar 2025 | 2B | Quality improvements |
| 0.9.6 | Apr 2025 | 2B | + Distilled variant |
| 0.9.7 | May 2025 | 13B | Multiscale rendering (LTXV-13B) |
| 0.9.8 | Jul 2025 | 13B | 60-second video barrier broken |
