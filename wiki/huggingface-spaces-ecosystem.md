---
title: HuggingFace Spaces Ecosystem
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-huggingface-spaces-ecosystem.md
  - raw/community-adoption-metrics.md
tags:
  - huggingface
  - spaces
  - community
  - deployment
  - adoption
---
# HuggingFace Spaces Ecosystem

The HuggingFace Spaces ecosystem around [[ltx-video-overview\|LTX Video]] is one of the most active indicators of community adoption. As of April 2026, there are 468+ Spaces related to LTX-Video and 26+ Spaces specifically for LTX-2/2.3.

## Official Lightricks Spaces

| Space | Description | Likes | Status |
|-------|-------------|-------|--------|
| LTX Video Fast | Ultra-fast LTX 0.9.8 13B distilled | 1,490 | Paused |
| LTX 2.3 Distilled | Cinematic videos with audio from text/images | 299 | Running |
| LTX-2 Video Fast | Fast high-quality video with audio | 216 | Paused |
| LTX-2 | High quality video with audio generation | 78 | Paused |
| Control Canny LTX Video Fast | Fast controlled video gen with 0.9.7 distilled | 32 | Paused |

## Top Community Spaces

| Space | Creator | Description | Likes | Status |
|-------|---------|-------------|-------|--------|
| LTX-2 Video [Turbo] | alexnasa | Fast generation with Flash Attention 3 | 398 | Running on Zero |
| LTX 2.3 Sync | linoyts | Portrait animation and lipsync | 120 | Running on Zero |
| LTX 2.3 First-Last Frame | linoyts | Frame-controlled generation | 96 | Running on Zero |
| LTX-2-LoRAs-Camera-Control-Dolly | prithivMLmods | Camera motion control | 58 | Running on Zero |
| Ltx2 Audio To Video | multimodalart | Lip-sync from audio | 49 | Running on Zero |
| LTX-2.3 Video [Turbo] | ZeroCollabs | Free ZeroGPU generation | 40 | Running on Zero |
| LTX-2 First Last Frame | linoyts | Frame control (LTX-2) | 37 | Featured |
| LTX 2.3 Distilled | cbensimon | Video from text/images/audio | 13 | Running |
| Ltx-2.3 FFLFwith Lora | rahul7star | Animated videos with LoRA | 7 | Running |

## Space Categories

### Video Generation (Standard)
Standard text-to-video and image-to-video Spaces implementing the [[lightricks-company]] official pipelines with minor modifications.

### Audio-Video Generation
Spaces leveraging [[ltx-2-overview\|LTX-2]]/2.3's joint audio-video capability, including text-to-audio-video and image-to-audio-video.

### Lip-Sync / Portrait Animation
Specialized Spaces for generating lip-synced talking head videos from audio input plus a reference image. The linoyts "Sync" Space is the most popular in this category.

### Camera/Motion Control
Spaces implementing camera control LoRAs and motion tracking for guided video generation (e.g., dolly, crane, tracking shots).

### First-Last Frame Control
Spaces that enable specifying start and end frames for controlled video generation, addressing a common [[community-feature-requests\|feature request]].

### Turbo/Optimized
Community-optimized Spaces using Flash Attention 3, SageAttention 2, and other techniques for faster inference.

## Deployment Pattern

Most community Spaces run on HuggingFace's "Zero" tier (free GPU access), making LTX-Video accessible without any hardware requirements. This significantly lowers the barrier to entry for users who want to experiment with video generation.

## Space Health

Many older LTX-Video (0.9.x) Spaces show runtime errors, paused status, or build errors. This reflects the rapid evolution of the model ecosystem -- older Spaces become incompatible as the underlying libraries and model versions change.

## References

- [[adoption-metrics]]
- [[community-feedback]]
- [[creative-showcases]]
