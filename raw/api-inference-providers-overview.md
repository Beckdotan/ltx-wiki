# LTX-Video Inference Providers Overview

**Source:** https://huggingface.co/Lightricks/LTX-Video (demo links section)
**Date:** 2026-04-13

---

## Overview

LTX-Video models can be accessed through multiple inference providers, ranging from self-hosted to fully managed cloud APIs. This document provides a comparison of all known hosting options.

---

## Provider Comparison

| Provider | Type | API? | Models Available | Auth Method | Pricing Model |
|----------|------|------|-----------------|-------------|---------------|
| **LTX Studio** | Web app | No (UI only) | 13B mix, 13B distilled | Account login | Free tier + paid plans |
| **fal.ai** | Serverless API | Yes | 13B dev, 13B distilled, standard | API key | Per-second GPU billing |
| **Replicate** | Serverless API | Yes | LTX-Video (various) | API token | Per-second GPU billing |
| **Hugging Face** | API + Endpoints | Yes | All variants | HF token | Free tier + paid endpoints |
| **Self-hosted** | Local inference | N/A | All variants | N/A | Your own GPU costs |
| **ComfyUI** | Local UI | N/A | All variants | N/A | Your own GPU costs |

---

## Detailed Provider Information

### 1. LTX Studio (app.ltx.studio)
- **URL:** https://app.ltx.studio/
- **Type:** Commercial web application by Lightricks
- **Access:** Browser-based, no coding required
- **Models:** 13B mix (`?videoModel=ltxv-13b`), 13B distilled (`?videoModel=ltxv`)
- **API:** No public API available
- **Best for:** Non-technical users, quick experimentation

### 2. fal.ai
- **URL:** https://fal.ai/models/fal-ai/ltx-video
- **Type:** Serverless GPU inference platform
- **Access:** REST API, Python SDK (`fal-client`), JavaScript SDK (`@fal-ai/client`)
- **Models:** 13B dev, 13B distilled, standard LTX-Video
- **Endpoints:**
  - `fal-ai/ltx-video` (text-to-video)
  - `fal-ai/ltx-video/image-to-video` (image-to-video)
  - `fal-ai/ltx-video-13b-dev/image-to-video` (13B dev)
  - `fal-ai/ltx-video-13b-distilled/image-to-video` (13B distilled)
- **Best for:** Developers wanting fast API access with good SDK support

### 3. Replicate
- **URL:** https://replicate.com/lightricks/ltx-video
- **Type:** Serverless model hosting platform
- **Access:** REST API, Python SDK (`replicate`), JavaScript SDK (`replicate`)
- **Models:** LTX-Video (versions managed by Replicate)
- **Features:** Webhooks, streaming, versioned predictions
- **Best for:** Developers familiar with the Replicate ecosystem

### 4. Hugging Face
- **URL:** https://huggingface.co/Lightricks/LTX-Video
- **Type:** Model hub + inference API + dedicated endpoints
- **Access:** HF Inference API, Inference Endpoints, direct model download
- **Models:** All variants (13B dev, 13B distilled, 2B, FP8, upscalers, ICLoRA)
- **SDK:** `huggingface_hub`, `diffusers`
- **Best for:** Researchers, those wanting full model access and flexibility

### 5. Self-Hosted (Local)
- **GitHub:** https://github.com/Lightricks/LTX-Video
- **Type:** Local inference on your own GPU
- **Access:** Python API via diffusers, CLI via inference.py
- **Models:** All variants (downloaded from HF)
- **Requirements:** CUDA GPU with 6-24GB+ VRAM depending on model
- **Best for:** Privacy-sensitive use cases, custom workflows, fine-tuning

### 6. ComfyUI
- **GitHub:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Type:** Node-based visual workflow interface
- **Access:** GUI (local installation)
- **Models:** All variants
- **Best for:** Artists, visual workflow designers, complex multi-step pipelines

---

## Feature Comparison by Provider

| Feature | LTX Studio | fal.ai | Replicate | HF Endpoints | Self-Hosted |
|---------|-----------|--------|-----------|-------------|-------------|
| Image-to-Video | Yes | Yes | Yes | Yes | Yes |
| Text-to-Video | Yes | Yes | Yes | Yes | Yes |
| Video-to-Video | Unclear | Varies | Varies | Yes | Yes |
| Multi-Condition | No | No | No | Yes | Yes |
| ICLoRA Control | No | No | No | Yes | Yes |
| Upscaler Pipeline | Built-in | Varies | Varies | Yes | Yes |
| Custom Fine-tunes | No | No | Custom versions | Yes | Yes |
| Batch Processing | No | Yes (queue) | Yes (queue) | Yes | Yes |
| Webhooks | No | Yes | Yes | Yes | N/A |

---

## Pricing Guidance (as of 2026)

| Provider | Cost Estimate | Notes |
|----------|---------------|-------|
| LTX Studio | Free tier + paid | Limited free generations |
| fal.ai | ~$0.01-0.10/video | Per-second GPU billing |
| Replicate | ~$0.01-0.05/video | Per-second GPU billing |
| HF Endpoints | ~$1-5/hour | Dedicated GPU instance |
| Self-Hosted | GPU electricity | Requires GPU purchase/rental |

*Prices are approximate and subject to change. Check provider websites for current rates.*

---

## Quick Decision Guide

- **Just want to try it:** LTX Studio (web UI, no setup)
- **Building an app (Python):** fal.ai or Replicate
- **Building an app (JavaScript):** fal.ai or Replicate
- **Need maximum control:** Self-hosted with diffusers
- **Complex visual workflows:** ComfyUI
- **Research/fine-tuning:** Self-hosted from Hugging Face
- **Production at scale:** HF Inference Endpoints or fal.ai

---

## Related Links

- **LTX Studio:** https://app.ltx.studio/
- **fal.ai:** https://fal.ai/models/fal-ai/ltx-video
- **Replicate:** https://replicate.com/lightricks/ltx-video
- **Hugging Face:** https://huggingface.co/Lightricks/LTX-Video
- **GitHub:** https://github.com/Lightricks/LTX-Video
- **ComfyUI Nodes:** https://github.com/Lightricks/ComfyUI-LTXVideo
