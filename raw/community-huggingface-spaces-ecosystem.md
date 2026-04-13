# LTX Video HuggingFace Spaces Ecosystem

**Source:** https://huggingface.co/spaces?search=ltx+video, https://huggingface.co/spaces?search=ltx+2&sort=likes

---

## Overview

The HuggingFace Spaces ecosystem around LTX Video is one of the most active indicators of community adoption. As of April 2026, there are 468+ Spaces related to LTX-Video and 26+ Spaces specifically for LTX-2/2.3.

## Official Lightricks Spaces

| Space | Description | Likes | Status |
|-------|-------------|-------|--------|
| LTX Video Fast | Ultra-fast LTX 0.9.8 13B distilled | 1,490 | Paused |
| LTX 2.3 Distilled | Cinematic videos with audio from text/images | 299 | Running |
| LTX-2 Video Fast | Fast high-quality video with audio | 216 | Paused |
| LTX-2 | High quality video with audio generation | 78 | Paused |
| Control Canny LTX Video Fast | Fast controlled video gen with 0.9.7 distilled | 32 | Paused |

## Top Community Spaces (LTX-2/2.3)

| Space | Creator | Description | Likes |
|-------|---------|-------------|-------|
| LTX-2 Video [Turbo] | alexnasa | Fast generation with FA3 | 398 |
| LTX 2.3 Sync | linoyts | Portrait animation & lipsync | 120 |
| LTX 2.3 First-Last Frame | linoyts | Frame-controlled generation | 96 |
| LTX-2-LoRAs-Camera-Control-Dolly | prithivMLmods | Camera motion control | 58 |
| Ltx2 Audio To Video | multimodalart | Lip-sync from audio | 49 |
| LTX-2.3 Video [Turbo] | ZeroCollabs | Free ZeroGPU generation | 40 |
| LTX-2 First Last Frame | linoyts | Frame control (LTX-2) | 37 |
| LTX 2.3 Distilled | cbensimon | Video from text/images/audio | 13 |
| Ltx-2.3 FFLFwith Lora | rahul7star | Animated videos with LoRA | 7 |
| LTX 2.3 Distilled | cpuai | Video with audio from text | 5 |

## Community Spaces (LTX-Video Original)

| Space | Creator | Likes |
|-------|---------|-------|
| LTX-Video-Playground | tsqn | 4 |
| LTX-Video-Playground | 1inkusFace | 4 |
| LTX-Video-Playground | cocktailpeanut | 8 |
| LTX-Video-Playground | svjack | 3 |
| LTX-Video | ford442 | 4 |

## Space Categories

### 1. Video Generation (Standard)
Standard text-to-video and image-to-video Spaces, often implementing the Lightricks official pipelines with minor modifications.

### 2. Audio-Video Generation
Spaces leveraging LTX-2/2.3's joint audio-video capability, including text-to-audio-video and image-to-audio-video.

### 3. Lip-Sync / Portrait Animation
Specialized Spaces for generating lip-synced talking head videos from audio input + reference image.

### 4. Camera/Motion Control
Spaces implementing camera control LoRAs and motion tracking for guided video generation.

### 5. First-Last Frame Control
Spaces that enable specifying start and end frames for controlled video generation.

### 6. Turbo/Optimized
Community-optimized Spaces using Flash Attention 3 and other techniques for faster inference.

## Deployment Pattern

Most community Spaces run on HuggingFace's "Zero" tier (free GPU access), making LTX-Video accessible without any hardware requirements. This significantly lowers the barrier to entry for users who want to experiment with video generation.

## Space Health

Many older LTX-Video (0.9.x) Spaces show:
- "Runtime error" status
- "Paused" status
- "Build error" status

This reflects the rapid evolution of the model ecosystem - older Spaces become incompatible as the underlying libraries and model versions change.
