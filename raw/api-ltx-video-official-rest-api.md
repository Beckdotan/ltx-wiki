# LTX Video Official REST API (docs.ltx.video)

**Source:** https://docs.ltx.video/
**Fetched:** 2026-04-13

---

## Overview

Lightricks operates an official REST API for LTX Video generation at `api.ltx.video`. This is a production-grade API that generates video with synchronized audio in a single HTTP request -- no polling, webhooks, or complex infrastructure required. The response is a binary MP4 file returned directly.

**Base URL:** `https://api.ltx.video/v1/`
**Documentation:** https://docs.ltx.video/
**Developer Console:** https://console.ltx.video/
**Playground:** https://console.ltx.video/playground/

---

## Authentication

All API requests require a Bearer token in the Authorization header.

- **Get API Key:** Visit https://console.ltx.video to generate your API key
- **Header Format:** `Authorization: Bearer YOUR_API_KEY`
- **Token Type:** Bearer token associated with your account

### Security Best Practices
- Never commit API keys to version control
- Avoid exposing keys in client-side code
- Use environment variables for storage
- Rotate keys periodically

### Error Handling
- `401 Unauthorized` - Missing, incorrect, or expired API key
- `429 Too Many Requests` - Rate limit exceeded (includes `Retry-After` header)

---

## Available Endpoints

| Endpoint | Method | URL | Description |
|----------|--------|-----|-------------|
| Text-to-Video | POST | `/v1/text-to-video` | Generate video from text prompt |
| Image-to-Video | POST | `/v1/image-to-video` | Animate a static image |
| Audio-to-Video | POST | `/v1/audio-to-video` | Generate video synced to audio |
| Retake | POST | `/v1/retake` | Re-generate a section of video |
| Extend | POST | `/v1/extend` | Lengthen an existing video |
| Upload | POST | `/v1/upload` | Get pre-signed upload URL |

---

## Models

### LTX-2.3 (Latest)
| Model ID | Tier | Features |
|----------|------|----------|
| `ltx-2-3-fast` | Fast | Speed-optimized, rapid prototyping |
| `ltx-2-3-pro` | Pro | Highest fidelity, all features |

### LTX-2
| Model ID | Tier | Features |
|----------|------|----------|
| `ltx-2-fast` | Fast | Speed-optimized, landscape only |
| `ltx-2-pro` | Pro | Higher quality, all features |

### Feature Availability by Tier

| Task | Fast | Pro |
|------|------|-----|
| Text-to-video | Yes | Yes |
| Image-to-video | Yes | Yes |
| Audio-to-video | No | Yes |
| Retake | No | Yes |
| Extend | No | Yes |

### Specifications
- **Resolution:** Up to 4K (3840x2160)
- **Frame Rate:** 24-50 fps
- **Duration:** Up to 20 seconds (varies by model/resolution)
- **Aspect Ratios:** LTX-2.3 supports portrait and landscape; LTX-2 landscape only

---

## Endpoint: Text-to-Video

**POST** `https://api.ltx.video/v1/text-to-video`

### Request Body

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | Yes | -- | Text description of desired video |
| `model` | string | Yes | -- | Model ID (e.g., `ltx-2-3-pro`) |
| `duration` | integer | Yes | -- | Video length in seconds |
| `resolution` | string | Yes | -- | Output resolution (e.g., `1920x1080`) |
| `fps` | integer | No | 24 | Frames per second |
| `generate_audio` | boolean | No | true | Generate AI audio matching the scene |
| `camera_motion` | string | No | -- | Camera effect (see options below) |

### Camera Motion Options
`dolly_in`, `dolly_out`, `dolly_left`, `dolly_right`, `jib_up`, `jib_down`, `static`, `focus_shift`

### Example (cURL)
```bash
curl -X POST https://api.ltx.video/v1/text-to-video \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "A majestic eagle soaring through clouds at sunset",
    "model": "ltx-2-3-pro",
    "duration": 8,
    "resolution": "1920x1080"
  }' \
  --output generated_video.mp4
```

### Example (Python)
```python
import requests

response = requests.post(
    "https://api.ltx.video/v1/text-to-video",
    json={
        "prompt": "A majestic eagle soaring through clouds at sunset",
        "model": "ltx-2-3-pro",
        "duration": 8,
        "resolution": "1920x1080"
    },
    headers={"Authorization": "Bearer YOUR_API_KEY"}
)

with open("generated_video.mp4", "wb") as f:
    f.write(response.content)
```

---

## Endpoint: Image-to-Video

**POST** `https://api.ltx.video/v1/image-to-video`

### Request Body

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_uri` | string | Yes | -- | Image URL (used as first frame) |
| `prompt` | string | Yes | -- | Description of desired animation |
| `model` | string | Yes | -- | Model ID |
| `duration` | integer | Yes | -- | Video length in seconds |
| `resolution` | string | Yes | -- | Output resolution |
| `fps` | integer | No | 24 | Frames per second |
| `generate_audio` | boolean | No | true | Generate AI audio |
| `last_frame_uri` | string | No | -- | Final frame image (LTX-2.3 only) |
| `camera_motion` | string | No | -- | Camera effect |

### Example (cURL)
```bash
curl -X POST https://api.ltx.video/v1/image-to-video \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_uri": "https://example.com/sunset.jpg",
    "prompt": "Clouds drifting across the sky",
    "model": "ltx-2-3-pro",
    "duration": 8,
    "resolution": "1920x1080"
  }' \
  --output animated_video.mp4
```

---

## Endpoint: Audio-to-Video

**POST** `https://api.ltx.video/v1/audio-to-video`

### Request Body

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `audio_uri` | string | Yes | -- | Audio URL (2-20 seconds) |
| `image_uri` | string | Conditional | -- | Required if prompt absent |
| `prompt` | string | Conditional | -- | Required if image_uri absent |
| `resolution` | string | No | auto | `1920x1080` or `1080x1920` |
| `guidance_scale` | number | No | 5/9* | CFG parameter (*9 when image provided) |
| `model` | string | No | `ltx-2-3-pro` | `ltx-2-pro` or `ltx-2-3-pro` |

Output: 25 fps video synced to audio. Pro tier only.

---

## Endpoint: Retake

**POST** `https://api.ltx.video/v1/retake`

Re-generates a specific section of an existing video.

### Request Body

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `video_uri` | string | Yes | -- | Input video URL (min 73 frames) |
| `prompt` | string | No | -- | Description of changes |
| `start_time` | number | Yes | -- | Start position in seconds |
| `duration` | number | Yes | -- | Section length (minimum 2s) |
| `mode` | string | No | `replace_audio_and_video` | `replace_audio`, `replace_video`, or `replace_audio_and_video` |
| `resolution` | string | No | auto | Output resolution |
| `model` | string | No | `ltx-2-3-pro` | Model ID (Pro tier only) |

---

## Endpoint: Extend

**POST** `https://api.ltx.video/v1/extend`

Extends an existing video from the beginning or end.

### Request Body

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `video_uri` | string | Yes | -- | Input video (16:9 or 9:16, max 4K, min 73 frames) |
| `duration` | number | Yes | -- | Extension length (2-20 seconds, max 480 frames at 24fps) |
| `prompt` | string | No | -- | Instructions for generated content |
| `mode` | string | No | `end` | `start` or `end` |
| `model` | string | No | `ltx-2-3-pro` | Model ID (Pro tier only) |
| `context` | number | No | -- | Seconds of input to use as reference (max 20s) |

---

## Endpoint: Upload

**POST** `https://api.ltx.video/v1/upload`

Creates a pre-signed upload URL for media files.

### Response
```json
{
  "upload_url": "https://...",
  "storage_uri": "ltx://...",
  "expires_at": "2026-04-13T12:00:00Z",
  "required_headers": { ... }
}
```

### Upload Flow
1. POST to `/v1/upload` with API key
2. PUT file to `upload_url` with `required_headers`
3. Use `storage_uri` in generation requests
4. Files available for 24 hours

---

## Response Format

### Success (200)
- **Content-Type:** `application/octet-stream`
- **Body:** Binary MP4 video file
- **Header:** `x-request-id` for tracking

### Errors
```json
{
  "type": "error",
  "error": {
    "type": "error_type",
    "message": "Human-readable message"
  }
}
```

### Error Codes

| Status | Type | Description |
|--------|------|-------------|
| 400 | `invalid_request_error` | Invalid parameters |
| 401 | `authentication_error` | Missing/invalid API key |
| 402 | `insufficient_funds_error` | Not enough credits |
| 413 | `request_too_large` | Payload exceeds limit |
| 422 | `content_filtered_error` | Safety filter rejection |
| 429 | `rate_limit_error` | Rate limit exceeded |
| 429 | `concurrency_limit_error` | Too many concurrent requests |
| 500 | `api_error` | Server error |
| 503 | `service_unavailable` | Temporarily unavailable |
| 504 | -- | Request timeout |

---

## Input Format Constraints

### Upload Methods
| Method | Image Limit | Video/Audio Limit | Notes |
|--------|------------|-------------------|-------|
| Cloud Storage (upload endpoint) | 100 MB | 100 MB | Files persist 24 hours |
| HTTPS URL | 15 MB | 32 MB | Public URLs only, 10/30s timeout |
| Data URI (Base64) | 7 MB | 15 MB | ~33% encoding overhead |

### Supported Formats
- **Images:** PNG, JPEG, WebP
- **Videos:** MP4, MOV, MKV (H.264, H.265 codecs)
- **Audio:** WAV, MP3, M4A, OGG (AAC-LC profile required for AAC)

---

## Rate Limits

Two types of limits are enforced:
1. **Concurrency limits** - Maximum simultaneous requests
2. **Rate limits** - Maximum requests per time window

When exceeded, API returns `429` with `Retry-After` header (seconds).

Specific numerical limits are not publicly documented; contact support for details.

---

## Related Links

- **Docs Home:** https://docs.ltx.video/
- **Quickstart:** https://docs.ltx.video/quickstart
- **Authentication:** https://docs.ltx.video/authentication
- **Models:** https://docs.ltx.video/models
- **Pricing:** https://docs.ltx.video/pricing
- **Error Reference:** https://docs.ltx.video/errors
- **Developer Console:** https://console.ltx.video/
