---
title: Wan Video
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-wan-video.md
  - raw/open-source-comparison.md
  - raw/related-work-and-comparisons.md
tags:
  - competitor
  - alibaba
  - video-generation
  - open-source
  - moe
---

# Wan Video

Wan Video is Alibaba's open-source video generation model family, developed by the Tongyi Lab. It is widely regarded as one of the top open-source video generation models as of early 2026, particularly praised for its motion realism. The series has evolved rapidly through Wan 2.1, Wan 2.1-VACE, Wan 2.2, and Wan 2.2-S2V.

## Key Facts

- **Developer:** Alibaba (Tongyi Lab)
- **Parameters:** 1.3B (lightweight), 5B (mid), 14B (full)
- **Architecture:** Mixture of Experts (MoE) diffusion backbone
- **Max resolution:** 1080p native (Wan 2.2)
- **FPS:** 24 fps
- **Duration:** Up to 5 seconds per generation
- **License:** Apache 2.0
- **Downloads:** 6.9M+ on Hugging Face and ModelScope

## Version History

### Wan 2.1 (February 2025)
- Four models: 14B and 1.3B parameter variants
- Top of the VBench leaderboard at time of release
- First video generation model supporting text effects in Chinese and English
- Generates 5-second 480p video in under 4 minutes on an RTX 4090

### Wan 2.1-VACE (May 2025)
- Video All-in-one Creation and Editing model
- First open-source model providing unified video generation and editing
- Supports camera trajectory controls, subject locking, background stabilization

### Wan 2.2 (2025)
- Introduces Mixture of Experts (MoE) architecture with high-noise and low-noise expert models
- Full HD (1080p) output without upscaling
- Same VRAM footprint as Wan 2.1 on significantly more training data
- Variants: T2V-1.3B, T2V-A14B, I2V-5B, I2V-A14B, TI2V-5B

### Wan 2.2-S2V (August 2025)
- Speech-to-Video model for digital human creation
- Converts portrait photos into film-quality avatars capable of speaking, singing, and performing

## Capabilities

- Text-to-video, image-to-video, text+image-to-video (TI2V)
- Speech-to-video (S2V variant)
- Video editing and creation (VACE variant)
- Advanced camera trajectory controls (pans, zooms, focus pulls)
- Subject locking and background stabilization
- LoRA fine-tuning support
- Motion intensity control

## Strengths

- **Highest motion realism** among open-source models as of March 2026
- **MoE architecture** distributes denoising across specialized experts, scaling efficiently
- **Lowest entry barrier** -- 1.3B model runs on approximately 8 GB VRAM
- **Native 1080p** without upscaling (Wan 2.2)
- **Apache 2.0 license** permits unrestricted commercial use
- **Drop-in weight swap** from Wan 2.1 to 2.2 with no infrastructure changes

## Weaknesses

- **Slower generation** -- 10-12 minutes for 5 seconds at 720p on an H100
- **High VRAM for best quality** -- 14B model needs 60-80 GB VRAM
- **No native audio** -- video only
- **Shorter clip duration** -- limited to approximately 5 seconds per generation

## Comparison to LTX

In the [[open-source-video-generation-landscape]], Wan 2.2 leads in motion realism and has the lowest VRAM entry point. However, [[ltx-2-overview|LTX-2]] is dramatically faster (5-10x), generates native 4K with synchronized audio, and offers a broader product ecosystem (desktop app, studio, MCP integration). The [[related-work-and-comparisons|LTX-2 paper]] reports LTX-2 is approximately 18x faster than Wan 2.1 on an H100 and ranked higher on the Artificial Analysis benchmark.

## See Also

- [[open-source-video-generation-landscape]]
- [[hunyuan-video]]
- [[video-generation-architectures]]
