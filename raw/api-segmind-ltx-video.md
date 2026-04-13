# Segmind: LTX Video API

## Sources
- https://www.segmind.com/models/ltx-video/api
- https://www.segmind.com/models/ltx-video
- https://www.segmind.com/models/ltx-2-fast/api
- https://www.segmind.com/models/ltx-2-pro
- https://docs.segmind.com/home/api-reference

---

## Overview

Segmind provides serverless API access to LTX Video models with code examples in Python, JavaScript, and cURL. LTX-Video on Segmind generates 24 FPS videos at 768x512 resolution in real-time.

---

## Endpoint

```
POST https://api.segmind.com/v1/ltx-video
```

---

## Authentication

| Header | Value |
|--------|-------|
| `x-api-key` | Your Segmind API key |
| `Content-Type` | `application/json` |

---

## Available Models

| Model | Endpoint |
|-------|----------|
| LTX Video (original) | `https://api.segmind.com/v1/ltx-video` |
| LTX 2 Fast | `https://api.segmind.com/v1/ltx-2-fast` |
| LTX 2 Pro | `https://api.segmind.com/v1/ltx-2-pro` |

---

## Request Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `prompt` | string | Text prompt for video generation (longer, descriptive prompts work better) |
| `negative_prompt` | string | What to avoid in generation |
| `cfg` | float | Classifier-free guidance scale (default: 3) |
| `seed` | integer | Random seed for reproducibility (e.g., 2357108) |
| `steps` | integer | Number of inference steps (default: 30) |
| `length` | integer | Number of frames (default: 97) |
| `target_size` | integer | Target resolution size (default: 640) |
| `aspect_ratio` | string | Aspect ratio of output (e.g., "3:2"). Ignored if image is provided. |

---

## Python Example

```python
import requests

url = "https://api.segmind.com/v1/ltx-video"

headers = {
    "x-api-key": "YOUR_API_KEY",
    "Content-Type": "application/json"
}

data = {
    "prompt": "A detailed description of the scene you want to generate...",
    "negative_prompt": "worst quality, blurry",
    "cfg": 3,
    "seed": 2357108,
    "steps": 30,
    "length": 97,
    "target_size": 640,
    "aspect_ratio": "3:2"
}

response = requests.post(url, headers=headers, json=data)
print(response.status_code)
```

---

## Response Format

- Successful responses return status code 200 with JSON output
- Error messages are provided in the response body
- Standard HTTP status codes used for error handling

---

## Notes

- The model needs long descriptive prompts for best quality
- Short prompts will produce lower quality results
- Aspect ratio is ignored when an input image is provided
- Code examples available in Python, JavaScript, and cURL

---

## Related Links

- **API Documentation:** https://www.segmind.com/models/ltx-video/api
- **Segmind API Reference:** https://docs.segmind.com/home/api-reference
- **Model Page:** https://www.segmind.com/models/ltx-video
