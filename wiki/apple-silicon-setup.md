---
title: Apple Silicon Setup
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/tutorial-apple-silicon-setup.md
  - raw/github-community-tools-wrappers.md
tags:
  - apple-silicon
  - mac
  - mps
  - setup
  - tutorial
---

# Apple Silicon Setup

Community members have documented how to run [[ltx-video-overview|LTX-Video]] on Apple Silicon Macs (M1, M3, M4). The process requires specific PyTorch version configuration to avoid output corruption.

## Critical Requirement: Torch Version

**Torch 2.5 or above causes noise output on Apple Silicon.** This is the single most important detail for Mac users.

**Solution:** Downgrade to Torch 2.4.1:

```bash
pip install torch==2.4.1
```

This breaks compatibility with JC2 (which requires Torch 2.5+). Users must choose between JC2 compatibility and LTX-Video on Apple Silicon.

## Performance by Hardware

| Mac Model | RAM | Processing Time |
|-----------|-----|----------------|
| MacBook Pro M4 | 48GB | Working (time not specified) |
| MacBook Pro M3 | -- | ~5 minutes per video |
| MacBook Pro M1 | 16GB | ~15 minutes per video |

## Recommended Settings

- **Model:** Use the 2B parameter model for best compatibility
- **Frame limit:** Keep videos to roughly 4 seconds to avoid quality degradation
- **Resolution:** Start with lower resolutions

## Known Issues

1. **Noise output with Torch 2.5+** -- Generated video appears as noise and lines if the wrong Torch version is used
2. **Quality degradation with many frames** -- Limit to roughly 4 seconds
3. **ComfyUI noise** -- Same Torch version issue manifests in ComfyUI

## Community Resources

- YouTube guide by monk05 for M4 setup: https://www.youtube.com/watch?v=lvD9l4b2pzQ

## MLX Support Status

While there is no official MLX implementation from [[lightricks-company|Lightricks]], the community has developed multiple MLX ports that bypass the PyTorch/Torch version issues entirely:

### Blaizzy/mlx-video (190 stars)

The most mature MLX port. Supports [[ltx-2-overview|LTX-2]] (19B), Wan2.1 (1.3B/14B), and Wan2.2.

- **URL:** https://github.com/Blaizzy/mlx-video
- **LTX-2 features:** Text-to-video, image-to-video, audio-to-video, joint audio-video, 2x spatial upscaling, Gemma prompt enhancement
- **Pipelines:** distilled, dev, dev-two-stage, dev-two-stage-hq
- **Requirements:** macOS Apple Silicon, Python >= 3.11, MLX >= 0.22.0

### james-see/ltx-video-mac (137 stars)

Native macOS SwiftUI app using MLX framework for [[ltx-2-overview|LTX-2]] generation.

- **URL:** https://github.com/james-see/ltx-video-mac
- **Releases:** 83 (latest v2.3.52, April 2026)
- **Features:** Synchronized audio, text-to-speech (ElevenLabs/MLX-Audio), 54 music genre presets
- **Requirements:** macOS 14.0+, Apple Silicon, 32GB RAM minimum (64GB+ recommended)

### baisampayans/ltx-mlx (1 star)

Pure MLX implementation claiming 3.4x faster than PyTorch for LTX-Video inference.

- **URL:** https://github.com/baisampayans/ltx-mlx

See [[github-community-tools]] for more community tools.

## See Also

- [[hardware-requirements]] -- General GPU and VRAM guidance
- [[hardware-accessibility]] -- Running on low-end hardware
- [[installation-quickstart]] -- General installation instructions
- [[github-community-tools]] -- Full community tools listing
