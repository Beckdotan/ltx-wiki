---
title: Hugging Face Inference
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/api-huggingface-inference-providers.md
  - raw/api-inference-providers-overview.md
  - raw/integration-cloud-platforms.md
tags:
  - inference
  - cloud
  - huggingface
  - api
---

# Hugging Face Inference

Hugging Face serves as both a model hub and a unified inference gateway for LTX Video. It provides direct model downloads, a free serverless inference API, dedicated Inference Endpoints for production, and routes requests to third-party backend providers like [[fal-ai]] and [[replicate]].

**Model Hub:** https://huggingface.co/Lightricks/LTX-Video
**Inference API Docs:** https://huggingface.co/docs/api-inference
**Inference Endpoints:** https://huggingface.co/docs/inference-endpoints
**Inference Providers:** https://huggingface.co/docs/inference-providers

See also: [[inference-providers-overview]]

## Serverless Inference API

Free serverless inference API for supported models. For video generation, requests may be routed to partner providers.

**Endpoint:**
```
POST https://api-inference.huggingface.co/models/Lightricks/LTX-Video
```

**Authentication:**
```
Authorization: Bearer hf_YOUR_TOKEN
```

### Python Client

```bash
pip install huggingface_hub
```

```python
from huggingface_hub import InferenceClient

client = InferenceClient(token="hf_YOUR_TOKEN")

video = client.text_to_video(
    prompt="A sunset over the ocean",
    model="Lightricks/LTX-Video",
)
```

## Inference Endpoints (Dedicated)

For production use, Hugging Face Inference Endpoints provide dedicated GPU instances:

```python
from huggingface_hub import create_inference_endpoint

endpoint = create_inference_endpoint(
    name="ltx-video-endpoint",
    repository="Lightricks/LTX-Video-0.9.8-dev",
    framework="pytorch",
    task="text-to-video",
    accelerator="gpu",
    instance_type="nvidia-a100",
    instance_size="x1",
    region="us-east-1",
)

endpoint.wait()  # Wait for deployment
```

Approximate cost: $1-5/hour for a dedicated GPU instance.

## Third-Party Providers via HF Gateway

Hugging Face partners with inference providers that can be accessed transparently through the HF API:

- **[[fal-ai]]** -- Endpoint `fal-ai/ltx-video`, supports image-to-video and text-to-video
- **[[replicate]]** -- Model `lightricks/ltx-video`, accessible via Replicate API or HF gateway

## Hugging Face Spaces

Over 100 community Spaces host LTX Video demos. Notable Spaces include:

| Space | Creator | Purpose | Likes |
|-------|---------|---------|-------|
| LTX-2 Video [Turbo] | alexnasa | Fast generation with FA3 | 398 |
| LTX 2.3 Distilled | Lightricks | Cinematic video + audio | 299 |
| LTX 2.3 Sync | linoyts | Portrait animation/lipsync | 120 |
| LTX 2.3 First-Last Frame | linoyts | Frame-controlled generation | 96 |
| Camera Control Dolly | prithivMLmods | Camera motion control | 58 |
| Audio To Video | multimodalart | Lip-sync from audio | 49 |

These run on HuggingFace's free Zero GPU tier.

## Model Downloads

```python
from huggingface_hub import snapshot_download

# Download entire model
snapshot_download(
    repo_id="Lightricks/LTX-Video-0.9.8-dev",
    local_dir="./ltx-video-13b-dev"
)
```

## Available Model Variants

All LTX model variants are hosted on Hugging Face, including:
- LTX-Video (original 2B)
- LTX-Video 13B dev and 13B distilled
- LTX-2, LTX-2.3
- FP8 quantized versions
- Upscaler models
- ICLoRA weights

LTX Video is noted as the most downloaded open-source video model on Hugging Face.

## Feature Comparison with Other Providers

Hugging Face (via self-hosted diffusers or Inference Endpoints) provides the broadest model and feature support:
- All model variants accessible
- Multi-condition generation
- ICLoRA control
- Custom fine-tune support
- Full upscaler pipeline integration

This makes it the most flexible option for researchers and developers who need capabilities beyond what managed API providers offer.
