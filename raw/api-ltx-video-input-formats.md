# LTX Video API Input Formats

**Source:** https://docs.ltx.video/input-formats
**Fetched:** 2026-04-13

---

## Upload Methods

### 1. Cloud Storage (Upload Endpoint)
- **Max Size:** 100 MB
- **Persistence:** Files available for 24 hours
- **Flow:** POST to `/v1/upload`, receive pre-signed URL, PUT file to URL
- **Usage:** Use returned `storage_uri` in generation requests

### 2. HTTPS URLs
- **Image Limit:** 15 MB
- **Video/Audio Limit:** 32 MB
- **Requirements:** Public access, domain names only (no IP addresses)
- **Timeouts:** 10 seconds for images, 30 seconds for video/audio

### 3. Data URI (Base64)
- **Image Limit:** 7 MB
- **Video/Audio Limit:** 15 MB
- **Note:** Base64 encoding adds approximately 33% to file size

---

## Supported Image Formats

| Format | MIME Type |
|--------|----------|
| PNG | `image/png` |
| JPEG | `image/jpeg` |
| WebP | `image/webp` |

No specific resolution constraints documented.

---

## Supported Video Formats

| Container | MIME Type | Supported Codecs |
|-----------|----------|-----------------|
| MP4 | `video/mp4` | H.264, H.265 |
| MOV | `video/quicktime` | H.264, H.265 |
| MKV | `video/x-matroska` | H.264, H.265 |

---

## Supported Audio Formats

| Format | Supported Codecs |
|--------|-----------------|
| WAV | AAC-LC, MP3, Vorbis, FLAC |
| MP3 | MP3 only |
| M4A | AAC-LC |
| OGG | Vorbis |

**Important Notes:**
- AAC requires AAC-LC profile (1024 samples/frame)
- HE-AAC variants are NOT supported
- PCM audio is NOT supported

---

## Upload API Reference

### Create Upload
**POST** `https://api.ltx.video/v1/upload`

**Request:** API key in Authorization header (no body required)

**Response:**
```json
{
  "upload_url": "https://pre-signed-url...",
  "storage_uri": "ltx://storage-reference",
  "expires_at": "2026-04-13T13:00:00Z",
  "required_headers": {
    "Content-Type": "video/mp4"
  }
}
```

### Upload Flow
1. Call `POST /v1/upload` with API key
2. Extract `upload_url` and `required_headers`
3. `PUT` file to `upload_url` with required headers
4. Use `storage_uri` in subsequent API calls as `image_uri`, `video_uri`, or `audio_uri`

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

---

## Related Links

- **Input Formats:** https://docs.ltx.video/input-formats
- **Upload API:** https://docs.ltx.video/api-documentation/api-reference/upload/create-upload
