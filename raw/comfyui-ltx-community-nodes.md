# Community ComfyUI Nodes for LTX Video

## Sources
- [GitHub - logtd/ComfyUI-LTXTricks](https://github.com/logtd/ComfyUI-LTXTricks)
- [GitHub - m4rio/ComfyUI-LTXTricks-RUNWAY](https://github.com/m4rio/ComfyUI-LTXTricks-RUNWAY)
- [GitHub - RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management](https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management)
- [GitHub - ID-LoRA/ID-LoRA-LTX2.3-ComfyUI](https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI)
- [GitHub - chengzeyi/Comfy-WaveSpeed](https://github.com/chengzeyi/Comfy-WaveSpeed)
- [GitHub - LTX-2-desktop/ComfyUI-LTX-2.3](https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3)
- [GitHub - wildminder/awesome-ltx2](https://github.com/wildminder/awesome-ltx2)
- [GitHub - EricRollei/Local_LLM_Prompt_Enhancer](https://github.com/EricRollei/Local_LLM_Prompt_Enhancer)
- [Kijai/LTXV2_comfy on HuggingFace](https://huggingface.co/Kijai/LTXV2_comfy)
- [ComfyUI-LTXTricks detailed guide | RunComfy](https://www.runcomfy.com/comfyui-nodes/ComfyUI-LTXTricks)

## Overview

Beyond the official Lightricks-maintained ComfyUI-LTXVideo extension, the community has developed several third-party node packs that extend LTX Video's capabilities in ComfyUI. Some of these have been deprecated as their features were merged into the official repository, while others provide unique functionality not available elsewhere.

---

## ComfyUI-LTXTricks (DEPRECATED)

**Repository:** [logtd/ComfyUI-LTXTricks](https://github.com/logtd/ComfyUI-LTXTricks)
**Status:** Deprecated — All functionality migrated to official ComfyUI-LTXVideo repository
**Stars:** 512 | **License:** GPL-3.0

### Features Provided (Historical)

1. **RF-Inversion** — Semantic image inversion using rectified stochastic differential equations
2. **RF-Edit (RF-Solver-Edit)** — Video editing through flow-based inversion techniques
3. **FlowEdit** — Inversion-free text-based video editing using pre-trained flow models
4. **Image and Video to Video (I+V2V)** — Video-to-video transformation while referencing a static image
5. **STG (Spatiotemporal Skip Guidance)** — Enhanced video diffusion sampling

### Migration Notes

Users should migrate to the official repository at [Lightricks/ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo). Core ComfyUI has integrated key functionality:
- Use `LTXVKeyFrameConditioning` instead of legacy interpolation nodes
- Use `LTXVSequenceConditioning` instead of legacy frame setting nodes

### Fork: ComfyUI-LTXTricks-RUNWAY

A fork by m4rio exists at [m4rio/ComfyUI-LTXTricks-RUNWAY](https://github.com/m4rio/ComfyUI-LTXTricks-RUNWAY) that adapts the LTXTricks nodes for additional runway-style workflows.

---

## ComfyUI_LTX-2_VRAM_Memory_Management

**Repository:** [RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management](https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management)
**Status:** Active

### Purpose

Dramatically reduces VRAM usage for LTX-2 video generation in ComfyUI, enabling generation of 800-900+ frame videos (at 1920x1088) on a single 24GB GPU.

### How It Works

LTX-2's FeedForward layers create massive intermediate tensors that normally limit video length. This node pack chunks those operations to reduce peak memory by up to 8x, without any quality loss.

### Key Features

- Enables high-resolution outputs and high frame numbers
- Multi-GPU distribution support for distributing VRAM across multiple GPUs
- Allows generation of videos that would be impossible on a single GPU
- No quality degradation — purely a memory optimization

### Installation

Clone into ComfyUI custom_nodes directory. No additional dependencies required beyond ComfyUI.

---

## ID-LoRA-LTX2.3-ComfyUI

**Repository:** [ID-LoRA/ID-LoRA-LTX2.3-ComfyUI](https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI)
**Status:** Active (partially merged into upstream ComfyUI as of March 2026)

### Purpose

Custom ComfyUI nodes for generating videos with audio-visual identity based on a reference voice and image, built on LTX-2.3.

### Key Features

- **Identity Preservation** — Transfers a speaker's vocal identity from a reference audio clip into a generated talking-head video
- **Unified Latent Space** — First method to personalize visual appearance and voice within a single generative pass
- **One-Stage and Two-Stage Pipelines** — Two-stage generates at target resolution then refines at 2x with spatial upsampling
- **Built on LTX-2.3 (22B)** — Leverages the full audio-video generation capabilities

### Available Models

- CelebVHQ-3K variant
- TalkVid-3K variant (available on HuggingFace)

### Upstream Integration

As of March 24, 2026, native ComfyUI ID-LoRA support was merged in PR #13111, adding the `LTXVReferenceAudio` node. Original ID-LoRA weights work as-is without conversion.

---

## Comfy-WaveSpeed

**Repository:** [chengzeyi/Comfy-WaveSpeed](https://github.com/chengzeyi/Comfy-WaveSpeed)
**Status:** Active (WIP)

### Purpose

All-in-one inference optimization solution for ComfyUI that includes LTX Video support among other models.

### LTX-Specific Features

- **First Block Cache (FBCache)** — Uses the residual output of the first transformer block as cache indicator. If the difference between current and previous residual output is small enough, it reuses the previous output. Speedup of 1.5x to 3.0x with acceptable accuracy loss.
- **Enhanced torch.compile** — Improves PyTorch compilation with LoRA compatibility
- Supports LTXV in both native and non-native modes

### Supported Models

FLUX, LTXV, HunyuanVideo, SD3.5, SDXL, and more.

---

## Kijai/LTXV2_comfy

**Platform:** [HuggingFace - Kijai/LTXV2_comfy](https://huggingface.co/Kijai/LTXV2_comfy)
**Status:** Active

### Purpose

Separated checkpoint implementation of LTX2 designed for alternative loading methods in ComfyUI, particularly useful for GGUF quantized model support.

### Key Features

- Provides separated LTX2 checkpoints for alternative model loading
- Hosts VAE models: `LTX2_audio_vae_bf16.safetensors` and `LTX2_video_vae_bf16.safetensors`
- Text encoder: `ltx-2-19b-embeddings_connector_bf16.safetensors`
- Compatible with GGUF quantized models (requires up-to-date GGUF nodes)

### Usage with GGUF

Requires ComfyUI-KJNodes for model loading. Newer LTX2 models need GGUF nodes that support reading model config from metadata.

---

## ComfyUI-LTX-2.3 (Desktop Utility)

**Repository:** [LTX-2-desktop/ComfyUI-LTX-2.3](https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3)
**Status:** Active

### Purpose

Lightweight utility for automatic deployment of LTX-2.3 in a local ComfyUI environment. Automates downloads, checksum verification, and dependency installation.

### Key Features

- **Automatic Path Discovery** — Locates models/unet, models/vae, and custom_nodes folders automatically
- **Incremental Updates** — SHA256 hash verification prevents re-downloading existing files
- **Dependency Installation** — Automatically runs pip install via embedded Python interpreter
- **One-Click Setup** — Download and run `LTX2.3_ComfyUI.exe` from Releases section

### Installation

1. Download `LTX2.3_ComfyUI.exe` from Releases
2. Run the executable
3. Wait for process to complete (downloads ~18 GB of model weights)
4. Launch ComfyUI and refresh browser

---

## Local LLM Prompt Enhancer

**Repository:** [EricRollei/Local_LLM_Prompt_Enhancer](https://github.com/EricRollei/Local_LLM_Prompt_Enhancer)
**Status:** Active

### Purpose

AI-powered prompt enhancement nodes for ComfyUI using local LLMs via LM-Studio, Ollama, etc. Enhances prompts specifically for Text-to-Video and Image-to-Video workflows including LTX Video, Wan, and others.

### Supported Targets

Wan, Wan-Image, Qwen, Qwen-edit, Flux, SDXL, Pony, and LTX Video models.

---

## awesome-ltx2 (Resource List)

**Repository:** [wildminder/awesome-ltx2](https://github.com/wildminder/awesome-ltx2)
**Status:** Active

### Purpose

Curated list of all available LTX-2 models, text encoders, workflows, and LoRAs for ComfyUI. An essential reference for finding community resources.

### Contents

- Complete model listings (full weights, transformers-only, GGUF quantizations)
- Text encoder options including abliterated Gemma variants
- LoRA collection (Union Control, Motion Track, Outpaint, Cameraman, Audio-Reactive, etc.)
- Workflow templates and examples
- Community tools and utilities
