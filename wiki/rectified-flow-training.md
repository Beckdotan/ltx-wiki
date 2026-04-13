---
title: Rectified-Flow Training
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/architecture-video-dit-transformer.md
  - raw/ltx-video-architecture-paper-details.md
  - raw/ltx-video-architecture-technical.md
  - raw/ltx-video-transformer-dit-architecture.md
tags:
  - training
  - rectified-flow
  - diffusion
  - multi-resolution
  - ltx-video
---

# Rectified-Flow Training

LTX-Video uses rectified-flow training for the [[video-dit-transformer]], following the approach established by SD3 (Stable Diffusion 3). This training methodology defines how the model learns to denoise video latents.

## Formulation

### Forward Process

The noising process interpolates linearly between clean data and noise:

```
z_t = (1 - t) * z_0 + t * epsilon
```

where z_0 is the clean latent, epsilon is Gaussian noise, and t is the diffusion timestep.

### Prediction Target

The model predicts velocity rather than noise:

```
v = epsilon - z_0
```

Velocity prediction provides a more evenly-difficult task across timesteps compared to noise prediction.

### Denoising Step

```
z_{t-dt} = z_t - dt * v_t^theta
```

## Timestep Scheduling

Timesteps are sampled from a **log-normal distribution** (not uniform), following SD3:

- **Shifted toward higher-noise regions** depending on the number of tokens in the sequence
- Rationale: Higher resolutions need more noising to maintain signal-to-noise ratio (from the SimpleDiffusion finding)
- **PDF clamped** at percentiles 0.5 and 99.9 to prevent starvation at the distribution tails (ensuring the model sees both very clean and very noisy examples)

## Multi-Resolution Training

The model is trained simultaneously on multiple width/height/duration combinations:

- All samples are resized to approximately the same token count
- **Stochastic token dropping** at rates of 0-20% fixes token counts across sequences
- This eliminates the need for complex token-packing or padding strategies
- The model generalizes well to unseen resolution/duration combinations after diverse training

## Image Training

Images are included alongside videos as one of the resolution-duration combinations:

- Image datasets enrich concept diversity with concepts not typically present in video datasets
- Enabled by the causal design of the [[video-vae]], which naturally encodes single images as valid latent representations

## Optimizer and Fine-Tuning

- **Optimizer**: ADAM-W
- **Post-pretraining**: Fine-tuning on a high-aesthetic video subset for quality improvement

## Distillation

Distilled variants achieve 15x faster inference:
- Do not require Classifier-Free Guidance (CFG) or Spatiotemporal Guidance (STG)
- Support sampling with 8 (recommended) or fewer diffusion steps
- Trade slight quality reduction for major speed gains

## Inference Steps

- **Dev models**: 20-50 steps typical (paper benchmark: 20 steps)
- **Distilled models**: As few as 7-8 steps
