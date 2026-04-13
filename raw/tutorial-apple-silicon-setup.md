# Tutorial: Running LTX Video on Apple Silicon (Mac M1/M3/M4)

**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions/26

---

## Overview

Community members have documented how to run LTX-Video on Apple Silicon Macs, including M1, M3, and M4 chips. The process requires specific configuration to avoid common issues.

## Critical Requirement: Torch Version

**Torch 2.5 or above causes noise output on Apple Silicon.** This is the most important configuration detail.

**Solution:** Downgrade to Torch 2.4.1

```bash
pip install torch==2.4.1
```

**Note:** This breaks compatibility with JC2 (which requires Torch 2.5+). Users must choose between JC2 compatibility and LTX-Video on Apple Silicon.

## Performance by Hardware

| Mac Model | RAM | Processing Time |
|-----------|-----|----------------|
| MacBook Pro M3 | - | ~5 minutes per video |
| MacBook Pro M1 | 16GB | ~15 minutes per video |
| MacBook Pro M4 | 48GB | Working (time not specified) |

## Recommended Settings

- **Frame limit:** Keep videos to ~4 seconds to avoid quality degradation
- **Resolution:** Start with lower resolutions
- **Model:** Use the 2B parameter model for best compatibility

## Community YouTube Guide

monk05 created a video guide after successfully running LTX-Video on M4:
- **URL:** https://www.youtube.com/watch?v=lvD9l4b2pzQ

## Known Issues

1. **Noise output with Torch 2.5+** - The most common issue; downgrade to 2.4.1
2. **Quality degradation with many frames** - Limit to ~4 seconds
3. **ComfyUI noise** - Generated video appears as noise and lines if Torch version is wrong

## Future: MLX Support Request

Community discussion #22 on LTX-2.3 requests Swift MLX support, which would provide native Apple Silicon optimization without the Torch compatibility issues. This remains an open feature request.

## SwiftUI/MLX Status

As of April 2026, there is no official MLX implementation for any LTX model. The community has expressed strong interest (Discussion #22 on LTX-2.3), but this remains a gap in the ecosystem.
