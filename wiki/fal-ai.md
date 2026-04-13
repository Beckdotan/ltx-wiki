---
title: fal.ai
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/api-fal-ai-ltx-video.md
  - raw/api-fal-ai-ltx.md
  - raw/inference-providers-overview.md
  - raw/cloud-deployment-platforms.md
tags:
  - inference
  - cloud
  - api
  - fal-ai
  - lora-training
---

# fal.ai

fal.ai is a serverless GPU inference platform that hosts multiple LTX Video model variants. It is listed as an official deployment partner by Lightricks and offers the broadest feature set among third-party inference providers, including LoRA training via API.

**Website:** https://fal.ai
**Dashboard:** https://fal.ai/dashboard
**Docs:** https://docs.fal.ai

See also: [[inference-providers-overview]]

## Authentication

All API calls require a key set as the `FAL_KEY` environment variable or passed via header:

```
Authorization: Key YOUR_FAL_KEY
```

Keys are managed at https://fal.ai/dashboard/keys

## Available Model Endpoints

### LTX Video (Original)
- `fal-ai/ltx-video` -- Text-to-video
- `fal-ai/ltx-video/image-to-video` -- Image-to-video
- `fal-ai/ltx-video-13b-dev/image-to-video` -- 13B dev (highest quality I2V)
- `fal-ai/ltx-video-13b-distilled/image-to-video` -- 13B distilled (fast I2V)

### LTX Video 2.0
- `fal-ai/ltx-2/text-to-video` -- Pro T2V
- `fal-ai/ltx-2/image-to-video` -- Pro I2V
- `fal-ai/ltx-2/text-to-video/fast` -- Fast T2V
- `fal-ai/ltx-2/image-to-video/fast` -- Fast I2V
- `fal-ai/ltx-2/extend-video` -- Video extension
- `fal-ai/ltx-2/video-to-video` -- Video-to-video

### LTX-2.3
- `fal-ai/ltx-2.3/text-to-video` -- T2V (standard and fast)
- `fal-ai/ltx-2.3/image-to-video` -- I2V (standard and fast)
- `fal-ai/ltx-2.3/extend-video` -- Video extension

### LTX-2 19B
- `fal-ai/ltx-2-19b/video-to-video/lora` -- V2V with LoRA
- Control endpoints for pose, depth, and canny edge guidance

### LoRA Training and Inference
- `fal-ai/ltx-video-trainer` -- Original LTX trainer
- `fal-ai/ltx2-video-trainer` -- LTX-2 trainer
- `fal-ai/ltx-video-lora` -- LoRA T2V inference
- `fal-ai/ltx-video-lora/image-to-video` -- LoRA I2V inference
- `fal-ai/ltx-video-lora/multiconditioning` -- Multi-conditioning LoRA

## SDKs

### Python
```bash
pip install fal-client
```

```python
import fal_client

result = fal_client.subscribe("fal-ai/ltx-2/text-to-video", arguments={
    "prompt": "A cowboy walking through a dusty town...",
    "duration": 8,
    "resolution": "1080p",
    "fps": 25,
    "generate_audio": True
})
```

### JavaScript
```bash
npm install @fal-ai/client
```

Note: `@fal-ai/serverless-client` has been deprecated in favor of `@fal-ai/client`.

```javascript
import { fal } from "@fal-ai/client";

const result = await fal.subscribe("fal-ai/ltx-video/image-to-video", {
  input: {
    prompt: "A cinematic aerial view of a city at sunset",
    image_url: "https://example.com/input.jpg",
    num_frames: 97,
  },
});
```

### Queue-Based (Async) Usage

```python
handler = fal_client.submit("fal-ai/ltx-video/image-to-video", arguments={...})
status = handler.status()
result = handler.get()  # blocks until complete
```

### REST API (cURL)

```bash
curl -X POST \
  -H "Authorization: Key $FAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "A sunset over the ocean", "num_frames": 97}' \
  https://queue.fal.run/fal-ai/ltx-video/image-to-video
```

## Key Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `prompt` | string | required | Text description of desired video |
| `image_url` | string | optional | URL of conditioning image (PNG, JPEG, WebP, AVIF, HEIF) |
| `duration` | number | varies | Duration in seconds |
| `resolution` | string | varies | Output resolution ("1080p", "1440p", "2160p") |
| `fps` | integer | varies | Frames per second |
| `num_frames` | integer | 97 | Number of output frames |
| `num_inference_steps` | integer | 30 | Denoising steps (40+ for quality, 20-30 for speed) |
| `guidance_scale` | float | 7.5 | CFG scale (4-6 creative, 7-9 constrained) |
| `seed` | integer | random | Reproducibility seed |
| `generate_audio` | boolean | false | Enable audio generation |

## Output Format

```json
{
  "video": {
    "url": "https://fal.media/files/.../output.mp4",
    "content_type": "video/mp4",
    "file_name": "output.mp4",
    "file_size": 1234567
  },
  "seed": 42,
  "timings": { "inference": 12.5 }
}
```

## Pricing

### Video Generation (per second of output)

| Variant | 1080p | 1440p | 2160p (4K) |
|---------|-------|-------|-------------|
| Pro | $0.06/s | $0.12/s | $0.24/s |
| Fast | $0.04/s | $0.08/s | $0.16/s |

### Other Operations
- Audio-to-video: $0.10/s
- Extend-video: $0.10/s
- Retake-video: $0.10/s

### LoRA Training
- $0.0048 per step
- Default 2000-step run: ~$9.60
- Training completes in approximately 30 minutes

Pay-per-second with no minimums.

## LoRA Training API

fal.ai is the only provider offering a LoRA training API for LTX Video. Dataset requirements:

- 10-50 training videos demonstrating target style or motion
- Supported video formats: .mp4, .mov, .avi, .mkv
- Supported image formats: .png, .jpg, .jpeg
- Dataset must contain ONLY videos OR ONLY images (no mixed datasets)
- Provide a URL to a zip archive with training data

```python
result = fal_client.subscribe("fal-ai/ltx2-video-trainer", arguments={
    "dataset_url": "https://example.com/training-data.zip",
    "steps": 2000,
    "trigger_word": "my_style"
})
```

Training guides:
- https://fal.ai/learn/devs/ltx-2-video-trainer-user-guide
- https://fal.ai/learn/devs/ltx-2-video-trainer-prompt-guide

## Hugging Face Integration

fal.ai is accessible as a backend provider through the [[huggingface-inference]] API, meaning requests to `Lightricks/LTX-Video` on HF may be routed to fal.ai infrastructure.
