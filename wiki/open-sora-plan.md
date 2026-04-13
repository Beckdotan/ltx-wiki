---
title: Open-Sora-Plan
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-open-sora.md
  - raw/related-work-and-comparisons.md
tags:
  - competitor
  - peking-university
  - video-generation
  - open-source
  - research
---

# Open-Sora-Plan

Open-Sora-Plan is an open-source video generation project from Peking University (PKU Yuan Group) that aims to reproduce Sora's capabilities. It is distinct from [[open-sora]], which is developed by HPC-AI Tech. The project emphasizes scalability and introduced a breakthrough model called Helios capable of minute-scale video synthesis.

## Key Facts

- **Developer:** PKU Yuan Group (Peking University) and Tuzhan AI
- **Breakthrough model:** Helios
- **Duration:** Minute-scale video synthesis (Helios)
- **FPS:** 19.5 FPS on a single H100 GPU (Helios)
- **Focus:** Research and scalable architectures

## Technical Components

- **Wavelet-Flow Variational Autoencoder** -- advanced video compression
- **Joint Image-Video Skiparse Denoiser** -- unified denoising architecture
- **Various condition controllers** for guided generation

## Strengths

- **Minute-scale video synthesis** with Helios -- longer than most competitors
- **Academic rigor** from Peking University research team
- **Novel architectural components** (Wavelet-Flow VAE, Skiparse Denoiser)
- **Active community development**

## Weaknesses

- **More research-oriented** than production-ready
- **Fewer consumer GPU optimizations**
- **Smaller community** compared to [[wan-video]] or [[hunyuan-video]]
- **Less ecosystem integration** -- limited ComfyUI support

## Comparison to LTX

The [[related-work-and-comparisons|LTX-Video paper]] cites Open-Sora Plan as a model that LTX-Video significantly outperforms in human evaluation. Open-Sora-Plan's main contribution is the research on minute-scale video synthesis with Helios, demonstrating that longer-form generation is achievable. However, it is not production-ready and lacks the resolution, audio, speed, and ecosystem features of [[ltx-2-overview|LTX-2]].

## See Also

- [[open-sora]]
- [[open-source-video-generation-landscape]]
- [[video-generation-architectures]]
