---
title: LTX Video Model Variants
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-all-versions-overview.md
  - raw/ltx-video-overview-all-versions.md
  - raw/ltx-video-huggingface-model-cards.md
  - raw/dev-model-versions-and-weights.md
  - raw/model-ltx-video-versions-huggingface.md
tags:
  - ltx-video
  - model-variants
  - weights
  - distilled
  - fp8
  - iclora
---
# LTX Video Model Variants

This page provides a complete inventory of all [[ltx-video-overview|LTX-Video]] model variants, organized by version and type. For download locations, see [[ltx-video-huggingface]].

## Variant Types Explained

| Type | Description |
|------|-------------|
| **Dev** | Full-quality model; highest output quality, requires more VRAM and inference steps (~30 steps, CFG+STG) |
| **Distilled** | Knowledge-distilled variant; 15x faster, 8 inference steps, no CFG/STG guidance required |
| **FP8** | Float 8-bit quantized; ~50% memory reduction vs BF16 with minimal quality loss |
| **Mix** | Multi-scale workflow combining dev + distilled for balanced speed/quality |
| **ICLoRA** | Image-conditioned LoRA adapters for controlled generation (depth, pose, canny, detailer) |
| **LoRA128** | Rank-128 LoRA adapter to convert dev model into distilled behavior |
| **Upscaler** | Auxiliary models for spatial (2x resolution) or temporal (frame interpolation) upscaling |

## Complete Model Inventory

### v0.9.8 Models (Latest)

#### 13B Models

| Model ID | Size | Type | File Size | Key Feature |
|----------|------|------|-----------|-------------|
| ltxv-13b-0.9.8-dev | 13B | Dev | ~28.6 GB | Highest quality, latest version |
| ltxv-13b-0.9.8-distilled | 13B | Distilled | ~28.6 GB | Fast inference, 8 steps |
| ltxv-13b-0.9.8-mix | 13B | Mix | -- | Balanced speed-quality, multi-scale rendering |
| ltxv-13b-0.9.8-fp8 | 13B | FP8 | ~15.7 GB | Memory efficient |
| ltxv-13b-0.9.8-distilled-fp8 | 13B | Distilled+FP8 | ~15.7 GB | Fast + memory efficient |

#### 2B Models

| Model ID | Size | Type | File Size | Key Feature |
|----------|------|------|-----------|-------------|
| ltxv-2b-0.9.8-distilled | 2B | Distilled | ~6.34 GB | Lightweight, fast |
| ltxv-2b-0.9.8-distilled-fp8 | 2B | Distilled+FP8 | ~4.46 GB | Most efficient variant overall |

#### Auxiliary Models (0.9.8)

| Model ID | Type | Purpose |
|----------|------|---------|
| ltxv-spatial-upscaler-0.9.8 | Upscaler | 2x spatial resolution increase |
| ltxv-temporal-upscaler-0.9.8 | Upscaler | Frame interpolation |
| ICLoRA Depth (0.9.8) | Control | Depth map conditioning |
| ICLoRA Pose (0.9.8) | Control | Pose keypoint conditioning |
| ICLoRA Canny (0.9.8) | Control | Canny edge conditioning |
| ICLoRA Detailer (0.9.8) | Control | Enhanced fine detail generation |

### v0.9.7 Models

#### Core Models

| Model ID | Size | Type | File Size | Key Feature |
|----------|------|------|-----------|-------------|
| ltxv-13b-0.9.7-dev | 13B | Dev | ~28.6 GB | First 13B release, highest quality |
| ltxv-13b-0.9.7-distilled | 13B | Distilled | ~28.6 GB | Fast, 7 inference steps |
| ltxv-13b-0.9.7-fp8 | 13B | FP8 | ~15.7 GB | Memory efficient |
| ltxv-13b-0.9.7-distilled-fp8 | 13B | Distilled+FP8 | ~15.7 GB | Fast + memory efficient |
| ltxv-13b-0.9.7-mix | 13B | Mix | -- | Multi-scale workflow |
| ltxv-13b-0.9.7-distilled-lora128 | LoRA | Adapter | ~1.33 GB | Dev-to-distilled conversion |

#### ICLoRA Adapters (0.9.7)

| Model ID | Conditioning Type |
|----------|------------------|
| ltxv-13b-0.9.7-ICLoRA-depth | Depth map |
| ltxv-13b-0.9.7-ICLoRA-pose | Pose keypoints |
| ltxv-13b-0.9.7-ICLoRA-canny | Canny edges |
| ltxv-13b-0.9.7-ICLoRA-detailer | Enhanced details |

#### Auxiliary Models (0.9.7)

| Model ID | Type | Purpose |
|----------|------|---------|
| ltxv-spatial-upscaler-0.9.7 | Upscaler | 2x spatial resolution increase |
| ltxv-temporal-upscaler-0.9.7 | Upscaler | Frame interpolation |

### v0.9.6 Models

| Model ID | Size | Type | File Size | Key Feature |
|----------|------|------|-----------|-------------|
| ltxv-2b-0.9.6-dev | 2B | Dev | ~6.34 GB | Good quality, low VRAM |
| ltxv-2b-0.9.6-distilled | 2B | Distilled | ~6.34 GB | 15x faster, real-time capable |

### v0.9.5 and Earlier

| Model ID | Size | Notes |
|----------|------|-------|
| LTX-Video-0.9.5 | 2B | Last single-variant 2B model |
| LTX-Video-0.9.1 | 2B | Quality refinement over 0.9.0 |
| LTX-Video (0.9.0) | 2B | Original release |

## VRAM Requirements Guide

| Model | Precision | Approx. VRAM |
|-------|-----------|-------------|
| 2B distilled-fp8 | FP8 | ~6-8 GB |
| 2B distilled | BF16 | ~8-10 GB |
| 13B distilled-fp8 | FP8 | ~16-20 GB |
| 13B dev-fp8 | FP8 | ~16-20 GB |
| 13B distilled | BF16 | ~30+ GB |
| 13B dev | BF16 | ~30+ GB |

Note: Diffusers supports `group_offloading` for ~10 GB VRAM usage and `fp8 layerwise weight-casting` for further memory reduction.

## Choosing a Variant

- **Highest quality:** `ltxv-13b-0.9.8-dev` (requires most VRAM)
- **Best speed/quality balance:** `ltxv-13b-0.9.8-mix` (multi-scale rendering)
- **Fastest 13B:** `ltxv-13b-0.9.8-distilled-fp8` (8 steps, quantized)
- **Most efficient overall:** `ltxv-2b-0.9.8-distilled-fp8` (~4.46 GB, runs on consumer GPUs)
- **Controlled generation:** Use ICLoRA adapters on top of dev/distilled models
- **Resolution improvement:** Chain with spatial upscaler for 2x resolution boost

## Community Ecosystem

As of the data collection date, the LTX-Video ecosystem on [[ltx-video-huggingface|Hugging Face]] includes:

- 567,927 monthly downloads
- 24 model adapters
- 25 fine-tunes
- 16 community quantizations (including GGUF, AWQ)
- 100+ community Spaces

## References

- [[ltx-video-overview]] -- Model family overview
- [[ltx-video-huggingface]] -- Download locations and integration
- [[ltx-video-versions]] -- Version timeline
- [[ltx-video-capabilities]] -- Capabilities per version
