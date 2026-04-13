---
title: Community Tools and Wrappers
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/github-community-tools-wrappers.md
  - raw/github-community-forks-derivatives.md
tags:
  - github
  - community
  - tools
  - wrappers
  - mlx
  - apple-silicon
  - multi-model
  - serverless
---

# Community Tools and Wrappers

A rich ecosystem of community tools has emerged around [[ltx-video-overview|LTX Video]], spanning ComfyUI integrations, desktop apps, Apple Silicon ports, serverless deployments, multi-model frameworks, and experimental projects. This page covers standalone tools and wrappers (for ComfyUI-specific node packs, see [[comfyui-ltx-community-nodes]]).

## Desktop and Native Apps

### james-see/ltx-video-mac

- **URL:** https://github.com/james-see/ltx-video-mac
- **Stars:** 137 | **Forks:** 14 | **License:** MIT
- **Language:** Swift (92.6%), Python (4.8%)
- **Releases:** 83 (latest: v2.3.52, April 2026)

Native macOS SwiftUI app for AI video generation with synchronized audio using [[ltx-2-overview|LTX-2]] on [[apple-silicon-setup|Apple Silicon]] via the MLX framework.

**Features:**
- Text-to-video and image-to-video
- Synchronized audio generation
- Text-to-speech voiceover (ElevenLabs cloud or MLX-Audio local)
- AI-generated background music with 54 genre presets
- Generation queue with real-time progress
- Gemma [[prompt-engineering|prompt enhancement]]

**Requirements:** macOS 14.0+, Apple Silicon, 32GB RAM minimum (64GB+ recommended), 20-42GB disk

### LTX-2-desktop/LTX-2.3

- **URL:** https://github.com/LTX-2-desktop/LTX-2.3
- Community-packaged LTX-2.3 with dynamic CPU offloading and 8-bit/4-bit quantization
- Targets mid-range PCs, emphasizes uncensored generation

See also the official [[ltx-desktop]] for the Lightricks-maintained desktop app.

## Multi-Model Frameworks

### deepbeepmeep/Wan2GP (WanGP)

- **URL:** https://github.com/deepbeepmeep/Wan2GP
- **Stars:** 5,200 | **Forks:** 747

"The best Open Source Video Generative Models Accessible to the GPU Poor."

**Supported models:** [[wan-video|Wan 2.1/2.2]], [[hunyuan-video|Hunyuan Video]], LTX Video (2.0, 2.3, 2-ID variants), Flux, Qwen3, MMAudio, and more.

**LTX-specific integration:** LTX Video is a primary video generation engine with IC-LoRA and control video support.

**Key features:**
- VRAM optimization (as low as 6GB for some models)
- Legacy GPU support (RTX 10XX-20XX through modern cards)
- INT8, FP8, GGUF, NVFP4, Nunchaku quantization
- Mask editor, prompt enhancer, pose/depth extractors
- Headless batch queue processing
- Docker, conda, and one-click installer support

### wajason/Nano_Cinema_AI_Video_Studio

- **URL:** https://github.com/wajason/Nano_Cinema_AI_Video_Studio
- **Stars:** 5

All-in-one local AI video production studio orchestrating Llama-3 (script), SDXL-Turbo (visuals), EdgeTTS (audio), and LTX-Video (motion). No API fees, full privacy.

## Apple Silicon / MLX Ports

### Blaizzy/mlx-video

- **URL:** https://github.com/Blaizzy/mlx-video
- **Stars:** 190 | **Forks:** 30 | **License:** MIT | **Commits:** 135

MLX-based inference and finetuning package for [[apple-silicon-setup|Apple Silicon Macs]].

**Supported models:** LTX-2 (19B), Wan2.1 (1.3B/14B), Wan2.2

**LTX-2 features via MLX:**
- Text-to-video, image-to-video, audio-to-video
- Joint audio-video synthesis
- Multiple pipelines (distilled, dev, dev-two-stage, dev-two-stage-hq)
- 2x spatial upscaling
- Prompt enhancement via Gemma

**Requirements:** macOS Apple Silicon, Python >= 3.11, MLX >= 0.22.0

### baisampayans/ltx-mlx

- **URL:** https://github.com/baisampayans/ltx-mlx
- **Stars:** 1

LTX-Video on Apple Silicon -- pure MLX implementation claiming 3.4x faster than PyTorch.

## Serverless and Cloud Deployments

### MushroomFleet/ltxv-modal

- **URL:** https://github.com/MushroomFleet/ltxv-modal
- **Stars:** 5

LTX-Video deployment on [[modal|Modal]] -- serverless AI video generation with cloud GPU scaling.

## CLI Tools

### AshutoshIIT1234/ltx-cli

- **URL:** https://github.com/AshutoshIIT1234/ltx-cli
- **Stars:** 2

A reproducible CLI for LTX-2 video generation with presets, configs, and smart defaults.

## Experimental / Niche

### audiohacking/ltx.cpp

- **URL:** https://github.com/audiohacking/ltx.cpp
- **Stars:** 3 | **Language:** C++

Unstable, experimental LTX 2.3 [[gguf-quantizations|GGUF]] inference in C++. Very early stage.

### Divhanthelion/ltx2

- **URL:** https://github.com/Divhanthelion/ltx2
- **Stars:** 8 | **Language:** Shell

LTX-2 generation on [[jetson-thor-deployment|NVIDIA Jetson Thor]] edge device. See the dedicated wiki page for details.

### Jonnnty/LTX-MultiMotion

- **URL:** https://github.com/Jonnnty/LTX-MultiMotion
- **Stars:** 2

Re-engineers encoder and decoder to enable dynamic motion generation for varying character counts.

## HuggingFace Diffusers (Native Integration)

The HuggingFace [[diffusers-integration|diffusers]] library has native LTX-Video pipeline support:

- Source: https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/ltx/pipeline_ltx_image2video.py
- Docs: https://github.com/huggingface/diffusers/blob/main/docs/source/en/api/pipelines/ltx_video.md

**Note:** The default Diffusers pipeline is described as "suboptimal" for LTXV models since it does not include STG or other inference-time tricks. The official LTX-Video repo's inference script is recommended for best quality.

## Related Pages

- [[github-official-repositories]] -- Official Lightricks repositories
- [[github-community-forks]] -- Community forks and derivatives
- [[comfyui-ltx-community-nodes]] -- ComfyUI-specific community nodes
- [[apple-silicon-setup]] -- Apple Silicon configuration guide
- [[desktop-video-tools]] -- Desktop tool competitive landscape
- [[ltx-integration-projects]] -- Full integration landscape
