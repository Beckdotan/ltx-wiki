---
title: LTX Video Overview
type: overview
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-all-versions-overview.md
  - raw/ltx-video-overview-all-versions.md
  - raw/ltx-models-ltxv-ltx2.md
  - raw/ltx-video-capabilities-per-version.md
tags:
  - ltx-video
  - lightricks
  - video-generation
  - dit
  - overview
---
# LTX Video Overview

LTX-Video (LTXV) is an open-source video generation model family developed by [[lightricks]]. It was the first [[dit-architecture]]-based (Diffusion Transformer) model capable of generating high-quality videos in real-time. The model produces videos at up to 1216x704 resolution and 30 FPS, faster than they can be watched.

## Key Facts

- **Developer:** [[lightricks]]
- **Initial release:** November 2024 (v0.9.0)
- **Latest LTX-Video version:** v0.9.8 (mid-July 2025)
- **Successor:** [[ltx-2]] (October 2025), [[ltx-2-3]] (March 2026)
- **Architecture:** [[dit-architecture]] (Diffusion Transformer), built on Pixart-alpha
- **Parameter counts:** 2B (original) and 13B (from v0.9.7)
- **Paper:** arXiv:2501.00103

## Core Capabilities

LTX-Video supports a broad range of video generation workflows:

- **Text-to-video (T2V)** -- Generate video from text prompts (all versions)
- **Image-to-video (I2V)** -- Animate still images into video sequences (all versions)
- **Video-to-video (V2V)** -- Transform or extend existing video (v0.9.7+)
- **Multi-keyframe conditioning** -- Condition on multiple images/videos at specified frame positions (v0.9.5+)
- **Video extension** -- Extend videos forward or backward (v0.9.5+)
- **IC-LoRA control** -- Depth, pose, canny edge conditioning (v0.9.7+)
- **Spatial upscaling** -- 2x resolution increase in latent space (v0.9.7+)
- **Temporal upscaling** -- Frame interpolation for smoother video (v0.9.8+)
- **LoRA fine-tuning** -- Custom model adaptation (v0.9.7+)

See [[ltx-video-capabilities]] for the full capability matrix across versions.

## Output Specifications

- **Native resolution:** 1216x704 at 30 FPS (v0.9.1+); 768x512 at 24 FPS (v0.9.0)
- **Maximum frames:** 257 (must follow 8n+1 pattern)
- **Maximum duration:** Up to 60 seconds (v0.9.7+); ~8.5 seconds (v0.9.1-0.9.6)
- **Resolution constraint:** Height and width must be divisible by 32
- **Language:** English prompts only; elaborate, descriptive prompts perform best

## Model Family

The LTX-Video model family includes multiple [[ltx-video-model-variants]]:

| Category | Description |
|----------|-------------|
| **Dev** | Full-quality models for highest output quality |
| **Distilled** | 15x faster inference, 8 steps, no CFG/STG needed |
| **FP8** | Quantized for ~50% memory reduction |
| **Mix** | Multi-scale workflow combining dev + distilled |
| **ICLoRA** | Controlled generation adapters (depth, pose, canny, detailer) |
| **Upscalers** | Spatial (2x) and temporal (frame interpolation) |

## Version History

See [[ltx-video-versions]] for the full version timeline and [[ltx-video-changelog]] for detailed changes between versions.

| Version | Date | Parameters | Key Milestone |
|---------|------|------------|---------------|
| 0.9.0 | Nov 2024 | 2B | Initial open-source release |
| 0.9.1 | Dec 2024 | 2B | Smoother motion, improved physics |
| 0.9.5 | Mar 2025 | 2B | Keyframe conditioning, commercial license |
| 0.9.6 | Apr 2025 | 2B | Distilled variant, 15x faster inference |
| 0.9.7 | May 2025 | 13B + 2B | 13B scale-up, multiscale rendering, upscalers |
| 0.9.8 | Jul 2025 | 13B + 2B | IC-LoRA, detailer, 60s videos, multi-condition |

## Integration

LTX-Video can be used via multiple methods. See [[ltx-video-huggingface]] for details.

- **HuggingFace Diffusers** -- `LTXPipeline`, `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`
- **[[comfyui-integration]]** -- Official `ComfyUI-LTXVideo` node package
- **Local inference** -- Official Python codebase on GitHub
- **Online platforms** -- LTX Studio, Fal.ai, Replicate

## Licensing

- v0.9.0: RAIL-M license (November 2024)
- v0.9.5: OpenRail-M license (commercial use allowed)
- v0.9.6+: LTX-Video Open-Weights License
- 13B model (v0.9.7+): Free for enterprises under $10M annual revenue

## Prompt Guidelines

- Use English only
- Elaborate, detailed descriptions (~200 words) yield best results
- Describe subject, action, environment, lighting, camera angles
- Negative prompts supported (e.g., "worst quality, inconsistent motion, blurry, jittery, distorted")

## References

- [[ltx-video-versions]] -- Full version timeline
- [[ltx-video-capabilities]] -- Capability matrix across versions
- [[ltx-video-model-variants]] -- Complete model inventory
- [[ltx-video-changelog]] -- Version-by-version improvements
- [[ltx-video-architecture]] -- Core technical architecture
- [[ltx-video-huggingface]] -- HuggingFace weights and integration
- [[ltx-video-to-ltx-2]] -- Evolution to LTX-2 and LTX-2.3
