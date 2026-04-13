---
title: ComfyUI LTX Video Integration Overview
type: overview
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/ComfyUI-LTXVideo
  - https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
  - https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
  - https://blog.comfy.org/p/ltxv-day-1-comfyui
  - https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo
  - https://ltx.io/model/model-blog/comfyui-workflow-guide
  - raw/comfyui-ltx-version-history.md
tags:
  - comfyui
  - ltx-video
  - integration
  - overview
---

# ComfyUI LTX Video Integration Overview

ComfyUI is the recommended interface for working with [[ltx-video-overview|LTX Video]] models. LTX Video received day-1 native support in ComfyUI from its initial release, and [[ltx-2-overview|LTX-2]] is built directly into ComfyUI core, making basic functionality available without any custom node installation.

## Architecture

The integration operates at two levels:

- **ComfyUI Core** -- LTX-2 model loading, basic sampling, KSampler compatibility, and VAE decoding are built into ComfyUI itself. This enables basic text-to-video and image-to-video workflows out of the box.
- **ComfyUI-LTXVideo Extension** -- The [[comfyui-ltxvideo-official-nodes|official Lightricks extension]] adds advanced capabilities including IC-LoRA control, prompt enhancement, low-VRAM loaders, tiled processing, audio-video generation controls, and spatial/temporal upscaling.

## Repository

- **URL:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Stars:** ~3,400 | **Forks:** ~368
- **Languages:** Python (93.7%), JavaScript (6.3%)

## Integration Timeline

| Version | Date | Parameters | ComfyUI Milestone |
|---------|------|-----------|-------------------|
| v0.9 | Nov 2024 | 2B | Day-1 ComfyUI support; official custom nodes branded "LTXVideo"; 768x512 @ 24fps; T5-XXL text encoder |
| v0.9.1 | Late 2024 | 2B | Low VRAM improvements; spatio-temporal skip guidance; all-in-one workflows (vid2vid, txt2vid, img2vid) |
| v0.9.5 | Early 2025 | 2B | Day-1 ComfyUI support; multi-keyframe control; longer video; updated docs on docs.comfy.org |
| v0.9.7 | Mid 2025 | 13B | Florence2 autocaptioning integration; start/end frame animation; `LTXVPreprocess -> LTXVAddGuide` chain |
| LTX-2 | Late 2025 | 19B | **Native ComfyUI core integration** (built-in, not just custom nodes); IC-LoRA control; Gemma text encoder; day-0 support |
| LTX-2.3 | Early 2026 | 22B | Audio generation; Gemma 3 12B; spatial/temporal upscalers; [[comfyui-ltx-node-reference\|MultimodalGuider]]; one-stage and two-stage pipelines; desktop auto-installer |

### Ecosystem Events

| Date | Event |
|------|-------|
| Late 2024 | ComfyUI-LTXTricks community nodes created (logtd) |
| Mar 2026 | ID-LoRA merged upstream into ComfyUI (PR #13111) via LTXVReferenceAudio |
| Mar 2026 | ComfyUI-LTXTricks deprecated; functionality migrated to official ComfyUI-LTXVideo repo |

### ComfyUI-LTXTricks Lifecycle

ComfyUI-LTXTricks was a community node pack by logtd providing RF-Inversion, RF-Edit, FlowEdit, I+V2V, and STG. It peaked at 512 GitHub stars before being deprecated. Its functionality was replaced by `LTXVKeyFrameConditioning` and `LTXVSequenceConditioning` in core ComfyUI. See [[comfyui-ltx-community-nodes]] for current active community nodes.

## Supported Workflow Types

- **Text-to-Video** -- Generate video from text prompts
- **Image-to-Video** -- Animate static images
- **Video-to-Video** -- Transform existing footage
- **Audio-to-Video** -- Synchronized audio-video generation (LTX-2.3)
- **Frame Interpolation** -- Smooth transitions between keyframes
- **Video Extension** -- Extend video via sequence conditioning

See [[comfyui-ltx-workflows]] for detailed workflow documentation.

## Processing Pipelines

- **Single-stage** -- Generate directly at target resolution (quick prototyping)
- **Two-stage** -- Generate at half resolution, upscale in latent space, refine (production quality)
- **Full pipeline** -- Spatial + temporal upscaling for maximum quality

See [[comfyui-ltx-two-stage-pipeline]] for the upscaling pipeline.

## System Requirements

| Component | Requirement |
|-----------|-------------|
| GPU | CUDA-compatible, 32GB+ VRAM recommended |
| Disk | 100-160 GB free for models, environment, outputs |
| RAM | 16 GB minimum, 32 GB recommended |
| Python | 3.10+ |
| CUDA | 12.2+ |
| PyTorch | 2.1.2+ |

Lower VRAM configurations are possible with [[comfyui-ltx-performance-tips|optimization strategies]].

## Installation

The extension installs via [[comfyui-manager-ltx-setup|ComfyUI Manager]] (recommended), Git URL, or manual clone. Models download automatically on first use.

## Related Pages

- [[comfyui-ltxvideo-official-nodes]] -- Official node extension details
- [[comfyui-ltx-node-reference]] -- Complete node reference
- [[comfyui-ltx-community-nodes]] -- Third-party node packs
- [[comfyui-ltx-lora-training-control]] -- LoRA and IC-LoRA control system
- [[comfyui-ltx-performance-tips]] -- VRAM optimization and speed tips
- [[comfyui-ltx-model-comparison]] -- Comparison with other video models in ComfyUI
- [[comfyui-ltx-workflow-tutorials]] -- Tutorial catalog
- [[ltx-awesome-resources]] -- Curated directory of models, LoRAs, workflows, and tutorials
- [[ltx-video-versions]] -- Full LTX Video version timeline
- [[ltx-2-version-history]] -- LTX-2 release history
- [[audio-video-generation]] -- Audio-video generation concept and workflows
