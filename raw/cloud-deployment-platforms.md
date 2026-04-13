# Cloud Deployment Platforms for LTX Video

## fal.ai

### Overview
fal.ai provides hosted inference and training APIs for LTX Video models with Pro and Fast variants.

### Available Models
- LTX-2.3 Image-to-Video
- LTX-2.3 Text-to-Video
- LTX Video 2.0 Pro Image-to-Video
- LoRA-enabled variants

### Features
- Text-to-video, image-to-video, audio-to-video
- LoRA training service for custom models
- Developer documentation and guides
- Self-serve API access

### Resources
- **Image-to-Video:** https://fal.ai/models/fal-ai/ltx-2.3/image-to-video
- **Developer Guide:** https://fal.ai/learn/devs/ltx-video-2-pro-image-to-video-developer-guide

## Replicate

### Overview
Hosted inference API for LTX Video models with CLI, API, and web UI access.

### Features
- Managed cloud inference
- Simple API integration
- Listed as official deployment option in LTX documentation

## Modal

### Overview
Serverless GPU infrastructure for deploying LTX-Video via CLI, API, and web UI.

### Features
- Animate images with LTX-Video
- Three access methods: CLI, API, web UI
- Serverless (no infrastructure management)

### Documentation
- **URL:** https://modal.com/docs/examples/image_to_video

## DigitalOcean GPU Droplets

### Overview
Tutorial-supported deployment on DigitalOcean's GPU infrastructure.

### Recommended Hardware
- NVIDIA H100 or H200 GPU (recommended)
- NVIDIA A6000 (sufficient but slower)

### Tutorials
- "How to Generate Videos with LTX-2.3 on DigitalOcean GPU Droplets"
  - **URL:** https://www.digitalocean.com/community/tutorials/generate-videos-ltx-23
- "LTX-2 Brings Open-Source Audio-Visual Generation"
  - **URL:** https://www.digitalocean.com/community/tutorials/ltx-2-video-generation-audio-video-model

## Vast.ai

### Overview
GPU marketplace with LTX Video available in AI Model Library.

### Features
- Rent GPU hardware for LTX Video inference
- Cost-effective compared to managed platforms
- Full control over deployment

### URL
- https://vast.ai/model/ltx-video

## RunComfy

### Overview
Browser-based ComfyUI environment with LTX models pre-loaded.

### Features
- LTX 2 API access with unified parameters
- Sample code for integration
- Browser-based LoRA training
- No local GPU required

### URL
- https://www.runcomfy.com/models/ltx/ltx-2/fast/text-to-video/api

## LTX API (Official Lightricks)

### Overview
Official API from Lightricks for LTX video generation.

### Features
- Text encoding (free)
- Video generation (paid)
- Retake functionality (paid)
- Prompt enhancement (included)
- Used by LTX Desktop for API mode

### Playground
- https://console.ltx.video/playground/

## Self-Hosting Options

### Requirements
- NVIDIA GPU with CUDA support
- Minimum 12GB VRAM (modest settings: 512-768 width, short clips)
- 16GB+ VRAM recommended for full quality
- 32GB+ VRAM for 4K generation

### Deployment Methods
- Clone GitHub repo and run inference scripts
- Docker containers
- Python virtual environments with pip/uv

### Hardware Guidance (from community)
| VRAM | Capability |
|------|-----------|
| 8GB | Very limited, tight and frustrating |
| 12GB | Usable for modest settings (512-768 width) |
| 16GB | Good for standard generation |
| 24GB | Comfortable for most workflows |
| 32GB+ | Full 4K generation capability |

## Licensing for Cloud Deployment
- Free for academic research
- Free for companies with < $10M ARR
- Commercial license required above $10M ARR threshold
- **Source:** https://huggingface.co/Lightricks/LTX-Video

## Sources
- https://fal.ai/models/fal-ai/ltx-2.3/image-to-video
- https://modal.com/docs/examples/image_to_video
- https://www.digitalocean.com/community/tutorials/generate-videos-ltx-23
- https://vast.ai/model/ltx-video
- https://www.runcomfy.com/models/ltx/ltx-2/fast/text-to-video/api
- https://ltx.io/model/ltx-2
