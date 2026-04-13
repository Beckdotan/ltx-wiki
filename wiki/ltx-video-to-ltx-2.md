---
title: LTX Video to LTX-2 Evolution
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-models-ltxv-ltx2.md
  - raw/model-evolution-ltx-video-to-ltx-2.3.md
  - raw/model-ltx-video-versions-huggingface.md
tags:
  - ltx-video
  - ltx-2
  - ltx-2-3
  - evolution
  - lightricks
  - audio-video
---
# LTX Video to LTX-2 Evolution

This page traces the evolution of the [[lightricks]] video generation model family from [[ltx-video-overview|LTX-Video]] through [[ltx-2]] and [[ltx-2-3]].

## Complete Model Family Timeline

| Model | Date | Parameters | Key Milestone |
|-------|------|------------|---------------|
| LTX-Video 0.9.0 | Nov 2024 | 2B | First open-source real-time DiT video model |
| LTX-Video 0.9.1 | Dec 2024 | 2B | Quality refinement |
| LTX-Video 0.9.5 | Mar 2025 | 2B | Commercial license, keyframe conditioning |
| LTX-Video 0.9.6 | Apr 2025 | 2B | Distillation (15x speed) |
| LTX-Video 0.9.7 | May 2025 | 13B | Scale-up, FP8, ICLoRA, upscalers |
| 60-Second Barrier | Jul 2025 | -- | Lightricks breaks 60-second GenAI video barrier |
| LTX-Video 0.9.8 | Jul 2025 | 13B + 2B | Full ecosystem maturation |
| [[ltx-2]] | Oct 2025 | 19B | Rename from LTXV; first joint audio-video DiT model |
| LTX-2 Open-Source | Jan 2026 | 19B | Full codebase, weights, and tooling released |
| [[ltx-2-3]] | Mar 2026 | 22B | Latest version, powers LTX Desktop |

## LTX-Video Era (Nov 2024 -- Jul 2025)

See [[ltx-video-versions]] for the complete LTX-Video version timeline.

The LTX-Video line progressed through six versions, growing from 2B to 13B parameters, introducing distillation, FP8 quantization, ICLoRA control adapters, and spatial/temporal upscalers.

## Transition to LTX-2 (October 2025)

### What Changed

LTX-Video was renamed to **LTX-2** in October 2025, marking a fundamental leap:

- **Joint audio-video generation** -- First open-source DiT-based audio-video foundation model
- **Parameters:** 19B (14B video + 5B audio)
- **Native 4K resolution** at up to 50 FPS
- **Synchronized audio and video** generated in a single pass
- **Up to 20 seconds** per generation

### Architecture Changes from LTX-Video to LTX-2

| Component | LTX-Video | LTX-2 |
|-----------|-----------|-------|
| Parameters | 2B / 13B | 19B (14B video + 5B audio) |
| Text encoder | T5-XXL | Gemma 3 12B |
| Audio | None | 5B audio stream |
| Audio-video coupling | N/A | Bidirectional cross-attention |
| Prompt handling | Standard | Thinking tokens for better adherence |
| CFG | Standard | Modality-aware CFG |
| Max resolution | 1216x704 | Native 4K |
| Max FPS | 30 | 50 |
| Max duration | 60 seconds | 20 seconds per generation |

### New Repositories

- **GitHub:** https://github.com/Lightricks/LTX-2
- **Hugging Face:** https://huggingface.co/Lightricks/LTX-2

### LTX-2 Pipeline Options

1. **TI2VidTwoStagesPipeline** -- Production-quality with 2x upsampling (recommended)
2. **TI2VidTwoStagesHQPipeline** -- Higher quality using res_2s sampler
3. **TI2VidOneStagePipeline** -- Rapid prototyping
4. **DistilledPipeline** -- Fastest inference with 8 predefined sigmas
5. **ICLoraPipeline** -- Video-to-video transformations
6. **KeyframeInterpolationPipeline** -- Inter-frame generation
7. **A2VidPipelineTwoStage** -- Audio-conditioned generation

### LTX-2 Open-Source Release (January 2026)

The complete codebase, weights, and tooling were made publicly available, making LTX-2 the first production-ready model combining truly open audio and video generation with native 4K output and synchronized expressive sound.

## LTX-2.3 (March 2026)

### Overview

- **Parameters:** 22B (20.9B)
- **Two variants:**
  - **LTX-2.3 Pro** -- Best-in-class quality, supports all endpoints (audio-to-video, retake, extend)
  - **LTX-2.3 Fast** -- Blazing fast generation, supports text-to-video and image-to-video
- **Powers:** LTX Desktop application

### Improvements over LTX-2

- Sharper fine details (new latent space/VAE)
- Cleaner audio (improved data filtering)
- Stronger image-to-video with more natural motion
- Enhanced prompt adherence
- Multiple spatial upscalers (x2, x1.5)
- Temporal upscaler (x2 FPS)
- Distilled model (8-step inference)

### Checkpoints

- `ltx-2.3-22b-dev.safetensors`
- `ltx-2.3-22b-distilled.safetensors`
- Distilled-lora-384
- Spatial upscalers (x2, x1.5)
- Temporal upscaler

### Hugging Face

- **URL:** https://huggingface.co/Lightricks/LTX-2.3
- **Monthly downloads:** 1.66M (highest of any version)

## IC-LoRA Evolution

| Generation | Models |
|-----------|--------|
| LTX-Video 0.9.7-0.9.8 | ICLoRA depth, pose, canny, detailer (individual adapters) |
| [[ltx-2]] | Union-Control (combined), Detailer, Canny-Control (individual) |
| [[ltx-2-3]] | Union-Control (Canny + Depth + Pose), Motion-Track-Control |

## Community Ecosystem Growth

| Version | Downloads/Month | Quantizations | Adapters | Fine-tunes |
|---------|----------------|--------------|----------|-----------|
| LTX-Video | 567K | 16 | 24 | 25 |
| [[ltx-2]] | 907K | 8 | 49 | 54 |
| [[ltx-2-3]] | 1.66M | 14 | 20 | 27 |

## Technical Requirements Progression

| Requirement | LTX-Video | LTX-2/2.3 |
|------------|-----------|-----------|
| Python | >= 3.10.5 | >= 3.12 |
| CUDA | >= 12.2 | > 12.7 |
| PyTorch | >= 2.1.2 | ~= 2.7 |
| Precision | bfloat16, fp8 | bfloat16, fp8, fp4 (nvfp4) |

## Package Structure (LTX-2+)

- **ltx-core** -- Model implementation and inference utilities
- **ltx-pipelines** -- High-level generation pipelines
- **ltx-trainer** -- Training and adaptation tools

## Licensing Across Generations

- **Code:** Apache 2.0 (LTX-2+)
- **Model weights:** Free for companies under $10M annual revenue
- **Above threshold:** Contact [[lightricks]] for commercial terms

## Core Capabilities (LTX-2/2.3)

Beyond what LTX-Video offered, LTX-2 and 2.3 add:

- **Audio-to-Video** -- Generate visuals synchronized to audio
- **Video Region Regeneration (Retakes)** -- Re-generate specific video segments
- **Video Interpolation** -- Generate frames between keyframes
- **Joint audio-video generation** in a single model pass

## References

- [[ltx-video-overview]] -- LTX-Video model family overview
- [[ltx-video-versions]] -- LTX-Video version timeline
- [[ltx-video-architecture]] -- LTX-Video architecture (foundation for LTX-2)
- [[ltx-2]] -- LTX-2 model details
- [[ltx-2-3]] -- LTX-2.3 model details
- [[lightricks]] -- Developer organization
