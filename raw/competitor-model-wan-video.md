# Wan Video (Alibaba) -- Model Competitor to LTX Video

## Overview

Wan Video is Alibaba's open-source video generation model family, developed by the Tongyi Lab. The series has rapidly evolved through multiple versions (Wan 2.1, Wan 2.1-VACE, Wan 2.2, Wan 2.2-S2V) and is widely regarded as one of the top open-source video generation models available.

## Versions and Evolution

### Wan 2.1 (February 2025)
- Four models released: 14B and 1.3B parameter variants
- Top of the VBench leaderboard at time of release
- First video generation model to support text effects in both Chinese and English
- Generates 5-second 480p video in under 4 minutes on an RTX 4090

### Wan 2.1-VACE (May 2025)
- Video All-in-one Creation and Editing model
- First open-source model providing a unified solution for various video generation and editing tasks
- Supports camera trajectory controls, subject locking, background stabilization

### Wan 2.2 (2025)
- Upgraded open-source model building on Wan 2.1
- Introduces Mixture of Experts (MoE) architecture with high-noise and low-noise expert models
- Supports full HD (1080p) output without upscaling
- Same VRAM footprint as Wan 2.1 but trained on significantly more data
- Available in T2V-1.3B, T2V-A14B, I2V-5B, I2V-A14B, and TI2V-5B variants

### Wan 2.2-S2V (August 2025)
- Speech-to-Video model for digital human creation
- Converts portrait photos into film-quality avatars capable of speaking, singing, and performing

## Technical Specifications

| Spec | Details |
|------|---------|
| **Parameters** | 1.3B (lightweight) / 5B (mid) / 14B (full) |
| **Architecture** | Mixture of Experts (MoE) diffusion backbone |
| **Resolution** | 480p, 720p, 1080p (Wan 2.2) |
| **FPS** | 24 fps |
| **Duration** | Up to 5 seconds (standard generation) |
| **VRAM (1.3B)** | ~8.19 GB minimum |
| **VRAM (5B / TI2V)** | ~10-24 GB |
| **VRAM (14B)** | ~60-80 GB |
| **License** | Apache 2.0 |
| **Inference Steps** | 27-40 (configurable) |

## Capabilities

- Text-to-video generation
- Image-to-video generation
- Text+Image-to-video generation (TI2V)
- Speech-to-video (S2V variant)
- Video editing and creation (VACE variant)
- Advanced camera trajectory controls (pans, zooms, focus pulls)
- Subject locking and background stabilization
- LoRA fine-tuning support
- Motion intensity control

## Strengths

1. **Highest motion realism** among open-source models as of March 2026
2. **MoE architecture** distributes denoising across specialized experts, scaling efficiently without linear compute increase
3. **Extremely low entry barrier** -- the 1.3B model runs on ~8 GB VRAM, compatible with almost all consumer GPUs
4. **Native 1080p** in Wan 2.2 without upscaling
5. **Apache 2.0 license** permits unrestricted commercial use
6. **Massive community adoption** -- 6.9M+ downloads on Hugging Face and ModelScope
7. **Proven ecosystem** -- drop-in weight swap from Wan 2.1 to 2.2, no infrastructure changes needed
8. **Smooth, detailed textures** and professional-grade motion quality

## Weaknesses

1. **Slower generation speed** -- a 5-second 720p video takes 10-12 minutes on an H100; 4-5 minutes at 720p for standard clips
2. **High VRAM for best quality** -- the 14B model needs 60-80 GB VRAM, out of reach for most consumers
3. **No native audio generation** -- video only; audio must be added separately
4. **Shorter clip duration** -- limited to ~5 seconds per generation
5. **Heavier ComfyUI setup** required compared to some competitors

## Comparison to LTX Video

| Dimension | Wan 2.2 | LTX-2.3 |
|-----------|---------|---------|
| **Parameters** | 1.3B-14B | 2B-22B |
| **Max Resolution** | 1080p | Native 4K |
| **Speed** | 4-5 min for 5s at 720p | Under 1 min for 5s at 720p |
| **Audio** | No | Yes (native synchronized audio) |
| **Motion Quality** | Best-in-class | Very good, slightly behind Wan |
| **VRAM (entry)** | ~8 GB | ~12 GB (8 GB with quantization) |
| **License** | Apache 2.0 | Apache 2.0 (revenue cap for commercial) |
| **Ecosystem** | ComfyUI, standalone | ComfyUI, LTX Desktop, LTX Studio, MCP |

**Key Takeaway:** Wan 2.2 leads in motion realism and has the lowest VRAM entry point, but LTX-2.3 is dramatically faster (5-10x), generates native 4K with synchronized audio, and offers a more complete ecosystem (desktop app, studio product, MCP integration). Choose Wan for maximum motion quality; choose LTX for speed, audio, and production workflow.

## Sources

- [Alibaba Introduces Open-Source Model for Video Creation and Editing](https://www.alibabacloud.com/blog/alibaba-introduces-open-source-model-for-video-creation-and-editing_602226)
- [Alibaba Unveils its Latest Open-Source Video Generation Model](https://www.alibabacloud.com/blog/alibaba-unveils-its-latest-open-source-video-generation-model_602167)
- [Alibaba's Wan 2.1: A New Era in Open-Source Video Generation](https://www.pixazo.ai/blog/alibaba-wan-open-source-video-generation-model)
- [Alibaba Releases Wan 2.1 (Latenode)](https://latenode.com/blog/ai-technology-language-models/ai-in-business-applications/alibaba-releases-wan-2-1)
- [Wan 2.2 by Alibaba - Advanced Open-Source AI Video Generator](https://www.atlabs.ai/blog/wan-2-2-by-alibaba-advanced-open-source-ai-video-generator)
- [Wan AI Official](https://wan.video/)
- [Wan2.2 GitHub](https://github.com/Wan-Video/Wan2.2)
- [Wan2.2-T2V-A14B on Hugging Face](https://huggingface.co/Wan-AI/Wan2.2-T2V-A14B)
- [LTX 2.3 vs WAN 2.2 Comparison](https://crepal.ai/blog/aivideo/ltx-2-3-vs-wan-2-2/)
- [Open-Source I2V Showdown: Hunyuan vs Wan vs LTXV](https://medium.com/@lada.huang2017/open-source-image-to-video-model-showdown-2025-hunyuan-vs-wan-2-1-vs-ltxv-3d14dd3565a5)
