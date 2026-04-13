---
title: LTX Video API Pricing
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/pricing
  - https://ltx.io/model/api/pricing
tags:
  - api
  - pricing
  - ltx-video
  - costs
---

# LTX Video API Pricing

The [[ltx-video-api]] bills per second of output video. Higher resolution and premium (Pro) models cost proportionally more. There are no minimum charges, and failed generations (4xx/5xx errors) are not billed.

## Text-to-Video and Image-to-Video

Pricing is the same for both endpoints.

### ltx-2-fast

| Resolution | Cost per Second |
|-----------|----------------|
| 1920x1080 | $0.04 |
| 2560x1440 | $0.08 |
| 3840x2160 | $0.16 |

### ltx-2-pro

| Resolution | Cost per Second |
|-----------|----------------|
| 1920x1080 | $0.06 |
| 2560x1440 | $0.12 |
| 3840x2160 | $0.24 |

### ltx-2-3-fast (updated April 1, 2026)

| Resolution | Cost per Second | Previous Price |
|-----------|----------------|---------------|
| 1920x1080 / 1080x1920 | $0.06 | $0.04 |
| 2560x1440 / 1440x2560 | $0.12 | $0.08 |
| 3840x2160 / 2160x3840 | $0.24 | $0.16 |

### ltx-2-3-pro (updated April 1, 2026)

| Resolution | Cost per Second | Previous Price |
|-----------|----------------|---------------|
| 1920x1080 / 1080x1920 | $0.08 | $0.06 |
| 2560x1440 / 1440x2560 | $0.16 | $0.12 |
| 3840x2160 / 2160x3840 | $0.32 | $0.24 |

## Audio-to-Video, Retake, and Extend

All three endpoints are Pro tier only, priced the same regardless of model generation:

| Model | Resolution | Cost per Second |
|-------|-----------|----------------|
| ltx-2-pro | 1920x1080 | $0.10 |
| ltx-2-3-pro | 1920x1080 | $0.10 |

## Cost Examples

### 8-second video at 1080p

| Model | Total Cost |
|-------|-----------|
| ltx-2-fast | $0.32 |
| ltx-2-pro | $0.48 |
| ltx-2-3-fast | $0.48 |
| ltx-2-3-pro | $0.64 |

### 10-second video at 4K

| Model | Total Cost |
|-------|-----------|
| ltx-2-fast | $1.60 |
| ltx-2-pro | $2.40 |
| ltx-2-3-fast | $2.40 |
| ltx-2-3-pro | $3.20 |

## Important Notes

- Billing is based on **output duration**, not generation time.
- Failed generations (4xx/5xx errors) are **not billed**.
- A `402 insufficient_funds_error` is returned when the account lacks credits.
- Credits are managed in the developer console at https://console.ltx.video/.
- No free tier has been documented.

## Related Pages

- [[ltx-video-api]]
- [[ltx-video-api-models]]
- [[ltx-video-api-errors]]
