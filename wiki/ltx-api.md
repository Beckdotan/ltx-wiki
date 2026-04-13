---
title: LTX API
type: product
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/welcome
  - https://ltx.io/
  - https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
tags:
  - ltx-api
  - developer-tools
  - ai-video
  - lightricks
---

# LTX API

The LTX API is [[lightricks-company]]'s developer API for programmatic video generation, part of the [[ltx-ecosystem]]. It provides HTTP endpoints for generating videos from text, images, and audio.

- **Documentation:** https://docs.ltx.video
- **Developer Portal:** https://ltx.dev

## Key Characteristics

- Cloud-hosted HTTP API
- Per-second billing model
- Multiple resolution tiers up to 4K
- Supports LTX-2 and LTX-2.3 models
- Single HTTP call returns a video (no polling or webhooks required)
- Audio generation support

## Endpoints

- **Text-to-Video** - Generate video from text prompts
- **Image-to-Video** - Create video from static images
- **Audio-to-Video** - Generate visuals synchronized to audio

## Usage in LTX Desktop

[[ltx-desktop]] uses the LTX API in two ways:

- **Text encoding:** Free (included with API key)
- **Video generation:** Paid (used when running in API mode on unsupported hardware like macOS)
- **Prompt enhancement:** Included with API key

## Integration with AI Agents

The LTX API can be called from any environment including Claude Code via standard HTTP requests. See [[ltx-mcp-integration]] for details on agent integration patterns.

## Third-Party API Hosting

Several platforms also host LTX models as API services. See [[ltx-integration-projects]] for details on fal.ai, Replicate, Modal, and other hosting options.

## Developer Program

Developers actively building with the LTX API can apply to the [[ltx-builders-program]] for early access to features and direct communication with the model team.
