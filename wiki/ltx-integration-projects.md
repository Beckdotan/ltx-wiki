---
title: LTX Integration Projects
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/open-source-model/integration-tools/
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://ltx.io/model/ltx-2
  - https://github.com/Lightricks/ComfyUI-LTXVideo
  - https://fal.ai/models/fal-ai/ltx-2.3/image-to-video
  - https://modal.com/docs/examples/image_to_video
tags:
  - integrations
  - comfyui
  - diffusers
  - api
  - ltx
  - developer-tools
---

# LTX Integration Projects

The [[ltx-ecosystem]] has a broad range of first-party, cloud platform, and community integrations for deploying and using LTX Video models.

## First-Party Integrations

### ComfyUI (Official Nodes)

- **Repo:** https://github.com/Lightricks/ComfyUI-LTXVideo
- Official custom nodes for [[ltx-2-overview]] video generation in ComfyUI
- LTX-2 integrated into ComfyUI core; supplementary repo provides advanced nodes
- 3,400+ stars, actively maintained
- Featured in the [[nvidia-ltx-partnership]] workflow guides

### HuggingFace Diffusers

- LTX Video integrated into the Diffusers Python library
- Pipelines: `LTXPipeline`, `LTXImageToVideoPipeline`, `LTXConditionPipeline`, `LTX2Pipeline`
- LoRA loading support via standard Diffusers API
- Single-file checkpoint loading via `from_single_file()`
- Docs: https://huggingface.co/docs/diffusers/api/pipelines/ltx_video

### PyTorch Native API

- Direct Python inference without Diffusers abstraction
- Full control over all features via `ltx-pipelines` package
- Eight specialized pipeline implementations
- Docs: https://docs.ltx.video/open-source-model/integration-tools/pytorch-api

### LTX-Video-Trainer

- Official fine-tuning toolkit for LoRA and full fine-tuning
- **Repo:** https://github.com/Lightricks/LTX-Video-Trainer
- Apache 2.0 license

## Cloud Platform Integrations

### fal.ai

- LTX-2.3 available in Pro and Fast variants
- Text-to-video, image-to-video, audio-to-video
- Also provides LoRA training service for LTX models
- URL: https://fal.ai/models/fal-ai/ltx-2.3/image-to-video

### Replicate

- Hosted inference API for LTX Video models
- CLI, API, and web UI access
- URL: https://replicate.com/lightricks/ltx-video

### Modal

- Deploy LTX-Video via CLI, API, and web UI
- Serverless GPU infrastructure
- Docs: https://modal.com/docs/examples/image_to_video

### DigitalOcean GPU Droplets

- Tutorial for deploying LTX-2.3 on GPU Droplets
- Recommended: NVIDIA H100 or H200 (also works on A6000)
- Tutorial: https://www.digitalocean.com/community/tutorials/generate-videos-ltx-23

### Vast.ai

- LTX Video available in AI Model Library
- Build and deploy on Vast.ai GPU marketplace
- URL: https://vast.ai/model/ltx-video

### RunComfy

- LTX 2 API access with unified parameters and sample code
- Browser-based ComfyUI with LTX models pre-loaded
- Also offers browser-based LoRA training
- URL: https://www.runcomfy.com/models/ltx/ltx-2/fast/text-to-video/api

## Third-Party Training Services

### WaveSpeedAI

- LTX-2 19B Video LoRA Trainer and IC-LoRA Trainer
- LTX-2.3 Image-to-Video and Text-to-Video LoRA services
- No cold starts; training jobs start immediately
- Pricing: $0.35-$7.00 per training job (100-2000 steps)
- URL: https://wavespeed.ai/models/wavespeed-ai/ltx-2-19b/video-lora-trainer

### Oxen.ai

- Character LoRA training for LTX-2
- Upload data, automated GPU orchestration
- Tutorial: https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/

## Community Projects

- **ComfyUI-LTXTricks** (deprecated, merged into official nodes) - Advanced techniques: RF-Inversion, RF-Edit, FlowEdit
- **Phr00t/LTX2-Rapid-Merges** - Community model merges
- **a-r-r-o-w/LTX-Video-diffusers** - Community Diffusers-compatible conversion
- **OpenArt Workflows** - Community ComfyUI workflows (video-to-video, image-to-video, low-VRAM)
- **Civitai Community** - GGUF quantized models, IC-LoRA Detailer models, VideoFlow workflows

## External AI Service Integration

- **Google Gemini** - [[ltx-desktop]] supports Gemini for AI prompt suggestions and upcoming Bridge Shots feature
- **fal.ai Image Generation** - [[ltx-desktop]] uses fal.ai for text-to-image generation
