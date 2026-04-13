# ComfyUI-LTXVideo - Official ComfyUI Custom Nodes

**Source:** https://github.com/Lightricks/ComfyUI-LTXVideo
**Date:** 2026-04-13

---

## Overview

ComfyUI-LTXVideo is the official set of ComfyUI custom nodes for running LTX-Video models within the ComfyUI node-based workflow interface. It is developed and maintained by Lightricks.

**Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo

---

## Installation

### Via ComfyUI Manager (Recommended)
1. Open ComfyUI Manager
2. Search for "LTXVideo"
3. Install the ComfyUI-LTXVideo package

### Manual Installation
```bash
cd ComfyUI/custom_nodes
git clone https://github.com/Lightricks/ComfyUI-LTXVideo.git
cd ComfyUI-LTXVideo
pip install -r requirements.txt
```

---

## Pre-built Workflows

The repository includes several ready-to-use workflow JSON files:

| Workflow File | Model | Use Case |
|---------------|-------|----------|
| `ltxv-13b-i2v-base.json` | 13B Dev | Image-to-video (full quality) |
| `ltxv-13b-i2v-mixed-multiscale.json` | 13B Mixed | Balanced speed/quality with upscaling |
| `ltxv-13b-dist-i2v-base.json` | 13B Distilled | Fast image-to-video |
| `ltxv-13b-i2v-base-fp8.json` | 13B FP8 | Low-VRAM image-to-video |
| `ltxv-2b-dist-i2v-base.json` | 2B Distilled | Lightweight image-to-video |

---

## Custom Nodes Provided

### Core Nodes
- **LTXVLoader** - Load LTX-Video model checkpoint
- **LTXVSampler** - Run inference/sampling
- **LTXVCondition** - Create conditioning from images/videos
- **LTXVPromptEncode** - Encode text prompts
- **LTXVVideoOutput** - Export generated video

### Utility Nodes
- **LTXVUpscaler** - Apply spatial/temporal upscaling
- **LTXVICLoRA** - Load and apply ICLoRA adapters for controlled generation
- **LTXVMultiCondition** - Combine multiple conditioning inputs

---

## Model Download

Models are automatically downloaded from Hugging Face when first used, or can be manually placed in:
```
ComfyUI/models/checkpoints/    # For main model files
ComfyUI/models/loras/          # For ICLoRA adapter files
```

---

## Workflow: Image-to-Video (13B)

Basic workflow structure:
1. **Load Model** (LTXVLoader) -> Select `ltxv-13b-0.9.8-dev` or `distilled`
2. **Load Image** (ComfyUI Image Loader) -> Input reference image
3. **Create Condition** (LTXVCondition) -> Set frame_index=0
4. **Encode Prompt** (LTXVPromptEncode) -> Enter text description
5. **Sample** (LTXVSampler) -> Configure steps, resolution, frames
6. **Decode** (VAE Decode)
7. **Export** (LTXVVideoOutput) -> Save as MP4

---

## Controlled Generation with ICLoRA

ComfyUI-LTXVideo supports ICLoRA adapters for conditioned generation:

- **Depth conditioning** - Use depth maps to control spatial layout
- **Pose conditioning** - Use pose estimation for character animation
- **Canny edge conditioning** - Use edge detection for structural guidance

### ICLoRA Models
Available on Hugging Face:
- `Lightricks/LTX-Video` (ICLoRA subfolder variants)

---

## Hardware Requirements

Same as base LTX-Video models:
- 13B models: 16GB+ VRAM recommended
- 13B FP8: 12GB+ VRAM
- 2B models: 8GB+ VRAM
- 2B FP8: 6GB+ VRAM

---

## Related Links

- **Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **ComfyUI:** https://github.com/comfyanonymous/ComfyUI
- **LTX-Video Models:** https://huggingface.co/Lightricks/LTX-Video
