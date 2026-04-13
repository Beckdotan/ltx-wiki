# LTX-2 Architecture Deep Dive

## Overview

LTX-2 is built on an asymmetric dual-stream Diffusion Transformer (DiT) architecture that processes video and audio simultaneously through specialized streams. This represents a fundamental departure from LTX Video (0.9.x), which was a video-only model.

## Asymmetric Dual-Stream Transformer

### Core Design
- **Total parameters:** ~19B (LTX-2) / ~22B (LTX-2.3)
- **Video stream:** 14B parameters, processes spatiotemporal video latents
- **Audio stream:** 5B parameters, processes 1D temporal audio latents
- **Transformer depth:** 48 layers
- **Design rationale:** Video has fundamentally higher information density than audio, so more parameters are allocated to video

### Each Dual-Stream Block Performs (Sequentially):
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

### Architecture
LTX-2 uses Gemma 3-12B Instruct as its text encoder backbone, a decoder-only LLM that processes text tokens into embeddings.

### Text Conditioning Pipeline (3 stages):
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
- Gemma 3 provides native multilingual prompt understanding
- Supports generating coherent audiovisual tracks from non-English prompts

### LTX-2.3 Improvement
- 4x larger text connector with gated attention architecture
- Complex multi-subject prompts resolve more accurately

## Video VAE

### Specifications
- Inherits the highly efficient latent space from LTX Video (0.9.x)
- **Compression ratio:** 1:192
- Encodes video frames into spatiotemporal latents
- LTX-2.3 features a redesigned VAE trained on higher-quality data

### LTX-2.3 VAE Improvements
- Sharper fine details
- More realistic textures (especially denim, linen, brushed steel)
- Cleaner edges
- Less "beauty filter" effect

## Audio VAE

### Architecture
- Causal audio VAE producing high-fidelity 1D latent space
- Optimized for diffusion-based training and inference
- Parameters: base_channels=128, latent_channels=8, sample_rate=16000, is_causal=True

### Processing Pipeline
1. Audio input converted to mel spectrograms at 16 kHz
2. Encoded by causal audio VAE into 1D temporal latent sequences
3. Processed through the audio stream of the dual-stream transformer
4. Decoded by audio VAE back to mel spectrograms
5. Neural vocoder reconstructs 24 kHz waveform

### Design Benefits
- Modality-appropriate positional embeddings (1D vs 3D)
- Independent compression levels for each signal type
- Precise control over model capacity allocated to vision vs sound

## Modality-Aware Classifier-Free Guidance (Bimodal CFG)

### Problem
Standard CFG treats audio and video as a single unit. Increasing guidance for video quality affects audio synchronization and vice versa.

### Solution
LTX-2 introduces a novel Bimodal CFG scheme with two independent parameters:
1. **Text guidance strength** -- controls prompt adherence
2. **Cross-modal alignment strength** -- controls audio-video synchronization

### Technical Details
- Extends standard CFG with an independent guidance term for the complementary modality
- Allows independent modulation of textual conditioning and inter-modal alignment
- Setting modality_scale > 1.0 (e.g., 3.0) improves audio-visual sync
- Increasing cross-modal guidance promotes mutual information refinement

### Benefits
- Fine-grained control over both prompt adherence and cross-modal synchronization
- No trade-offs between video quality and audio synchronization
- Improved temporal synchronization and semantic coherence

## Architecture Changes from LTX Video (0.9.x) to LTX-2

| Aspect | LTX Video (0.9.x) | LTX-2 |
|--------|-------------------|-------|
| Model type | Single-stream DiT | Asymmetric dual-stream DiT |
| Parameters | 2B (base) / 13B (0.9.7) | 19B (14B video + 5B audio) |
| Modalities | Video only | Video + Audio jointly |
| Text encoder | T5-XXL | Gemma 3-12B |
| Audio support | None | Native causal audio VAE + vocoder |
| CFG | Standard | Bimodal (modality-aware) |
| Max resolution | 1216x704 | Native 4K (3840x2160) |
| Max FPS | 30 | 50 |
| Max duration | ~5 seconds | 20 seconds |
| Cross-attention | Text-to-video only | Text + audio-video bidirectional |

## Sources

- [LTX-2 ArXiv Paper (2601.03233)](https://arxiv.org/abs/2601.03233)
- [LTX-2 ArXiv HTML](https://arxiv.org/html/2601.03233v1)
- [LTX-2 GitHub -- ltx-core README](https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-core/README.md)
- [LTX-2 Open Source Video Generation -- DigitalOcean](https://www.digitalocean.com/community/tutorials/ltx-2-video-generation-audio-video-model)
- [Introl Blog -- LTX-2 Audiovisual Diffusion](https://introl.com/blog/ltx-2-audiovisual-diffusion-synchronized-video-audio-2026)
- [LTX-2 Model Page](https://ltx.io/model/ltx-2)
- [LTX-2.3 Model Page](https://ltx.io/model/ltx-2-3)
