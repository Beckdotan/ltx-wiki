# LTX Video Inference Providers: Comprehensive Overview

## Sources
- https://ltx.io/model/api
- https://fal.ai/ltx-2.3
- https://replicate.com/lightricks/ltx-video
- https://www.segmind.com/models/ltx-video
- https://wavespeed.ai/docs/docs-api/wavespeed-ai/ltx-2.3-text-to-video
- https://modal.com/docs/examples/ltx
- https://www.runpod.io/blog/ltxvideo-comfyui-runpod-setup
- https://www.runcomfy.com/comfyui-workflows/ltx-2-comfyui-workflow-real-time-video-generation-speed
- https://huggingface.co/Lightricks/LTX-Video
- https://huggingface.co/Lightricks/LTX-2.3

---

## Overview

LTX Video models are available through multiple inference providers, ranging from the official Lightricks API to third-party cloud platforms and self-hosted options. This document compares all known providers.

---

## Provider Comparison

### 1. Official LTX API (Lightricks)

| Property | Value |
|----------|-------|
| **URL** | https://api.ltx.video |
| **Console** | https://console.ltx.video |
| **Models** | LTX-2 fast/pro, LTX-2.3 fast/pro |
| **Endpoints** | Text-to-video, Image-to-video, Audio-to-video, Extend, Retake, Upload |
| **Max Resolution** | 4K (3840x2160) |
| **Max Duration** | 20 seconds |
| **Auth** | Bearer token |
| **Pricing** | $0.04-0.32/s depending on model and resolution |
| **Self-hosted option** | Yes (isolated deployments) |

### 2. fal.ai

| Property | Value |
|----------|-------|
| **URL** | https://fal.ai |
| **Models** | LTX Video (original), LTX-2 Pro/Fast, LTX-2.3, LTX-2 19B |
| **Endpoints** | T2V, I2V, extend, retake, control, LoRA training, LoRA inference |
| **Auth** | FAL_KEY env variable |
| **SDKs** | Python (`fal-client`), JavaScript (`@fal-ai/client`) |
| **Pricing** | $0.04-0.24/s; training $0.0048/step |
| **Unique Feature** | LoRA training API (~30 min, ~$9.60) |

### 3. Replicate

| Property | Value |
|----------|-------|
| **URL** | https://replicate.com/lightricks/ltx-video |
| **Models** | LTX-Video (original) |
| **Resolution** | 768x512 at 24 FPS |
| **Hardware** | Nvidia L40S |
| **Prediction Time** | ~22 seconds |
| **Auth** | REPLICATE_API_TOKEN |
| **SDKs** | Python (`replicate`), JavaScript (`replicate`) |
| **Pricing** | ~$0.021/run (~47 runs/$1) |
| **Unique Feature** | Docker-based self-hosting via cog |

### 4. Segmind

| Property | Value |
|----------|-------|
| **URL** | https://www.segmind.com/models/ltx-video |
| **Endpoint** | `https://api.segmind.com/v1/ltx-video` |
| **Models** | LTX Video, LTX 2 Fast, LTX 2 Pro |
| **Auth** | x-api-key header |
| **Unique Feature** | Simple REST API with immediate response |

### 5. WaveSpeed AI

| Property | Value |
|----------|-------|
| **URL** | https://wavespeed.ai |
| **Models** | LTX-2.3, LTX-2 19B, LTX-Video 0.9.7 |
| **Endpoints** | T2V, I2V, Audio-to-video, Extend, Retake, Control |
| **Unique Feature** | No cold starts, portrait video (9:16) support |

### 6. Modal

| Property | Value |
|----------|-------|
| **URL** | https://modal.com |
| **Type** | Self-deployed serverless |
| **Models** | Any LTX model (user deploys) |
| **Unique Feature** | Custom deployment, Volume caching, warm container reuse |
| **Docs** | https://modal.com/docs/examples/ltx |

### 7. RunPod

| Property | Value |
|----------|-------|
| **URL** | https://www.runpod.io |
| **Type** | Cloud GPU rental + serverless endpoints |
| **Models** | Any LTX model (user deploys) |
| **Templates** | Community templates on Civitai |
| **Unique Feature** | Raw GPU access, serverless endpoints |

### 8. RunComfy

| Property | Value |
|----------|-------|
| **URL** | https://www.runcomfy.com |
| **Type** | Managed ComfyUI hosting |
| **Models** | LTX-2, LTX-2.3 via ComfyUI workflows |
| **Pricing** | Pay-as-you-go + Pro plan |
| **Unique Feature** | Pre-configured LTX workflows, Playground UI |

### 9. Hugging Face (Self-hosted via Diffusers)

| Property | Value |
|----------|-------|
| **URL** | https://huggingface.co/Lightricks |
| **Type** | Model hosting + library integration |
| **Models** | All LTX models (LTX-Video, LTX-2, LTX-2.3) |
| **Library** | `diffusers` (Python) |
| **Unique Feature** | Most downloaded open-source video model on HF |

---

## Pricing Comparison (Text-to-Video, 1080p)

| Provider | Fast/Standard | Pro/Quality |
|----------|--------------|-------------|
| Official LTX API (v2) | $0.04/s | $0.06/s |
| Official LTX API (v2.3) | $0.06/s | $0.08/s |
| fal.ai | $0.04/s | $0.06/s |
| Replicate | ~$0.021/run (~$0.001/s) | -- |
| Segmind | Varies | Varies |
| WaveSpeed AI | Varies | Varies |
| Self-hosted | GPU cost only | GPU cost only |

---

## Feature Availability by Provider

| Feature | Official API | fal.ai | Replicate | Segmind | WaveSpeed |
|---------|-------------|--------|-----------|---------|-----------|
| Text-to-Video | Yes | Yes | Yes | Yes | Yes |
| Image-to-Video | Yes | Yes | Yes | Yes | Yes |
| Audio-to-Video | Yes | Yes | No | No | Yes |
| Video Extend | Yes | Yes | No | No | Yes |
| Video Retake | Yes | Yes | No | No | Yes |
| LoRA Training | No | Yes | No | No | No |
| LoRA Inference | No | Yes | No | No | No |
| ControlNet | No | Yes | No | No | Yes |
| 4K Output | Yes | Yes | No | No | Yes |
| Audio Generation | Yes | Yes | No | No | Yes |

---

## Self-Hosting Requirements

For running LTX models locally or on your own infrastructure:

| Model | Min VRAM | Recommended VRAM |
|-------|----------|-----------------|
| LTX-Video 2B (distilled) | 8 GB | 16 GB |
| LTX-Video 13B | 16 GB | 24 GB |
| LTX-2 19B | 24 GB | 48 GB |
| LTX-2.3 22B | 24 GB | 48 GB |
| With FP8 quantization | ~40% less | ~40% less |
