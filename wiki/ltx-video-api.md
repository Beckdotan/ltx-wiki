---
title: LTX Video REST API
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/
  - https://docs.ltx.video/welcome
  - https://docs.ltx.video/quickstart
  - https://docs.ltx.video/authentication
  - https://ltx.io/model/api
tags:
  - api
  - ltx-video
  - rest-api
  - lightricks
  - video-generation
---

# LTX Video REST API

The LTX Video REST API is a production-grade HTTP API operated by [[lightricks-company]] for AI video generation with synchronized audio. It is powered by the [[ltx-2-huggingface|LTX-2]] and [[ltx-2-3-huggingface|LTX-2.3]] open-source models and is documented at `docs.ltx.video`.

## Key Characteristics

- **Single-call generation**: Submit a request, receive a binary MP4 file directly -- no polling, webhooks, or job queues required.
- **Synchronized audio**: Video and audio are generated together in one pass.
- **Up to 4K resolution** (3840x2160) and **up to 20 seconds** per request.
- **Portrait and landscape** orientations supported (LTX-2.3 supports both; LTX-2 is landscape only).

## Access Points

| Resource | URL |
|----------|-----|
| Base URL | `https://api.ltx.video/v1/` |
| Documentation | https://docs.ltx.video/ |
| Developer Console | https://console.ltx.video/ |
| API Playground | https://console.ltx.video/playground/ |

## Authentication

All requests require a Bearer token in the `Authorization` header:

```
Authorization: Bearer YOUR_API_KEY
```

API keys are generated at the [[ltx-video-api|Developer Console]] (https://console.ltx.video/). Both `Authorization` and `Content-Type: application/json` headers are required for POST requests.

### Security Best Practices

- Never commit API keys to version control.
- Avoid exposing keys in client-side code.
- Store credentials in environment variables (e.g., `LTXV_API_KEY`).
- Rotate keys periodically.

## Endpoints

The API provides six endpoints. See [[ltx-video-api-endpoints]] for full parameter details.

| Endpoint | Method | Path | Description |
|----------|--------|------|-------------|
| Text-to-Video | POST | `/v1/text-to-video` | Generate video from a text prompt |
| Image-to-Video | POST | `/v1/image-to-video` | Animate a static image |
| Audio-to-Video | POST | `/v1/audio-to-video` | Generate video synced to audio |
| Retake | POST | `/v1/retake` | Re-generate a section of video |
| Extend | POST | `/v1/extend` | Lengthen an existing video |
| Upload | POST | `/v1/upload` | Get a pre-signed upload URL |

## Models

See [[ltx-video-api-models]] for model tiers and feature availability.

| Model ID | Generation | Tier |
|----------|------------|------|
| `ltx-2-fast` | LTX-2 | Fast |
| `ltx-2-pro` | LTX-2 | Pro |
| `ltx-2-3-fast` | LTX-2.3 | Fast |
| `ltx-2-3-pro` | LTX-2.3 | Pro |

## Response Format

Successful requests return `200` with `Content-Type: application/octet-stream` (binary MP4). An `x-request-id` header is included for tracking. Errors return JSON -- see [[ltx-video-api-errors]].

## Rate Limits

Two types of limits are enforced: **concurrency limits** (simultaneous requests) and **rate limits** (requests per time window). Exceeding either returns HTTP 429 with a `Retry-After` header. Specific numerical limits are not publicly documented; contact support for increases.

## Pricing

Billed per second of output video. See [[ltx-video-api-pricing]] for full rate tables.

## Third-Party API Access

LTX models are also available through third-party platforms:

- **fal.ai** -- https://fal.ai/ltx-2.3
- **Segmind** -- https://www.segmind.com/models/ltx-video/api
- **Modal** -- https://modal.com/docs/examples/ltx

## Deployment Options

- **Managed API**: The hosted API at `api.ltx.video`; rate-limited; ideal for exploratory and production use.
- **Self-hosted**: Requires 24 GB VRAM minimum; lower marginal cost; full batching control.
- **Isolated deployments**: For enterprise, financial, or governmental organizations with strict requirements.

## API Changelog (2026)

- **April 1, 2026**: Updated pricing for LTX-2.3 text-to-video and image-to-video endpoints.
- **February 18, 2026**: New `v1/extend` endpoint for extending video duration.
- **January 19, 2026**: New `v1/upload` endpoint for uploading assets via signed URLs.

## Related Pages

- [[ltx-video-api-endpoints]]
- [[ltx-video-api-models]]
- [[ltx-video-api-pricing]]
- [[ltx-video-api-prompting-guide]]
- [[ltx-video-api-input-formats]]
- [[ltx-video-api-errors]]
- [[ltx-video-api-docs-sitemap]]
- [[ltx-studio]]
