---
title: ComfyUI LTX Video Workflows
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
  - https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
  - https://openart.ai/workflows
  - https://civitai.com/models/2524321/smooth-workflow-ltx-i2vt2v
  - https://civitai.com/models/995093/ltx-image-to-video-with-stg-caption-and-clip-extend-workflow
  - https://stable-diffusion-art.com/ltx-video/
  - https://www.runcomfy.com/comfyui-workflows/ltx-2-comfyui-workflow-real-time-video-generation-speed
  - https://www.mimicpc.com/workflows/ltxv-video
  - raw/social-reddit-comfyui-workflows.md
  - raw/social-youtube-tutorials-reviews.md
tags:
  - comfyui
  - ltx-video
  - workflows
  - txt2vid
  - img2vid
  - vid2vid
---

# ComfyUI LTX Video Workflows

[[ltx-video-overview|LTX Video]] supports three primary workflow types in ComfyUI: Text-to-Video (txt2vid), Image-to-Video (img2vid), and Video-to-Video (vid2vid). These can be used individually or combined. LTX-2.3 additionally supports audio-synchronized generation.

All workflows follow the general pipeline: **Model Loading -> Prompt Encoding -> Conditioning Setup -> Sampling -> Decoding**

## Text-to-Video (txt2vid)

### Workflow Structure

1. **Load Model** -- LTXVLoader or CheckpointLoaderSimple
2. **Text Encoding** -- Gemma/CLIPTextEncode for positive and negative prompts
3. **Empty Latent Video** -- Blank latent space at specified resolution and frames
4. **Sampling** -- KSampler or [[comfyui-ltx-node-reference|LTXVNormalizingSampler]]
5. **VAE Decode** -- Convert latents to pixel space
6. **Video Output** -- VHS_VideoCombine to produce video file

### Recommended Settings

| Parameter | Value |
|-----------|-------|
| Resolution | 768x512 (landscape) or 512x768 (portrait) |
| Frames | 65 (multiples of 8 + 1) |
| FPS | 24-25 |
| Steps | 20-30 |
| CFG | 3.0-7.0 |
| Sampler | euler or euler_ancestral |
| Scheduler | normal or sgm_uniform |
| Denoise | 1.0 |

### Prompting Tips

- Include action descriptions, scene transitions, cinematography terminology
- Describe motion: "camera slowly pans left", "subject walks toward camera"
- Negative prompt: "worst quality, inconsistent motion, blurry, jittery, distorted, watermarks"
- Consider using the LTXVPromptEnhancer node or [[comfyui-ltx-node-reference|GemmaAPITextEncode]] for better results

## Image-to-Video (img2vid)

### Workflow Structure

1. **Load Image** -- LoadImage node
2. **Load Model** -- LTXVLoader
3. **Image Preprocessing** -- LTXVPreprocess
4. **Add Guide** -- LTXVAddGuide to condition on reference image
5. **Text Encoding** -- Describe desired motion
6. **Sampling** -- KSampler with lower denoise
7. **VAE Decode** and **Video Output**

### Multi-Frame Control

Chain `LoadImage -> LTXVPreprocess -> LTXVAddGuide` nodes to set multiple guiding images (first frame, end frame, or keyframes).

### Recommended Settings

| Parameter | Value |
|-----------|-------|
| Resolution | Match source aspect ratio (768x512 or 512x768) |
| Frames | 97 (~4 seconds at 24fps) |
| CFG | 3.0-5.0 (lower maintains reference consistency) |
| Steps | 15-20 |
| Denoise | 0.7-0.9 (lower preserves more of original) |

### Tips

- Images auto-resize to model base resolutions
- Use Florence2 autocaptioning to generate accurate source descriptions
- Replace "photo" / "image" with "video" in auto-generated captions
- Lower CFG helps maintain consistency with reference image

## Video-to-Video (vid2vid)

### Workflow Structure

1. **Load Source Video** -- VHS_LoadVideo
2. **Load Model** -- LTXVLoader
3. **Video Encoding** -- VAE Encode source frames
4. **Text Encoding** -- Describe desired transformation
5. **Sampling** -- KSampler with low denoise
6. **VAE Decode** and **Video Output**

### Recommended Settings

| Parameter | Value |
|-----------|-------|
| CFG | 2.0-4.0 |
| Steps | 10-15 |
| Denoise | 0.3-0.7 (lower preserves more source) |

### IC-LoRA Control for vid2vid

For precise control, use [[comfyui-ltx-lora-training-control|IC-LoRA]] to separate motion from styling:
- **Canny** -- Edge preservation from source
- **Depth** -- Camera and spatial geometry preservation
- **Pose** -- Human motion transfer

## Essential Custom Nodes

| Node Package | Source | Purpose |
|--------------|--------|---------|
| ComfyUI-LTXVideo | Lightricks (official) | Core LTX Video nodes |
| ComfyUI-GGUF | city96 | Running quantized models on limited VRAM |
| ComfyUI-VideoHelperSuite (VHS) | Community | Video file I/O and MP4 outputs |

## Community All-in-One Workflows

### jaguar_pesky_18 (OpenArt)
- Supports vid2vid, txt2vid, img2vid in one workflow
- Uses LTXV 0.9.1 with spatio-temporal skip guidance
- Low VRAM design with mode toggle switches

### Smooth Workflow LTX (Civitai)
- Combined I2V and T2V in single workflow
- Designed for consistent, smooth output

### LTX IMAGE to VIDEO with STG (Civitai)
- Version 9.5 targeting LTX 0.9.7 Distilled
- Integrates Florence2 captioning, CLIP extension, STG guidance

## Community-Validated Settings

From Reddit and tutorial creators:
- Use the "Dev" model paired with a distilled LoRA
- CFG around 4.0 with 20 inference steps
- This combination provides high-fidelity stability while accelerating render time

## Technical Constraints

- **Resolution:** Must be a multiple of 32
- **Frame count:** Must follow multiples of 8 + 1 (9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 161, 257)
- **Max recommended resolution:** 720x1280
- **Max recommended frames:** 257
- **Output location:** `ComfyUI/output/`

## Long Video Generation

- Generate segments separately to manage VRAM
- Maintain style consistency through consistent prompts
- Use [[comfyui-ltx-node-reference|LTXVSequenceConditioning]] to extend from last frame
- Assemble segments in post-production
- Use temporal upscaler to double frame rates

## Multi-Stage Pipeline Patterns

### Two-Stage (Most Popular)

Widely validated as the optimal balance between coherence and sharpness:
- **Stage 1:** Generate at half target resolution focusing on motion structure
- **Stage 2:** LTXVLatentUpsampler for 2x spatial upscale with refinement

See [[comfyui-ltx-two-stage-pipeline]] for full details.

### Three-Stage (Community Fix)

Adds an intermediate refinement pass to address stiff motion artifacts from standard two-stage. Resolves rigid movements that can damage fine details.

## LTX 2.3 Quality Improvements

Community noted significant improvements in LTX 2.3 over previous versions:
- Vastly improved latent space and larger text connector
- Better preservation of fine details: skin textures, fabric movements, hair
- Stronger adherence to complex, multi-sentence prompts

## Related Pages

- [[comfyui-ltx-integration-overview]] -- Integration overview
- [[comfyui-ltx-two-stage-pipeline]] -- Upscaling pipeline details
- [[comfyui-ltx-node-reference]] -- Node documentation
- [[comfyui-ltx-lora-training-control]] -- LoRA and IC-LoRA control
- [[comfyui-ltx-workflow-tutorials]] -- Tutorial catalog
- [[comfyui-ltx-performance-tips]] -- Optimization tips
- [[comfyui-ltx-model-comparison]] -- Comparison with other video models
