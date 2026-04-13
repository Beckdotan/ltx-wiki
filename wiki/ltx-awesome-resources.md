---
title: LTX Video Awesome Resources Directory
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/comfyui-ltx-awesome-resources.md
  - https://github.com/wildminder/awesome-ltx2
  - https://huggingface.co/RuneXX/LTX-2.3-Workflows
  - https://huggingface.co/Kijai/LTXV2_comfy
  - https://huggingface.co/GitMylo/LTX-2-comfy_gemma_fp8_e4m3fn
tags:
  - ltx-video
  - resources
  - models
  - loras
  - workflows
  - community
  - comfyui
---

# LTX Video Awesome Resources Directory

A curated directory of repositories, model downloads, LoRA collections, workflow sources, tutorials, and community hubs for [[ltx-video-overview|LTX Video]] and [[ltx-2-overview|LTX-2]]. The primary upstream source is the [awesome-ltx2](https://github.com/wildminder/awesome-ltx2) repository by wildminder on GitHub.

## Official Repositories

| Repository | Description | URL |
|-----------|-------------|-----|
| Lightricks/LTX-Video | Original LTX Video repository | https://github.com/Lightricks/LTX-Video |
| Lightricks/LTX-2 | Current official Python inference + LoRA trainer | https://github.com/Lightricks/LTX-2 |
| Lightricks/ComfyUI-LTXVideo | Official ComfyUI custom nodes | https://github.com/Lightricks/ComfyUI-LTXVideo |

## Official Documentation

| Resource | URL |
|---------|-----|
| LTX Official Docs | https://docs.ltx.video/ |
| ComfyUI Official Docs (LTX-2.3) | https://docs.comfy.org/tutorials/video/ltx/ltx-2-3 |
| ComfyUI Official Docs (LTX-0.9.5) | https://docs.comfy.org/tutorials/video/ltxv |
| LTX Blog | https://ltx.io/model/model-blog/ |

## Model Downloads (HuggingFace)

### Checkpoints

| Model | Format | Source |
|-------|--------|--------|
| LTX-2.3 22B Dev | BF16 Safetensors | Lightricks/LTX-2.3 |
| LTX-2.3 22B Distilled | BF16 Safetensors | Lightricks/LTX-2.3 |
| LTX-2 19B | BF16 Safetensors | Lightricks/LTX-2 |
| LTXV2 Comfy (separated checkpoints) | Various | Kijai/LTXV2_comfy |
| LTXV2 GGUF quantized | GGUF | Kijai/LTXV2_comfy |

See [[gguf-quantizations]] for details on quantized model formats.

### Text Encoders

| Encoder | Description | Source |
|---------|-------------|--------|
| Gemma 3 12B IT | Official text encoder for LTX-2.3 | Google |
| Gemma 3 12B IT QAT Q4 | Quantized variant | Google |
| Gemma FP8 e4m3fn | FP8 quantized for ComfyUI | GitMylo/LTX-2-comfy_gemma_fp8_e4m3fn |
| Abliterated Gemma 3 12B | Unrestricted variant | FusionCow (community) |

### VAE Models

| Model | Purpose | Source |
|-------|---------|--------|
| LTX2_video_vae_bf16.safetensors | Video encoding/decoding | Kijai/LTXV2_comfy |
| LTX2_audio_vae_bf16.safetensors | Audio encoding/decoding | Kijai/LTXV2_comfy |

See [[video-vae]] for VAE architecture details.

### Upscaler Models

| Model | Purpose |
|-------|---------|
| ltx-2.3-spatial-upscaler-x2-1.0.safetensors | 2x resolution upscale |
| ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors | 1.5x resolution upscale |
| ltx-2.3-temporal-upscaler-x2-1.0.safetensors | 2x frame rate |

See [[ltxv-spatial-upscaler]] for upscaler documentation.

## LoRA Collection

### Official Lightricks LoRAs

| LoRA | Type | Purpose |
|------|------|---------|
| Union Control IC-LoRA | Control | Unified Canny + Depth + Pose |
| Motion Track IC-LoRA | Control | Sparse point trajectory tracking |
| Pose Control IC-LoRA | Control | Human pose-guided generation |
| Detailer LoRA | Enhancement | Video quality refinement |
| Cameraman v1 | Camera | General camera control |
| Dolly variants | Camera | Forward/backward movement |
| Jib variants | Camera | Vertical movement |
| Distilled LoRA (384) | Speed | Distilled model optimization |
| Squish LoRA | Effect | Aspect ratio manipulation |

See [[camera-control-loras]] and [[lora-training]] for further details.

### Community LoRAs

| LoRA | Creator | Purpose |
|------|---------|---------|
| IC-LoRA-Upgrade | Community | Enhanced upgrade effects |
| IC-LoRA-Outpaint | Community | Outpainting extension |
| Audio-Reactive LoRA | Community | Audio-reactive visual effects |
| VBVR-lora-I2V | Community | Complex visual reasoning |
| Transition LoRA | valiantcat | Scene transition effects |
| ID-LoRA CelebVHQ-3K | AviadDahan | Identity preservation (celebrity) |
| ID-LoRA TalkVid-3K | AviadDahan | Identity preservation (talking) |

See [[audio-video-generation]] for ID-LoRA usage in audio-video workflows.

## Community Custom Nodes

For detailed descriptions of each node pack, see [[comfyui-ltx-community-nodes]].

| Node Pack | Purpose | URL |
|-----------|---------|-----|
| ComfyUI-LTXVideo | Official advanced nodes | https://github.com/Lightricks/ComfyUI-LTXVideo |
| ComfyUI_LTX-2_VRAM_Memory_Management | VRAM optimization | https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management |
| ID-LoRA-LTX2.3-ComfyUI | Identity preservation | https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI |
| Comfy-WaveSpeed | Inference acceleration | https://github.com/chengzeyi/Comfy-WaveSpeed |
| ComfyUI-LTX-2.3 (Desktop) | Auto-installer | https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3 |
| ComfyUI-GGUF | GGUF model support | https://github.com/city96/ComfyUI-GGUF |
| ComfyUI-KJNodes | Alternative model loading | Via Kijai |
| Local_LLM_Prompt_Enhancer | AI prompt enhancement | https://github.com/EricRollei/Local_LLM_Prompt_Enhancer |

## Workflow Collections

### Official

| Source | URL |
|--------|-----|
| ComfyUI-LTXVideo example_workflows/ | Bundled with extension |
| ComfyUI Docs (LTX-2.3 examples) | https://docs.comfy.org/tutorials/video/ltx/ltx-2-3 |
| ComfyUI Docs (LTX-0.9.5 examples) | https://docs.comfy.org/tutorials/video/ltxv |

### Community

| Source | URL |
|--------|-----|
| RuneXX/LTX-2.3-Workflows | https://huggingface.co/RuneXX/LTX-2.3-Workflows |
| OpenArt LTX workflows | https://openart.ai (search "LTX") |
| Civitai LTX workflows | https://civitai.com (search "LTX Video Workflows") |
| RunComfy LTX workflows | https://www.runcomfy.com/comfyui-workflows (search "LTX") |

See [[comfyui-ltx-workflows]] for workflow type documentation.

## Tutorial and Guide Collections

| Resource | Focus | URL |
|---------|-------|-----|
| ComfyUI Wiki | Step-by-step beginner guide | https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide |
| ComfyUI-Box | Complete workflow guide | https://comfyui-box.com/advanced-tutorial/ltx-workflow/ |
| Stable Diffusion Art | Model-specific tutorials | https://stable-diffusion-art.com/ltx-video/ |
| WaveSpeedAI Blog | Quickstart + optimization | https://wavespeed.ai/blog/ |
| CrePal | Installation + upscaling | https://crepal.ai/blog/aivideo/ |
| Next Diffusion | Step-by-step tutorials | https://www.nextdiffusion.ai/tutorials/ |
| ThinkDiffusion | Cloud + speed focus | https://learn.thinkdiffusion.com/ |
| LTX Blog | Official guides | https://ltx.io/model/model-blog/ |

## Community Hubs

| Platform | Purpose | URL |
|---------|---------|-----|
| LTX Discord | Official community support | Via docs.ltx.video |
| ComfyUI Discord | General ComfyUI help | Via comfy.org |
| ComfyUI Forum | Technical discussions | https://forum.comfy.org |
| r/StableDiffusion | Reddit community | https://reddit.com/r/StableDiffusion |
| HuggingFace Discussions | Model-specific discussions | Per-model discussion tabs |

See [[discord-community]] and [[reddit-community-discussions]] for community activity details.

## Comparison and Analysis Resources

| Resource | URL |
|---------|-----|
| Open source video model comparisons | https://www.comfyonline.app/blog/open-source-video-generation-models-comparisons |
| Best Open-Source Video Generation Models | https://www.whitefiber.com/blog/best-open-source-video-generation-model |
| 31 Open-Source AI Video Models | https://aifreeforever.com/blog/open-source-ai-video-models-free-tools-to-make-videos |
| 6 Best ComfyUI Text-to-Video Models | https://www.mimicpc.com/learn/creating-ai-videos-with-comfyui-text-to-video-workflows |

See [[ltx-model-comparisons]] for in-depth model comparison analysis.

## Related Pages

- [[comfyui-ltx-integration-overview]] -- ComfyUI integration overview
- [[comfyui-ltx-community-nodes]] -- Detailed community node descriptions
- [[comfyui-ltx-workflows]] -- Workflow documentation
- [[comfyui-ltx-performance-tips]] -- Performance optimization
- [[ltx-video-versions]] -- LTX Video version timeline
- [[ltx-2-version-history]] -- LTX-2 release history
- [[audio-video-generation]] -- Audio-video generation
