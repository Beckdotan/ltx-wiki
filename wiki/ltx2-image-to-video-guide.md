---
title: LTX-2 Image-to-Video Guide
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-ltx2-image-to-video-guide.md
tags:
  - ltx2
  - image-to-video
  - usage-guide
  - animation
---

# LTX-2 Image-to-Video Guide

[[ltx2-open-source-overview|LTX-2]] generates synchronized video and audio from a single source image, ideal for animating static frames while maintaining character consistency.

## Prerequisites

- [[ltx2-comfyui-integration|ComfyUI with LTX-2 nodes]] installed, or [[ltx2-diffusers-pipeline|PyTorch API]]
- Minimum 32GB VRAM GPU (see [[ltx2-system-requirements]])
- Source image (PNG, JPG, or WebP)

## Resolution Options

| Resolution | Aspect Ratio | Notes |
|-----------|-------------|-------|
| 1920x1080 | 16:9 | Full HD landscape |
| 1080x1920 | 9:16 | Portrait |
| 1280x720 | 16:9 | HD, lower VRAM |
| 768x512 | 3:2 | Fast iteration |
| 640x640 | 1:1 | Square |

All dimensions must be divisible by 32.

## Frame Settings

| Setting | Value |
|---------|-------|
| Maximum frames | 257 (~10 seconds at 25fps) |
| Recommended | 121-161 frames |
| Fast iteration | 65-97 frames |
| Pattern | Must follow 8n+1 |

## Frame Rate Options

| FPS | Use Case |
|-----|----------|
| 24 | Cinematic |
| 25 | Standard default |
| 30 | Smooth motion |
| 48-60 | High-speed content |

## Sampling Configuration

| Parameter | Distilled | Full |
|-----------|-----------|------|
| Steps | 8 | 20-50 |
| CFG | 3.0-4.0 recommended | 2.0-5.0 |
| Sampler | euler (fast testing) | dpmpp_2m or res_2s |

## Prompt Structure for Image Animation

Effective prompts for image-to-video should include:

- **Motion description** -- What should move and how
- **Camera movement** -- Shot type and direction
- **Character actions** -- Reactions and gestures
- **Audio elements** -- Dialogue, music, ambient sounds

Example:
```
"Camera slowly zooms in while the character turns their head to
look at the sunset. The wind gently moves their hair. Birds chirp
in the background as warm golden light fills the scene."
```

## First-to-Last Frame (LTX-2.3)

LTX-2.3 models support providing both a first frame and a last frame image for controlled interpolation between two keyframes:

- `image_uri` -- First frame of the video
- `last_frame_uri` -- Target end frame (LTX-2.3 only)

## Two-Stage Workflow

1. **Stage 1:** Base generation at half resolution for efficient testing
2. **Stage 2:** LTXVUpscale node applies 2x upscaling for final quality

Tiled decoding minimizes memory usage during final decode.

## Output Configuration

| Setting | Options |
|---------|---------|
| Format | MP4 (default), MOV, WebM |
| Codec | H.264 (compatibility), H.265 (smaller files) |
| Audio | Automatically embedded from audio decoder |

## LoRA Customization

- Load LoRA adapters for style, motion, and character consistency
- Distilled LoRA: strength 0.6 recommended for Stage 2 with full model
- For structural control via depth, pose, or edge signals, see [[ltx2-ic-lora-guide]]

## See Also

- [[ltx2-text-to-video-guide]]
- [[ltx2-ic-lora-guide]]
- [[ltx2-comfyui-integration]]
- [[ltx2-comfyui-nodes-reference]]
- [[ltx2-diffusers-pipeline]]
