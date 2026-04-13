---
title: Community Forks and Derivatives
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/github-community-forks-derivatives.md
tags:
  - github
  - community
  - forks
  - derivatives
  - open-source
---

# Community Forks and Derivatives

The [[github-official-repositories|Lightricks/LTX-Video]] repository has approximately 960 forks on GitHub. Most are simple clones without significant modifications, but several community derivatives add meaningful functionality. The `ltx-video` GitHub topic lists 18 repositories.

## Notable Derivatives

### LTX-2-desktop/LTX-2.3

- **URL:** https://github.com/LTX-2-desktop/LTX-2.3
- Unofficial community packaging of LTX-2.3 emphasizing:
  - "Commercial-grade generation quality on par with Google Veo 3"
  - No censorship or content restrictions
  - Dynamic CPU offloading and 8-bit/4-bit quantization
  - Designed to run on mid-range PCs

### KONAKONA666/LTX-Video (LTXVideo Q8)

- **URL:** https://github.com/KONAKONA666/LTX-Video
- Community fork focusing on [[fp8-quantization|FP8 quantized inference]]

### sayakpaul/q8-ltx-video

- **URL:** https://github.com/sayakpaul/q8-ltx-video
- Tutorial/guide repo showing how to use Q8 kernels with HuggingFace [[diffusers-integration|diffusers]] to optimize inference on ADA GPUs

## GitHub Topic: ltx-video (18 repositories)

| Repository | Owner | Stars | Description |
|-----------|-------|-------|-------------|
| [[github-community-tools#deepbeepmeepwan2gp-wangp|Wan2GP]] | deepbeepmeep | 5,200 | Multi-model video gen for GPU poor (Wan, Hunyuan, LTX, Flux) |
| LTX-Video-Trainer | Lightricks | 424 | [[ltx-video-trainer|Official community trainer]] |
| [[github-community-tools#blaizzymlx-video|mlx-video]] | Blaizzy | 190 | MLX-based inference/finetuning for Mac |
| bottube | Scottcjn | 173 | AI-native video platform with Proof of Physical AI |
| [[github-community-tools#james-seeltx-video-mac|ltx-video-mac]] | james-see | 137 | Native macOS SwiftUI app for LTX-Video |
| ComfyUI-Wan-VACE-Video-Joiner | stuttlepress | 73 | Smooth transitions between clips via Wan VACE |
| [[jetson-thor-deployment|ltx2]] | Divhanthelion | 8 | LTX2 on NVIDIA Jetson Thor |
| ltxv-modal | MushroomFleet | 5 | Serverless deployment on [[modal|Modal]] |
| Nano_Cinema_AI_Video_Studio | wajason | 5 | All-in-one local AI video production studio |
| [[github-community-tools#audiohackingltxcpp|ltx.cpp]] | audiohacking | 3 | Experimental LTX 2.3 GGUF inference in C++ |
| ltx-desktop-unlockeds | geethudinoyt | 3 | Unlocked LTX desktop variant |
| ltx-cli | AshutoshIIT1234 | 2 | Reproducible CLI with presets and configs |
| memory-maker | unicodeveloper | 2 | Memory Maker (TypeScript) |
| LTX-MultiMotion | Jonnnty | 2 | Dynamic motion gen for varying character counts |
| ltx-mlx | baisampayans | 1 | LTX-Video on Apple Silicon, pure MLX, 3.4x faster |
| tio-magic-animation | Tio-Magic-Company | 0 | Animation toolkit supporting multiple models |
| ComfyUI-RogoAI-4DTimelapse | RogoAI-Takeji | 0 | 4D Timelapse for ComfyUI with LTX-Video 2 |
| run-comfyui-ltx | jalberty2018 | 0 | LTX-2 inference with ComfyUI |

## awesome-ltx2 (Curated Resource List)

- **URL:** https://github.com/wildminder/awesome-ltx2
- **Stars:** 286 | **Forks:** 23 | **Last updated:** April 13, 2026

A curated list of models, text encoders, and tools for [[ltx-2-overview|LTX-2]]. Covers:

- All model variants (LTX-2.3, LTX-2 19B) with sizes and quantization options
- [[gguf-quantizations|GGUF quantizations]] (Q2_K through Q4_K, 12-18 GB range)
- [[lora-ecosystem|LoRA adapters]] (distilled, ranks 105-384)
- Spatial and temporal upscalers
- Community tools like LTX2.3-Multifunctional
- Prompting guide and basics documentation

## Standard Forks (No Significant Modifications)

Most of the ~960 forks are simple clones used for personal experimentation or contributing back upstream. Examples include y2k7380/ltx-video and t1seungy/ltx-video.

## Related Pages

- [[github-official-repositories]] -- Official Lightricks repositories
- [[github-community-tools]] -- Community-built tools and wrappers
- [[community-models-finetunes]] -- Community models on HuggingFace
- [[adoption-metrics]] -- Fork and star metrics
