---
title: RoPE Positional Encoding
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/architecture-video-dit-transformer.md
  - raw/architecture-video-dit-vae.md
  - raw/ltx-video-architecture-paper-details.md
  - raw/ltx-video-transformer-dit-architecture.md
tags:
  - rope
  - positional-encoding
  - transformer
  - attention
  - ltx-video
---

# RoPE Positional Encoding

Rotary Positional Embeddings (RoPE) with normalized fractional coordinates is the positional encoding scheme used in the [[video-dit-transformer]]. It replaces traditional absolute positional embeddings and is central to LTX-Video's ability to generalize across multiple resolutions and durations.

## Variants Tested

Three positional encoding approaches were evaluated:

1. **Absolute positional embeddings** -- baseline
2. **RoPE with fractional coordinates** -- improved
3. **RoPE with fractional coordinates normalized by predefined maximum coordinates** -- winner

The normalized variant was selected for production use.

## Coordinate System

- **Spatial coordinates**: Computed in pixels relative to a predefined maximum resolution
- **Temporal coordinates**: Computed in seconds relative to a predefined maximum duration
- **FPS-aware**: Incorporating the original FPS into the temporal embedding produces more natural motion generation

The use of fractional coordinates (rather than integer positions) is what enables training and inference at arbitrary resolutions and durations that were not seen during training.

## Frequency Spacing

A critical and somewhat counterintuitive design decision:

- LTX-Video uses **exponential frequency spacing** (NOT inverse-exponential)
- Many open-source RoPE implementations use inverse-exponential spacing
- Ablation showed exponential spacing **consistently produces lower training loss**
- This aligns with theoretical work suggesting that truncating lower frequencies improves performance

## Multi-Resolution Generalization

The normalized fractional coordinate system is the key enabler of multi-resolution support:

- During training, the model sees multiple width/height/duration combinations simultaneously
- At inference, the model can generate at resolutions and durations never seen in training
- The normalization ensures that positional encoding values remain in a consistent range regardless of the actual resolution

## LTX-2 Extension

In the dual-stream [[video-dit-transformer]] architecture of LTX-2:
- **Video stream**: Uses 3D RoPE (spatial + temporal)
- **Audio stream**: Uses 1D RoPE (temporal only)
- **Cross-modal attention**: Uses temporal 1D RoPE for alignment between video and audio (only temporal, not spatial)
