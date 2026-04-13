---
title: LTX-Video 0.9.1
type: source
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-091-release.md
  - raw/ltx-video-early-versions-090-091-095.md
tags:
  - ltx-video
  - release
  - v0.9.1
  - lightricks
---
# LTX-Video 0.9.1

LTX-Video 0.9.1 was released in **December 2024** as an incremental quality update over [[ltx-video-090]]. Announced by LTX Studio on X (Twitter), it was positioned as "a big update to LTX Video" focusing on motion quality, image-to-video fidelity, and introducing a training-free video enhancement capability.

## Release Details

- **Release date:** December 2024
- **Parameters:** 2B (unchanged from [[ltx-video-090]])
- **Architecture:** Same DiT + [[video-vae]] as 0.9.0
- **Repository:** [Lightricks/LTX-Video-0.9.1](https://huggingface.co/Lightricks/LTX-Video-0.9.1)

## Improvements Over 0.9.0

### Motion Quality
- Smoother, more natural and fluid video movement
- Improved physical plausibility in generated scenes
- Reduced artifacts and visual noise

### Image-to-Video
- Significant boost in image-to-video generation quality
- Enhanced fidelity when conditioning on input images

### Video Enhancement
- First **training-free generated video enhancement** capability
- Ability to improve generated video quality post-generation without additional model training

### Speed
- Maintained the faster-than-real-time generation speeds established by [[ltx-video-090]]

## Output Specifications

| Property | Value |
|----------|-------|
| Native resolution | 1216x704 |
| Resolution constraint | Divisible by 32 |
| Frame rate | 30 fps |
| Max frames | 257 (8n+1 pattern) |
| Best under | 720x1280, 257 frames |

## Inference Configuration

- **Inference steps:** ~50
- **Guidance:** [[classifier-free-guidance]] (CFG)
- **Negative prompts:** Supported ("worst quality, inconsistent motion, blurry, jittery, distorted")
- **Pipeline:** `LTXPipeline` and `LTXImageToVideoPipeline` via Diffusers
- **Requirements:** Python 3.10.5+, CUDA 12.2, PyTorch >= 2.1.2

## Significance

Version 0.9.1 was a quality-focused refinement that preserved the architecture and speed of [[ltx-video-090]] while improving visual output. It sits in the middle of the [[ltx-video-early-versions-overview]] lineage, between the initial release and [[ltx-video-095]].

## References

- [Hugging Face repository](https://huggingface.co/Lightricks/LTX-Video-0.9.1)
- [LTX Studio announcement on X](https://x.com/LTXStudio/status/1870099463416803594)
