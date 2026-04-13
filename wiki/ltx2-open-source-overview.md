---
title: LTX-2 Open Source Model Overview
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-ltx2-open-source-overview.md
tags:
  - ltx2
  - open-source
  - model-overview
  - lightricks
---

# LTX-2 Open Source Model Overview

LTX-2.3 is a DiT-based audio-video foundation model developed by Lightricks. It generates synchronized video and audio within a single model, evolving from the original LTX-Video into a multimodal generation system.

## Key Capabilities

- Produces up to approximately 20 seconds of synchronized audio-video content
- Supports high resolutions and frame rates depending on hardware
- Generates dialogue, lip movement, and ambient audio in a single pass (no separate alignment step)
- Renders dynamic scenes with stable motion, consistent character identity, and strong frame-to-frame coherence
- LoRA-based customization, camera-aware motion logic, and multimodal inputs (text, image, video, audio, depth maps)
- Compact latent space and step-distilled architecture enabling execution on consumer-grade GPUs

## Model Variants

| Model | Parameters | Description |
|-------|-----------|-------------|
| LTX-2.3 (latest) | ~20B | Audio-video sync, portrait support |
| LTX-2 | ~19B | Audio-video model, landscape orientation |
| LTX-Video 0.9.8 | 13B / 2B | Original video-only model |

## Open Source vs API

| Feature | Open Source | Official API |
|---------|-----------|-------------|
| Self-hosted | Yes | No (cloud) |
| Full model access | Yes | No |
| Custom fine-tuning | Yes | No |
| LoRA / IC-LoRA | Yes | Via API features |
| Audio generation | Yes | Yes |
| No GPU required | No | Yes |
| Production SLA | No | Yes |
| Managed infrastructure | No | Yes |

## Getting Started Options

1. **[[ltx2-comfyui-integration|ComfyUI]]** -- Recommended for most users; visual node-based workflow
2. **LTX Desktop** -- Standalone application with GUI
3. **[[ltx2-diffusers-pipeline|PyTorch API]]** -- Full programmatic control via `ltx-pipelines`
4. **[[ltx2-diffusers-pipeline|Diffusers]]** -- Simplified Python API via Hugging Face

## Resources

- Research paper: https://arxiv.org/abs/2601.03233
- API Developer Console: https://console.ltx.video/playground/
- GitHub (LTX-2): https://github.com/Lightricks/LTX-2
- Hugging Face (LTX-2): https://huggingface.co/Lightricks/LTX-2
- Hugging Face (LTX-2.3): https://huggingface.co/Lightricks/LTX-2.3

## See Also

- [[ltx2-system-requirements]]
- [[ltx2-comfyui-integration]]
- [[ltx2-diffusers-pipeline]]
- [[ltx2-training]]
