# LTX Video API Pricing

**Source:** https://docs.ltx.video/pricing
**Fetched:** 2026-04-13

---

## Billing Model

Video generation is billed **per second of output video**. Higher resolution and premium (Pro) models have proportionally higher costs. Pricing is the same for text-to-video and image-to-video endpoints.

---

## Text-to-Video & Image-to-Video Pricing

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

### ltx-2-3-fast (Updated April 1, 2026)
| Resolution | Cost per Second | Previous Price |
|-----------|----------------|---------------|
| 1920x1080 / 1080x1920 | $0.06 | $0.04 |
| 2560x1440 / 1440x2560 | $0.12 | $0.08 |
| 3840x2160 / 2160x3840 | $0.24 | $0.16 |

### ltx-2-3-pro (Updated April 1, 2026)
| Resolution | Cost per Second | Previous Price |
|-----------|----------------|---------------|
| 1920x1080 / 1080x1920 | $0.08 | $0.06 |
| 2560x1440 / 1440x2560 | $0.16 | $0.12 |
| 3840x2160 / 2160x3840 | $0.32 | $0.24 |

---

## Audio-to-Video, Retake & Extend Pricing

All three endpoints (Pro tier only):
| Resolution | Cost per Second |
|-----------|----------------|
| 1920x1080 (ltx-2-pro) | $0.10 |
| 1920x1080 (ltx-2-3-pro) | $0.10 |

---

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

---

## Notes

- No free tier information was found in the documentation
- Billing is based on output duration, not generation time
- Failed generations (4xx/5xx errors) are not billed
- 402 `insufficient_funds_error` returned when account lacks credits
- Credits can be managed in the developer console at https://console.ltx.video/

---

## Related Links

- **Pricing Page:** https://docs.ltx.video/pricing
- **Developer Console:** https://console.ltx.video/
- **Models:** https://docs.ltx.video/models
