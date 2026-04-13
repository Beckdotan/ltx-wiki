# LTX Video Generation Models (LTXV and LTX-2)

## Overview

The LTX model family is a series of open-source AI video generation models developed by Lightricks. The models power both the LTX Studio commercial platform and the LTX Desktop open-source application, and are available to developers through APIs, open weights, and integration tools.

## Model Evolution Timeline

### LTX Video (LTXV) - November 2024
- **Parameters:** 2 billion
- **First release** of Lightricks' open-source video generation model
- Text-to-video and image-to-video capabilities
- Could generate 5-second, 768x512 resolution videos at 24 FPS in 2-4 seconds (faster than real-time)
- Released as open source under Apache 2.0
- **GitHub:** https://github.com/Lightricks/LTX-Video
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-Video

### LTXV Iterations (2024-2025)
- Multiple checkpoint versions: 0.9.6, 0.9.7, 0.9.8
- Both standard and distilled variants available
- Incremental improvements in quality and capabilities

### LTXV-13B - May 2025
- **Parameters:** 13 billion
- Capable of generating 5 seconds of video in 4 seconds
- Significant quality improvement over 2B model

### 60-Second Barrier - July 2025
- Lightricks announced breaking the 60-second barrier for GenAI videos

### LTX-2 - October 2025
- Renamed from LTXV to LTX-2
- First DiT-based (Diffusion Transformer) audio-video foundation model
- Native 4K resolution at up to 50 FPS
- Synchronized audio and video generation in one pass
- Capable of generating synchronized audio and video
- **GitHub:** https://github.com/Lightricks/LTX-2
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2

### LTX-2 Full Open-Source Release - January 2026
- Complete codebase, weights, and tooling made publicly available
- First production-ready model combining truly open audio and video generation
- Native 4K output with synchronized expressive sound
- Full access to model weights, inference, and training code

### LTX-2.3 - March 2026
- **Parameters:** 22 billion (20.9B)
- Latest version, powers LTX Desktop
- Two variants:
  - **LTX-2.3 Pro** - Best-in-class quality, supports all endpoints (audio-to-video, retake, extend)
  - **LTX-2.3 Fast** - Blazing fast generation, supports text-to-video and image-to-video
- Improvements: sharper fine details (new latent space/VAE), cleaner audio (improved data filtering), stronger image-to-video with more natural motion
- Checkpoints: `ltx-2.3-22b-dev.safetensors`, `ltx-2.3-22b-distilled.safetensors`

## Core Capabilities (LTX-2/2.3)

- **Text-to-Video** - Generate videos from text descriptions
- **Image-to-Video** - Animate static images with realistic motion
- **Audio-to-Video** - Generate visuals synchronized to audio
- **Video Interpolation** - Generate frames between keyframes
- **Video Region Regeneration (Retakes)** - Re-generate specific segments
- **Video Extension** - Extend videos forward or backward
- **Multi-keyframe Conditioning** - Keyframe-based animation control
- **Video-to-Video Transformations** - Transform existing footage
- **LoRA Fine-tuning** - Custom model adaptation
- **IC-LoRA** - Image control adaptations (depth, pose, edge, motion)

## Technical Details

### Architecture
- DiT-based (Diffusion Transformer) architecture
- Joint audio-video generation in a single model
- Supports FP8 quantization for memory efficiency
- Compatible with xFormers and Flash Attention 3

### Performance
- Up to 50 FPS at native 4K resolution
- Up to 20 seconds per generation
- Faster-than-real-time generation in Fast mode

### Pipeline Options (LTX-2)
1. **TI2VidTwoStagesPipeline** - Production-quality with 2x upsampling (recommended)
2. **TI2VidTwoStagesHQPipeline** - Higher quality using res_2s sampler
3. **TI2VidOneStagePipeline** - Rapid prototyping
4. **DistilledPipeline** - Fastest inference with 8 predefined sigmas
5. **ICLoraPipeline** - Video-to-video transformations
6. **KeyframeInterpolationPipeline** - Inter-frame generation
7. **A2VidPipelineTwoStage** - Audio-conditioned generation

### Package Structure
- **ltx-core** - Model implementation and inference utilities
- **ltx-pipelines** - High-level generation pipelines
- **ltx-trainer** - Training and adaptation tools

### Prompting Guidelines
- Use detailed, chronological descriptions (~200 words)
- Incorporate cinematographic language
- Describe movements, appearances, environment, and lighting

## Licensing

- Code: Apache 2.0
- Model weights: Free for companies under $10M annual revenue
- Above threshold: Contact Lightricks for commercial terms

## Sources

- [LTXV Models Blog Post](https://ltx.studio/blog/ltxv-models)
- [LTX-2 GitHub Repository](https://github.com/Lightricks/LTX-2)
- [LTX-Video GitHub Repository](https://github.com/Lightricks/LTX-Video)
- [LTX-2 HuggingFace](https://huggingface.co/Lightricks/LTX-2)
- [LTX-2.3 HuggingFace](https://huggingface.co/Lightricks/LTX-2.3)
- [LTX-2 Wikipedia](https://en.wikipedia.org/wiki/LTX-2)
- [LTX-2 Product Page](https://ltx.io/model/ltx-2)
- [LTXV Model Page](https://ltx.io/model/ltxv)
- [LTX-2 ArXiv Paper](https://arxiv.org/html/2601.03233v1)
- [Lightricks LTX-2 Open-Source Announcement (GlobeNewsWire)](https://www.globenewswire.com/news-release/2026/01/06/3213304/0/en/Lightricks-Open-Sources-LTX-2-the-First-Production-Ready-Audio-and-Video-Generation-Model-With-Truly-Open-Weights.html)
- [What Is LTX 2.3 - CrePal](https://crepal.ai/blog/aivideo/what-is-ltx-2-3/)
