---
title: LTX-Video Early Versions Overview (0.9.0-0.9.6)
type: overview
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-early-versions-090-091-095.md
  - raw/ltx-video-090-initial-release.md
  - raw/ltx-video-091-release.md
  - raw/ltx-video-095-release.md
  - raw/ltx-video-096-release.md
  - raw/ltx-video-096-distillation-milestone.md
tags:
  - ltx-video
  - overview
  - evolution
  - lightricks
---
# LTX-Video Early Versions Overview (0.9.0-0.9.6)

This page traces the evolution of LTX-Video from its initial open-source release through the [[ltx-video-distillation]] milestone, covering the period from November 2024 to approximately April 2025.

## Version Timeline

| Version | Date | Key Change |
|---------|------|------------|
| [[ltx-video-090]] | November 2024 | Initial release; first real-time DiT video model |
| [[ltx-video-091]] | December 2024 | Motion quality, I2V improvements, video enhancement |
| [[ltx-video-095]] | March 5, 2025 | Keyframe conditioning, commercial license, ComfyUI |
| [[ltx-video-096]] | ~April 2025 | [[ltx-video-distillation]] introduced; dev/distilled split |

## Architectural Consistency (0.9.0-0.9.6)

All early versions share the same core architecture established in [[ltx-video-090]]:

- **Model size:** ~2B parameters (1.9B transformer)
- **Architecture:** [[diffusion-transformer]] (DiT) with 28 blocks, 2048 hidden dimension
- **[[video-vae]]:** Custom spatiotemporal VAE with 1:192 compression ratio (32x32x8, 128 channels)
- **Text encoder:** T5-XXL
- **Training paradigm:** Rectified-flow with velocity prediction
- **Resolution constraints:** Divisible by 32
- **Frame constraints:** 8n+1 pattern, up to 257 frames

The improvements from 0.9.0 through 0.9.5 were achieved through training refinements (data, hyperparameters, fine-tuning) rather than architectural changes. The first structural change came with [[ltx-video-096]], which introduced the distilled variant.

## Evolution of Capabilities

### Generation Modes
- **0.9.0:** Text-to-video + image-to-video
- **0.9.1:** Added training-free video enhancement
- **0.9.5:** Added keyframe conditional support (first/last/intermediate frame conditioning)
- **0.9.6:** Dev and distilled inference modes

### Licensing
- **0.9.0-0.9.1:** RAIL-M license (restrictive)
- **0.9.5+:** OpenRail-M license (commercial use permitted)

### Inference Efficiency
- **0.9.0-0.9.5:** ~50 inference steps with [[classifier-free-guidance]]
- **0.9.6 dev:** 20-50 steps with CFG/STG
- **0.9.6 distilled:** **8 steps, no guidance required** -- 15x speedup

### Default Resolution
- **0.9.0:** 768x512 (benchmark)
- **0.9.1:** 1216x704 (native)
- **0.9.5-0.9.6:** 1216x704 (default), best under 720x1280

## Quality Progression

Each version brought incremental quality improvements:

1. **[[ltx-video-090]]:** Established baseline; outperformed CogVideoX 2B and PyramidFlow in human evaluation
2. **[[ltx-video-091]]:** Smoother motion, improved physics, cleaner visuals, better I2V
3. **[[ltx-video-095]]:** Better prompt adherence, enhanced detail, reduced artifacts
4. **[[ltx-video-096]]:** Further prompt adherence and motion improvements; distilled variant trades slight quality for dramatic speed gains

## The Distillation Inflection Point

Version 0.9.6 marks the boundary between the "early" and "modern" eras of LTX-Video:

- **Before 0.9.6:** Single model per version, ~50 steps, CFG required, ~12 GB VRAM
- **After 0.9.6:** [[ltx-video-dev-vs-distilled]] split becomes standard, distilled models need only ~8 steps with no guidance, ~8 GB VRAM

This pattern carried forward into the 13B-parameter models (v0.9.7, v0.9.8), making [[ltx-video-distillation]] one of the most impactful changes in the model's history.

## What Came Next

The early 2B-only era gave way to two major expansions:
- **v0.9.7:** Scale-up to 13B parameters, [[fp8-quantization]], spatial/temporal upscalers, ICLoRA conditioning
- **v0.9.8:** Ecosystem maturation with 2B + 13B distilled variants, mix-mode workflows

## References

- See individual version pages for detailed source links
