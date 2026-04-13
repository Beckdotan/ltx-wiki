# LTX-Video: Realtime Video Latent Diffusion

## Metadata
- **Title:** LTX-Video: Realtime Video Latent Diffusion
- **Authors:** Yoav HaCohen, Nisan Chiprut, Benny Brazowski, Daniel Shalem, Dudu Moshe, Eitan Richardson, Eran Levin, Guy Shiran, Nir Zabari, Ori Gordon, Poriya Panet, Sapir Weissbuch, Victor Kulikov, Yaki Bitterman, Zeev Melumian, Ofir Bibi
- **Affiliation:** Lightricks
- **Date:** December 30, 2024 (arXiv: 2501.00103)
- **Source URL:** https://arxiv.org/abs/2501.00103
- **GitHub:** https://github.com/Lightricks/LTX-Video
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-Video

## Abstract

We introduce LTX-Video, a transformer-based latent diffusion model that adopts a holistic approach to video generation by seamlessly integrating the responsibilities of the Video-VAE and the denoising transformer. Unlike existing methods, which treat these components as independent, LTX-Video aims to optimize their interaction for improved efficiency and quality. At its core is a carefully designed Video-VAE that achieves a high compression ratio of 1:192, with spatiotemporal downscaling of 32x32x8 pixels per token, enabled by relocating the patchifying operation from the transformer's input to the VAE's input. Operating in this highly compressed latent space enables the transformer to efficiently perform full spatiotemporal self-attention, which is essential for generating high-resolution videos with temporal consistency. However, the high compression inherently limits the representation of fine details. To address this, our VAE decoder is tasked with both latent-to-pixel conversion and the final denoising step, producing the clean result directly in pixel space. This approach preserves the ability to generate fine details without incurring the runtime cost of a separate upsampling module. Our model supports diverse use cases, including text-to-video and image-to-video generation, with both capabilities trained simultaneously. It achieves faster-than-real-time generation, producing 5 seconds of 24 fps video at 768x512 resolution in just 2 seconds on an Nvidia H100 GPU, outperforming all existing models of similar scale. The source code and pre-trained models are publicly available, setting a new benchmark for accessible and scalable video generation.

## Key Contributions

1. **Holistic approach to latent diffusion:** LTX-Video seamlessly integrates the Video-VAE and the denoising transformer, optimizing their interaction within a compressed latent space and sharing the denoising objective between the transformer and the VAE's decoder.

2. **High-compression Video-VAE with novel loss functions:** By relocating the patchifying operation to the VAE and introducing novel loss functions, they achieve a 1:192 compression ratio with spatiotemporal downsampling of 32x32x8, enabling generation of high-quality videos at unprecedented speed.

3. **Fast, accessible, high-quality video generation model:** A faster-than-real-time text-to-video and image-to-video model with fewer than 2B parameters.

## Architecture Details

### Video-VAE
- **Compression ratio:** 1:192 (compared to 1:48 or 1:96 for other models)
- **Spatiotemporal downscaling:** 32x32x8 pixels per token
- **Latent channels:** 128 (vs typical 16)
- **Pixels-to-tokens ratio:** 1:8192 (vs typical 1:1024 or 1:2048)
- **No patchifier needed** -- patchifying is relocated from transformer input to VAE encoder input
- **Causal encoder** using 3D Causal Convolutions
- **Denoising decoder** with diffusion-timestep conditioning and multi-layer noise injection
- **Architecture:** 3D convolutions (found to work slightly better than separable 2D+1D convolutions)

### VAE Loss Functions
- **Pixel reconstruction (MSE)**
- **Video DWT Loss (L1):** Spatio-temporal Discrete Wavelet Transform -- computes 8 3D DWT transforms for input and reconstructed videos, uses L1 distance
- **Perceptual (LPIPS)**
- **Reconstruction GAN (rGAN):** Novel adaptation where discriminator sees both original and reconstructed samples concatenated, determining which is original vs reconstructed (rather than traditional real vs fake)
- **Multi-layer noise injection:** Following StyleGAN, noise injected at several decoder layers with learned per-channel noise level
- **Uniform log-variance:** Single predicted log-variance shared across all channels to prevent KL loss from "sacrificing" channels

### Shared Diffusion Objective
- The VAE decoder is trained as a diffusion model that maps noisy latents to clean pixels at varying noise levels
- The decoder performs the final denoising step (from noisy latent directly to clean pixels)
- Trained with noise levels in range [0, 0.2] corresponding to final diffusion timestep
- Uses adaptive normalization layers conditioned on timestep (following DDPM/U-Net approach)
- Cannot be applied iteratively (different dimensionality input/output), but handles the critical final step

### Video Transformer
- **Base architecture:** PixArt-alpha (extension of DiT)
- **Model size:** ~2B parameters (before distillation)
- **Positional embeddings:** RoPE with normalized fractional coordinates (coordinates computed in pixels and seconds relative to predefined max resolution/duration)
- **RoPE frequency spacing:** Exponential (not inverse-exponential as in many open-source implementations)
- **Normalization:** RMSNorm (replacing LayerNorm), QK Normalization on queries and keys before dot-product attention
- **Text conditioning:** Cross-attention (found to work better than MM-DiT)
- **Text encoder:** T5-XXL
- **Full spatiotemporal self-attention** across all tokens

### Image Conditioning (Image-to-Video)
- Per-token diffusion timestep mechanism: conditioning tokens get a small timestep (e.g., t_c = 0), while generation tokens start at t = 1
- First frame encoded by causal VAE into latent tensor with temporal dimension of 1
- Concatenated with noise latents, no additional parameters or special tokens required
- During training, occasionally set first-frame token timestep to small random value

### Training Methodology
- **Loss formulation:** Rectified Flow (velocity prediction v = epsilon - z_0)
- **Timestep scheduling:** Log-normal distribution shifted toward higher-noise regions depending on number of tokens (following SD3 and SimpleDiffusion recommendations)
- **Multi-resolution training:** Simultaneously trained on multiple resolution/duration combinations; stochastic token dropping (0-20%) to fix token counts
- **Image training:** Included alongside video training as one resolution-duration combination
- **Optimizer:** ADAM-W
- **Fine-tuning:** Post pre-training fine-tuning on high-aesthetic video subset
- **Data:** Publicly available data supplemented with licensed material

### Data Preparation Pipeline
- **Quality control:** Siamese Network trained on tens of thousands of manually tagged image pairs for aesthetic scoring; samples below threshold filtered out
- **Motion filtering:** Videos with insignificant motion removed
- **Aspect ratio filtering:** Black bars cropped out
- **Captioning:** Internal automatic image and video captioner for re-captioning entire training set
- **Fine-tuning data:** Selectively uses most aesthetic content

## Evaluation Results

### Human Survey (20 participants, 1000 prompts per task)
Compared against models of similar scale (~2B parameters):
- **Text-to-Video:** LTX-Video significantly outperforms Open-Sora Plan, CogVideoX (2B), and PyramidFlow
- **Image-to-Video:** LTX-Video significantly outperforms all compared models
- Videos generated at 768x512, 5 seconds, 40 diffusion steps

### Speed
- **2 seconds** to generate 121 frames at 768x512 pixels with 20 diffusion steps on Nvidia H100
- **Faster-than-real-time** generation (generates 5 seconds of video in 2 seconds)
- Fastest video generation model of its kind at time of release

## Limitations
- Sensitive to prompt formulation quality
- Limited to short videos (up to ~10 seconds)
- Domain-specific generalization not extensively tested

## Model Versions (from HuggingFace)
| Model | Parameters | Notes |
|-------|-----------|-------|
| ltxv-13b-0.9.8-dev | 13B | Highest quality, more VRAM |
| ltxv-13b-0.9.8-distilled | 13B | Faster, less VRAM |
| ltxv-13b-0.9.8-mix | 13B | Balanced speed-quality |
| ltxv-2b-0.9.8-distilled | 2B | Smallest, lightest |
| Various FP8 quantized versions | - | Memory efficient |

## References to Related Work Cited
- Sora, MovieGen, CogVideoX, Open-Sora Plan, PyramidFlow, HunyuanVideo
- DC-VAE (high spatial compression for text-to-image)
- PixArt-alpha, DiT, SD3, FLUX.1
- StyleGAN (multi-layer noise injection)
- SimpleDiffusion (noise scheduling)
