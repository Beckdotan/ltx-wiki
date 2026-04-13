---
title: Community ComfyUI Nodes for LTX Video
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management
  - https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI
  - https://github.com/chengzeyi/Comfy-WaveSpeed
  - https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3
  - https://github.com/EricRollei/Local_LLM_Prompt_Enhancer
  - https://github.com/wildminder/awesome-ltx2
  - https://huggingface.co/Kijai/LTXV2_comfy
  - https://github.com/m4rio/ComfyUI-LTXTricks-RUNWAY
  - https://www.runcomfy.com/comfyui-nodes/ComfyUI-LTXTricks
tags:
  - comfyui
  - ltx-video
  - community
  - nodes
---

# Community ComfyUI Nodes for LTX Video

Beyond the [[comfyui-ltxvideo-official-nodes|official Lightricks extension]], the community has developed several third-party node packs for [[comfyui-ltx-integration-overview|LTX Video in ComfyUI]]. Some have been deprecated as their features merged upstream; others provide unique functionality.

## ComfyUI_LTX-2_VRAM_Memory_Management

**Repository:** https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management
**Status:** Active

Dramatically reduces VRAM usage by chunking FeedForward layer operations. LTX-2's FeedForward layers create massive intermediate tensors that normally limit video length. This node pack chunks those operations to reduce peak memory by up to 8x with zero quality loss.

**Key capabilities:**
- Enables 800-900+ frame videos at 1920x1088 on a single 24GB GPU
- Multi-GPU distribution support
- No quality degradation -- purely a memory optimization
- Critical for high-resolution, long-duration generation

Install by cloning into ComfyUI `custom_nodes` directory. No additional dependencies needed.

See [[comfyui-ltx-performance-tips]] for how this fits into broader VRAM optimization strategy.

## ID-LoRA-LTX2.3-ComfyUI

**Repository:** https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI
**Status:** Active (partially merged upstream as of March 2026)

Custom nodes for generating videos with audio-visual identity based on reference voice and image, built on [[ltx-2-overview|LTX-2.3]].

**Key features:**
- **Identity preservation** -- Transfers vocal identity from reference audio into talking-head video
- **Unified latent space** -- First method to personalize visual appearance and voice in a single generative pass
- **One-stage and two-stage pipelines** available
- Built on LTX-2.3 22B

**Models:** CelebVHQ-3K and TalkVid-3K variants (HuggingFace).

**Upstream integration:** As of March 24, 2026, native ComfyUI ID-LoRA support was merged (PR #13111) via the `LTXVReferenceAudio` node. Original weights work without conversion.

See [[comfyui-ltx-lora-training-control]] for broader LoRA documentation.

## Comfy-WaveSpeed

**Repository:** https://github.com/chengzeyi/Comfy-WaveSpeed
**Status:** Active (WIP)

All-in-one inference optimization for ComfyUI with LTX Video support.

**LTX-specific features:**
- **First Block Cache (FBCache)** -- Reuses transformer block outputs when residual differences are small. 1.5x to 3.0x speedup with acceptable quality trade-off.
- **Enhanced torch.compile** -- Improved PyTorch compilation with LoRA compatibility
- Supports LTXV in native and non-native modes

Also supports FLUX, HunyuanVideo, SD3.5, SDXL, and more.

See [[comfyui-ltx-performance-tips]] for speed optimization context.

## Kijai/LTXV2_comfy

**Platform:** https://huggingface.co/Kijai/LTXV2_comfy
**Status:** Active

Separated checkpoint implementation of LTX-2 for alternative loading methods, particularly GGUF quantized model support.

**Provides:**
- Separated LTX-2 checkpoints for alternative model loading
- VAE models: `LTX2_audio_vae_bf16.safetensors`, `LTX2_video_vae_bf16.safetensors`
- Text encoder: `ltx-2-19b-embeddings_connector_bf16.safetensors`
- GGUF quantized model compatibility (requires ComfyUI-GGUF nodes from city96)

Requires ComfyUI-KJNodes for model loading. Essential for low-VRAM setups using quantized models.

## ComfyUI-LTX-2.3 (Desktop Utility)

**Repository:** https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3
**Status:** Active

One-click deployment utility for LTX-2.3 in local ComfyUI environments.

**Features:**
- Automatic path discovery for models/unet, models/vae, custom_nodes
- SHA256 hash verification prevents re-downloading
- Automatic dependency installation via embedded Python
- Downloads ~18 GB of model weights

Download `LTX2.3_ComfyUI.exe` from Releases and run.

## Local LLM Prompt Enhancer

**Repository:** https://github.com/EricRollei/Local_LLM_Prompt_Enhancer
**Status:** Active

AI-powered prompt enhancement using local LLMs via LM-Studio, Ollama, etc. Supports LTX Video, Wan, FLUX, SDXL, Pony, and other targets.

## awesome-ltx2 (Resource List)

**Repository:** https://github.com/wildminder/awesome-ltx2
**Status:** Active

Curated list of all available LTX-2 models, text encoders, workflows, and LoRAs. Contents include:
- Complete model listings (full weights, transformers-only, GGUF quantizations)
- Text encoder options including abliterated Gemma variants
- LoRA collection (Union Control, Motion Track, Outpaint, Cameraman, Audio-Reactive, etc.)
- Workflow templates and community tools

## ComfyUI-LTXTricks (Deprecated)

See [[comfyui-ltxtricks]] for the deprecated community node pack whose features have been merged into the official extension.

## Related Pages

- [[comfyui-ltxvideo-official-nodes]] -- Official Lightricks extension
- [[comfyui-ltxtricks]] -- Deprecated LTXTricks pack
- [[comfyui-manager-ltx-setup]] -- Installing extensions via ComfyUI Manager
- [[comfyui-ltx-performance-tips]] -- Performance optimization
- [[ltx-awesome-resources]] -- Full directory of models, LoRAs, workflows, and tutorials
