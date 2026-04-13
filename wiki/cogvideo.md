---
title: CogVideo / CogVideoX
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-cogvideo.md
  - raw/open-source-comparison.md
  - raw/related-work-and-comparisons.md
tags:
  - competitor
  - zhipu-ai
  - video-generation
  - open-source
  - expert-transformer
---

# CogVideo / CogVideoX

CogVideo and its successor CogVideoX are video generation models developed by Zhipu AI. CogVideoX features an expert transformer architecture and advanced 3D VAE compression. The models power Zhipu's consumer product "Qingying" for text-to-video and image-to-video services. While significant when released, CogVideoX has been surpassed by newer models in the [[open-source-video-generation-landscape]].

## Key Facts

- **Developer:** Zhipu AI (also known as Zhizhu AI / ZAI)
- **Parameters:** 2B, 5B
- **Architecture:** Expert Transformer with 3D Causal VAE (8x8x4 compression)
- **Max resolution:** 1360x768 (CogVideoX 1.5-5B)
- **FPS:** 8-16 fps
- **Duration:** 5-10 seconds
- **License:** Apache 2.0
- **First publication:** ICLR 2023 (original CogVideo)

## Version History

### CogVideo (2022-2023)
- Original video generation model, published at ICLR 2023

### CogVideoX-2B (2024)
- 2 billion parameters, FP16 precision
- Same lineage as "Qingying" consumer product

### CogVideoX-5B (2024)
- 5 billion parameters, BF16 precision
- Resolution: 720x480

### CogVideoX 1.5-5B (2024-2025)
- 10-second videos at 1360x768 resolution
- I2V variant supports any-resolution video generation

### CogKit (March 2025)
- Fine-tuning and inference framework for the CogVideoX series

## Architecture

- **3D VAE:** Compresses raw video data to 2% of its original size with 8x8x4 compression, significantly reducing training costs
- **Expert Transformer Block:** Aligns text with video modalities using adaptive LayerNorm
- **MM-DiT style:** Uses a joint text-video architecture; the [[related-work-and-comparisons|LTX-Video paper]] found that cross-attention outperformed this approach

## Strengths

- Strong text-video alignment through expert transformer architecture
- Efficient 3D VAE compression (2% of original data size)
- Apache 2.0 license
- Quantizable to run on free Colab T4 GPUs
- Fine-tuning framework (CogKit) for customization
- Any-resolution support in 1.5-5B-I2V

## Weaknesses

- Lower resolution (max 1360x768) compared to newer models
- Slow inference -- approximately 1000 seconds on A100, 550 seconds on H100
- Lower FPS (8-16 fps vs 24-50 fps in newer models)
- No native audio generation
- Surpassed by [[wan-video]], [[hunyuan-video]], and [[ltx-2-overview|LTX-2]] in benchmarks
- Slower development cadence compared to competitors

## Comparison to LTX

The [[related-work-and-comparisons|LTX-Video paper]] reports that LTX-Video significantly outperforms CogVideoX-2B in human evaluation at similar parameter scale. LTX-2 surpasses CogVideoX across nearly every dimension: native 4K vs 1360x768, up to 50 FPS vs 16 FPS, 20 seconds vs 10 seconds duration, and native synchronized audio.

## See Also

- [[open-source-video-generation-landscape]]
- [[video-generation-architectures]]
