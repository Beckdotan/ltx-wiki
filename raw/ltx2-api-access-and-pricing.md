# LTX-2 API Access and Pricing

## Official LTX-2 API

### Overview
- **Documentation:** https://docs.ltx.video
- **Playground:** https://console.ltx.video/playground
- **API Page:** https://ltx.io/model/api
- **Pricing Page:** https://ltx.io/model/api/pricing

### Available Endpoints (LTX-2.3)

LTX-2.3 offers 7 endpoints across two model tiers:

| Endpoint | LTX-2.3 Fast | LTX-2.3 Pro |
|----------|-------------|-------------|
| Text-to-Video | Yes | Yes |
| Image-to-Video | Yes | Yes |
| Audio-to-Video | No | Yes |
| Retake (Standard) | No | Yes |
| Retake (Fast) | Yes | No |
| Extend | No | Yes |

### Pricing Structure (Effective April 1, 2026)

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
- $0.10 per second of video output

### Pre-April 2026 Pricing
- Fast 1080p: $0.04/s (increased to $0.06/s on April 1)
- Pro 1080p: $0.08/s (increased to $0.16/s on April 1)

### API Access
- Sign up at https://console.ltx.video
- Help Center: https://support.ltx.studio/hc/en-us/articles/32477894635410

## Third-Party API Providers

### fal.ai

#### Available Models
- LTX Video 2.0 Pro (Image-to-Video)
- LTX-2 19B general models

#### Pricing
- ~$0.0018 per megapixel of generated video data
- 6-second 1080p video with Pro model: ~$0.36
- 121-frame video at 1280x720 (~112 megapixels): ~$0.20

#### LoRA Training
- fal.ai also offers LTX-2 Video Trainer for custom LoRA fine-tuning

#### Documentation
- Developer guide: https://fal.ai/learn/devs/ltx-video-2-pro-image-to-video-developer-guide

### Replicate

#### Available Endpoints
- **lightricks/ltx-2-retake** -- Video retake/regeneration
  - $0.10 per video second output
  - File size limit: under 100 MB
  - Max input: 20 seconds

#### URL
- https://replicate.com/lightricks/ltx-2-retake

### Other Providers

#### WaveSpeedAI
- Offers LTX-2 inference API
- IC-LoRA trainer endpoint
- https://wavespeed.ai

#### Atlas Cloud
- Unified API access for LTX-2 video models
- Competitive pricing
- https://www.atlascloud.ai/collections/ltx2

#### Pixazo
- LTX-2 19B API for cinematic image-to-video and audio-synchronized generation
- https://www.pixazo.ai

#### Scenario
- LTX-2 19B suite integration
- https://help.scenario.com/en/articles/ltx-models-the-essentials/

## Self-Hosted / Local API Options

### Native PyTorch API (ltx-pipelines)
- Full-featured local inference
- Repository: https://github.com/Lightricks/LTX-2
- Packages: ltx-core, ltx-pipelines, ltx-trainer
- Requirements: Python 3.10+, CUDA 12.7+, PyTorch ~2.7, 32GB+ VRAM (recommended)

### ComfyUI Server
- Run ComfyUI with LTX-2 nodes as a local API server
- GitHub: https://github.com/Lightricks/ComfyUI-LTXVideo

### LTX Desktop
- Desktop application with built-in model server
- Free for companies under $10M annual revenue
- No per-generation cost

## Cost Comparison: API vs Local

| Approach | Cost Model | Best For |
|----------|-----------|---------|
| Official LTX API | Per-second pricing | Production workloads, no GPU investment |
| fal.ai | Per-megapixel pricing | Integration into existing fal workflows |
| Replicate | Per-second pricing | Quick prototyping, Replicate ecosystem |
| Local (GPU) | One-time hardware cost | High volume, privacy, full control |
| LTX Desktop | Free (under $10M revenue) | Teams wanting local GUI |

## Sources

- [LTX-2 API Page](https://ltx.io/model/api)
- [LTX-2 API Pricing](https://ltx.io/model/api/pricing)
- [LTX Pricing Documentation](https://docs.ltx.video/pricing)
- [LTX-2.3 API Guide -- WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-api-endpoints-guide/)
- [fal.ai LTX Video 2.0 Pro](https://fal.ai/learn/devs/ltx-video-2-pro-image-to-video-developer-guide)
- [Replicate LTX-2 Retake](https://replicate.com/lightricks/ltx-2-retake)
- [LTX-2.3 Pricing -- WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-pricing-api-cost-2026/)
- [API Comparison 2026 -- TeamDay](https://www.teamday.ai/blog/ai-image-video-api-providers-comparison-2026)
