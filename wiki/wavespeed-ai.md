---
title: WaveSpeed AI
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/api-wavespeed-ai-ltx.md
  - raw/inference-providers-overview.md
tags:
  - inference
  - cloud
  - api
  - wavespeed
---

# WaveSpeed AI

WaveSpeed AI provides a REST inference API for LTX Video models with no cold starts and affordable pricing. It hosts multiple LTX model variants including LTX-2.3, LTX-2 19B, and legacy LTX-Video, and is one of the few providers supporting the full range of LTX-2.3 capabilities including audio-video generation and portrait mode.

**Website:** https://wavespeed.ai
**Docs:** https://wavespeed.ai/docs
**Blog:** https://wavespeed.ai/blog

See also: [[inference-providers-overview]]

## Available Endpoints

### LTX-2.3
| Endpoint | Description |
|----------|-------------|
| `ltx-2.3-text-to-video` | Text-to-video generation |
| `ltx-2.3-image-to-video` | Image-to-video generation |

### LTX-2 19B
| Endpoint | Description |
|----------|-------------|
| `ltx-2-19b-text-to-video` | Text-to-video with 19B model |
| `ltx-2-19b-control` | ControlNet generation (pose, depth, canny edge) |
| `ltx-2-video-extend` | Video extension |

### LTX-Video (Legacy)
| Endpoint | Description |
|----------|-------------|
| `ltx-video-v097-i2v-720p` | Image-to-video at 720p |
| `ltx-video-v097-i2v-480p` | Image-to-video at 480p |

## LTX-2.3 Capabilities

LTX-2.3 is a DiT-based audio-video foundation model that generates synchronized video and audio within a single model. Key features:

- Motion and audio generated as one coherent output
- Video at 480p, 720p, or 1080p
- Duration from 5 to 20 seconds
- Native 9:16 portrait mode for social platforms (Instagram Reels, TikTok, YouTube Shorts)
- Audio preservation, generation, or removal options

### Seven Endpoint Types on WaveSpeed

1. **Text-to-Video (Standard)** -- Quality pass with better motion consistency and cleaner textures
2. **Text-to-Video (Fast)** -- Quick reads on framing and motion ideas
3. **Image-to-Video** -- Animate single frames; useful for logos or mockups
4. **Audio-to-Video** -- Pacing cues from audio ("giving the model a metronome")
5. **Extend-Video** -- Append duration to existing clips with shared seed parameters
6. **Retake-Video** -- Regenerate segments while preserving constraints
7. **System/Utility** -- Job polling infrastructure

## LTX-2 19B ControlNet

Generates synchronized audio-video (up to 20 seconds) from video input with:
- Pose guidance
- Depth guidance
- Canny edge guidance
- Audio preservation, generation, or removal

## Recommended Parameters

| Parameter | Recommendation |
|-----------|---------------|
| Duration and frames | 4-8s at 16-24 fps for stable motion |
| Seed | Required for consistency across retakes and extensions |
| Guidance/CFG | 4-6 for creative freedom; 7-9 for constrained outputs |
| Negative prompts | Motion-focused (e.g., "avoid fast zooms") rather than visual descriptors |

## Deployment Options

### Managed API
- No cold starts
- Rate limits apply
- Ideal for exploratory batches (12 clips or fewer)

### Self-Hosted
- Lower marginal cost
- Requires 24 GB VRAM minimum
- Full batching control
- Setup friction

## Portrait Video (9:16) Workflow

For portrait sequences longer than 8-10 seconds:
- Use extend rather than regenerate to preserve subject consistency
- Native 9:16 output supported for social media platforms
- Optimal for Instagram Reels, TikTok, and YouTube Shorts

## Unique Differentiators

Compared to other providers in the [[inference-providers-overview]]:
- No cold starts on managed API
- Native portrait video (9:16) support
- Full LTX-2.3 and 19B model support (unlike [[replicate]] and [[segmind]])
- ControlNet support (shared only with [[fal-ai]])
