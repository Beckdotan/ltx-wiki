# LTX-Video Model Architecture - Technical Deep Dive

**Source:** https://arxiv.org/abs/2501.00103
**Source:** https://arxiv.org/html/2501.00103v1
**Source:** https://ltx.studio/blog/ltxv-models
**Paper:** "LTX-Video: Realtime Video Latent Diffusion"

## Architecture Overview

LTX-Video is built on a Diffusion Transformer (DiT) foundation -- a class of generative models that combines the iterative refinement process of diffusion models with the sequence modeling power of transformers. Unlike traditional diffusion UNets, the DiT architecture scales more efficiently with compute and data.

The key innovation is the integration of the Video-VAE and denoising transformer as unified components rather than independent modules, optimizing their interaction within a compressed latent space.

## Transformer Specifications

| Parameter | Value |
|-----------|-------|
| Base model parameters | 1.9B (original), 13B (v0.9.7+) |
| Transformer blocks | 28 |
| Hidden dimension | 2048 |
| FFN dimension factor | 4x |
| Attention type | Self + Cross attention |
| Cross-attention | Text conditioning via T5-XXL |
| Positional embeddings | Rotary Position Embeddings (RoPE) |
| RoPE frequency spacing | Exponential (outperforms inverse-exponential) |
| Normalization | RMSNorm for QK normalization (replacing LayerNorm) |

## Comparative Architecture Table

| Metric | LTX-Video | CogVideoX | PyramidFlow | HunyuanVideo | MovieGen |
|--------|-----------|-----------|-------------|--------------|----------|
| Base size | 1.9B | 2B/5B | 2B | 13B | 30B |
| Hidden dim | 2048 | 1920/3072 | 1536 | 3072 | 6144 |
| VAE compression | 32x32x8, 128ch | 8x8x4, 16ch | 8x8x8, 16ch | 8x8x4, 16ch | 8x8x8, 16ch |
| Transformer blocks | 28 | 30/42 | 24 | 60 | 48 |
| Patchifier | 1x1x1 (none) | 2x2x1 | 2x2x1 | 2x2x1 | 2x2x1 |

## Video-VAE Architecture

### Compression
- **Spatiotemporal downscaling:** 32 x 32 x 8 pixels per token
- **Total compression ratio:** 1:192 (2x more than comparable models at 1:48 or 1:96)
- **Output channels:** 128 (vs. 16 channels in CogVideoX)
- **Pixels-to-tokens ratio:** 1:8192 (vs. 1:1024-1:2048 in comparable models)
- **Spatial compression ratio:** 8 (for VAE tiling)

### Key Innovation: Patchification Relocation
Unlike traditional models that apply patchifying at the transformer input (typically 2x2x1), LTX-Video relocates the patchifying operation to the VAE's input. The patchifier is effectively 1x1x1 (none needed at the transformer level) because the VAE already handles the extreme compression. This reduces redundancy and improves efficiency.

### Encoder
- Causal encoder utilizing 3D causal convolutions
- First frame encoded separately as a distinct latent frame
- 3D convolutions slightly outperform separable convolutions (per ablation)
- Causal VAEs preferable to non-causal for simultaneous image/video training

### Decoder (Denoising Decoder)
- Performs both latent-to-pixel conversion AND the final denoising step
- Produces clean result directly in pixel space
- Diffusion-timestep conditioning integrated into decoder
- Multi-layer noise injection (StyleGAN-inspired)
- Uniform log-variance across channels
- Trained with noise levels in range [0, 0.2]
- Per ablation: decoder performing final denoising step (t=0.05) strongly preferred over standard (t=0.0)

### VAE Loss Functions
- Pixel reconstruction (MSE)
- Video Discrete Wavelet Transform loss (8 3D DWT transforms, L1)
- Perceptual loss (LPIPS)
- Reconstruction GAN (novel approach comparing real vs. reconstructed pairs)
  - Significantly reduces artifacts at 1:192 compression ratio vs. traditional GAN

## Text Encoder

- **Model:** T5-XXL
- **Integration:** Cross-attention conditioning on text tokens
- **Language:** English only

## Training Details

### Data
- Publicly available data supplemented with licensed material
- Aesthetic filtering via Siamese Network trained on tens of thousands of manually-tagged image pairs
- Motion filtering to remove videos with insignificant motion
- Multi-resolution training with stochastic token dropping (0-20%)
- Simultaneous training on images and videos

### Optimization
- **Optimizer:** ADAM-W
- **Timestep scheduling:** Log-normal distribution shifted toward higher-noise regions based on token count
- **Percentile clamping:** 0.5 to 99.9
- **Multi-resolution training:** Various width/height/duration combinations
- **Diffusion steps (inference):** 20-40 steps for dev models, 8 steps for distilled models

## Multiscale Rendering (13B models)

Introduced with the 13B parameter model (v0.9.7), multiscale rendering is a layered generation process:

1. **Coarse pass:** Model drafts in lower detail first to capture coarse motion using fewer resources
2. **Progressive refinement:** Progressively adds structure, lighting, and micro-motion
3. **Built-in upscaling:** For 1080p output, first renders at 960x540, then upscales 2x

This approach mimics how professional artists work -- starting with broad strokes before layering fine details. Motion is prioritized first, then detail is infused. Result: render times up to 30x faster than comparable-sized models.

## Distillation Approach

Distilled variants achieve 15x faster inference than non-distilled models:
- Do not require classifier-free guidance (CFG)
- Do not require spatio-temporal guidance (STG)
- Support sampling with 8 (recommended) or fewer diffusion steps
- Trade slight quality reduction for major speed gains

## Image-to-Video Conditioning

- Per-token timestep conditioning
- Conditioning tokens set to timestep t_c (small value, e.g., 0)
- Non-conditioning tokens set to t=1
- "First-frame conditioning" via causal VAE encoding

## Human Evaluation Benchmarks

Text-to-Video win rates (1,000 prompts, 20 evaluators):
| Model | Win Rate |
|-------|----------|
| LTX-Video (2B) | 85% |
| PyramidFlow | 51% |
| CogVideoX 2B | 38% |
| Open-Sora Plan | 20% |

Image-to-Video win rates (1,000 image-prompt pairs):
| Model | Win Rate |
|-------|----------|
| LTX-Video (2B) | 91% |
| CogVideoX 2B | 47% |
| PyramidFlow | 35% |
| Open-Sora Plan | 20% |
