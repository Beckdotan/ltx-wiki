# ComfyUI LTX Video Workflow Tutorials and Guides

## Sources
- [LTX Video Workflow Step-by-Step Guide | ComfyUI Wiki](https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide)
- [LTX Workflow in ComfyUI: The Complete Guide | ComfyUI Box](https://comfyui-box.com/advanced-tutorial/ltx-workflow/)
- [How to run LTX Video 13B on ComfyUI (image-to-video) | Stable Diffusion Art](https://stable-diffusion-art.com/ltxv-13b/)
- [How to use LTX Video 0.9.5 on ComfyUI | Stable Diffusion Art](https://stable-diffusion-art.com/ltx-video-0-9-5/)
- [How to run LTX Video 13B 0.9.7 in ComfyUI | Next Diffusion](https://www.nextdiffusion.ai/tutorials/how-to-run-ltx-video-13b-0-9-7-in-comfyui)
- [ComfyUI Video Generation Model Workflow Guide (LTX-2.3) | LTX Blog](https://ltx.io/model/model-blog/comfyui-workflow-guide)
- [LTX-2.3 ComfyUI Workflow Examples | ComfyUI Docs](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [AI Video Creation Made Easy: How to Install and Use LTXV in ComfyUI | Medium](https://medium.com/@wxbxtxr/ai-video-creation-made-easy-how-to-install-and-use-the-ltxv-model-in-comfyui-b2a969cd9f41)
- [How to Install LTX-2 in ComfyUI (No Custom Nodes) | CrePal](https://crepal.ai/blog/aivideo/blog-how-to-install-ltx-2-in-comfyui/)
- [How to Install LTX 2.3 in ComfyUI: Step-by-Step Guide | CrePal](https://crepal.ai/blog/aivideo/how-to-install-ltx-2-3-comfyui/)
- [LTX-2 ComfyUI Quickstart: First Video in 10 Minutes | WaveSpeedAI](https://wavespeed.ai/blog/posts/blog-ltx-2-comfyui-quickstart/)
- [Meet LTX: Video Generation Speed Unleashed in ComfyUI | ThinkDiffusion](https://learn.thinkdiffusion.com/meet-ltx-video-generation-speed-unleashed-in-comfyui/)
- [NVIDIA Video Generation Guide | GeForce News](https://www.nvidia.com/en-us/geforce/news/rtx-ai-video-generation-guide/)
- [LTX 2.3 Multi-Stage Latent Upscaling Workflow | CrePal](https://crepal.ai/blog/aivideo/ltx-2-3-multi-stage-latent-upscaling-comfyui/)

## Overview

This document catalogues the major tutorials and guides available for setting up and using LTX Video workflows in ComfyUI, ranging from beginner-friendly quickstarts to advanced multi-stage pipeline configurations.

---

## Tutorial 1: ComfyUI Wiki — LTX Video Workflow Step-by-Step Guide

**URL:** https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide
**Target Audience:** Beginners to Intermediate
**Model Version:** LTX Video 2B (v0.9)

### What It Covers

- Updating ComfyUI and installing the ComfyUI-LTXVideo custom node
- Downloading and placing required model files (checkpoint + text encoder)
- Building a complete workflow from scratch with node-by-node instructions

### Key Node Configuration

- **LTXVLoader** — Selects the main model file
- **LTXVCLIPModelLoader** — Selects PixArt encoder (for older versions)
- **LTXVModelConfigurator** — Sets resolution, frame count, FPS, conditioning options
- **CLIPTextEncode** — Positive/negative prompt encoding
- **CFGGuider** — Prompt guidance strength (range 2-7)
- **KSamplerSelect** — Use Euler sampler
- **BasicScheduler** — 10-25 steps, normal scheduler
- **RandomNoise** — Noise generation with optional seed
- **SamplerCustomAdvanced** — Execute sampling
- **VAEDecode** — Decode frames
- **VHS_VideoCombine** — Merge final video

### Generation Mode Recommendations

| Mode | Resolution | Frames | Steps | CFG |
|------|-----------|--------|-------|-----|
| txt2vid | 768x512 | 65 | 20 | 4-7 |
| img2vid | Match source | - | 15-20 | 3-5 |
| vid2vid | Match source | - | 10-15 | 2-4 |

### Software Requirements

- Python 3.10.5+
- CUDA 12.2+
- PyTorch 2.1.2+

---

## Tutorial 2: ComfyUI-Box — LTX Workflow: The Complete Guide

**URL:** https://comfyui-box.com/advanced-tutorial/ltx-workflow/
**Target Audience:** Intermediate
**Model Version:** LTX-2.3

### What It Covers

- Building Text-to-Video (T2V) and Image-to-Video (I2V) pipelines
- Using LTX 2.3 with the Gemma 3 text encoder
- Two-stage pipeline with spatial upscaling
- Practical optimization tips

---

## Tutorial 3: Stable Diffusion Art — LTX Video 13B on ComfyUI

**URL:** https://stable-diffusion-art.com/ltxv-13b/
**Target Audience:** Intermediate to Advanced
**Model Version:** LTXV-13B (0.9.7)

### What It Covers

- Installing the 13B parameter model (6x larger than original 2B)
- Image-to-video workflow with the larger model
- Multi-frame generation (start frame, end frame, keyframes)
- Florence2 integration for autocaptioning

### Key Details

- LTXV-13B generates at fixed base resolutions: 768x512 (landscape) or 512x768 (portrait)
- Default 97 frames = ~4 seconds at 24fps
- Images automatically resized to match model's base resolutions
- Better details, prompt adherence, and coherence vs. 2B model

---

## Tutorial 4: LTX Blog — ComfyUI Video Generation Model Workflow Guide (LTX-2.3)

**URL:** https://ltx.io/model/model-blog/comfyui-workflow-guide
**Target Audience:** All levels
**Model Version:** LTX-2.3

### What It Covers

- Complete installation walkthrough
- Optimal settings for text-to-video and image-to-video
- Nodes that save VRAM and time
- Common mistakes that reduce quality
- Audio-video generation

### Key Tips from This Guide

- Start with low resolution (480x720) and 41-81 frames
- Use distilled model for iteration, full model for final renders
- Close unused workflows and clear cache between generations
- Apply upscaling for highest quality results

---

## Tutorial 5: WaveSpeedAI — LTX-2 ComfyUI Quickstart: First Video in 10 Minutes

**URL:** https://wavespeed.ai/blog/posts/blog-ltx-2-comfyui-quickstart/
**Target Audience:** Beginners
**Model Version:** LTX-2

### What It Covers

- Getting from zero to first video in 10 minutes
- Built-in LTX-2 nodes (loader, sampler, preview)
- Sample workflow that runs end-to-end without manual wiring

---

## Tutorial 6: WaveSpeedAI — LTX-2.3 ComfyUI Setup: Two-Stage Pipeline

**URL:** https://wavespeed.ai/blog/posts/ltx-2-3-comfyui-setup-two-stage-pipeline/
**Target Audience:** Advanced
**Model Version:** LTX-2.3

### What It Covers

- Two-stage pipeline architecture
- VRAM fixes and optimization
- Gemma encoder setup and alternatives (GGUF, API)
- Spatial upscaling configuration

### Pipeline Structure

1. **Stage 1:** Generate at half target resolution — motion, anatomy, scene coherence
2. **Spatial Upscale:** LTXVLatentUpsampler performs 2x upscale in latent space
3. **Stage 2:** Refinement pass at doubled resolution
4. **Decode:** Final VAE decode to pixel space

---

## Tutorial 7: Next Diffusion — LTX Video 13B 0.9.7 in ComfyUI

**URL:** https://www.nextdiffusion.ai/tutorials/how-to-run-ltx-video-13b-0-9-7-in-comfyui
**Target Audience:** Intermediate
**Model Version:** LTXV-13B (0.9.7)

### What It Covers

- 0.9.7 features: animate between start and end frames
- Step-by-step installation
- Image-to-video workflow optimization
- Content creator and hobbyist use cases

---

## Tutorial 8: CrePal — LTX 2.3 Multi-Stage Latent Upscaling Workflow

**URL:** https://crepal.ai/blog/aivideo/ltx-2-3-multi-stage-latent-upscaling-comfyui/
**Target Audience:** Advanced
**Model Version:** LTX-2.3

### What It Covers

- Spatial and temporal upscaling pipeline
- Multi-stage latent upscaling for maximum quality
- Hardware requirements per configuration
- VRAM management for upscaling operations

---

## Tutorial 9: CrePal — How to Install LTX 2.3 in ComfyUI

**URL:** https://crepal.ai/blog/aivideo/how-to-install-ltx-2-3-comfyui/
**Target Audience:** Beginners
**Model Version:** LTX-2.3

### What It Covers

- Step-by-step installation of LTX-2.3
- Model file downloads and placement
- Node descriptions and initial workflow setup

---

## Tutorial 10: ThinkDiffusion — Meet LTX: Video Generation Speed Unleashed

**URL:** https://learn.thinkdiffusion.com/meet-ltx-video-generation-speed-unleashed-in-comfyui/
**Target Audience:** Beginners to Intermediate

### What It Covers

- Overview of LTX Video's speed advantages
- Cloud-based ComfyUI setup with LTX
- Workflow examples and speed comparisons

---

## Tutorial 11: NVIDIA — RTX AI Video Generation Guide

**URL:** https://www.nvidia.com/en-us/geforce/news/rtx-ai-video-generation-guide/
**Target Audience:** General
**Scope:** Multiple models including LTX Video

### What It Covers

- Hardware recommendations for AI video generation
- LTX Video among other supported models
- RTX GPU optimization tips

---

## Tutorial 12: ComfyUI Official Docs — LTX-2.3 Workflow Examples

**URL:** https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
**Target Audience:** All levels
**Model Version:** LTX-2.3

### What It Covers

- Official ComfyUI documentation for LTX-2.3
- Downloadable example workflow JSON files
- Audio-video generation workflows
- Quality improvement tips and best practices

---

## Tutorial 13: Medium — AI Video Creation Made Easy with LTXV in ComfyUI

**URL:** https://medium.com/@wxbxtxr/ai-video-creation-made-easy-how-to-install-and-use-the-ltxv-model-in-comfyui-b2a969cd9f41
**Target Audience:** Beginners

### What It Covers

- Beginner-friendly installation guide
- Basic workflow concepts
- First video generation walkthrough

---

## Tutorial 14: Apatero — ComfyUI LTX 2.3 Workflow: Audio-Synced Video

**URL:** https://apatero.com/blog/comfyui-ltx-2-3-audio-video-workflow-tutorial-2026
**Target Audience:** Intermediate to Advanced
**Model Version:** LTX-2.3

### What It Covers

- Audio-synchronized video generation
- MultimodalGuider node for independent audio/video control
- Cross-modal guidance tuning
