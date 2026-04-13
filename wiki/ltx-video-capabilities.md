---
title: LTX Video Capabilities
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-capabilities-per-version.md
  - raw/ltx-video-capabilities-resolution-fps.md
  - raw/ltx-video-overview-all-versions.md
tags:
  - ltx-video
  - capabilities
  - resolution
  - fps
  - generation-modes
---
# LTX Video Capabilities

This page documents the capabilities of [[ltx-video-overview|LTX-Video]] across all versions, including generation modes, resolution specs, inference speeds, and feature introductions.

## Generation Mode Matrix

| Capability | v0.9.0 | v0.9.1 | v0.9.5 | v0.9.6 | v0.9.7 | v0.9.8 |
|------------|--------|--------|--------|--------|--------|--------|
| Text-to-video | Yes | Yes | Yes | Yes | Yes | Yes |
| Image-to-video | Yes | Improved | Improved | Yes | Yes | Yes |
| Video-to-video | -- | -- | -- | -- | Yes | Yes |
| Multi-keyframe conditioning | -- | -- | Yes | Yes | Yes | Yes |
| Video extension (fwd/bwd) | -- | -- | Partial | Yes | Yes | Yes |
| IC-LoRA control | -- | -- | -- | -- | -- | Yes |
| LoRA fine-tuning | -- | -- | -- | -- | Yes | Yes |
| Spatial upscaling (2x) | -- | -- | -- | -- | Yes | Yes |
| Temporal upscaling | -- | -- | -- | -- | -- | Yes |
| Training-free enhancement | -- | Yes | -- | -- | -- | -- |

## Resolution and Frame Specifications

| Spec | v0.9.0 | v0.9.1 | v0.9.5 | v0.9.6 | v0.9.7 | v0.9.8 |
|------|--------|--------|--------|--------|--------|--------|
| Native resolution | 768x512 | 1216x704 | 1216x704 | 1216x704 | 1216x704 | 1216x704 |
| Max recommended | 768x512 | 720x1280 | 720x1280 | 720x1280 | 720x1280 | 720x1280 |
| FPS | 24 | 30 | 24-30 | 30 | 30 | 30 |
| Max frames | 257 | 257 | 257 | 257 | 257 | 257 |
| Max duration | ~5 sec | ~8.5 sec | ~8.5 sec | ~8.5 sec | ~60 sec | ~60 sec |

### Resolution Constraints

- **Divisibility:** Height and width must be divisible by 32 (due to [[ltx-video-vae]] spatial compression ratio of 32)
- **Frame count:** Must follow the 8n+1 pattern (valid: 9, 17, 25, 33, ..., 257)
- **Auto-padding:** Non-compliant inputs are padded with -1, then cropped to desired size

### Common Resolutions Used

- 768x512 (paper benchmark)
- 704x480
- 832x480
- 704x1216 (portrait)
- 1216x704 (landscape, native)
- 720x1280 (HD, max recommended)

## Generation Modes Detail

### Text-to-Video (T2V)

Available in all versions. Generate video entirely from a text prompt. Supports negative prompts for quality improvement. Best results with elaborate, detailed English prompts describing subject, action, environment, lighting, and camera angle.

### Image-to-Video (I2V)

Available in all versions. Animate a still image into a video sequence. Uses timestep-based conditioning (no extra parameters needed). The conditioning image is encoded as the first frame by the causal [[ltx-video-vae]]. Conditioning strength is adjustable.

### Video-to-Video (V2V)

Available from v0.9.7+. Transform or extend existing video based on a new prompt. Uses `denoise_strength` parameter to control how much of the original video to preserve.

### Multi-Condition Generation (v0.9.5+)

Condition on multiple images and/or videos at specified frame positions. Each condition has a `frame_index` specifying its position in the output. Adjustable `conditioning_strength` per condition (default: 1.0). Example: first frame from Image A, frame 40 from Image B.

### Controlled Generation via ICLoRA (v0.9.7+)

Available through [[ltx-video-iclora]] adapters:

- **Depth conditioning** -- Generate video following a depth map sequence
- **Pose conditioning** -- Generate video following pose keypoints
- **Canny edge conditioning** -- Generate video following edge maps
- **Detailer** -- Enhanced fine detail generation

### Upscaling (v0.9.7+)

- **[[ltx-video-spatial-upscaler]]** -- 2x resolution increase in latent space
- **Temporal upscaler** -- Frame interpolation for smoother video (v0.9.8+)
- Both operate on latent representations for efficiency

## Inference Step Recommendations

| Version/Variant | Recommended Steps | CFG Required | STG Required |
|-----------------|-------------------|--------------|--------------|
| v0.9.0 (2B) | 40-50 | Yes | Yes |
| v0.9.1 (2B) | 50 | Yes | Yes |
| v0.9.5 (2B) | 50 | Yes | Yes |
| v0.9.6-dev (2B) | ~30 | Yes | Yes |
| v0.9.6-distilled (2B) | 8 | No | No |
| v0.9.7-dev (13B) | 30 | Yes | Yes |
| v0.9.7-distilled (13B) | 8 | No | No |
| v0.9.8-dev (13B) | 30 | Yes | Yes |
| v0.9.8-distilled (13B) | 8 | No | No |

## Generation Speed

| Version | Speed (H100) | Notes |
|---------|-------------|-------|
| v0.9.0 | 5 sec video in 2 sec (768x512, 24fps) | Faster than real-time |
| v0.9.1 | ~Same as 0.9.0 | Maintained speed |
| v0.9.5 | ~Same | Real-time at 768x512 |
| v0.9.6-distilled | 15x faster than dev | Real-time on H100 |
| v0.9.7 | 30x faster than comparable models | Multiscale rendering |
| v0.9.8 | Slightly faster than 0.9.7 | Incremental improvement |

### Speed by Model Variant

| Model | Steps | Speed Characteristic |
|-------|-------|---------------------|
| 2B dev | 20-50 | Faster-than-real-time |
| 2B distilled | 7-10 | 15x faster than dev, real-time |
| 13B dev | 20-30 | Slower, highest quality |
| 13B distilled | 7 | Faster, good quality |
| 13B mix | varies | Balanced speed-quality |

## Model Size Progression

| Version | Parameters | Variants |
|---------|-----------|----------|
| v0.9.0 | 2B (1.9B transformer) | Single model |
| v0.9.1 | 2B | Single model |
| v0.9.5 | 2B | Single model |
| v0.9.6 | 2B | Dev + Distilled (first time) |
| v0.9.7 | 13B (new) + 2B | Dev, Distilled, FP8, LoRA, Mix |
| v0.9.8 | 13B + 2B | Dev, Distilled, FP8, IC-LoRA detailer |

## References

- [[ltx-video-overview]] -- Model family overview
- [[ltx-video-versions]] -- Version timeline
- [[ltx-video-model-variants]] -- Full model inventory
- [[ltx-video-architecture]] -- Technical architecture details
