---
title: Segmind
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/api-segmind-ltx-video.md
  - raw/inference-providers-overview.md
tags:
  - inference
  - cloud
  - api
  - segmind
---

# Segmind

Segmind provides serverless API access to LTX Video models with a simple REST interface. It generates 24 FPS videos at 768x512 resolution and offers code examples in Python, JavaScript, and cURL.

**Website:** https://www.segmind.com/models/ltx-video
**API docs:** https://docs.segmind.com/home/api-reference

See also: [[inference-providers-overview]]

## Available Models

| Model | Endpoint |
|-------|----------|
| LTX Video (original) | `https://api.segmind.com/v1/ltx-video` |
| LTX 2 Fast | `https://api.segmind.com/v1/ltx-2-fast` |
| LTX 2 Pro | `https://api.segmind.com/v1/ltx-2-pro` |

## Authentication

| Header | Value |
|--------|-------|
| `x-api-key` | Your Segmind API key |
| `Content-Type` | `application/json` |

## Request Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text prompt (longer, descriptive prompts work better) |
| `negative_prompt` | string | optional | What to avoid in generation |
| `cfg` | float | 3 | Classifier-free guidance scale |
| `seed` | integer | optional | Random seed for reproducibility |
| `steps` | integer | 30 | Number of inference steps |
| `length` | integer | 97 | Number of frames |
| `target_size` | integer | 640 | Target resolution size |
| `aspect_ratio` | string | optional | Aspect ratio (e.g., "3:2"); ignored if image is provided |

## Python Example

```python
import requests

url = "https://api.segmind.com/v1/ltx-video"

headers = {
    "x-api-key": "YOUR_API_KEY",
    "Content-Type": "application/json"
}

data = {
    "prompt": "A detailed description of the scene...",
    "negative_prompt": "worst quality, blurry",
    "cfg": 3,
    "seed": 2357108,
    "steps": 30,
    "length": 97,
    "target_size": 640,
    "aspect_ratio": "3:2"
}

response = requests.post(url, headers=headers, json=data)
```

## Notes

- The model produces best results with long, descriptive prompts
- Short prompts will produce lower quality results
- Aspect ratio parameter is ignored when an input image is provided
- Responses return standard HTTP status codes with JSON output

## Limitations

Compared to [[fal-ai]] and [[wavespeed-ai]], Segmind offers a more limited feature set -- it supports text-to-video and image-to-video but does not offer audio-to-video, video extend, retake, LoRA, or ControlNet capabilities.
