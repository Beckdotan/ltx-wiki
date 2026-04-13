---
title: LTX-2 Architecture
type: technical
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-architecture-deep-dive.md
  - raw/ltx2-evolution-from-ltxv-09x.md
tags:
  - ltx-2
  - architecture
  - diffusion-transformer
  - dit
  - dual-stream
  - vae
  - gemma
---

# LTX-2 Architecture

[[ltx-2-overview|LTX-2]] is built on an asymmetric dual-stream Diffusion Transformer (DiT) architecture that processes video and audio simultaneously through specialized streams.

## Asymmetric Dual-Stream Transformer

### Core Design

- **Total parameters:** ~19B ([[ltx-2-overview|LTX-2]]) / ~22B ([[ltx-2.3-model|LTX-2.3]])
- **Video stream:** 14B parameters, processes spatiotemporal video latents
- **Audio stream:** 5B parameters, processes 1D temporal audio latents
- **Transformer depth:** 48 layers
- **Design rationale:** Video has fundamentally higher information density than audio, so more parameters are allocated to video

### Each Dual-Stream Block Performs (Sequentially)

1. **Self-Attention** -- within same modality
2. **Text Cross-Attention** -- for prompt conditioning
3. **Audio-Visual Cross-Attention** -- bidirectional inter-modal exchange with temporal positional embeddings
4. **Feed-Forward Network (FFN)** -- for refinement
5. **Cross-modality AdaLN** -- for shared timestep conditioning

### Positional Encoding

- **Video:** 3D positional embeddings (spatial + temporal)
- **Audio:** 1D positional embeddings (temporal only)
- Temporal positional embeddings in cross-attention layers enable audio-video synchronization

## Text Encoder: Gemma 3-12B

LTX-2 uses Gemma 3-12B Instruct as its text encoder backbone, a decoder-only LLM that processes text tokens into embeddings.

### Text Conditioning Pipeline (3 Stages)

1. **Gemma 3 Backbone** -- processes text tokens into embeddings across all layers
2. **Multi-Layer Feature Extraction** -- extracts features from ALL decoder layers (not just final layer), capturing both low-level phonetic structure and high-level semantic intent
3. **Text Connector with Thinking Tokens** -- a refined module employing multi-token prediction for enhanced prompt understanding

### Thinking Tokens

- Additional tokens appended to the text sequence
- Act as an internal workspace where information is aggregated before diffusion begins
- Improve adherence to complex prompts
- Make speech generation more expressive and context-aware
- Critical for phonetic and semantic accuracy of generated speech

### Multilingual Support

Gemma 3 provides native multilingual prompt understanding, supporting coherent audiovisual tracks from non-English prompts.

### LTX-2.3 Text Connector Improvement

[[ltx-2.3-model|LTX-2.3]] features a 4x larger text connector with gated attention architecture, enabling complex multi-subject prompts to resolve more accurately.

## Video VAE

- Inherits the highly efficient latent space from [[ltx-video-to-ltx-2-evolution|LTX Video (0.9.x)]]
- **Compression ratio:** 1:192
- Encodes video frames into spatiotemporal latents
- [[ltx-2.3-model|LTX-2.3]] features a redesigned VAE with sharper fine details, more realistic textures (denim, linen, brushed steel), cleaner edges, and less "beauty filter" effect

## Audio VAE

- Causal audio VAE producing high-fidelity 1D latent space
- Optimized for diffusion-based training and inference
- Parameters: `base_channels=128, latent_channels=8, sample_rate=16000, is_causal=True`

### Audio Processing Pipeline

1. Audio input converted to mel spectrograms at 16 kHz
2. Encoded by causal audio VAE into 1D temporal latent sequences
3. Processed through the audio stream of the dual-stream transformer
4. Decoded by audio VAE back to mel spectrograms
5. Neural vocoder reconstructs 24 kHz waveform

## Modality-Aware Classifier-Free Guidance (Bimodal CFG)

Standard CFG treats audio and video as a single unit. Increasing guidance for video quality affects audio synchronization and vice versa.

LTX-2 introduces a novel Bimodal CFG scheme with two independent parameters:

1. **Text guidance strength** -- controls prompt adherence
2. **Cross-modal alignment strength** -- controls audio-video synchronization

Setting `modality_scale > 1.0` (e.g., 3.0) improves audio-visual sync. This extends standard CFG with an independent guidance term for the complementary modality, providing fine-grained control over both prompt adherence and cross-modal synchronization without trade-offs.

## Architecture Comparison: LTX Video vs LTX-2

| Aspect | LTX Video (0.9.x) | LTX-2 |
|--------|-------------------|-------|
| Model type | Single-stream DiT | Asymmetric dual-stream DiT |
| Parameters | 2B (base) / 13B (0.9.7) | 19B (14B video + 5B audio) |
| Modalities | Video only | Video + Audio jointly |
| Text encoder | T5-XXL | Gemma 3-12B |
| Audio support | None | Native causal audio VAE + vocoder |
| CFG | Standard | Bimodal (modality-aware) |
| Cross-attention | Text-to-video only | Text + audio-video bidirectional |

## Related Pages

- [[ltx-2-overview]] -- High-level model overview
- [[ltx-2-code-structure]] -- Monorepo layout and package organization
- [[ltx-2.3-model]] -- LTX-2.3 architectural improvements
- [[ltx-video-to-ltx-2-evolution]] -- Evolution from LTX Video 0.9.x
- [[ltx-2-capabilities]] -- What these architectural choices enable
