# WaveSpeed AI: LTX Video API

## Sources
- https://wavespeed.ai/blog/posts/ltx-2-3-api-endpoints-guide/
- https://wavespeed.ai/blog/posts/ltx-2-3-pricing-api-cost-2026/
- https://wavespeed.ai/docs/docs-api/wavespeed-ai/ltx-2.3-text-to-video
- https://wavespeed.ai/docs/docs-api/wavespeed-ai/ltx-2.3-image-to-video
- https://wavespeed.ai/docs/docs-api/wavespeed-ai/ltx-2-video-extend
- https://wavespeed.ai/docs/docs-api/wavespeed-ai/ltx-2-19b-text-to-video
- https://wavespeed.ai/docs/docs-api/wavespeed-ai/ltx-2-19b-control
- https://wavespeed.ai/blog/posts/ltx-2-3-portrait-video-9-16-workflow-2026/

---

## Overview

WaveSpeed AI provides a ready-to-use REST inference API for LTX Video models with best performance, no cold starts, and affordable pricing. The platform hosts multiple LTX model variants including LTX-2.3, LTX-2 19B, and older LTX-Video versions.

---

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

---

## LTX-2.3 Capabilities

LTX-2.3 is a DiT-based audio-video foundation model designed to generate synchronized video and audio within a single model, with improved audio and visual quality as well as enhanced prompt adherence. Key features:

- Generates motion and audio together as one coherent output
- Video at 480p, 720p, or 1080p
- Duration from 5 to 20 seconds
- Native 9:16 portrait mode for social platforms (Instagram Reels, TikTok, YouTube Shorts)
- Audio preservation, generation, or removal options

---

## LTX-2.3 Seven Endpoint Types

1. **Text-to-Video (Standard)** -- Quality pass: slower, better motion consistency, cleaner textures
2. **Text-to-Video (Fast)** -- Scout pass: quick reads on framing and motion ideas
3. **Image-to-Video** -- Animate single frames; useful for logos or mockups
4. **Audio-to-Video** -- Pacing cues from audio ("giving the model a metronome")
5. **Extend-Video** -- Append duration to existing clips with shared seed parameters
6. **Retake-Video** -- Regenerate segments while preserving constraints
7. **System/Utility** -- Job polling infrastructure

---

## Recommended Parameters

| Parameter | Recommendation |
|-----------|---------------|
| Duration and frames | 4-8s at 16-24 fps for stable motion |
| Seed | Required for consistency across retakes and extensions |
| Guidance/CFG | 4-6 for creative freedom; 7-9 for constrained outputs |
| Negative prompts | Motion-focused (e.g., "avoid fast zooms") rather than visual descriptors |

---

## LTX-2 19B ControlNet

Generates synchronized audio-video (up to 20s) from video input with:
- Pose guidance
- Depth guidance
- Canny edge guidance
- Audio preservation, generation, or removal

---

## Deployment Options

### Managed API
- Higher per-minute costs
- Rate limits
- Ideal for exploratory batches (12 clips or fewer)
- No cold starts

### Self-Hosted
- Lower marginal cost
- Requires 24 GB VRAM minimum
- Setup friction
- Full batching control

---

## Pricing

LTX-2.3 has open weights available for free download on Hugging Face. The API offers both managed and self-hosted options with different cost structures. For current pricing, see: https://wavespeed.ai/docs

---

## Portrait Video (9:16) Workflow

For portrait sequences longer than 8-10 seconds:
- Use extend rather than regenerate to preserve subject consistency
- Native 9:16 output supported for social media platforms
- Optimal for Instagram Reels, TikTok, and YouTube Shorts

---

## Related Links

- **Documentation:** https://wavespeed.ai/docs
- **Blog:** https://wavespeed.ai/blog
- **LTX-2.3 Pricing Guide:** https://wavespeed.ai/blog/posts/ltx-2-3-pricing-api-cost-2026/
