# LTX API - Developer Video Generation API

## Overview

The LTX API provides video generation capabilities through a simple HTTP interface. It generates video with synchronized audio from text, images, and audio inputs through a single HTTP call that returns one video back -- no polling, webhooks, or infrastructure to manage.

**Documentation:** https://docs.ltx.video/welcome
**Quick Start:** https://docs.ltx.video/quickstart
**Pricing:** https://docs.ltx.video/pricing

## API Endpoints

### 1. Text-to-Video
- Converts text descriptions into videos with synchronized audio
- Supports up to 4K resolution and 20-second outputs

### 2. Image-to-Video
- Animates static images with realistic motion
- Preserves visual identity of the source image
- Supports up to 4K resolution and 20-second outputs

### 3. Audio-to-Video
- Generates visuals synchronized to provided audio tracks
- Supports dialogue, music, or ambient sound as input

### 4. Retake (Video Edit)
- Re-generates specific time ranges of existing videos
- Can replace video, audio, or both

### 5. Extend (Video Extension)
- Lengthens videos from beginning or end
- Maintains continuity with existing content

## Model Variants

### LTX-2 Models
- **ltx-2-fast** - Optimized for speed
- **ltx-2-pro** - Optimized for quality

### LTX-2.3 Models (March 2026)
- **ltx-2-3-fast** - Blazing fast generation; supports text-to-video and image-to-video
- **ltx-2-3-pro** - Best-in-class quality; supports all endpoints including audio-to-video, retake, and extend

## Pricing (Per Second of Output Video)

### Text-to-Video & Image-to-Video

| Model | 1920x1080 | 2560x1440 | 3840x2160 |
|-------|-----------|-----------|-----------|
| ltx-2-fast | $0.04/sec | $0.08/sec | $0.16/sec |
| ltx-2-pro | $0.06/sec | $0.12/sec | $0.24/sec |
| ltx-2-3-fast | $0.06/sec | $0.12/sec | $0.24/sec |
| ltx-2-3-pro | $0.08/sec | $0.16/sec | $0.32/sec |

*Note: LTX-2.3 prices updated April 1, 2026 (increased from previous rates)*

### Audio-to-Video, Retake & Extend
- **$0.10/sec** at 1920x1080 resolution
- Available for ltx-2-pro and ltx-2-3-pro models only

### Pricing Model
- Billed per second of output video
- Higher resolution and premium models cost proportionally more
- No minimums mentioned
- No free tier mentioned in documentation

## Key Technical Features

- **Output Format:** Video with synchronized audio (dialogue, music, ambient sound generated together with visuals)
- **Infrastructure:** Described as "infrastructure-grade reliability" with predictable performance and stable outputs
- **Resolution:** Up to 4K (3840x2160)
- **Duration:** Up to 20 seconds per individual request
- **Both portrait and landscape** orientations supported (e.g., 1080x1920 and 1920x1080)

## Getting Started

1. Generate API key via developer console
2. Follow quick start guide for initial requests
3. Select model (fast vs. pro)
4. Pay per-second of generated video

## Third-Party API Access

LTX models are also available through third-party platforms:
- **fal.ai** - https://fal.ai/ltx-2.3
- **Segmind** - https://www.segmind.com/models/ltx-video/api
- **Modal** - https://modal.com/docs/examples/ltx

## Sources

- [LTX API Documentation](https://docs.ltx.video/welcome)
- [LTX API Pricing](https://docs.ltx.video/pricing)
- [LTX API Changelog](https://docs.ltx.video/api-changelog)
- [LTX-2 Video Generation API](https://ltx.io/model/api)
- [LTX-2 API Pricing Page](https://ltx.io/model/api/pricing)
- [LTX Quick Start](https://docs.ltx.video/quickstart)
- [LTX-2.3 on fal.ai](https://fal.ai/ltx-2.3)
