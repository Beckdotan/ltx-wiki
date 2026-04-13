---
title: Classifier-Free Guidance in LTX-Video
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-096-distillation-milestone.md
  - raw/ltx-video-distilled-models-and-optimization.md
  - raw/ltx-video-distilled-optimized-variants.md
tags:
  - ltx-video
  - cfg
  - stg
  - guidance
  - inference
  - distillation
---
# Classifier-Free Guidance in LTX-Video

Classifier-Free Guidance (CFG) is a standard inference technique used in diffusion models to improve output quality and prompt adherence. In LTX-Video, guidance has both a standard form (CFG) and a video-specific extension (STG, Spatio-Temporal Guidance). The elimination of both through [[ltx-video-distillation]] was one of the most impactful optimizations in the model's history.

## How CFG Works

During inference, the model performs **two forward passes** per diffusion step:

1. **Conditional pass:** The model generates output conditioned on the text prompt
2. **Unconditional pass:** The model generates output with no prompt (or a null/negative prompt)

The final output is computed by amplifying the difference between the conditional and unconditional predictions:

```
output = unconditional + guidance_scale * (conditional - unconditional)
```

Higher guidance scales produce outputs that more closely follow the prompt but may introduce artifacts.

## Spatio-Temporal Guidance (STG)

STG extends the guidance concept to video-specific dimensions. The LTX-Video dev models use STG alongside CFG to improve temporal coherence and spatial consistency across frames. Like CFG, STG requires additional computation per step.

## Computational Cost

Guidance roughly **doubles the computation per inference step** because each step requires two complete forward passes through the model. For a 2B-parameter model running 50 steps, this means effectively running 100 forward passes.

## Elimination Through Distillation

The [[ltx-video-distillation|distilled variants]] introduced in [[ltx-video-096]] eliminate the need for both CFG and STG. This is achieved by training the distilled model to internalize the guidance signal -- it learns to produce guided-quality outputs from a single forward pass.

### Impact on Performance

| Metric | Dev (with guidance) | Distilled (no guidance) |
|--------|-------------------|------------------------|
| Forward passes per step | 2 | 1 |
| Recommended steps | 30-50 | 8 |
| Total forward passes | 60-100 | 8 |
| Relative speed | 1x | ~15x |

The combined effect of halving the per-step cost and reducing step count by ~4-6x explains the ~15x total speedup.

## Negative Prompts

In versions that use CFG ([[ltx-video-090]], [[ltx-video-091]], [[ltx-video-095]], and the dev variants of [[ltx-video-096]]+), negative prompts are supported and recommended:

> "worst quality, inconsistent motion, blurry, jittery, distorted"

Negative prompts replace the null prompt in the unconditional pass, steering the model away from undesirable outputs. Distilled models do not use negative prompts since they do not perform unconditional passes.

## References

- See [[ltx-video-distillation]] for how distillation eliminates guidance
- See [[ltx-video-dev-vs-distilled]] for the practical comparison
