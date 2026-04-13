# LTX Official REST API Documentation

## Sources
- https://docs.ltx.video/welcome
- https://docs.ltx.video/quickstart
- https://docs.ltx.video/authentication
- https://docs.ltx.video/pricing
- https://docs.ltx.video/api-changelog
- https://ltx.io/model/api
- https://console.ltx.video

---

## Overview

The LTX API is a production-grade REST API for video generation, powered by the LTX-2 and LTX-2.3 models from Lightricks. It enables video generation with synchronized audio through a single HTTP call, returning complete videos without polling or webhooks. The system supports up to 4K resolution and 20-second outputs per request.

The API is built on "the most downloaded open-source video model on Hugging Face."

## Base URL

```
https://api.ltx.video
```

## Developer Console

- **Console URL**: https://console.ltx.video
- **Playground**: https://console.ltx.video/playground/

---

## Authentication

All LTX API requests require authentication using API keys.

### Obtaining an API Key

Sign in to the Developer Console at https://console.ltx.video to generate API keys.

### Bearer Token Format

```
Authorization: Bearer YOUR_API_KEY
```

### Required Headers

| Header | Value | Required |
|--------|-------|----------|
| `Authorization` | `Bearer YOUR_API_KEY` | Yes |
| `Content-Type` | `application/json` | Yes (POST requests) |

### Environment Variable Setup

**Bash:**
```bash
export LTXV_API_KEY="your_api_key_here"
```

**Python:**
```python
import os
api_key = os.environ.get("LTXV_API_KEY")
```

**Node.js:**
```javascript
const apiKey = process.env.LTXV_API_KEY;
```

### Security Best Practices

- Never store keys in version control systems
- Avoid exposing keys in client-side code
- Store credentials in environment variables
- Implement periodic key rotation

### Error Responses

| Status Code | Description |
|-------------|-------------|
| 401 Unauthorized | Invalid API key, missing authorization header, expired key, or malformed header |
| 429 Too Many Requests | Rate limit exceeded (based on subscription tier) |

---

## Endpoints

### 1. Text-to-Video

**Endpoint:** `POST https://api.ltx.video/v1/text-to-video`

Generate video from a text description. Describe a scene, camera movement, and mood -- the API returns a complete video with matching audio.

**Example Request:**
```bash
curl -X POST https://api.ltx.video/v1/text-to-video \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "A beautiful sunset over the ocean with gentle waves",
    "model": "ltx-2-3-pro",
    "duration": 8,
    "resolution": "1920x1080"
  }'
```

**Response:** MP4 video file with associated metadata headers including a unique request identifier.

### 2. Image-to-Video

**Endpoint:** `POST https://api.ltx.video/v1/image-to-video`

Animate a still image with realistic motion, depth, and audio by providing a reference image and a prompt describing the desired motion. Preserves the visual identity of the source image.

**Parameters:**
- `image_uri` - URL or base64 of the source image
- `prompt` - Description of desired motion and dynamics
- `model` - Model variant (e.g., `ltx-2-3-pro`)
- `duration` - Output duration in seconds
- `resolution` - Output resolution (e.g., `1920x1080`)

### 3. Audio-to-Video

Generate video driven by an audio track. Supply dialogue, music, or ambient sound and the API produces visuals synchronized to the audio. Optional image conditioning supported.

**Note:** Requires Pro model.

### 4. Retake

**Endpoint:** `POST https://api.ltx.video/v1/retake`

Re-generate a specific section of an existing video without starting over. Select a time range and mode (replace video, audio, or both). The model strongly attends to surrounding frames, ensuring new content blends naturally with original motion, lighting, and tone.

- Select any 2-16 second segment within a video
- Regenerate just that moment while preserving surrounding footage

**Note:** Requires Pro model.

### 5. Extend

**Endpoint:** `POST https://api.ltx.video/v1/extend`

Lengthen videos from the beginning or end while maintaining continuity. Added February 18, 2026. For portrait sequences longer than 8-10 seconds, extend rather than regenerate to preserve subject consistency.

**Note:** Requires Pro model.

### 6. Upload

**Endpoint:** `POST https://api.ltx.video/v1/upload`

Upload assets via signed URLs. Added January 19, 2026.

---

## Supported Models

| Model | Type | Best For |
|-------|------|----------|
| `ltx-2-fast` | Speed-optimized | Quick iterations, prototyping |
| `ltx-2-pro` | Quality-optimized | Production output |
| `ltx-2-3-fast` | Latest, speed-optimized | Quick iterations with latest model |
| `ltx-2-3-pro` | Latest, quality-optimized | Production output with latest model |

---

## Pricing

Video generation is billed per second of output video. Higher resolution and premium models have proportionally higher costs. Per-second pricing with no minimums or hidden fees.

### Text-to-Video and Image-to-Video

| Model | 1080p | 1440p | 2160p (4K) |
|-------|-------|-------|-------------|
| ltx-2-fast | $0.04/s | $0.08/s | $0.16/s |
| ltx-2-pro | $0.06/s | $0.12/s | $0.24/s |
| ltx-2-3-fast | $0.06/s | $0.12/s | $0.24/s |
| ltx-2-3-pro | $0.08/s | $0.16/s | $0.32/s |

*LTX 2.3 prices effective April 1, 2026.*

### Audio-to-Video, Retake, and Extend

| Model | 1080p |
|-------|-------|
| ltx-2-pro | $0.10/s |
| ltx-2-3-pro | $0.10/s |

---

## Rate Limits

API keys are subject to rate limits based on your plan. If you exceed your rate limit, you receive a `429 Too Many Requests` response. Contact support for rate limit increases.

---

## API Changelog (2026)

- **April 1, 2026**: Updated pricing for LTX-2.3 Text-to-Video and Image-to-Video endpoints
- **February 18, 2026**: New `v1/extend` endpoint for extending video duration
- **January 19, 2026**: New `v1/upload` endpoint for uploading assets via signed URLs

---

## Generation Workflow

All generation is asynchronous:
1. Submit a generation request
2. Receive a job ID
3. Poll job status
4. Retrieve completed video when status returns "complete"

Videos can be generated up to 4K resolution and 20 seconds per request.

---

## Deployment Options

The LTX Model supports:
- **Managed API**: Higher per-minute costs; rate limits; ideal for exploratory batches
- **Self-Hosted**: Lower marginal cost; requires 24 GB VRAM minimum; full batching control
- **Isolated deployments**: For organizations with strict requirements (enterprise, financial, governmental)
