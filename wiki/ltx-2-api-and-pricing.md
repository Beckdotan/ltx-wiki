---
title: LTX-2 API Access and Pricing
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-api-access-and-pricing.md
tags:
  - ltx-2
  - api
  - pricing
  - cloud
  - inference
---

# LTX-2 API Access and Pricing

API access options and pricing for [[ltx-2-overview|LTX-2]] and [[ltx-2.3-model|LTX-2.3]].

## Official LTX API

- **Documentation:** https://docs.ltx.video
- **Playground:** https://console.ltx.video/playground
- **API Page:** https://ltx.io/model/api

### Available Endpoints (LTX-2.3)

| Endpoint | LTX-2.3 Fast | LTX-2.3 Pro |
|----------|-------------|-------------|
| Text-to-Video | Yes | Yes |
| Image-to-Video | Yes | Yes |
| Audio-to-Video | No | Yes |
| Retake (Standard) | No | Yes |
| Retake (Fast) | Yes | No |
| Extend | No | Yes |

### Pricing (Effective April 1, 2026)

#### LTX-2.3 Fast

| Resolution | Price per second |
|-----------|-----------------|
| 720p | $0.04/s |
| 1080p | $0.06/s |

#### LTX-2.3 Pro

| Resolution | Price per second |
|-----------|-----------------|
| 720p | $0.08/s |
| 1080p | $0.16/s |
| 4K | $0.32/s |

#### LTX-2.3 Ultra

- Starting at $0.16/s
- Maximum fidelity for cinematic, branded, and VFX production
- 4K video at up to 50 FPS with synchronized audio
- 10-second sequences

#### Retake Endpoint

$0.10 per second of video output.

## Third-Party API Providers

### fal.ai

- Models: LTX Video 2.0 Pro (Image-to-Video), LTX-2 19B general
- Pricing: ~$0.0018 per megapixel; 6-second 1080p video ~$0.36; 121-frame 720p ~$0.20
- Also offers [[ltx-2-lora-training|LoRA training]] endpoint
- Docs: https://fal.ai/learn/devs/ltx-video-2-pro-image-to-video-developer-guide

### Replicate

- **lightricks/ltx-2-retake** -- Video retake/regeneration
- $0.10 per video second output
- File size limit: under 100 MB; max input: 20 seconds
- URL: https://replicate.com/lightricks/ltx-2-retake

### Other Providers

| Provider | Offering |
|----------|----------|
| WaveSpeedAI | Inference API + IC-LoRA trainer |
| Atlas Cloud | Unified API access, competitive pricing |
| Pixazo | Cinematic image-to-video and audio-synchronized generation |
| Scenario | LTX-2 19B suite integration |

## Self-Hosted / Local Options

### Native PyTorch API (ltx-pipelines)
Full-featured local inference via the LTX-2 repository. Requirements: Python 3.10+, CUDA 12.7+, PyTorch ~2.7, 32GB+ VRAM recommended.

### ComfyUI Server
Run ComfyUI with LTX-2 nodes as a local API server. GitHub: https://github.com/Lightricks/ComfyUI-LTXVideo

### [[ltx-desktop|LTX Desktop]]
Desktop application with built-in model server. Free for companies under $10M annual revenue. No per-generation cost.

## Cost Comparison: API vs Local

| Approach | Cost Model | Best For |
|----------|-----------|---------|
| Official LTX API | Per-second pricing | Production workloads, no GPU investment |
| fal.ai | Per-megapixel pricing | Integration into existing fal workflows |
| Replicate | Per-second pricing | Quick prototyping, Replicate ecosystem |
| Local (GPU) | One-time hardware cost | High volume, privacy, full control |
| LTX Desktop | Free (under $10M revenue) | Teams wanting local GUI |

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2-capabilities]] -- What you can access via these APIs
- [[ltx-desktop]] -- Local desktop alternative
- [[ltx-2-model-variants]] -- Self-hosted weight options
