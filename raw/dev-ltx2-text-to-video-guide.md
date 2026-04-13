# LTX-2 Text-to-Video Usage Guide

**Source:** https://docs.ltx.video/open-source-model/usage-guides/text-to-video
**Fetched:** 2026-04-13

---

## Overview

Generate synchronized video and audio entirely from text prompts using LTX-2 / LTX-2.3. No reference images or fixed frames required.

---

## Prerequisites
- ComfyUI with LTX-2 nodes installed (or PyTorch API)
- Minimum 32GB VRAM GPU

---

## Model Selection

| Model | Quality | Speed | Steps |
|-------|---------|-------|-------|
| Full checkpoint | Higher quality | Slower | 20-50 |
| Distilled checkpoint | Slightly reduced | Faster | 4-8 |

Text Encoder (Gemma 3) processes prompts automatically in both cases.

---

## Resolution Options

| Resolution | Aspect Ratio |
|-----------|-------------|
| 768x512 | 3:2 |
| 512x768 | 2:3 |
| 704x512 | 4:3 |
| 512x704 | 3:4 |
| 640x640 | 1:1 |

Higher resolutions require more VRAM.

---

## Video Parameters

### Frame Count
- Maximum: 257 frames (~10 seconds at 25fps)
- Recommended: 121-161 frames
- Quick iterations: 65-97 frames

### Frame Rate
- 25 fps - Standard
- 30 fps - Fast action
- 24 fps - Cinematic feel
- Must remain consistent across all workflow nodes

---

## Prompt Engineering

Effective prompts include four key elements:
1. **Scene description** - Environment, lighting, atmosphere
2. **Character details** - Appearance, behavior, clothing
3. **Camera motion** - Shot type, movement direction
4. **Audio cues** - Dialogue (in quotes), music, ambient sounds

"Longer, descriptive prompts consistently produce better motion, coherence, and audio alignment."

---

## Sampling Configuration

| Parameter | Distilled | Full |
|-----------|-----------|------|
| Steps | 4-8 | 20-50 |
| CFG Scale | 1.0 | 2.0-5.0 (3.0-3.5 recommended) |
| Sampler | euler | dpmpp_2m |
| Seed | Fixed for repro, random for variety | Same |

---

## Two-Stage Generation

1. **Stage 1:** Base generation at specified resolution
2. **Stage 2:** Upscale pass (typically 2x) to refine details

This approach is more efficient than generating at full resolution directly.

---

## Audio/Video Decoding

Audio and video generate in a single pass but are decoded separately:
- Tiled decoding minimizes VRAM usage
- Audio and video merge during final processing for synchronized output

---

## Advanced Options

### LoRA Integration
- Load LoRA adapters for style, motion, and character customization
- Recommended strength: 0.6 for distilled LoRA in Stage 2
- Full model workflows support additional LoRA loaders

---

## Related Links

- **Text-to-Video Guide:** https://docs.ltx.video/open-source-model/usage-guides/text-to-video
- **Prompting Guide:** https://docs.ltx.video/api-documentation/prompting-guide
- **Image-to-Video Guide:** https://docs.ltx.video/open-source-model/usage-guides/image-to-video
