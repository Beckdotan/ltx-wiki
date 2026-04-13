---
title: Official GitHub Repositories
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/github-ltx-video-official.md
  - raw/github-ltx-2-official.md
  - raw/github-lightricks-other-official-repos.md
tags:
  - github
  - official
  - repositories
  - lightricks
  - open-source
---

# Official GitHub Repositories

[[lightricks-company]] maintains seven public GitHub repositories directly related to the [[ltx-ecosystem]]. The organization has 153 total public repositories, but these seven form the core of the LTX Video open-source offering.

## Repository Overview

| Repository | Stars | Forks | Language | License | Purpose |
|------------|-------|-------|----------|---------|---------|
| [LTX-Video](https://github.com/Lightricks/LTX-Video) | ~10,000 | ~960 | Python | OpenRail-M | Original model and inference |
| [LTX-2](https://github.com/Lightricks/LTX-2) | ~5,800 | ~891 | Python (100%) | See repo | Next-gen audio-video model (monorepo) |
| [ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo) | ~3,400 | ~368 | Python (94%), JS (6%) | -- | [[comfyui-ltx-integration-overview|ComfyUI integration]] nodes |
| [LTX-Desktop](https://github.com/Lightricks/LTX-Desktop) | ~1,400 | ~268 | TypeScript (72%), Python (26%) | Apache-2.0 | [[ltx-desktop|Desktop video generation app]] |
| [LTX-Video-Trainer](https://github.com/Lightricks/LTX-Video-Trainer) | 424 | 58 | Python (100%) | Apache-2.0 | [[ltx-video-trainer|LoRA and fine-tuning toolkit]] |
| [LTX-Video-Q8-Kernels](https://github.com/Lightricks/LTX-Video-Q8-Kernels) | 81 | 18 | Python (38%), CUDA (35%), C++ (18%) | -- | [[fp8-quantization|FP8 CUDA kernels]] |
| [ltx-analytics-agents](https://github.com/Lightricks/ltx-analytics-agents) | 3 | -- | Python | -- | Product data analytics (internal) |

## Lightricks/LTX-Video

The original and most-starred LTX repository. Houses the LTX-Video model family (2B and 13B parameter variants, versions v0.9.0 through v0.9.8).

- **Paper:** arXiv:2501.00103
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-Video
- **Documentation:** https://docs.ltx.video
- **Open Issues:** 78 (of ~276+ total filed)
- **Formal GitHub Releases:** None (users clone the repo directly)

### Key Features

- [[ltx-video-overview|Multimodal conditioning]]: image-to-video, video extension, multi-keyframe support
- Native 4K resolution, up to 50 FPS
- [[fp8-quantization|FP8 quantization]] for reduced VRAM
- [[lora-ecosystem|LoRA and IC-LoRA]] support
- Distilled variants with 15x faster inference
- Up to 60 seconds of video generation (v0.9.8)

### Installation

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference]
```

Requires Python 3.10.5+, PyTorch >= 2.1.2, CUDA 12.2 (or MPS on macOS with PyTorch 2.3.0 or >= 2.6).

See [[ltx-video-code-structure]] for the repository layout.

### Release Timeline

| Date | Version | Key Milestone |
|------|---------|---------------|
| December 2024 | v0.9.0 | Initial release, paper published |
| March 2025 | v0.9.5 | Commercial-use OpenRail-M license |
| April 2025 | -- | Enhanced prompt understanding |
| May 2025 | v0.9.7 | 13B model |
| July 2025 | v0.9.8 | Distilled models, up to 60 seconds |
| October 2025 | -- | LTX-2 announced as successor |

## Lightricks/LTX-2

The successor to LTX-Video, organized as a monorepo with three independent packages. First DiT-based audio-video foundation model with fully open weights, inference pipelines, and training code.

- **Paper:** arXiv:2601.03233
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2
- **Playground:** https://console.ltx.video/playground
- **Total Commits:** 27
- **Watchers:** 58

### Key Architecture

- [[ltx-2-architecture|Dual-stream transformer]]: 14B-parameter video stream + 5B-parameter audio stream
- Bidirectional cross-modal attention for synchronized generation
- Text encoder: Gemma 3 (12B) with multi-layer feature extraction
- 4x larger text connector vs LTX-Video
- Causal audio VAE producing high-fidelity 1D latent space

### Available Pipelines (8 total)

| Pipeline | Purpose |
|----------|---------|
| TI2VidTwoStagesPipeline | Text/image-to-video (recommended, 2x upsampling) |
| TI2VidTwoStagesHQPipeline | High-quality two-stage (res_2s sampler) |
| TI2VidOneStagePipeline | Quick prototyping (single-stage) |
| DistilledPipeline | Fastest inference (8 predefined sigmas) |
| ICLoraPipeline | Video/image transformation (distilled model) |
| KeyframeInterpolationPipeline | Between image keyframes |
| A2VidPipelineTwoStage | Audio-conditioned generation |
| RetakePipeline | Selective regeneration of time regions |

### Main Checkpoints

- `ltx-2.3-22b-dev.safetensors` (development version)
- `ltx-2.3-22b-distilled.safetensors` (distilled version)
- Spatial upscalers: x1.5 and x2 versions
- Temporal upscaler: x2 frame interpolation
- Distilled LoRA: `ltx-2.3-22b-distilled-lora-384.safetensors`

### Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
uv sync --frozen
source .venv/bin/activate
```

See [[ltx-2-code-structure]] for the monorepo layout.

## Lightricks/ComfyUI-LTXVideo

Custom nodes extending ComfyUI for [[ltx-2-overview|LTX-2]] video generation. LTX-2 is built into ComfyUI core; this repo provides additional nodes and workflows beyond what ships with ComfyUI.

- **Stars:** ~3,400 | **Forks:** ~368 | **Watchers:** 32
- **System Requirements:** CUDA GPU with 32GB+ VRAM, 100GB+ disk

### Supported Features

- Text-to-video and image-to-video generation
- Video-to-video refinement (detailing)
- IC-LoRA controlled generation (depth, pose, edges)
- Motion tracking and camera control
- Two-stage pipeline with spatial and temporal upsampling
- Union IC-LoRA (unified depth + edge control)
- Tiled VAE decoding for efficiency
- Looping sampler for seamless video
- Low VRAM optimization options

### Installation

Via ComfyUI Manager -- search for "LTXVideo" and install. Models download automatically on first use.

See [[comfyui-ltxvideo-official-nodes]] and [[comfyui-ltx-community-nodes]] for more.

## Lightricks/LTX-Video-Trainer

Community trainer enabling LoRA training, full fine-tuning, and video-to-video transformation workflows on custom datasets.

- **Stars:** 424 | **Forks:** 58 | **Commits:** 85
- **Authors:** Matan Ben Yosef, Naomi Ken Korem, Tavi Halperin (2025)
- For LTX-2 training, users are directed to the `ltx-trainer` package within the LTX-2 monorepo

See [[ltx-video-trainer]] for full details.

## Lightricks/LTX-Video-Q8-Kernels

Specialized CUDA kernels for FP8-quantized inference of LTX-Video models. Reduces memory requirements and computational overhead compared to full-precision.

- **Stars:** 81 | **Forks:** 18
- **Latest Release:** 0.0.4 (May 6, 2025)
- **Requires:** CUDA Toolkit 12.8 or later
- For ComfyUI integration, apply the "LTXVideo Q8 Patcher" node to loaded models

See [[fp8-quantization]] for quantization strategy details.

## Lightricks/ltx-analytics-agents

Product data agents for LTX. Likely internal analytics tooling with minimal community relevance (3 stars).

## Related Pages

- [[ltx-ecosystem]] -- Full LTX product and platform ecosystem
- [[ltx-video-code-structure]] -- LTX-Video repository layout
- [[ltx-2-code-structure]] -- LTX-2 monorepo layout
- [[github-community-forks]] -- Community forks and derivatives
- [[github-community-tools]] -- Community-built tools and wrappers
- [[github-issues-known-limitations]] -- Issues, discussions, and known limitations
- [[adoption-metrics]] -- Download and star metrics
