# fal.ai LTX Video API

## Sources
- https://fal.ai/models/fal-ai/ltx-video/image-to-video/api
- https://fal.ai/models/fal-ai/ltx-2/text-to-video/api
- https://fal.ai/models/fal-ai/ltx-2/image-to-video/api
- https://fal.ai/models/fal-ai/ltx-2.3/image-to-video/api
- https://fal.ai/models/fal-ai/ltx-2.3/text-to-video/api
- https://fal.ai/models/fal-ai/ltx-2/extend-video/api
- https://fal.ai/models/fal-ai/ltx-2/text-to-video/fast/api
- https://fal.ai/models/fal-ai/ltx-2/image-to-video/fast/api
- https://fal.ai/ltx-2.3
- https://fal.ai/pricing
- https://fal.ai/learn/devs/ltx-video-2-pro-image-to-video-developer-guide

---

## Overview

fal.ai provides a serverless AI inference platform hosting multiple LTX Video model variants. The platform offers REST APIs, client SDKs for Python and JavaScript, and interactive playgrounds for each model.

---

## Authentication

Set `FAL_KEY` as an environment variable in your runtime:

```bash
export FAL_KEY="your_fal_api_key"
```

---

## Available Model Endpoints

### LTX Video (Original)
- **Image-to-Video:** `fal-ai/ltx-video/image-to-video`
- **Text-to-Video:** `fal-ai/ltx-video`

### LTX Video 2.0 Pro
- **Text-to-Video:** `fal-ai/ltx-2/text-to-video`
- **Image-to-Video:** `fal-ai/ltx-2/image-to-video`
- **Extend Video:** `fal-ai/ltx-2/extend-video`
- **Video-to-Video:** `fal-ai/ltx-2/video-to-video`

### LTX Video 2.0 Fast
- **Text-to-Video:** `fal-ai/ltx-2/text-to-video/fast`
- **Image-to-Video:** `fal-ai/ltx-2/image-to-video/fast`

### LTX-2.3
- **Text-to-Video:** `fal-ai/ltx-2.3/text-to-video`
- **Image-to-Video:** `fal-ai/ltx-2.3/image-to-video`
- **Text-to-Video (Fast):** `fal-ai/ltx-2.3/text-to-video/fast`
- **Image-to-Video (Fast):** `fal-ai/ltx-2.3/image-to-video/fast`
- **Extend Video:** `fal-ai/ltx-2.3/extend-video`

### LTX Video-0.9.5
- **Extend:** `fal-ai/ltx-video-v095/extend`

### LTX Video Trainer
- **Training:** `fal-ai/ltx-video-trainer`
- **LTX-2 Trainer:** `fal-ai/ltx2-video-trainer`

### LTX Video LoRA Inference
- **Text-to-Video:** `fal-ai/ltx-video-lora`
- **Image-to-Video:** `fal-ai/ltx-video-lora/image-to-video`
- **Multi-conditioning:** `fal-ai/ltx-video-lora/multiconditioning`

### LTX-2 19B Variants
- **Video-to-Video LoRA:** `fal-ai/ltx-2-19b/video-to-video/lora`
- **Control:** (for pose, depth, canny edge guidance)

---

## SDK Installation

### Python
```bash
pip install fal-client
```

### JavaScript
```bash
npm install --save @fal-ai/client
```

Note: `@fal-ai/serverless-client` has been deprecated in favor of `@fal-ai/client`.

---

## Code Examples

### JavaScript: Text-to-Video

```javascript
import { fal } from "@fal-ai/client";

const { request_id } = await fal.queue.submit("fal-ai/ltx-2/text-to-video", {
  input: {
    prompt: "A cowboy walking through a dusty town...",
    duration: 8,
    resolution: "1080p",
    fps: 25,
    generate_audio: true
  }
});

// Check status
const status = await fal.queue.status("fal-ai/ltx-2/text-to-video", {
  requestId: request_id,
});

// Get result
const result = await fal.queue.result("fal-ai/ltx-2/text-to-video", {
  requestId: request_id,
});
```

### Python: Text-to-Video

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

---

## Key Request Parameters

### Image-to-Video
- `image_url` (required): Publicly accessible URL or base64 data URI (supports PNG, JPEG, WebP, AVIF, HEIF)
- `prompt` (required): Text describing motion and scene dynamics

### Text-to-Video
- `prompt` (required): Text description of the desired video
- `duration`: Video duration in seconds
- `resolution`: Output resolution (e.g., "1080p", "1440p", "2160p")
- `fps`: Frames per second (e.g., 25)
- `generate_audio`: Boolean to enable audio generation

### Configuration
- **Inference steps:** 40+ for quality, 20-30 for speed
- **Duration:** 4-8s at 16-24 fps is the sweet spot for stable motion
- **CFG/Guidance:** 4-6 for creative freedom, 7-9 for constrained outputs

---

## Pricing

### Text-to-Video and Image-to-Video

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
- Training typically completes in minutes

Pay-per-second with no minimums.

---

## LTX-2 Video Trainer (LoRA Training API)

### Overview
Train custom LTX-2 video generation models in ~30 minutes via API.

### Dataset Requirements
- Upload 10-50 training videos demonstrating your target style or motion
- Supported video formats: .mp4, .mov, .avi, .mkv
- Supported image formats: .png, .jpg, .jpeg
- Dataset must contain ONLY videos OR ONLY images (no mixed datasets)
- Provide a URL to a zip archive with the training data

### Training Parameters
- LoRA rank configuration
- Training steps (default: 2000)
- Custom trigger words

### Usage

```python
import fal_client

# Blocking (waits until training completes)
result = fal_client.subscribe("fal-ai/ltx2-video-trainer", arguments={
    "dataset_url": "https://example.com/training-data.zip",
    "steps": 2000,
    "trigger_word": "my_style"
})

# Non-blocking
handler = fal_client.submit("fal-ai/ltx2-video-trainer", arguments={...})
status = fal_client.status("fal-ai/ltx2-video-trainer", handler.request_id)
```

### Documentation
- User Guide: https://fal.ai/learn/devs/ltx-2-video-trainer-user-guide
- Prompt Guide: https://fal.ai/learn/devs/ltx-2-video-trainer-prompt-guide
- API Docs: https://fal.ai/models/fal-ai/ltx2-video-trainer/api
