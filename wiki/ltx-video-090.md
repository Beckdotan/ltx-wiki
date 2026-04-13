---
title: LTX-Video 0.9.0
type: source
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-090-initial-release.md
  - raw/ltx-video-early-versions-090-091-095.md
tags:
  - ltx-video
  - release
  - v0.9.0
  - lightricks
  - dit
---
# LTX-Video 0.9.0

LTX-Video 0.9.0 was [[lightricks-company]]' first open-source video generation model, released in **November 2024**. It was the first [[diffusion-transformer]]-based video generation model capable of generating high-quality videos in real-time. The accompanying research paper, "LTX-Video: Realtime Video Latent Diffusion" (arXiv:2501.00103), provided the technical foundation for all subsequent versions.

## Release Details

- **Release date:** November 2024
- **License:** RAIL-M (dated November 22, 2024)
- **Parameters:** ~2 billion (1.9B transformer, 28 blocks, 2048 hidden dimension)
- **Paper:** arXiv:2501.00103

## Architecture

The core architecture introduced in 0.9.0 remained consistent through [[ltx-video-095]]:

- **Transformer:** 1.9B parameters using the [[diffusion-transformer]] (DiT) paradigm
- **[[video-vae]]:** Custom spatiotemporal VAE with 32x32x8 compression, 128 channels, achieving a 1:192 compression ratio
- **Text encoder:** T5-XXL
- **Training:** Rectified-flow with velocity prediction
- **Key innovation:** Patchification relocated from the transformer to the VAE, enabling extreme compression. The VAE decoder handles both latent-to-pixel conversion and final denoising in pixel space (a "denoising decoder").

## Capabilities

- **Text-to-video generation** from English prompts
- **Image-to-video generation** from a single input image
- **Resolution:** Up to 768x512 (benchmark), supports various resolutions divisible by 32
- **Frame rate:** 24 fps (benchmark), with 30 fps capability
- **Frame count:** Up to 257 frames (must follow the 8n+1 pattern)
- **Speed:** 5 seconds of 24 fps video at 768x512 in ~2-4 seconds on an NVIDIA H100

## Inference Configuration

- **Inference steps:** ~50 recommended
- **Guidance:** [[classifier-free-guidance]] (CFG) used
- **Negative prompts:** Supported and recommended
- **Precision:** torch.bfloat16
- **Requirements:** Python 3.10.5+, CUDA 12.2, PyTorch >= 2.1.2

## Benchmark Results

The 2B model outperformed comparable-scale open-source models in human evaluation:

| Task | LTX-Video 0.9 | CogVideoX 2B | PyramidFlow |
|------|---------------|---------------|-------------|
| Text-to-video win rate | 85% | 38% | 51% |
| Image-to-video win rate | 91% | 47% | 35% |

## Significance

LTX-Video 0.9.0 demonstrated that real-time video generation was achievable at a fraction of the parameter count of competitors like HunyuanVideo (13B) and MovieGen (30B). It established the architectural foundation -- the DiT + high-compression [[video-vae]] design -- that would carry through all subsequent versions including [[ltx-video-091]], [[ltx-video-095]], and [[ltx-video-096]].

## References

- [Hugging Face repository](https://huggingface.co/Lightricks/LTX-Video)
- [arXiv paper](https://arxiv.org/abs/2501.00103)
- [RAIL-M License](https://static.lightricks.com/legal/ltxv-09-railm-license.pdf)
