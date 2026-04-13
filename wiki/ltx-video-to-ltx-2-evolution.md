---
title: Evolution from LTX Video to LTX-2
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-evolution-from-ltxv-09x.md
tags:
  - ltx-2
  - ltx-video
  - evolution
  - migration
  - architecture
---

# Evolution from LTX Video to LTX-2

[[ltx-2-overview|LTX-2]] represents a complete generational leap from LTX Video (LTXV 0.9.x), not an incremental update. The model was renamed from "LTXV" to "LTX-2" to reflect the magnitude of changes. Every major architectural component was redesigned or replaced.

## Parameter Scale Progression

| Model | Parameters | Notes |
|-------|-----------|-------|
| LTX Video 0.9.x (base) | 2B | Single-stream video DiT |
| LTXV 0.9.7 dev (13B) | 13B | Scaled-up video-only model |
| LTX-2 | 19B (14B video + 5B audio) | Dual-stream audio-video DiT |
| [[ltx-2.3-model|LTX-2.3]] | 22B | Further expanded architecture |

## Fundamental Architecture Changes

### From Single-Stream to Dual-Stream
- **LTX Video:** Single-stream Diffusion Transformer processing video latents only
- **LTX-2:** Asymmetric [[ltx-2-architecture|dual-stream DiT]] with separate video (14B) and audio (5B) streams coupled through bidirectional cross-attention

### From Video-Only to Audio-Video
- **LTX Video:** Generated silent video; audio required separate post-production
- **LTX-2:** Generates synchronized video and audio in a single inference pass

### Text Encoder Upgrade
- **LTX Video:** T5-XXL text encoder
- **LTX-2:** Gemma 3-12B Instruct with multi-layer feature extraction, thinking tokens, and multilingual support

### Guidance Mechanism
- **LTX Video:** Standard classifier-free guidance (CFG)
- **LTX-2:** Modality-aware bimodal CFG with independent control over text guidance and cross-modal alignment

### Audio Components (New in LTX-2)
- Causal audio VAE for encoding/decoding audio latents
- Neural vocoder for waveform reconstruction at 24 kHz
- Audio-video cross-attention layers with temporal positional embeddings

## Capability Improvements

| Capability | LTX Video 0.9.x | LTX-2 |
|-----------|-----------------|-------|
| Max resolution | 1216x704 | Native 4K (3840x2160) |
| Max FPS | 24-30 | 50 |
| Max duration | ~5 seconds | 20 seconds |
| Audio | None | Synchronized stereo |
| Portrait mode | Not native | Native 1080x1920 (LTX-2.3) |

## New Generation Modes in LTX-2

| Mode | LTX Video 0.9.x | LTX-2 |
|------|-----------------|-------|
| Text-to-video | Yes | Yes |
| Image-to-video | Yes | Yes |
| Audio-to-video | No | Yes |
| Video extension | No | Yes |
| Video retake | No | Yes |
| Multi-keyframe conditioning | Limited | Yes, with 3D camera logic |
| Keyframe interpolation | No | Yes |

## What Was Preserved

- Efficient latent space compression (1:192 ratio) -- a key speed advantage
- DiT-based architecture philosophy
- Open-source commitment
- ComfyUI and Diffusers integration support

## Efficiency

Despite growing from 2B to 19B parameters, LTX-2 is **18x faster** than Wan 2.2 (14B) per inference step, thanks to the inherited 1:192 compression ratio. See [[ltx-2-benchmarks]] for details.

## Repository Changes

- **LTX Video:** https://github.com/Lightricks/LTX-Video (single package)
- **LTX-2:** https://github.com/Lightricks/LTX-2 (monorepo with 3 packages: ltx-core, ltx-pipelines, ltx-trainer)

## Diffusers Integration Changes

- **LTX Video:** `LTXPipeline`, `LTXImageToVideoPipeline`
- **LTX-2:** `LTX2Pipeline`, `LTX2LatentUpsamplePipeline`, with two-stage generation support

## Licensing Changes

- **LTX Video:** Apache 2.0 (fully open)
- **LTX-2:** Code under Apache 2.0; model weights free for companies under $10M annual revenue; above that threshold requires contacting Lightricks

## Related Pages

- [[ltx-video-to-ltx-2]] -- Broader evolution timeline including LTX Video era
- [[ltx-2-overview]] -- LTX-2 model overview
- [[ltx-2-architecture]] -- Detailed architecture
- [[ltx-2-version-history]] -- Release timeline
- [[ltx-2-capabilities]] -- Full capability list
