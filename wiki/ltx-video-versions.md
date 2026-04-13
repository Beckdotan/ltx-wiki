---
title: LTX Video Version Timeline
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-all-versions-overview.md
  - raw/ltx-video-overview-all-versions.md
  - raw/ltx-video-version-changelog-improvements.md
  - raw/ltx-video-version-changes-changelog.md
  - raw/model-evolution-ltx-video-to-ltx-2.3.md
  - raw/comfyui-ltx-version-history.md
tags:
  - ltx-video
  - versions
  - timeline
  - lightricks
  - comfyui
---
# LTX Video Version Timeline

This page documents the complete version history of [[ltx-video-overview|LTX-Video]], from the initial v0.9.0 release through v0.9.8. For detailed changes between each version, see [[ltx-video-changelog]].

## Summary Timeline

```
Nov 2024  v0.9.0  First release, 2B, real-time generation
Dec 2024  v0.9.1  Motion/physics/visual quality improvements
Mar 2025  v0.9.5  Keyframe conditioning, commercial license, ComfyUI
Apr 2025  v0.9.6  Dev/distilled split, 15x faster distilled
May 2025  v0.9.7  13B model, multiscale rendering, spatial upscaler
Jul 2025  v0.9.8  IC-LoRA, detailer, 60s videos, multi-condition
```

## v0.9.0 -- Initial Release (November 2024)

- **Parameters:** 2B (1.9B transformer)
- **Resolution:** 768x512 at 24 FPS
- **VRAM:** ~6GB minimum
- **Key claim:** First [[diffusion-transformer]]-based model capable of real-time video generation
- **Speed:** 5 seconds of video generated in 2 seconds on H100; ~4 seconds on RTX 4090
- **Capabilities:** Text-to-video, image-to-video
- **Text Encoder:** PixArt / T5-XXL
- **Model File:** `ltx-video-2b-v0.9.safetensors`
- **Paper:** arXiv:2501.00103
- **License:** RAIL-M
- **Open-sourced** on GitHub and [[ltx-video-huggingface|Hugging Face]]
- **ComfyUI:** Day-1 native support (announced on blog.comfy.org); official custom nodes branded "LTXVideo"; available through ComfyUI Manager from day one

## v0.9.1 -- Quality Refinement (December 2024)

- **Parameters:** 2B (unchanged)
- **Model File:** `ltx-video-2b-v0.9.1.safetensors`
- **Improvements:** Smoother motion, improved physics, cleaner visuals
- **Enhanced:** Image-to-video quality significantly improved
- **New:** Training-free video enhancement capability; spatio-temporal skip guidance for more consistent video
- **Resolution upgraded to:** 1216x704 at 30 FPS
- **Inference steps:** ~50 recommended
- **VRAM:** Improved low-VRAM support
- **ComfyUI:** All-in-one workflows for vid2vid, txt2vid, and img2vid
- **Diffusers:** `LTXPipeline` and `LTXImageToVideoPipeline`

## v0.9.5 -- Mature 2B Model (March 5, 2025)

- **Parameters:** 2B (unchanged)
- **New features:**
  - Multi-keyframe conditional support (first/last/any frame)
  - Video extension (forward and backward)
  - [[comfyui-ltx-integration-overview]] day-1 support
- **License:** OpenRail-M (commercial use now allowed)
- **Improvements:** Reduced artifacts, better prompt adherence, longer video support
- **Last version** of the pure 2B-only model line
- **Community:** LoRA support added, training datasets released (Squish, Cakeify)

## v0.9.6 -- Distillation Milestone (April 2025)

- **Parameters:** 2B (unchanged)
- **Major change:** First dev/distilled split
  - `ltxv-2b-0.9.6-dev` -- Full quality, lower VRAM
  - `ltxv-2b-0.9.6-distilled` -- 15x faster, real-time capable, 8 inference steps
- **Distilled variant:** No CFG/STG guidance required
- **Improvements:** Enhanced prompt adherence, smoother motion, finer details
- **Default settings:** 1216x704 at 30 FPS

## v0.9.7 -- Major Scale-Up (May 6, 2025)

- **Parameters:** 13B (6.5x increase from 2B) -- the largest LTX-Video model
- **New features:**
  - [[video-vae|Spatial upscaler]] (2x resolution in latent space)
  - FP8 quantization for memory efficiency
  - ICLoRA conditioning adapters (depth, pose, canny, detailer)
  - Mix mode (multi-scale workflow combining dev + distilled)
  - LoRA128 adapter (convert dev to distilled behavior)
  - Multi-condition generation
  - LoRA fine-tuning support
- **Multiscale rendering:** Drafts at lower detail, then progressively refines; 30x faster than comparable-sized models
- **Video duration:** Up to 60 seconds
- **New Diffusers API:** `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`, `LTXVideoCondition`
- **12 new model variants** released (see [[ltx-video-model-variants]])

## v0.9.8 -- Ecosystem Maturation (Mid-July 2025)

- **Parameters:** 13B + 2B variants
- **New features:**
  - IC-LoRA control models (depth, pose, canny edge)
  - Detailer model for enhanced fine details
  - New 2B distilled checkpoints (both standard and FP8)
  - Video-to-video generation
  - Multi-condition generation refined
  - Temporal upscaler added
- **Improvements:** Better prompt understanding, slightly faster than 0.9.7
- **7 new model variants** released
- **Note:** Many 0.9.8 model pages on Hugging Face are gated (require authentication)
- **Known issue:** Some users reported plastic-looking skin in certain generations (GitHub Issue #230)

## Key Evolution Milestones

| Milestone | Version | Significance |
|-----------|---------|--------------|
| Real-time generation | 0.9.0 | First DiT model to achieve real-time |
| Quality refinement | 0.9.1 | Physics, motion, visual quality leap |
| Commercial license | 0.9.5 | OpenRail-M enables commercial use |
| Distillation | 0.9.6 | 15x speed, 8-step sampling breakthrough |
| 13B scale-up | 0.9.7 | Major quality jump, FP8, upscalers |
| Full ecosystem | 0.9.8 | IC-LoRA, detailer, 2B+13B variants |

## What Came Next

LTX-Video was succeeded by [[ltx-2-overview]] (October 2025), which introduced joint audio-video generation, and [[ltx-2.3-model]] (March 2026), which scaled to 22B parameters. See [[ltx-2-version-history]] for the LTX-2 release timeline.

## References

- [[ltx-video-overview]] -- Model family overview
- [[ltx-video-changelog]] -- Detailed changes between versions
- [[ltx-video-model-variants]] -- Complete model inventory
- [[ltx-video-capabilities]] -- Capability matrix
- [[ltx-video-to-ltx-2]] -- Evolution to LTX-2/2.3
- [[comfyui-ltx-integration-overview]] -- ComfyUI integration timeline
- [[ltx-awesome-resources]] -- Community resource directory
