---
title: LTX-2 Text-to-Video Guide
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-ltx2-text-to-video-guide.md
tags:
  - ltx2
  - text-to-video
  - usage-guide
  - prompting
---

# LTX-2 Text-to-Video Guide

Generate synchronized video and audio entirely from text prompts using [[ltx2-open-source-overview|LTX-2 / LTX-2.3]]. No reference images or fixed frames required.

## Prerequisites

- [[ltx2-comfyui-integration|ComfyUI with LTX-2 nodes]] installed, or [[ltx2-diffusers-pipeline|PyTorch API]]
- Minimum 32GB VRAM GPU (see [[ltx2-system-requirements]])

## Model Selection

| Model | Quality | Speed | Steps |
|-------|---------|-------|-------|
| Full checkpoint | Higher quality | Slower | 20-50 |
| Distilled checkpoint | Slightly reduced | Faster | 4-8 |

The Gemma 3 text encoder processes prompts automatically in both cases.

## Resolution Options

| Resolution | Aspect Ratio |
|-----------|-------------|
| 768x512 | 3:2 |
| 512x768 | 2:3 |
| 704x512 | 4:3 |
| 512x704 | 3:4 |
| 640x640 | 1:1 |

Higher resolutions require more VRAM. All dimensions must be divisible by 32.

## Video Parameters

### Frame Count

- Maximum: 257 frames (~10 seconds at 25fps)
- Recommended: 121-161 frames
- Quick iterations: 65-97 frames
- Must follow the 8n+1 pattern

### Frame Rate

- 25 fps -- Standard
- 30 fps -- Fast action
- 24 fps -- Cinematic feel
- Must remain consistent across all workflow nodes

## Prompt Engineering

Effective prompts include four key elements:

1. **Scene description** -- Environment, lighting, atmosphere
2. **Character details** -- Appearance, behavior, clothing
3. **Camera motion** -- Shot type, movement direction
4. **Audio cues** -- Dialogue (in quotes), music, ambient sounds

Longer, descriptive prompts consistently produce better motion, coherence, and audio alignment.

## Sampling Configuration

| Parameter | Distilled | Full |
|-----------|-----------|------|
| Steps | 4-8 | 20-50 |
| CFG Scale | 1.0 | 2.0-5.0 (3.0-3.5 recommended) |
| Sampler | euler | dpmpp_2m |
| Seed | Fixed for reproducibility, random for variety | Same |

## Two-Stage Generation

1. **Stage 1:** Base generation at specified resolution
2. **Stage 2:** Upscale pass (typically 2x) to refine details

This approach is more efficient than generating at full resolution directly.

## Audio/Video Decoding

Audio and video generate in a single pass but are decoded separately:

- Tiled decoding minimizes VRAM usage
- Audio and video merge during final processing for synchronized output

## LoRA Integration

- Load LoRA adapters for style, motion, and character customization
- Recommended strength: 0.6 for distilled LoRA in Stage 2
- Full model workflows support additional LoRA loaders

For IC-LoRA (structural control), see [[ltx2-ic-lora-guide]].

## See Also

- [[ltx2-image-to-video-guide]]
- [[ltx2-ic-lora-guide]]
- [[ltx2-comfyui-integration]]
- [[ltx2-comfyui-nodes-reference]]
- [[ltx2-diffusers-pipeline]]
