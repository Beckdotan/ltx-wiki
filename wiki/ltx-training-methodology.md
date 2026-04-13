---
title: LTX Training Methodology
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/training-methodology.md
  - raw/training-methodology-and-data.md
  - raw/ltx-video-training-data-and-methodology.md
tags:
  - training
  - methodology
  - rectified-flow
  - vae
  - data-pipeline
---
# LTX Training Methodology

This page documents the training methodology used to produce the base [[ltx-video-overview|LTX-Video]], [[ltx-2-overview]], and [[ltx-2.3-model]] foundation models. This is distinct from user-facing [[lora-training]] or fine-tuning, which adapts existing models to custom data.

## Rectified Flow Framework

LTX-Video uses Rectified Flow instead of standard DDPM-style diffusion:

- **Forward process:** `z_t = (1-t)*z_0 + t*epsilon`, with `t in [0,1]`
- **Noise:** `epsilon` sampled from standard normal `N(0,I)`
- **Prediction target:** velocity `v = epsilon - z_0` (following SD3)
- **Denoising step:** `z_{t-dt} = z_t - dt * v_t^theta`

This approach provides more balanced training difficulty across timesteps and enables straighter sampling trajectories, requiring fewer inference steps.

### Timestep Scheduling

- Sampled from a **log-normal distribution** (not uniform)
- Biased towards higher-noise regions, with bias dependent on token count
- Higher resolutions and longer videos receive more high-noise training to maintain proper SNR
- PDF clamped at percentiles 0.5 and 99.9 to prevent training starvation

### Optimizer

- **AdamW** optimizer for both pre-training and fine-tuning stages
- Specific learning rate and schedule were not disclosed in the paper

## Multi-Resolution Training

LTX-Video trains simultaneously on multiple resolution and duration combinations:

- All input samples contain approximately the same number of tokens (via resizing)
- **Stochastic token dropping** at rates of 0-20% per sample normalizes token counts, eliminating the need for complex packing or padding
- The model generalizes to unseen resolution/duration combinations after training
- Images are included as one resolution-duration variant, enabling **joint image-video training** that enriches concept diversity

## Training Stages

1. **Pre-training:** Full dataset with diverse content
2. **Fine-tuning:** Subset of high-aesthetic data for quality refinement

## Data Pipeline

### Data Sources

- Publicly available data supplemented with licensed material
- Designed for diversity and comprehensiveness across visual content types
- [[ltx-2-overview]] trained on a subset of the same dataset, focused on clips with significant audio components

### Quality Filtering

**Aesthetic model:**
- Siamese Network trained on tens of thousands of manually tagged image pairs
- Millions of samples labeled via a multi-labeling network; pairs sampled sharing at least one of the top three labels (minimizes distribution shifts)
- Network predicts aesthetic scores preserving order relations
- Samples below the aesthetic threshold are filtered out

**Motion filtering:**
- Videos with insignificant motion are removed
- Ensures training data contains meaningful temporal dynamics

**Aspect ratio standardization:**
- Black bars automatically cropped
- Maintains consistent aspect ratios across training data

### Captioning

**LTX-Video captioning:**
- Internal automatic image and video captioner re-captions the entire training set
- Detailed, elaborate English captions describing subject, action, environment, lighting, and camera movement
- Improved alignment between visual content and textual annotations

**LTX-2 captioning (enhanced multimodal):**
- New system describing both visual and auditory tracks
- Audio descriptions: music, ambient sounds, precise dialogue transcriptions with speaker/language/accent identification
- Visual descriptions: camera motion, lighting conditions, subject behavior
- Comprehensive yet factual, with no emotional interpretation

## VAE Training

The [[ltx-video-architecture|VAE]] is trained with four combined losses:

1. **Pixel reconstruction (MSE):** Standard pixel-wise reconstruction objective
2. **Video DWT (L1):** Spatio-temporal 3D Discrete Wavelet Transform applied to both input and reconstructed videos, producing 8 wavelet coefficients; L1 distance ensures high-frequency detail preservation
3. **Perceptual (LPIPS):** Learned perceptual similarity metric
4. **Reconstruction-GAN (rGAN):** Novel adversarial approach where the discriminator sees both original and reconstructed versions concatenated, deciding which is original; this relative comparison simplifies GAN training and improves stability over traditional adversarial approaches

### Denoising Decoder

- Decoder trained on noise levels `[0, 0.2]` from the diffusion schedule
- Multi-layer noise injection with learned per-channel noise levels
- Uniform log-variance: single predicted value shared across all latent channels
- Internal user study confirmed videos with denoising decoder (t=0.05) are strongly preferred over standard decoder (t=0.0), especially for high-motion content

## Image-to-Video Training

LTX-Video's approach to image-to-video conditioning extends Open-Sora:

- Uses diffusion timestep as conditioning indicator
- Different timestep and noising level per token
- During training, first-frame tokens are occasionally set to small random timestep values and noised accordingly
- Eliminates the need for special conditioning tokens or separate model paths

## LTX-2 Training Specifics

- Flow-matching loss for velocity field prediction (same Rectified Flow framework)
- **Gemma 3 (12B)** text encoder weights frozen during initial training stage
- Learnable projection matrix W jointly optimized briefly, then frozen
- Text connector blocks trained simultaneously with [[diffusion-transformer|DiT]] blocks
- Multi-layer feature extraction across all Gemma 3 decoder layers
- Joint audio-visual training from the start with shared flow-matching objective
- Cross-modality AdaLN enables inter-modal conditioning during training

## Scaling History

| Transition | Key Changes |
|-----------|-------------|
| LTX-Video to LTXV-13B | 2B to 13B parameters; multiscale rendering; maintained 1:192 VAE compression; 30x faster generation |
| LTXV to [[ltx-2-overview]] | Added 5B audio stream; upgraded T5-XXL to Gemma 3 12B; 28 to 48 transformer layers; bidirectional cross-attention |
| [[ltx-2-overview]] to [[ltx-2.3-model]] | Grew to 22B total parameters; rebuilt VAE (+2.5 dB PSNR); 4x larger gated-attention text connector; upgraded HiFi-GAN vocoder |

## Key Ablation Findings

- At 1:192 compression, standard GAN losses fail; the rGAN approach significantly reduces visible artifacts
- Exponential RoPE frequency spacing is confirmed superior to inverse-exponential spacing
- Compression artifacts are mitigated by the denoising decoder's last-step denoising

## Evaluation

From the LTX-Video paper:
- 1,000 text prompts for T2V evaluation; 1,000 image+prompt pairs for I2V (images from FLUX.1)
- 5-second videos at 768x512 with 40 diffusion steps
- 20-person human survey
- Compared against Open-Sora Plan, CogVideoX (2B), PyramidFlow
- Criteria: visual quality, motion fidelity, prompt adherence

## References

- arXiv:2501.00103 (LTX-Video paper)
- arXiv:2601.03233 (LTX-2 paper)
- [[ltx-video-architecture]]
- [[lora-training]]
- [[training-dataset-preparation]]
