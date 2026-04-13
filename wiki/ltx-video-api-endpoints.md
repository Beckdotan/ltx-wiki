---
title: LTX Video API Endpoints
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/api-documentation/api-reference/video-generation/text-to-video
  - https://docs.ltx.video/api-documentation/api-reference/video-generation/image-to-video
  - https://docs.ltx.video/api-documentation/api-reference/video-generation/audio-to-video
  - https://docs.ltx.video/api-documentation/api-reference/video-generation/retake
  - https://docs.ltx.video/api-documentation/api-reference/video-generation/extend
  - https://docs.ltx.video/api-documentation/api-reference/upload/create-upload
tags:
  - api
  - endpoints
  - ltx-video
  - video-generation
---

# LTX Video API Endpoints

Detailed parameter reference for all six endpoints of the [[ltx-video-api]]. All endpoints accept POST requests, require Bearer token authentication, and return binary MP4 data on success.

## Text-to-Video

**POST** `https://api.ltx.video/v1/text-to-video`

Generate video from a text description. The API returns a complete video with matching audio.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `prompt` | string | Yes | -- | Text description of desired video |
| `model` | string | Yes | -- | Model ID (see [[ltx-video-api-models]]) |
| `duration` | integer | Yes | -- | Video length in seconds (up to 20) |
| `resolution` | string | Yes | -- | Output resolution (e.g., `1920x1080`) |
| `fps` | integer | No | 24 | Frames per second (24-50) |
| `generate_audio` | boolean | No | true | Generate AI audio matching the scene |
| `camera_motion` | string | No | -- | Camera effect (see below) |

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

## Image-to-Video

**POST** `https://api.ltx.video/v1/image-to-video`

Animate a still image with realistic motion, depth, and audio. Preserves the visual identity of the source image.

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `image_uri` | string | Yes | -- | Image URL or base64 (used as first frame) |
| `prompt` | string | Yes | -- | Description of desired animation |
| `model` | string | Yes | -- | Model ID |
| `duration` | integer | Yes | -- | Video length in seconds |
| `resolution` | string | Yes | -- | Output resolution |
| `fps` | integer | No | 24 | Frames per second |
| `generate_audio` | boolean | No | true | Generate AI audio |
| `last_frame_uri` | string | No | -- | Final frame image (LTX-2.3 only) |
| `camera_motion` | string | No | -- | Camera effect |

See [[ltx-video-api-input-formats]] for supported image formats and upload methods.

## Audio-to-Video

**POST** `https://api.ltx.video/v1/audio-to-video`

Generate video synchronized to a provided audio track. Supports dialogue, music, or ambient sound. **Pro tier only.**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `audio_uri` | string | Yes | -- | Audio URL (2-20 seconds) |
| `image_uri` | string | Conditional | -- | Required if `prompt` is absent |
| `prompt` | string | Conditional | -- | Required if `image_uri` is absent |
| `resolution` | string | No | auto | `1920x1080` or `1080x1920` |
| `guidance_scale` | number | No | 5 (9 with image) | CFG parameter |
| `model` | string | No | `ltx-2-3-pro` | `ltx-2-pro` or `ltx-2-3-pro` |

Output is always 25 fps video synced to the audio duration.

## Retake

**POST** `https://api.ltx.video/v1/retake`

Re-generate a specific section of an existing video without starting over. The model attends to surrounding frames for natural blending. **Pro tier only.**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `video_uri` | string | Yes | -- | Input video URL (minimum 73 frames) |
| `prompt` | string | No | -- | Description of changes |
| `start_time` | number | Yes | -- | Start position in seconds |
| `duration` | number | Yes | -- | Section length (minimum 2 seconds) |
| `mode` | string | No | `replace_audio_and_video` | `replace_audio`, `replace_video`, or `replace_audio_and_video` |
| `resolution` | string | No | auto | Output resolution |
| `model` | string | No | `ltx-2-3-pro` | Model ID |

## Extend

**POST** `https://api.ltx.video/v1/extend`

Lengthen videos from the beginning or end while maintaining continuity. Added February 18, 2026. **Pro tier only.**

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `video_uri` | string | Yes | -- | Input video (16:9 or 9:16, max 4K, min 73 frames) |
| `duration` | number | Yes | -- | Extension length (2-20 seconds, max 480 frames at 24fps) |
| `prompt` | string | No | -- | Instructions for generated content |
| `mode` | string | No | `end` | `start` or `end` |
| `model` | string | No | `ltx-2-3-pro` | Model ID |
| `context` | number | No | -- | Seconds of input to use as reference (max 20s) |

Tip: For portrait sequences longer than 8-10 seconds, extend rather than regenerate to preserve subject consistency.

## Upload

**POST** `https://api.ltx.video/v1/upload`

Creates a pre-signed upload URL for media files. Added January 19, 2026.

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

1. POST to `/v1/upload` with your API key (no body required).
2. PUT file to `upload_url` with `required_headers`.
3. Use `storage_uri` as `image_uri`, `video_uri`, or `audio_uri` in generation requests.
4. Files available for 24 hours.

See [[ltx-video-api-input-formats]] for a Python upload example and size limits.

## Feature Availability by Tier

| Task | Fast | Pro |
|------|------|-----|
| Text-to-video | Yes | Yes |
| Image-to-video | Yes | Yes |
| Audio-to-video | No | Yes |
| Retake | No | Yes |
| Extend | No | Yes |

## Related Pages

- [[ltx-video-api]]
- [[ltx-video-api-models]]
- [[ltx-video-api-pricing]]
- [[ltx-video-api-input-formats]]
- [[ltx-video-api-errors]]
- [[ltx-video-api-prompting-guide]]
