# What Changed from LTX Video (0.9.x) to LTX-2

## Overview

LTX-2 represents a complete generational leap from LTX Video (LTXV 0.9.x), not an incremental update. The model was renamed from "LTXV" to "LTX-2" to reflect the magnitude of changes. Every major architectural component was redesigned or replaced.

## Parameter Scale

| Model | Parameters | Notes |
|-------|-----------|-------|
| LTX Video 0.9.x (base) | 2B | Single-stream video DiT |
| LTXV 0.9.7 dev (13B) | 13B | Scaled-up video-only model |
| LTX-2 | 19B (14B video + 5B audio) | Dual-stream audio-video DiT |
| LTX-2.3 | 22B | Further expanded architecture |

## Fundamental Architecture Changes

### From Single-Stream to Dual-Stream
- **LTX Video:** Single-stream Diffusion Transformer processing video latents only
- **LTX-2:** Asymmetric dual-stream DiT with separate video (14B) and audio (5B) streams coupled through bidirectional cross-attention

### From Video-Only to Audio-Video
- **LTX Video:** Generated silent video; audio required separate post-production
- **LTX-2:** Generates synchronized video and audio in a single inference pass, including dialogue, foley, ambient sounds, and music

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

### Resolution and Frame Rate
| Capability | LTX Video 0.9.x | LTX-2 |
|-----------|-----------------|-------|
| Max resolution | 1216x704 (0.9.6+) | Native 4K (3840x2160) |
| Max FPS | 24-30 | 50 |
| Max duration | ~5 seconds | 20 seconds |
| Audio | None | Synchronized stereo |
| Portrait mode | Not native | Native 1080x1920 (LTX-2.3) |

### Generation Modes
| Mode | LTX Video 0.9.x | LTX-2 |
|------|-----------------|-------|
| Text-to-video | Yes | Yes |
| Image-to-video | Yes | Yes |
| Audio-to-video | No | Yes |
| Video extension | No | Yes |
| Video retake | No | Yes |
| Multi-keyframe conditioning | Limited | Yes, with 3D camera logic |
| Keyframe interpolation | No | Yes |

### Quality Improvements
- **Motion:** Sharper, more natural motion with reduced artifacts
- **Detail:** Fine details (hair, skin pores, fabric textures) better preserved during motion
- **Prompt adherence:** Dramatically improved with Gemma 3 + thinking tokens
- **Temporal consistency:** Better maintained across longer sequences
- **Lip sync:** New capability enabled by joint audio-video generation

## Efficiency Changes

### Latent Space
- LTX-2 inherits the highly efficient 1:192 compression ratio from LTX Video
- This is a key advantage carried forward, enabling faster inference than competitors
- Despite 19B parameters, LTX-2 is 18x faster than Wan 2.2 (14B) per inference step

### New Optimization Capabilities
- NVFP4 quantization: 3x faster, 60% less VRAM (RTX 50 Series)
- NVFP8 quantization: 2x faster, 40% less VRAM
- GGUF quantization for consumer GPUs down to 8-10GB VRAM
- Multi-GPU inference support

## Repository and Ecosystem Changes

### Code Repository
- **LTX Video:** https://github.com/Lightricks/LTX-Video (single package)
- **LTX-2:** https://github.com/Lightricks/LTX-2 (monorepo with 3 packages: ltx-core, ltx-pipelines, ltx-trainer)

### LoRA Ecosystem
- LTX-2 introduced IC-LoRA (image control) supporting depth, pose, edges
- LTX-2 LoRAs are NOT compatible with LTX-2.3 (architecture changes)
- Official LoRA trainer included in the repository

### ComfyUI Integration
- **LTX Video:** ComfyUI-LTXVideo nodes for video generation
- **LTX-2:** Same repository expanded with multimodal guidance, advanced sampler, IC-LoRA control, text conditioning save/load

### Diffusers Integration
- **LTX Video:** LTXPipeline, LTXImageToVideoPipeline
- **LTX-2:** LTX2Pipeline, LTX2LatentUpsamplePipeline, with two-stage generation support

## Licensing Changes
- **LTX Video:** Apache 2.0 (fully open)
- **LTX-2:** Code under Apache 2.0; model weights free for companies under $10M annual revenue; above that threshold requires contacting Lightricks

## What Was Preserved
- Efficient latent space compression (1:192 ratio)
- DiT-based architecture philosophy
- Open-source commitment
- ComfyUI and Diffusers integration support

## Sources

- [LTX-2 Product Page](https://ltx.io/model/ltx-2)
- [LTX Video GitHub](https://github.com/Lightricks/LTX-Video)
- [LTX-2 GitHub](https://github.com/Lightricks/LTX-2)
- [LTX-2 ArXiv Paper](https://arxiv.org/abs/2601.03233)
- [LTX-2 Wikipedia](https://en.wikipedia.org/wiki/LTX-2)
- [Curious Refuge -- LTX 2 Review](https://curiousrefuge.com/blog/ltx2-ai-video-generator-review)
- [Multimodal Model Page](https://ltx.io/model)
