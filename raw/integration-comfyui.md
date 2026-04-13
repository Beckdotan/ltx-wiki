# LTX Video ComfyUI Integration

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://github.com/Lightricks/ComfyUI-LTXVideo, https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-2.3

---

## Overview

ComfyUI is the primary community integration platform for LTX Video. Lightricks maintains an official ComfyUI node package (`ComfyUI-LTXVideo`) and provides example workflows for all model versions.

## Official ComfyUI Repository

- **Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo
- Built-in LTXVideo nodes available via ComfyUI Manager
- Example workflows provided for all model versions (0.9.x, 2.0, 2.3)

## Supported Workflows

### LTX-Video (Original)
- Text-to-Video generation
- Image-to-Video generation
- Video-to-Video generation
- Multi-condition generation (multiple images/video segments at specific frames)
- Upsampling (spatial and temporal)
- IC-LoRA workflows (detailer, canny, depth)

### LTX-2 / LTX-2.3
- Text-to-Video with audio
- Image-to-Video with audio
- Audio-to-Video generation
- Video-to-Audio generation
- Text-to-Audio
- Multi-modal generation workflows
- Camera control LoRA workflows (dolly, jib, static)
- Union control (canny + depth + pose)
- Motion track control with spline overlays
- First-last frame controlled generation

## Key ComfyUI Nodes
- `LTXICLoRALoaderModelOnly` - Loads LoRA and extracts downscale factor
- `LTXAddVideoICLoRAGuide` - Adds small latent as guide
- LTX guidance parameter nodes
- LTX sampler nodes

## Community Findings

### Important Configuration Notes
- **Sigma values differ** between LTX-2 and LTX-2.3 - users must use official workflows from the LTX repository rather than ComfyUI built-in templates
- ComfyUI v0.6.0 had compatibility issues requiring a critical fix (Discussion #116)
- Installation via ComfyUI Manager sometimes produces errors (Discussion #102)

### Recommended Text Encoders for ComfyUI
- `gemma_3_12B_it.safetensors` (full precision)
- `unsloth/gemma-3-12b-it-bnb-4bit` (quantized, for lower VRAM)
- T5 encoders for LTX-Video 0.9.x models

## Community-Created ComfyUI Workflows

### LTX + Gemini Workflow (by rubanraja)
- Integrates Google Gemini API with LTX-Video in ComfyUI
- Available free on Patreon
- Simplifies video generation for non-technical creators
- Security note: community flagged exposed API keys in shared workflows

### Kijai's Transformers Implementation
- Alternative ComfyUI integration recommended for better results with LTX-2.3
- Reported to produce "splendid" outputs when used correctly

## Performance in ComfyUI

### LTX-2.3 on RTX 5090
- 1920x1088, 241 frames: ~222 seconds (two-stage pipeline)
- 1280x720, 481 frames: ~82 seconds (FP8, 8 steps)
- FP8 vs Full-Dev quality: "minimal and negligible" difference

### LTX-Video 2B on GTX 1650
- 640x480, 73 frames: 3-9 minutes
- VRAM usage: 2.7-3.5 GB
