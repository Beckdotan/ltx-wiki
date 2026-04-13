# LTX-Video on fal.ai

**Source:** https://fal.ai/models/fal-ai/ltx-video
**Date:** 2026-04-13

---

## Overview

fal.ai provides serverless GPU inference for LTX-Video models. Multiple model variants are available including the 13B dev and 13B distilled versions. fal.ai offers both REST API and client SDKs.

**Model endpoints:**
- 13B Dev (Image-to-Video): https://fal.ai/models/fal-ai/ltx-video-13b-dev/image-to-video
- 13B Distilled (Image-to-Video): https://fal.ai/models/fal-ai/ltx-video-13b-distilled/image-to-video
- Standard: https://fal.ai/models/fal-ai/ltx-video

---

## Authentication

All fal.ai API calls require a key:
```
Authorization: Key YOUR_FAL_KEY
```

Get your key at: https://fal.ai/dashboard/keys

---

## Python Client

### Installation
```bash
pip install fal-client
```

### Image-to-Video Example
```python
import fal_client

result = fal_client.subscribe(
    "fal-ai/ltx-video/image-to-video",
    arguments={
        "prompt": "A cinematic aerial view of a city at sunset",
        "image_url": "https://example.com/input.jpg",
        "num_frames": 97,
        "width": 768,
        "height": 512,
        "num_inference_steps": 30,
        "seed": 42,
    },
)

print(result["video"]["url"])
```

### Text-to-Video Example
```python
import fal_client

result = fal_client.subscribe(
    "fal-ai/ltx-video",
    arguments={
        "prompt": "Ocean waves crashing on a rocky shore during golden hour",
        "num_frames": 97,
        "width": 768,
        "height": 512,
    },
)

print(result["video"]["url"])
```

### Async / Queue-Based Usage
```python
import fal_client

# Submit to queue
handler = fal_client.submit(
    "fal-ai/ltx-video/image-to-video",
    arguments={
        "prompt": "A bird flying over a mountain landscape",
        "image_url": "https://example.com/bird.jpg",
    },
)

# Check status
status = handler.status()
print(status)

# Get result (blocks until complete)
result = handler.get()
print(result["video"]["url"])
```

---

## JavaScript Client

### Installation
```bash
npm install @fal-ai/client
```

### Usage
```javascript
import { fal } from "@fal-ai/client";

fal.config({
  credentials: process.env.FAL_KEY,
});

const result = await fal.subscribe("fal-ai/ltx-video/image-to-video", {
  input: {
    prompt: "A cinematic aerial view of a city at sunset",
    image_url: "https://example.com/input.jpg",
    num_frames: 97,
  },
});

console.log(result.data.video.url);
```

---

## REST API

### Submit Request
```bash
curl -X POST \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "A sunset over the ocean",
    "image_url": "https://example.com/input.jpg",
    "num_frames": 97
  }' \
  https://queue.fal.run/fal-ai/ltx-video/image-to-video
```

### Check Status
```bash
curl -H "Authorization: Key $FAL_KEY" \
  https://queue.fal.run/fal-ai/ltx-video/image-to-video/requests/REQUEST_ID/status
```

### Get Result
```bash
curl -H "Authorization: Key $FAL_KEY" \
  https://queue.fal.run/fal-ai/ltx-video/image-to-video/requests/REQUEST_ID
```

---

## Input Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of desired video |
| `image_url` | string | optional | URL of conditioning image |
| `video_url` | string | optional | URL of conditioning video |
| `num_frames` | integer | 97 | Number of output frames |
| `width` | integer | 768 | Output width |
| `height` | integer | 512 | Output height |
| `num_inference_steps` | integer | 30 | Denoising steps |
| `guidance_scale` | float | 7.5 | CFG scale |
| `seed` | integer | random | Reproducibility seed |
| `negative_prompt` | string | "" | Negative prompt |

---

## Output Format

```json
{
  "video": {
    "url": "https://fal.media/files/..../output.mp4",
    "content_type": "video/mp4",
    "file_name": "output.mp4",
    "file_size": 1234567
  },
  "seed": 42,
  "timings": {
    "inference": 12.5
  }
}
```

---

## Pricing

fal.ai uses per-second GPU billing:
- Pricing depends on GPU tier and model size
- 13B models use more expensive GPU instances
- Check https://fal.ai/pricing for current rates
- Webhook support for async processing

---

## Available Model Variants on fal.ai

| Endpoint | Model | Use Case |
|----------|-------|----------|
| `fal-ai/ltx-video` | Standard | Text-to-video |
| `fal-ai/ltx-video/image-to-video` | Standard | Image-to-video |
| `fal-ai/ltx-video-13b-dev/image-to-video` | 13B Dev | Highest quality I2V |
| `fal-ai/ltx-video-13b-distilled/image-to-video` | 13B Distilled | Fast I2V |

---

## Related Links

- **fal.ai Documentation:** https://docs.fal.ai
- **fal.ai Python Client:** https://github.com/fal-ai/fal
- **fal.ai JavaScript Client:** https://github.com/fal-ai/fal-js
- **Dashboard:** https://fal.ai/dashboard
