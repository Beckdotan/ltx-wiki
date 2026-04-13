---
title: LTX Video API Input Formats
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/input-formats
  - https://docs.ltx.video/api-documentation/api-reference/upload/create-upload
tags:
  - api
  - input-formats
  - upload
  - ltx-video
  - file-formats
---

# LTX Video API Input Formats

Supported media formats and upload methods for the [[ltx-video-api]]. These constraints apply to images, videos, and audio provided to the image-to-video, audio-to-video, retake, and extend endpoints.

## Upload Methods

| Method | Image Limit | Video/Audio Limit | Notes |
|--------|------------|-------------------|-------|
| Cloud Storage (upload endpoint) | 100 MB | 100 MB | Files persist 24 hours |
| HTTPS URL | 15 MB | 32 MB | Public URLs only, domain names only (no IPs), 10s image / 30s video timeout |
| Data URI (Base64) | 7 MB | 15 MB | ~33% encoding overhead |

The upload endpoint provides the highest size limits and is recommended for larger files.

## Supported Image Formats

| Format | MIME Type |
|--------|----------|
| PNG | `image/png` |
| JPEG | `image/jpeg` |
| WebP | `image/webp` |

No specific resolution constraints are documented for input images.

## Supported Video Formats

| Container | MIME Type | Supported Codecs |
|-----------|----------|-----------------|
| MP4 | `video/mp4` | H.264, H.265 |
| MOV | `video/quicktime` | H.264, H.265 |
| MKV | `video/x-matroska` | H.264, H.265 |

## Supported Audio Formats

| Format | Supported Codecs |
|--------|-----------------|
| WAV | AAC-LC, MP3, Vorbis, FLAC |
| MP3 | MP3 only |
| M4A | AAC-LC |
| OGG | Vorbis |

**Important constraints:**
- AAC requires the AAC-LC profile (1024 samples/frame).
- HE-AAC variants are **not** supported.
- PCM audio is **not** supported.

## Upload API Flow

The `/v1/upload` endpoint creates a pre-signed URL for uploading media.

1. `POST /v1/upload` with your API key (no body required).
2. Receive `upload_url`, `storage_uri`, `expires_at`, and `required_headers`.
3. `PUT` your file to `upload_url` with the provided `required_headers`.
4. Use `storage_uri` (an `ltx://` URI) as `image_uri`, `video_uri`, or `audio_uri` in generation requests.

### Python Example

```python
import requests

# Step 1: Get upload URL
upload_response = requests.post(
    "https://api.ltx.video/v1/upload",
    headers={"Authorization": "Bearer YOUR_API_KEY"}
).json()

# Step 2: Upload file
with open("my_image.jpg", "rb") as f:
    requests.put(
        upload_response["upload_url"],
        data=f.read(),
        headers=upload_response["required_headers"]
    )

# Step 3: Use storage_uri in generation
response = requests.post(
    "https://api.ltx.video/v1/image-to-video",
    json={
        "image_uri": upload_response["storage_uri"],
        "prompt": "The scene comes to life",
        "model": "ltx-2-3-pro",
        "duration": 8,
        "resolution": "1920x1080"
    },
    headers={"Authorization": "Bearer YOUR_API_KEY"}
)
```

## Related Pages

- [[ltx-video-api]]
- [[ltx-video-api-endpoints]]
- [[ltx-video-api-errors]]
