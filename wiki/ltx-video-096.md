---
title: LTX-Video 0.9.6
type: source
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-096-release.md
  - raw/ltx-video-096-distillation-milestone.md
  - raw/ltx-video-early-versions-090-091-095.md
tags:
  - ltx-video
  - release
  - v0.9.6
  - lightricks
  - distillation
  - milestone
---
# LTX-Video 0.9.6

LTX-Video 0.9.6, released approximately **April 2025**, was a pivotal release that introduced the [[ltx-video-dev-vs-distilled]] model variant pattern. While maintaining the 2B parameter architecture, it split into two distinct models optimized for different use cases: a dev (full-quality) variant and a [[ltx-video-distillation|distilled]] variant achieving **15x faster inference**.

## Release Details

- **Release date:** ~April 2025 (less than three weeks before v0.9.7 on May 6, 2025)
- **Parameters:** 2B (both variants)
- **File size:** ~6.34 GB each
- **Access:** Gated on Hugging Face

## Model Variants

### ltxv-2b-0.9.6-dev (Full-Quality)

| Property | Value |
|----------|-------|
| Parameters | 2B |
| Inference steps | 20-50 (standard) |
| Guidance | [[classifier-free-guidance]] (CFG) + STG used |
| VRAM | ~12 GB |
| Purpose | Final outputs requiring highest quality |

### ltxv-2b-0.9.6-distilled

| Property | Value |
|----------|-------|
| Parameters | 2B |
| Inference steps | 8 (recommended, can use fewer) |
| Guidance | **None required** -- no CFG or STG |
| VRAM | ~8 GB |
| Speed | 15x faster than dev variant |
| Purpose | Rapid iteration, lightweight deployment |

The distilled variant achieves its dramatic speedup through three compounding factors:
1. **[[ltx-video-distillation|Knowledge distillation]]** from the dev model
2. **Elimination of [[classifier-free-guidance]]** (CFG/STG), removing the need for dual forward passes
3. **Dramatically reduced inference steps** (8 vs 20-50)

## Quality Improvements Over 0.9.5

- Enhanced prompt adherence
- Smoother motion
- Finer details for more realistic and coherent outputs
- More stable generation overall

## Default Settings

- **Resolution:** 1216 x 704 pixels
- **Frame rate:** 30 fps
- **Max frames:** 257 (8n+1 pattern)

## Hugging Face Repositories

- `Lightricks/LTX-Video-0.9.6-dev` (gated access)
- `Lightricks/LTX-Video-0.9.6-distilled` (gated access)
- Config files: `ltxv-2b-0.9.6-dev.yaml`, `ltxv-2b-0.9.6-distilled.yaml`

## Significance

Version 0.9.6 was a critical milestone in the LTX-Video lineage:

1. **Introduced the dev/distilled split** that became the standard pattern for all subsequent releases (0.9.7, 0.9.8)
2. **Proved the architecture responds well to distillation**, setting the stage for the 13B distilled models
3. **Made real-time video generation accessible on consumer GPUs** (~8 GB VRAM for the distilled variant)
4. **Served as the bridge** between the 2B-only era ([[ltx-video-early-versions-overview]]) and the 13B scale-up in v0.9.7

The 0.9.6 models remain available and recommended for users with limited VRAM (8-12 GB) who prioritize speed over absolute quality.

## References

- [LTX Studio announcement on X](https://x.com/LTXStudio/status/1912894622927298809)
- [Hugging Face main page](https://huggingface.co/Lightricks/LTX-Video)
- [Patreon post](https://www.patreon.com/posts/ltx-video-v0-9-6-127046557)
