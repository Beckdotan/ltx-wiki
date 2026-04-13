# LTX Studio - Official Web Application

**Source:** https://app.ltx.studio/
**Date:** 2026-04-13

---

## Overview

LTX Studio is Lightricks' official commercial web application for AI video generation. It provides a motion workspace interface for generating videos using LTX-Video models without any coding or GPU setup.

---

## Access Points

| Interface | URL | Model |
|-----------|-----|-------|
| Motion Workspace (13B Mix) | https://app.ltx.studio/motion-workspace?videoModel=ltxv-13b | 13B mixed quality |
| Motion Workspace (Distilled) | https://app.ltx.studio/motion-workspace?videoModel=ltxv | 13B distilled (faster) |
| Main Application | https://app.ltx.studio/ | Default model |

---

## Features

### Motion Workspace
- Interactive video generation interface
- Image-to-video generation
- Text-to-video generation
- Real-time preview capabilities
- Multiple model variant selection

### Capabilities
- Upload reference images for conditioning
- Text prompt-based video generation
- Resolution and frame count configuration
- Seed control for reproducibility
- Video download in MP4 format

---

## Account & Authentication

- Requires Lightricks account (sign up at https://app.ltx.studio/)
- Free tier available with limited generations
- Paid plans for higher usage
- No API access from LTX Studio (use fal.ai, Replicate, or self-hosted for API access)

---

## Relationship to Open-Source

LTX Studio uses the same underlying LTX-Video models available on Hugging Face, but with:
- Additional post-processing and quality enhancements
- Optimized inference infrastructure
- User-friendly web interface
- No GPU hardware requirements for users

---

## Related Links

- **LTX Studio Website:** https://ltx.studio/
- **LTX Studio App:** https://app.ltx.studio/
- **Lightricks Company:** https://www.lightricks.com/
