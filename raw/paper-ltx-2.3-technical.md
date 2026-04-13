# LTX-2.3: Technical Deep Dive

## Metadata

- **Title:** LTX-2.3 (22B Open-Source Audio-Video Foundation Model)
- **Developer:** Lightricks
- **Release Date:** March 5, 2026
- **Model Page:** https://ltx.io/model/ltx-2-3
- **Hugging Face:** https://huggingface.co/Lightricks/LTX-2.3
- **Note:** No separate arxiv paper; builds on the LTX-2 paper (arXiv:2601.03233)

## Overview

LTX-2.3 is a 22-billion-parameter open-source video model generating native 4K at up to 50 FPS with synchronized audio in a single pass. It represents a significant upgrade over LTX-2 with four major architectural improvements: a rebuilt VAE, a gated attention text connector, improved HiFi-GAN vocoder, and refined latent space.

## Key Technical Improvements Over LTX-2

### 1. Rebuilt Video VAE

- Redesigned VAE producing sharper fine details, more realistic textures, and cleaner edges
- PSNR improvement of approximately 2.5 dB over predecessor
- Fine textures like hair, facial features, text, and edges are better preserved through the full generation pipeline
- Trained on higher-quality data for the updated latent space
- Sharper fabric, cleaner hair, and stable chrome reflections during camera moves

### 2. Gated Attention Text Connector

- 4x larger text connector than LTX-2
- Complex prompts with multiple subjects, spatial relationships, and stylistic instructions now resolve accurately
- Prompts are followed more closely due to gated attention mechanism
- Still uses Gemma 3 12B as text encoder

### 3. Improved HiFi-GAN Vocoder

- Cleaner audio generation with stereo output at 24 kHz
- Upgraded vocoder produces cleaner, more realistic audio
- Improved foley and ambient sound quality

### 4. Refined Latent Space

- Updated VAE trained on higher-quality data
- Fine textures, hair, text, and edge detail are better preserved
- Reduced artifacts in high-motion scenes

## Architecture Specifications

| Component | Specification |
|-----------|--------------|
| Total Parameters | 22B |
| Video Stream | ~14B parameters |
| Audio Stream | ~5B parameters |
| Text Connector | 4x larger than LTX-2 |
| Architecture Type | Dual-stream asymmetric DiT |
| Text Encoder | Gemma 3 12B |
| Context Window | 1,000 tokens |
| Max Resolution | 4K |
| Max Frame Rate | 50 fps |
| Max Duration | 20 seconds |
| Audio Output | Stereo 24 kHz |

## Core Architecture (inherited from LTX-2)

Built on a Diffusion Transformer (DiT) foundation that combines iterative refinement of diffusion models with sequence modeling power of transformers. The DiT architecture scales more efficiently with compute and data than traditional diffusion UNets, enabling processing of longer video sequences at higher resolutions.

- Dual-stream asymmetric diffusion transformer
- Bidirectional audio-video cross-attention with temporal RoPE
- Cross-modality AdaLN conditioning
- Modality-aware classifier-free guidance (modality-CFG)

## Performance Improvements

- Noticeably sharper output, particularly in scenes with complex textures and fine structures
- Better prompt adherence for complex multi-subject scenes
- Cleaner audio synthesis
- Maintained speed advantages of the LTX-2 architecture

## Sources

- https://ltx.io/model/ltx-2-3
- https://huggingface.co/Lightricks/LTX-2.3
- https://ltx2ai.net/blog/ltx-2-3-technical-deep-dive
- https://wavespeed.ai/blog/posts/ltx-2-3-whats-new-2026/
