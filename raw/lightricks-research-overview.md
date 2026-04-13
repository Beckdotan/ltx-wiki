# Lightricks Research Overview

## Source
- HuggingFace organization page: https://huggingface.co/Lightricks
- Paper references from LTX-Video, LTX-2, and AVControl papers
- Date: April 2026

## Company Profile

**Lightricks** is a verified technology company focused on generative AI, computer vision, audio/speech, and diffusion models for content editing and creation. Based in Israel (Jerusalem), with a 54-person research team on HuggingFace.

**Contact:** ltx-video@lightricks.com / ltx-2@lightricks.com

## Published Research Papers

### Core Video Generation Papers

| Paper | arXiv | Date | Topic |
|-------|-------|------|-------|
| LTX-Video: Realtime Video Latent Diffusion | 2501.00103 | Dec 2024 | Foundational video generation with 1:192 compression VAE |
| LTX-2: Efficient Joint Audio-Visual Foundation Model | 2601.03233 | Jan 2025/2026 | Joint audio-video generation with asymmetric dual-stream DiT |

### Control and Conditioning Papers

| Paper | arXiv | Date | Topic |
|-------|-------|------|-------|
| AVControl: Efficient Framework for Training Audio-Visual Controls | 2603.24793 | Mar 2026 | Modular LoRA-based control framework |
| CAFA: A Controllable Automatic Foley Artist | 2504.06778 | Apr 2025 | Video-to-audio foley generation |

### Collaborative Research

| Paper | arXiv | Date | Topic | Collaborators |
|-------|-------|------|-------|---------------|
| JUST-DUB-IT | 2601.22143 | Jan 2026 | Video dubbing | Tel Aviv University |
| ID-LoRA | 2603.10256 | Mar 2026 | Identity-driven A/V personalization | Tel Aviv University |

## Research Themes

### 1. High-Compression Latent Spaces
- Video-VAE with 1:192 compression ratio (32x32x8)
- 128-channel latent space
- Novel approach: relocating patchifier from transformer to VAE
- Shared denoising objective between transformer and VAE decoder
- Audio VAE with compact 128-dimensional latent tokens

### 2. Efficient Transformer Architectures
- DiT-based architectures for video generation
- Scaling from 2B to 13B to 19B to 22B parameters
- Asymmetric dual-stream design for multimodal generation
- RoPE with normalized fractional coordinates
- Cross-attention for text conditioning (vs MM-DiT)

### 3. Audio-Visual Synthesis
- Evolution from silent video (LTX-Video) to joint A/V (LTX-2)
- Bidirectional cross-attention between modalities
- Modality-aware classifier-free guidance
- Temporal synchronization via 1D temporal RoPE in cross-attention

### 4. Modular Control
- IC-LoRA (In-Context LoRA) paradigm
- Parallel canvas conditioning
- Per-modality LoRA adapters on frozen backbone
- 13 control modalities trained independently
- Data and compute efficient (few hundred to few thousand steps per modality)

### 5. Real-Time Generation
- Faster-than-real-time generation (core claim of LTX-Video)
- ~18x faster than comparable models (LTX-2 vs Wan 2.2)
- Distillation for further speedup (8 steps, CFG=1)
- Multi-scale, multi-tile inference for high-resolution output

### 6. Training Data and Captioning
- Custom aesthetic scoring via Siamese Networks
- Advanced video captioning (visual + auditory tracks)
- Motion and quality filtering pipelines
- Multi-resolution training with stochastic token dropping

## Key Technical Innovations by Lightricks

1. **Reconstruction GAN (rGAN):** Novel discriminator training where both original and reconstruction are shown together
2. **Video DWT Loss:** 3D Discrete Wavelet Transform loss for high-frequency detail preservation
3. **Shared Diffusion Objective:** VAE decoder performs final denoising step in pixel space
4. **Uniform Log-Variance:** Single shared log-variance across all latent channels
5. **Per-Token Timestep:** Different diffusion timestep per token for image conditioning
6. **Thinking Tokens:** Learnable tokens in text connector for enhanced prompt understanding
7. **Modality-Aware CFG:** Separate text and cross-modal guidance scales
8. **Parallel Canvas:** Reference signal as additional attention tokens for LoRA-based control

## Research Team (Key Names)

Based on paper authorship across publications:
- **Yoav HaCohen** - Project lead (LTX-Video, LTX-2)
- **Ofir Bibi** - Senior management (all papers)
- **Benny Brazowski** - Core team (LTX-Video, LTX-2)
- **Nisan Chiprut** - Core team (LTX-Video, LTX-2)
- **Tavi Halperin** - Controls/audio research (CAFA, AVControl, JUST-DUB-IT)
- **Naomi Ken Korem** - Controls research (AVControl, JUST-DUB-IT)
- **Eitan Richardson** - Core team (LTX-Video, LTX-2)
- **Victor Kulikov** - Core team (LTX-Video, LTX-2)
- **Roi Benita** - Audio research (CAFA, LTX-2)
- **Matan Ben Yosef** - LTX-Video Community Trainer lead

## Open-Source Releases

### Models
- LTX-Video (all versions through v0.9.8)
- LTX-2 (19B, all variants)
- LTX-2.3 (22B, all variants)
- All IC-LoRA control models
- Spatial and temporal upscalers
- Quantized versions (FP8, FP4)

### Code
- https://github.com/Lightricks/LTX-Video
- https://github.com/Lightricks/LTX-2
- https://github.com/Lightricks/ComfyUI-LTXVideo

### Datasets
- Canny-Control-Dataset
- Cakeify-Dataset
- Squish-Dataset

### Tools and Demos
- LTX-Studio (hosted demo)
- LTMarX (video watermarking)
- SAP x FLUX (Stage-Aware Prompting)
- Image Aligner API

## Other Research Areas (Inferred from HuggingFace)

- **Stage-Aware Prompting (SAP):** Applied to FLUX.2-klein-4B for text-to-image
- **Video Watermarking (LTMarX):** Lightweight watermarking for generated video
- **Image Alignment:** Color matching and alignment APIs
- **Gemma-3 QAT:** Quantization-aware training of Gemma-3 12B model
