# LTX-Video: Related Work and Competitive Landscape

## Source
- References from LTX-Video (arXiv:2501.00103)
- References from LTX-2 (arXiv:2601.03233)
- HuggingFace model pages

## Key Competing/Related Models

### Video Generation Models Compared Against LTX-Video

#### CogVideoX (2B)
- **arXiv:** 2408.06072
- **Authors:** Yang et al.
- **Architecture:** Expert transformer with MM-DiT style
- **VAE:** 8x8x4 compression, 16 channels
- **Comparison:** LTX-Video significantly outperforms in human evaluation at similar scale

#### Open-Sora Plan
- **Source:** PKU-Yuan Lab and Tuzhan AI (GitHub)
- **VAE:** 8x8x4 compression
- **Comparison:** LTX-Video significantly outperforms in human evaluation

#### PyramidFlow
- **arXiv:** 2410.05954
- **Authors:** Jin et al.
- **Architecture:** Pyramidal flow matching
- **VAE:** 8x8x8 compression
- **Comparison:** LTX-Video significantly outperforms in human evaluation

#### HunyuanVideo
- **arXiv:** 2412.03603
- **Authors:** Kong et al.
- **VAE:** 8x8x4 compression, 16 channels
- **Significance:** Large-scale system cited as concurrent work

### Models Compared Against LTX-2

#### WAN 2.1 / Wan 2.2
- **Authors:** Wang et al.
- **Parameters:** 14B (video only)
- **Comparison:** LTX-2 is ~18x faster on H100; LTX-2 ranked higher on Artificial Analysis

#### Veo 3 (Google)
- **Type:** Proprietary
- **Max duration:** 12 seconds
- **Comparison:** LTX-2 comparable in human preference; generates up to 20s (vs 12s)

#### Sora 2 (OpenAI)
- **Type:** Proprietary
- **Max duration:** 16 seconds
- **Comparison:** LTX-2 comparable in human preference; surpassed Sora 2 Pro on Artificial Analysis

#### Ovi
- **Type:** Open-source concurrent work
- **Architecture:** Two 5B streams fine-tuned from Wan 2.2
- **Max duration:** 10 seconds
- **Comparison:** LTX-2 significantly outperforms; faster inference

#### BridgeDiT
- **Type:** Concurrent T2AV
- **Architecture:** Combines existing T2V and T2A backbones
- **Comparison:** Higher computational overhead vs LTX-2

### Models Influencing LTX-Video Architecture

#### DiT (Diffusion Transformers)
- **arXiv:** ICCV 2023 (Peebles & Xie)
- **Significance:** Foundation architecture for transformer-based diffusion

#### PixArt-alpha
- **arXiv:** 2310.00426
- **Authors:** Chen et al.
- **Significance:** Direct base architecture for LTX-Video; extends DiT with open text conditioning via cross-attention

#### Stable Diffusion 3 (SD3)
- **Authors:** Esser et al. (ICML 2024)
- **Significance:** Introduced rectified flow transformers, log-normal timestep scheduling, MM-DiT architecture

#### FLUX.1
- **Source:** Black Forest Labs
- **Significance:** MM-DiT-based model; LTX-Video found cross-attention better than MM-DiT

#### Stable Video Diffusion (SVD)
- **arXiv:** 2311.15127
- **Authors:** Blattmann et al.
- **Significance:** Influential video diffusion model; different approach to image conditioning

### Models Influencing LTX-Video Design Decisions

#### DC-VAE
- **Authors:** Chen et al. (2024)
- **Significance:** Demonstrated that text-to-image transformers work better with higher spatial compression (up to 64 channels). Inspired LTX-Video's 128-channel approach.

#### SimpleDiffusion
- **arXiv:** ICML 2023
- **Authors:** Hoogeboom et al.
- **Significance:** Showed higher resolutions need more noising for proper SNR; influenced LTX-Video's resolution-dependent timestep scheduling

#### StyleGAN
- **Authors:** Karras et al. (CVPR 2019)
- **Significance:** Multi-layer noise injection in generator; adopted by LTX-Video for VAE decoder

#### MovieGen (Meta)
- **arXiv:** 2410.13720
- **Significance:** Used diffusion-based upsampler for high-res output (separate module, unlike LTX-Video's integrated approach)

### Models Influencing LTX-2 Design

#### Gemma 3
- **Authors:** Google (team2025gemma)
- **Parameters:** 12B
- **Significance:** Used as frozen multilingual text encoder for LTX-2 (replacing T5-XXL from LTX-Video)

#### HiFi-GAN
- **Authors:** Kong et al.
- **Significance:** Audio vocoder architecture adapted for LTX-2's stereo audio reconstruction

#### Flux Kontext
- **arXiv:** 2506.15742
- **Significance:** Inspired parallel canvas approach but required new RoPE dimension + large-scale data; AVControl avoids both costs via per-token timestep

## Control Framework Related Work

### ControlNet
- **arXiv:** 2302.05543
- **Significance:** Established spatial control paradigm for diffusion models; AVControl's approach avoids architectural changes

### VACE (All-in-One Video Creation)
- **arXiv:** ICCV 2025
- **Significance:** Monolithic control model requiring 200K training steps; AVControl achieves better results with ~55K total steps across 13 modalities

### In-Context LoRA
- **arXiv:** 2410.23775
- **Authors:** Huang et al.
- **Significance:** Inspired IC-LoRA approach but uses spatial concatenation which fails for video structural control

## Taxonomy of Approaches

### Video Generation Architectures
| Approach | Examples | LTX-Video Position |
|----------|---------|-------------------|
| DiT + MM-DiT | SD3, FLUX.1, CogVideoX | Cross-attention preferred |
| DiT + Cross-attention | PixArt-alpha, MovieGen | LTX-Video's choice |
| U-Net based | SVD, early models | Not used |

### Audio-Visual Generation Approaches
| Approach | Examples | LTX-2 Position |
|----------|---------|----------------|
| Sequential (V2A) | MMAudio, CAFA, FoleyCrafter | Sub-optimal (misses joint distribution) |
| Sequential (A2V) | DiffFoley | Sub-optimal |
| Symmetric dual-stream | Ovi, BridgeDiT | High overhead |
| Asymmetric dual-stream | **LTX-2** | Efficient, high quality |
| Proprietary joint | Veo 3 | Closed-source |

### VAE Compression Strategies
| Strategy | Spatial | Temporal | Channels | Model |
|----------|---------|----------|----------|-------|
| Standard + Patchifier | 8x8 | 4-8 | 16 | Most models |
| High compression, no patchifier | 32x32 | 8 | 128 | **LTX-Video** |
| Separate audio VAE | - | - | 128 | **LTX-2** (audio) |
