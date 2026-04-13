# Replicate: LTX-Video API

## Sources
- https://replicate.com/lightricks/ltx-video
- https://replicate.com/lightricks/ltx-video/api
- https://replicate.com/lightricks/ltx-video/api/schema
- https://replicate.com/lightricks/ltx-video/api/learn-more
- https://replicate.com/lightricks/ltx-video/api/api-reference
- https://replicate.com/chenxwh/ltx-video/readme

---

## Overview

LTX-Video is hosted on Replicate as a real-time video generation model that creates 24 FPS videos at 768x512 resolution from text or images. It is the first DiT-based video generation model capable of generating high-quality videos in real-time.

---

## Key Details

| Property | Value |
|----------|-------|
| **Model** | lightricks/ltx-video |
| **Cost** | ~$0.021 per run (~47 runs per $1) |
| **Hardware** | Nvidia L40S GPU |
| **Prediction time** | ~22 seconds |
| **Open source** | Yes (can run locally with Docker) |

---

## Authentication

Replicate uses API tokens for authentication:

```bash
export REPLICATE_API_TOKEN="your_token_here"
```

Get your token at: https://replicate.com/account/api-tokens

---

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

### Get Prediction Status

```bash
curl -s \
  -H "Authorization: Bearer $REPLICATE_API_TOKEN" \
  https://api.replicate.com/v1/predictions/PREDICTION_ID
```

---

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

For the complete schema, see: https://replicate.com/lightricks/ltx-video/api/schema

---

## Output

The API returns a URL to the generated MP4 video file hosted on Replicate's CDN.

---

## Pricing

- ~$0.021 per run (~47 runs per $1)
- Billed per-second based on GPU time (Nvidia L40S)
- See: https://replicate.com/pricing

---

## Webhooks

```bash
curl -s -X POST \
  -H "Authorization: Bearer $REPLICATE_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "version": "MODEL_VERSION_ID",
    "input": {
      "prompt": "A sunset over the ocean"
    },
    "webhook": "https://your-server.com/webhook",
    "webhook_events_filter": ["completed"]
  }' \
  https://api.replicate.com/v1/predictions
```

---

## Self-Hosting

The model is open source and can be run on your own computer with Docker via Replicate's `cog` tool.

---

## Community Versions

- `chenxwh/ltx-video` - Community implementation with additional features

---

## Related Links

- **Replicate Documentation:** https://replicate.com/docs
- **Python Client Docs:** https://replicate.com/docs/get-started/python
- **API Reference:** https://replicate.com/docs/reference/http
- **Model Page:** https://replicate.com/lightricks/ltx-video
