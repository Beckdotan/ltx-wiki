---
title: Replicate
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/api-replicate-ltx-video.md
  - raw/inference-providers-overview.md
  - raw/integration-cloud-platforms.md
tags:
  - inference
  - cloud
  - api
  - replicate
---

# Replicate

Replicate is a serverless model hosting platform that provides API access to LTX-Video. It is listed as an official deployment partner by Lightricks. The model runs on Nvidia L40S hardware with a typical prediction time of approximately 22 seconds.

**Model page:** https://replicate.com/lightricks/ltx-video
**Docs:** https://replicate.com/docs

See also: [[inference-providers-overview]]

## Key Details

| Property | Value |
|----------|-------|
| Model ID | `lightricks/ltx-video` |
| Cost | ~$0.021 per run (~47 runs per $1) |
| Hardware | Nvidia L40S GPU |
| Prediction time | ~22 seconds |
| Output | 768x512 at 24 FPS |
| Open source | Yes (can self-host with Docker via `cog`) |

## Authentication

```bash
export REPLICATE_API_TOKEN="your_token_here"
```

Tokens managed at: https://replicate.com/account/api-tokens

## API Usage

### Python

```bash
pip install replicate
```

```python
import replicate

output = replicate.run(
    "lightricks/ltx-video",
    input={
        "prompt": "A beautiful sunset over the ocean with gentle waves"
    }
)
```

### JavaScript

```bash
npm install replicate
```

```javascript
import Replicate from "replicate";

const replicate = new Replicate();
const output = await replicate.run("lightricks/ltx-video", {
  input: {
    prompt: "A beautiful sunset over the ocean with gentle waves"
  }
});
```

### cURL

```bash
curl -s -X POST \
  -H "Authorization: Bearer $REPLICATE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "version": "MODEL_VERSION_ID",
    "input": {
      "prompt": "A beautiful sunset over the ocean with gentle waves"
    }
  }' \
  https://api.replicate.com/v1/predictions
```

## Input Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of the desired video |
| `image` | URI | optional | Input image for image-to-video |
| `negative_prompt` | string | "" | What to avoid in generation |
| `width` | integer | 768 | Output width (divisible by 32) |
| `height` | integer | 512 | Output height (divisible by 32) |
| `num_frames` | integer | 97 | Number of frames (must be 8n+1) |
| `guidance_scale` | float | 7.5 | Classifier-free guidance scale |
| `num_inference_steps` | integer | 30 | Number of denoising steps |
| `seed` | integer | random | Random seed for reproducibility |

Full schema: https://replicate.com/lightricks/ltx-video/api/schema

## Output

The API returns a URL to the generated MP4 video file hosted on Replicate's CDN.

## Webhooks

Replicate supports webhook notifications for async prediction completion:

```bash
curl -s -X POST \
  -H "Authorization: Bearer $REPLICATE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "version": "MODEL_VERSION_ID",
    "input": { "prompt": "A sunset over the ocean" },
    "webhook": "https://your-server.com/webhook",
    "webhook_events_filter": ["completed"]
  }' \
  https://api.replicate.com/v1/predictions
```

## Self-Hosting

The model is open source and can be self-hosted using Replicate's `cog` tool with Docker.

## Community Versions

- `chenxwh/ltx-video` -- Community implementation with additional features

## Limitations

Compared to [[fal-ai]] and [[wavespeed-ai]], Replicate currently hosts only the original LTX-Video model and does not support newer variants (LTX-2, LTX-2.3, 19B). It also lacks audio-to-video, video extend, retake, LoRA, and ControlNet features.

## Hugging Face Integration

Replicate is accessible as a backend through the [[huggingface-inference]] API gateway.
