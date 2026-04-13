# Community-Built Tools, Wrappers, and UIs for LTX Video

> **Sources:**
> - <https://github.com/topics/ltx-video>
> - Various web searches and repo pages

## Overview

A rich ecosystem of community tools has emerged around LTX Video, spanning ComfyUI integrations, desktop apps, Apple Silicon ports, serverless deployments, and multi-model frameworks.

---

## ComfyUI Ecosystem

### 1. logtd/ComfyUI-LTXTricks (DEPRECATED)

> **URL:** <https://github.com/logtd/ComfyUI-LTXTricks>

| Metric | Value |
|--------|-------|
| Stars | 512 |
| Forks | 23 |
| License | GPL-3.0 |
| Status | DEPRECATED -- merged into official ComfyUI-LTXVideo |

A set of ComfyUI nodes providing additional control for the LTX Video model. Implementations include:

- **RF-Inversion:** Semantic image inversion using rectified SDEs
- **RF-Edit:** Controlled video editing through rectified flow manipulation
- **FlowEdit:** Inversion-free text-based editing using pre-trained flow models
- **I+V2V:** Video-to-video generation with reference image guidance
- **Enhance (STG):** Spatiotemporal Skip Guidance for improved diffusion sampling
- **Interpolation/Frame Setting:** Now integrated into core ComfyUI

All nodes and functionality have been moved to the official Lightricks/ComfyUI-LTXVideo repository.

### 2. ID-LoRA/ID-LoRA-LTX2.3-ComfyUI

> **URL:** <https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI>

Custom ComfyUI node for generating videos with audio-visual identity based on a reference voice and image. Native ID-LoRA support is now in upstream ComfyUI, including the LTXVReferenceAudio node for reference-audio speaker identity transfer.

### 3. LTX-desktop/ComfyUI-LTX-2.3

> **URL:** <https://github.com/LTX-desktop/ComfyUI-LTX-2.3>

A lightweight utility for automatic deployment of LTX-2.3 in ComfyUI. Automatically downloads weights, verifies checksums, and installs dependencies.

### 4. meageropoulos/ComfyUI_LTXVideo

> **URL:** <https://github.com/meageropoulos/ComfyUI_LTXVideo>

Alternative custom nodes collection for LTX-Video in ComfyUI.

### 5. ComfyUI-RogoAI-4DTimelapse

> **URL:** <https://github.com/RogoAI-Takeji/ComfyUI-RogoAI-4DTimelapse>

4D Timelapse video generation for ComfyUI -- traverse Time x Angle x Elevation with LTX-Video 2.

---

## Desktop and Native Apps

### 6. james-see/ltx-video-mac

> **URL:** <https://github.com/james-see/ltx-video-mac>

| Metric | Value |
|--------|-------|
| Stars | 137 |
| Forks | 14 |
| License | MIT |
| Language | Swift (92.6%), Python (4.8%) |
| Releases | 83 (latest: v2.3.52, April 2026) |

Native macOS SwiftUI app for AI video generation with synchronized audio using LTX-2 on Apple Silicon via MLX framework.

**Features:**
- Text-to-video and image-to-video
- Synchronized audio generation
- Text-to-speech voiceover (ElevenLabs cloud or MLX-Audio local)
- AI-generated background music with 54 genre presets
- Generation queue with real-time progress
- Gemma prompt enhancement

**Requirements:** macOS 14.0+, Apple Silicon, 32GB RAM minimum (64GB+ recommended), 20-42GB disk

### 7. LTX-2-desktop/LTX-2.3

> **URL:** <https://github.com/LTX-2-desktop/LTX-2.3>

Community-packaged LTX-2.3 with dynamic CPU offloading and 8-bit/4-bit quantization, targeting mid-range PCs. Emphasizes uncensored generation.

---

## Multi-Model Frameworks

### 8. deepbeepmeep/Wan2GP (WanGP)

> **URL:** <https://github.com/deepbeepmeep/Wan2GP>

| Metric | Value |
|--------|-------|
| Stars | 5,200 |
| Forks | 747 |

"The best Open Source Video Generative Models Accessible to the GPU Poor."

**Supported models:** Wan 2.1/2.2, Hunyuan Video, LTX Video (2.0, 2.3, 2-ID variants), Flux, Qwen3, MMAudio, and more.

**LTX integration:** LTX Video is a primary video generation engine with features including IC-LoRAs and control video support.

**Key features:**
- VRAM optimization (as low as 6GB for some models)
- Legacy GPU support (RTX 10XX-20XX) through modern cards
- INT8, FP8, GGUF, NV FP4, Nunchaku quantization
- Mask editor, prompt enhancer, pose/depth extractors
- Headless batch queue processing
- Docker, conda, and one-click installer support

### 9. wajason/Nano_Cinema_AI_Video_Studio

> **URL:** <https://github.com/wajason/Nano_Cinema_AI_Video_Studio>

| Stars | 5 |

All-in-one local AI video production studio orchestrating:
- Llama-3 (Script)
- SDXL-Turbo (Visuals)
- EdgeTTS (Audio)
- LTX-Video (Motion)

No API fees, full privacy.

---

## Apple Silicon / MLX Ports

### 10. Blaizzy/mlx-video

> **URL:** <https://github.com/Blaizzy/mlx-video>

| Metric | Value |
|--------|-------|
| Stars | 190 |
| Forks | 30 |
| License | MIT |
| Commits | 135 |

MLX-based inference and finetuning package for Apple Silicon Macs.

**Supported models:** LTX-2 (19B), Wan2.1 (1.3B/14B), Wan2.2

**LTX-2 features via MLX:**
- Text-to-Video, Image-to-Video, Audio-to-Video
- Joint audio-video synthesis
- Multiple pipelines (distilled, dev, dev-two-stage, dev-two-stage-hq)
- 2x spatial upscaling
- Prompt enhancement via Gemma

**Requirements:** macOS Apple Silicon, Python >= 3.11, MLX >= 0.22.0

### 11. baisampayans/ltx-mlx

> **URL:** <https://github.com/baisampayans/ltx-mlx>

| Stars | 1 |

LTX-Video on Apple Silicon -- pure MLX implementation claiming 3.4x faster than PyTorch.

---

## Serverless and Cloud Deployments

### 12. MushroomFleet/ltxv-modal

> **URL:** <https://github.com/MushroomFleet/ltxv-modal>

| Stars | 5 |

LTX-Video deployment on Modal -- serverless AI video generation with cloud GPU scaling.

---

## CLI Tools

### 13. AshutoshIIT1234/ltx-cli

> **URL:** <https://github.com/AshutoshIIT1234/ltx-cli>

| Stars | 2 |

A reproducible CLI for LTX-2 video generation with presets, configs, and smart defaults.

---

## Experimental / Niche

### 14. audiohacking/ltx.cpp

> **URL:** <https://github.com/audiohacking/ltx.cpp>

| Stars | 3 | Language | C++ |

Unstable, experimental LTX 2.3 GGUF inference in C++.

### 15. Divhanthelion/ltx2

> **URL:** <https://github.com/Divhanthelion/ltx2>

| Stars | 8 | Language | Shell |

LTX2 generation on NVIDIA Jetson Thor.

### 16. Jonnnty/LTX-MultiMotion

> **URL:** <https://github.com/Jonnnty/LTX-MultiMotion>

| Stars | 2 |

Re-engineers encoder and decoder to enable dynamic motion generation for varying character counts.

---

## Diffusers Integration (HuggingFace)

The HuggingFace `diffusers` library has native LTX-Video pipeline support:
- Source: <https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/ltx/pipeline_ltx_image2video.py>
- Docs: <https://github.com/huggingface/diffusers/blob/main/docs/source/en/api/pipelines/ltx_video.md>

Note: The default Diffusers pipeline is described as "suboptimal" for LTXV models since it does not include STG or other inference-time tricks. The official LTX-Video repo's inference script is recommended for best quality.
