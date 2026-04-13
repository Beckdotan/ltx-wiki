# LTX-Video 0.9.0 - Initial Release (November 2024)

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://www.tomsguide.com/ai/ai-image-video/lightricks-unveils-new-open-source-ai-video-model-an-impressive-focus-on-speed-and-motion
**Source:** https://static.lightricks.com/legal/ltxv-09-railm-license.pdf
**Source:** https://arxiv.org/abs/2501.00103

## Release Details

- **Release date:** November 2024
- **License:** RAIL-M (dated November 22, 2024)
- **Parameters:** ~2 billion (1.9B transformer)
- **Paper:** "LTX-Video: Realtime Video Latent Diffusion" (arXiv:2501.00103)

## What Was Announced

LTX-Video 0.9.0 was Lightricks' first open-source video generation model and the first DiT-based video generation model capable of generating high-quality videos in real-time. The company claimed it could generate five seconds of AI video in just four seconds on an Nvidia H100 GPU at 768x512 resolution, 24 fps.

## Capabilities at Launch

- **Text-to-video generation** from English prompts
- **Image-to-video generation** from a single input image
- **Resolution:** Up to 768x512 (demonstrated), with support for various resolutions divisible by 32
- **Frame rate:** 24 fps (initial demos), 30 fps capability
- **Frame count:** Up to 257 frames (must be 8n+1)
- **Speed:** Faster-than-real-time generation on H100

## Architecture at Launch

- **Transformer:** 1.9B parameters, 28 blocks, 2048 hidden dimension
- **Video-VAE:** 32x32x8 spatiotemporal compression, 128 channels, 1:192 compression ratio
- **Text encoder:** T5-XXL
- **Key innovation:** Patchification relocated from transformer to VAE, enabling extreme compression
- **Denoising decoder:** VAE decoder handles both latent-to-pixel and final denoising in pixel space

## Performance Benchmarks (from paper)

The 2B model outperformed comparable-scale open-source models in human evaluation:
- Text-to-video: 85% win rate (vs. CogVideoX 2B at 38%, PyramidFlow at 51%)
- Image-to-video: 91% win rate (vs. CogVideoX 2B at 47%, PyramidFlow at 35%)

## Infrastructure Requirements

- Python 3.10.5+
- CUDA 12.2
- PyTorch >= 2.1.2

## Significance

This was Lightricks' first foray into open-source AI video generation, challenging Big Tech players by democratizing access to high-quality video generation. The model was notable for achieving real-time generation speeds at a fraction of the parameter count of competitors like HunyuanVideo (13B) and MovieGen (30B).
