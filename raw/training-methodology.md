# LTX Video Training Methodology

## Overview

This document consolidates all known information about the training methodology used for the LTX model family, including data preparation, training procedures, loss functions, and scaling strategies.

## Data Pipeline

### Data Sources
- Publicly available data and licensed material
- Designed for diversity and comprehensiveness across visual content types
- LTX-2 trained on a subset of the same dataset, focused on clips with significant audio components

### Quality Filtering

**Aesthetic Filtering:**
- Siamese Network trained on manually-tagged image pairs
- CDN-driven approach for aesthetic evaluation
- Filters for both quality of generated output and training efficiency

**Motion Filtering:**
- Removes videos exhibiting insignificant motion
- Ensures training data contains meaningful temporal dynamics

**Aspect Ratio Standardization:**
- Automatic cropping of black bars
- Maintains consistent aspect ratios across training data

### Captioning

**LTX-Video:**
- Automated captioning across entire training set
- Metadata enhancement improves alignment between visual outputs and textual inputs

**LTX-2 (Multimodal Captioning):**
- Custom video captioning system generating comprehensive multimodal descriptions
- Audio descriptions: music, ambient sounds, precise dialogue transcriptions with speaker/language/accent identification
- Visual descriptions: camera motion, lighting conditions, subject behavior
- Integration: factual, emotionally-neutral descriptions of what is seen and heard

## Training Procedure

### Rectified Flow Framework

LTX-Video uses Rectified Flow instead of standard DDPM-style diffusion:
- Model predicts velocity: **v = epsilon - z_0** (instead of noise)
- Provides more balanced training difficulty across timesteps
- Enables straighter sampling trajectories, requiring fewer inference steps

### Timestep Scheduling
- Scheduler biased towards higher-noise regions, with bias dependent on the number of tokens
- Sampling from log-normal distribution
- Clamped at percentiles 0.5 and 99.9

### Multi-Resolution Training
- Simultaneous training on multiple resolution-duration combinations
- Includes images as one resolution-duration variant (enabling joint image+video training)
- Token count normalization using stochastic token dropping at rates ranging from 0% to 20%

### Optimizer
- AdamW optimizer for main training
- Subsequent fine-tuning on high-aesthetic data subset

## VAE Training

### Loss Functions

1. **Pixel Reconstruction Loss (MSE):** Standard reconstruction objective
2. **Video DWT Loss (L1):** Spatio-temporal 3D Discrete Wavelet Transform applied to both input and reconstructed videos, producing 8 wavelet coefficients. L1 distance ensures high-frequency detail preservation.
3. **Perceptual Loss (LPIPS):** Learned perceptual similarity metric
4. **Reconstruction-GAN (rGAN):** Novel adversarial training where discriminator sees both original and reconstructed versions concatenated, deciding which is original. This relative comparison simplifies GAN training and improves stability over traditional adversarial approaches.

### Denoising Decoder Training
- Decoder trained on noise levels [0, 0.2] from the diffusion schedule
- Multi-layer noise injection with learned per-channel noise levels
- Uniform log-variance: single predicted value shared across all channels

## LTX-2 Training Specifics

### Flow-Matching Loss
- Velocity field prediction (same Rectified Flow framework as LTX-Video)

### Text Encoder Integration
- Gemma 3 (12B) weights frozen during initial training stage
- Learnable projection matrix W jointly optimized
- Text connector blocks trained simultaneously with DiT blocks
- Multi-layer feature extraction across all Gemma 3 decoder layers

### Modality Training
- Joint audio-visual training from the start
- Audio and video trained with shared flow-matching objective
- Cross-modality AdaLN enables inter-modal conditioning during training

## Image-to-Video Training

LTX-Video's approach to image-to-video conditioning (extending Open-Sora):
- Uses diffusion timestep as conditioning indicator
- Different timestep and corresponding noising level per token
- During training: first frame tokens occasionally set to small random timestep values and noised accordingly
- This eliminates the need for special conditioning tokens or separate model paths

## Scaling Strategy

### LTX-Video to LTXV-13B
- Scaled from 2B to 13B parameters
- Introduced multiscale rendering for progressive detail generation
- Maintained same VAE architecture (1:192 compression)
- Achieved 30x faster generation than comparable-scale models

### LTXV to LTX-2
- Added audio stream (5B parameters) alongside video stream (14B)
- Upgraded text encoder from T5-XXL to Gemma 3 12B
- Increased from 28 to 48 transformer layers
- Added bidirectional cross-attention between modalities

### LTX-2 to LTX-2.3
- Grew to 22B total parameters
- Rebuilt VAE with +2.5 dB PSNR improvement
- 4x larger gated attention text connector
- Upgraded HiFi-GAN vocoder

## Multiscale Rendering (LTXV-13B)

Introduced in the 13B parameter version (v0.9.7):
- Model drafts at lower detail first to capture coarse motion
- Scene divided into tiles with progressively more detail
- Progressive layers: coarse motion -> structure -> lighting -> micro-motion
- Up to 30x faster than similar-sized models
- Enables consumer-grade GPU deployment

## Sources

- https://arxiv.org/abs/2501.00103
- https://arxiv.org/abs/2601.03233
- https://github.com/Lightricks/LTX-Video
- https://ltx.studio/blog/ltxv-models
- https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
