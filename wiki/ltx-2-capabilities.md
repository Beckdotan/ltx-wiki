---
title: LTX-2 Capabilities and Specifications
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-capabilities-and-specifications.md
tags:
  - ltx-2
  - capabilities
  - specifications
  - generation-modes
  - resolution
---

# LTX-2 Capabilities and Specifications

Technical specifications and generation capabilities of the [[ltx-2-overview|LTX-2]] model family.

## Core Specifications

### Resolution

- **Native 4K** (3840x2160) output
- Supports multiple resolutions: 720p, 1080p, 4K
- Default generation: 1216x704 at 30 FPS (base mode)
- [[ltx-2.3-model|LTX-2.3]] adds native portrait: up to 1080x1920 (9:16), trained on actual vertical data

### Frame Rate

- Up to **50 FPS** maximum
- Standard generation at 24-30 FPS
- 48 FPS practical for product spins and macro detail ([[ltx-2.3-model|LTX-2.3]])

### Duration

- Up to **20 seconds** of continuous video with synchronized stereo audio
- LTX-2.3 Fast: up to 20-second clips
- LTX-2.3 Pro: up to 10-second clips (prioritizes fidelity)

### Audio

- Synchronized stereo audio generation in a single pass
- Generates: dialogue, background noise, foley elements, ambient sounds, music
- Expressive lip sync and audio fidelity
- [[ltx-2.3-model|LTX-2.3]] features filtered training data and new vocoder

## Generation Modes

### Text-to-Video
Generate video from text descriptions. Supports detailed, chronological prompts (~200 words recommended). Multilingual via Gemma 3 text encoder.

### Image-to-Video
Animate static images with realistic motion. Preserves source image characteristics. Useful for logo bumps and mockup animation.

### Audio-to-Video
Generate video driven by an audio track. Supply dialogue, music, or ambient sound; the model produces synchronized visuals.

### Video Extension (Extend)
Lengthen existing videos from beginning or end. Preserves audio and visual continuity.

### Video Region Regeneration (Retake)
Selectively edit specific video segments. Fix jittery motion or odd camera moves without starting over.

### Multi-Keyframe Conditioning
Keyframe-based animation control with frame-level precision for complex scenes. Supports 3D camera logic.

### Video Interpolation
Generate frames between keyframes for smooth transitions between specified states.

### Video-to-Video Transformations
Transform existing footage using [[ltx-2-lora-training|IC-LoRA adapters]] for style transfer, motion transfer, and edge/depth/pose guidance.

## Performance Tiers

### LTX-2 (January 2026)

| Tier | Description |
|------|-------------|
| LTX-2 Fast | Optimized for speed and rapid iteration |
| LTX-2 Pro | Optimized for maximum quality |

### LTX-2.3 (March 2026)

| Tier | Description |
|------|-------------|
| LTX-2.3 Fast | 2x faster inference, 1/10 compute cost, up to 20s clips |
| LTX-2.3 Pro | Best quality, all endpoints, up to 10s clips |

## Speed

- **18x faster** than Wan 2.2 at comparable settings
- Fast mode: 2x inference speed vs Pro mode
- RTX 4090: 10-second 4K clip in 9-12 minutes (30-36 diffusion steps)
- RTX 3090: Same clip in 20-25 minutes
- Up to 50% lower compute cost than competing models

See [[ltx-2-benchmarks]] for detailed performance data.

## VRAM Requirements

| Setup | VRAM | Notes |
|-------|------|-------|
| Full bf16 model | 32GB+ | Recommended for full quality |
| FP8 model | 16GB+ | Comfortable 1080p |
| GGUF Q4_K_S | 10GB | RTX 3080; 960x544, 5s clip |
| Minimum viable | 12GB | RTX 3060, slower with RAM offloading |
| NVFP4 (RTX 50 Series) | ~13GB | 3x faster, 60% less VRAM |
| NVFP8 | ~19GB | 2x faster, 40% less VRAM |

See [[ltx-2-model-variants]] for all weight options and [[ltx-2-nvidia-optimization]] for GPU-specific optimizations.

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2-architecture]] -- How the architecture enables these capabilities
- [[ltx-2-model-variants]] -- Weight checkpoints and quantizations
- [[ltx-2-benchmarks]] -- Comparative benchmarks
- [[ltx-2-api-and-pricing]] -- Accessing these capabilities via API
