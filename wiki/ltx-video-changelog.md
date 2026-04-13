---
title: LTX Video Changelog
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-version-changelog-improvements.md
  - raw/ltx-video-version-changes-changelog.md
  - raw/ltx-video-capabilities-per-version.md
tags:
  - ltx-video
  - changelog
  - improvements
  - versions
---
# LTX Video Changelog

Detailed version-by-version changes for [[ltx-video-overview|LTX-Video]]. For a high-level timeline, see [[ltx-video-versions]].

## v0.9.0 -- Initial Release (November 2024)

**New:**
- First public release of LTX-Video by [[lightricks-company]]
- 2B parameter [[diffusion-transformer]]-based video generation model
- Novel [[video-vae|Video-VAE]] with 1:192 compression ratio (32x32x8, 128 channels)
- Denoising decoder (VAE performs final diffusion step in pixel space)
- Reconstruction GAN loss for VAE training
- Video DWT loss for high-frequency detail preservation
- T5-XXL text encoder with cross-attention conditioning
- RoPE with normalized fractional coordinates
- QK normalization with RMSNorm
- Per-token timestep conditioning for image-to-video
- Rectified-flow training with velocity prediction
- Multi-resolution training
- Text-to-video and image-to-video capabilities
- 768x512 at 24 FPS, up to 257 frames
- Faster-than-real-time generation (2s for 5s video on H100)
- Research paper published: arXiv:2501.00103
- Open-source on GitHub and Hugging Face
- Diffusers integration (`LTXPipeline`, `LTXImageToVideoPipeline`)

---

## v0.9.0 to v0.9.1 (November 2024 to December 2024)

**Improved:**
- Smoother motion with more natural and fluid video movement
- Improved physics with better physical plausibility in generated scenes
- Cleaner visuals with reduced artifacts and visual noise
- Significant boost to image-to-video quality
- Training-free video enhancement capability added
- Resolution upgraded from 768x512 to 1216x704 at 30 FPS

**Unchanged:**
- Same 2B parameter architecture
- Same inference requirements and API

---

## v0.9.1 to v0.9.5 (December 2024 to March 5, 2025)

**New:**
- Multi-keyframe conditional support (generate from first, last, or any intermediate frame)
- Video extension with greater flexibility (forward and backward)
- Commercial license introduced (OpenRail-M, replacing RAIL-M)
- [[comfyui-ltx-integration-overview]] with day-1 support
- LoRA support added
- Training datasets released (Squish, Cakeify)

**Improved:**
- Resolution and quality enhancements with reduced artifacts and smoother visuals
- Longer video generation support
- Better prompt understanding and adherence

**Unchanged:**
- Same 2B parameter architecture
- 1216x704 native resolution, 30 FPS

---

## v0.9.5 to v0.9.6 (March 2025 to April 2025)

**New:**
- First dev/distilled model split
  - `ltxv-2b-0.9.6-dev` -- Full-quality for final outputs
  - `ltxv-2b-0.9.6-distilled` -- 15x faster, 8 diffusion steps, no CFG/STG needed
- Real-time video generation achieved on consumer hardware

**Improved:**
- Enhanced prompt adherence
- Smoother motion
- Finer details with more realistic and coherent outputs
- Ultra-fast inference (8 diffusion steps for distilled model)

**Unchanged:**
- 2B parameters
- Same base [[ltx-video-architecture|architecture]]

---

## v0.9.6 to v0.9.7 (April 2025 to May 6, 2025)

This was the largest single update in LTX-Video's history.

**New:**
- **13B parameter model** -- 6.5x increase from 2B, massive quality improvement
- **Multiscale rendering** -- drafts at lower detail first, progressively adds structure, lighting, micro-motion; for 1080p: renders at 960x540 then upscales 2x; 30x faster than comparable-sized models
- **FP8 quantization** -- official quantized models with ~50% memory reduction
- **ICLoRA conditioning adapters:** depth, pose, canny edge, detailer
- **Spatial upscaler** -- 2x resolution increase in latent space
- **Mix mode** -- multi-scale workflow combining dev and distilled models
- **LoRA128 adapter** -- convert dev to distilled behavior
- **Multi-condition generation** -- condition on multiple images/videos at different frame positions
- **LoRA fine-tuning support**
- **New Diffusers API:** `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`, `LTXVideoCondition`

**Improved:**
- Breakthrough prompt adherence and physical understanding
- Enhanced video length up to 60 seconds
- Simplified [[comfyui-ltx-integration-overview]] flows (new nodes for img2vid, img2vid+extension, img2vid+keyframes)

**Models added:** 12 new variants (see [[ltx-video-model-variants]])

---

## v0.9.7 to v0.9.8 (May 2025 to Mid-July 2025)

**New:**
- **IC-LoRA control models** officially launched (depth, pose, canny edge)
- **Detailer model** for enhancing fine details (LTX-Video-ICLoRA-detailer-13B-0.9.8)
- **New 2B distilled checkpoints** -- both standard and FP8 versions
- **Video-to-video generation** -- conditioning on video segments
- **Multi-condition generation** refined
- **Temporal upscaler** -- frame interpolation added
- **2B distilled-fp8** -- most efficient variant ever (~4.46 GB)

**Improved:**
- Better prompt understanding and detail generation
- Slightly faster than 0.9.7
- Extended video generation confirmed at 60 seconds

**Pipeline change:** `LTXConditionPipeline` now supports multi-condition workflows

**Models added:** 7 new variants (see [[ltx-video-model-variants]])

**Known issues:**
- Some users reported plastic-looking skin in certain generations (GitHub Issue #230)
- Many 0.9.8 model pages on Hugging Face are gated

---

## Architecture Constants (Unchanged Across All Versions)

These fundamental design choices remained constant throughout the LTX-Video line:

- [[diffusion-transformer]]-based architecture (built on Pixart-alpha)
- [[video-vae|Video-VAE]] with 32x32x8 compression, 128 channels
- T5-XXL text encoder
- RoPE positional embeddings
- Cross-attention text conditioning
- Per-token timestep conditioning for I2V
- Rectified-flow training framework
- Resolution divisible by 32
- Frame count divisible by 8 + 1
- Maximum ~257 frames
- 24-30 FPS output

## References

- [[ltx-video-overview]] -- Model family overview
- [[ltx-video-versions]] -- Version timeline
- [[ltx-video-capabilities]] -- Capability matrix
- [[ltx-video-model-variants]] -- Complete model inventory
- [[ltx-video-architecture]] -- Core architecture details
