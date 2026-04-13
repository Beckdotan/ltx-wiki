---
title: LTX-2 Version History
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-version-history-and-releases.md
  - raw/comfyui-ltx-version-history.md
tags:
  - ltx-2
  - version-history
  - releases
  - timeline
  - comfyui
---

# LTX-2 Version History

A chronological record of [[ltx-2-overview|LTX-2]] releases and updates.

## Release Timeline

### LTX-2 Announcement -- October 2025

- Lightricks announced LTX-2 as a rename from LTXV
- First DiT-based audio-video foundation model
- Native 4K resolution at up to 50 FPS
- Synchronized audio and video generation in one pass
- API access initially rolled out to early partners
- **Not yet open source** at this point

### LTX-2 Open-Source Release -- January 6, 2026

- Complete codebase, weights, and tooling made publicly available
- **Parameters:** 19 billion (14B video + 5B audio)
- Apache 2.0 license for code; free for companies under $10M annual revenue
- Full access to model weights, inference pipelines, and training code
- ArXiv paper published: 2601.03233
- Download statistics: 84,353 downloads in December 2025 on Hugging Face; 795 GitHub stars at launch

### End-of-January LTX-2 Update -- Late January 2026

Focused on builder-requested improvements for real-world workflows. This was an incremental drop, not a version bump:

- New [[ltx-2-lora-training|IC-LoRA union control model]] supporting depth, pose, and edges automatically
- New ComfyUI nodes to remove Gemma text encoder from the critical path (addressing VRAM management)
- Saveable/loadable text conditioning as `.safetensors` files

### LTX-2.3 -- March 2026

- **Parameters:** 22 billion (upgraded from 19B)
- **Architecture:** DiT-based audio-video foundation model
- Released alongside [[ltx-desktop|LTX Desktop]]
- **Major new features:**
  - [[audio-video-generation|Audio generation]] -- First open-source model with synchronized audio-video output
  - Gemma 3 12B text encoder (upgraded from previous encoders)
  - Spatial upscalers (2x and 1.5x latent-space upscaling)
  - Temporal upscaler (2x frame rate doubling)
  - [[comfyui-ltx-node-reference|MultimodalGuider]] -- Independent audio/video guidance control
  - [[comfyui-ltx-node-reference|LTXVNormalizingSampler]] -- Statistical normalization for quality
  - GemmaAPITextEncode -- Free API-based text encoding
  - Conditioning save/load for reuse across sessions
  - ID-LoRA for identity-preserving audio-video generation
  - Union IC-LoRA combining Canny + Depth + Pose
  - Camera control LoRAs (dolly, jib movements)
  - Motion tracking via sparse point trajectory tracking
- **Model files:** `ltx-2.3-22b-dev.safetensors` (full), `ltx-2.3-22b-distilled.safetensors` (fast, 8-step generation), plus spatial/temporal upscaler models
- **ComfyUI:** Full workflow examples on docs.comfy.org; one-stage and two-stage pipeline support; desktop auto-installer utility; upstream ID-LoRA integration (PR #13111, March 2026)
- See [[ltx-2.3-model]] for full details

## Version Comparison: LTX-2 vs LTX-2.3

| Feature | LTX-2 (Jan 2026) | LTX-2.3 (Mar 2026) |
|---------|-------------------|---------------------|
| Parameters | 19B | 22B |
| VAE | Original | Redesigned (sharper) |
| Portrait support | Limited | Native 1080x1920 |
| Max FPS | 50 | 50 (48 FPS practical) |
| Text connector | Standard | 4x larger, gated attention |
| Audio quality | Good | Filtered training data, new vocoder |
| Upscalers | Spatial only | Spatial (1.5x/2x) + Temporal |
| LTX Desktop | No | Yes |
| LoRA compatibility | LTX-2 LoRAs | Not backward compatible |

## No Intermediate Versions

There is no LTX-2.1 or LTX-2.2. Versioning went directly from LTX-2 (base) to LTX-2.3.

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2.3-model]] -- LTX-2.3 details
- [[ltx-video-to-ltx-2-evolution]] -- Pre-LTX-2 history
- [[ltx-2-community-reception]] -- How each release was received
- [[ltx-video-versions]] -- Pre-LTX-2 version timeline (v0.9.x)
- [[comfyui-ltx-integration-overview]] -- ComfyUI integration timeline
- [[audio-video-generation]] -- Audio-video generation (LTX-2.3)
- [[ltx-awesome-resources]] -- Community resource directory
