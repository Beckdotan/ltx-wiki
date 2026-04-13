# ComfyUI-LTXVideo Integration

## Sources
- https://github.com/Lightricks/ComfyUI-LTXVideo/
- https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
- https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
- https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
- https://github.com/logtd/ComfyUI-LTXTricks

---

## Overview

ComfyUI-LTXVideo provides official custom nodes extending ComfyUI's capabilities for LTX Video generation. It offers workflows, advanced conditioning tools, and control mechanisms for video synthesis. ComfyUI is the recommended tool for the best balance of power and ease of use when working with LTX Video models.

- **Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo/
- **Category in ComfyUI:** "LTXVideo"

---

## Prerequisites

- ComfyUI installed (download from https://www.comfy.org/download)
- CUDA-compatible GPU with minimum 32 GB VRAM
- At least 16 GB system RAM (32 GB recommended)
- 100-160 GB free disk space for model weights, Python environment, and outputs

---

## Installation

### Via ComfyUI Manager (Recommended)

1. Open ComfyUI and access the Manager (Ctrl+M)
2. Select "Install Custom Nodes"
3. Search for "LTXVideo" and click Install
4. Restart ComfyUI after installation completes

Nodes appear under the "LTXVideo" category with automatic model downloads on first use.

---

## Required Model Downloads

### Core Models (place in `models/checkpoints`)
- LTX-2.3-22b (dev or distilled variants)

### Text Encoder (place in `models/text_encoders/gemma-3-12b-it-qat-q4_0-unquantized`)
- Gemma-3 12B text encoder (quantized)

### Upscalers (place in `models/latent_upscale_models`)
- Spatial upscalers (x2 and x1.5)
- Temporal upscaler (x2)

### Optional LoRAs (place in `models/loras`)
- Union Control (depth/edges)
- Motion tracking
- Detailer
- Pose control
- Camera control variants (dolly, jib movements)

---

## Available Nodes

### Core Nodes
- **GemmaAPITextEncode**: Free API-based text encoder for generating text embeddings
- **LTXVSaveConditioning**: Save text encodings to disk for reuse
- **LTXVLoadConditioning**: Load pre-saved text encodings
- **NormalizingSampler**: Applies statistical normalization to latents during generation to prevent overbaking and audio clipping

### Low-VRAM Nodes
- Dedicated loader nodes in `low_vram_loaders.py` for memory-constrained systems

---

## Available Workflows

### LTX-2.3 Workflows
- Text/image-to-video single-stage (full or distilled models)
- Two-stage pipelines with spatial/temporal upsampling
- IC-LoRA Union Control (supporting depth and edge conditions)
- Motion tracking for image-to-video generation

### LTX-2.0 (Legacy) Workflows
- Text-to-video (full and distilled variants)
- Image-to-video generation
- Video-to-video detailing
- IC-LoRA with depth, pose, and edge controls

---

## Basic Usage

1. In ComfyUI, click **Load** and select an example workflow from the LTXVideo nodes
2. Find the text encode node and enter your description
3. Click **Queue Prompt** to generate

---

## Key Features

- **Union IC-LoRA Model**: Combines depth and canny edge detection into a single unified LoRA, reducing memory overhead
- **Dynamic conditioning systems**
- **Sparse track controls**
- **Tiled VAE decoding** for memory efficiency
- **Low-VRAM optimization modes**

---

## Performance Optimization

- Use dedicated low-VRAM loader nodes for constrained systems
- Apply ComfyUI's `--reserve-vram` parameter: `python -m main --reserve-vram 5`
- Consider distilled model variants for faster inference
- Repository has 44 files organized into example workflows, configuration presets, system prompts, and utility modules

---

## Community Extensions

### ComfyUI-LTXTricks
- **Repository:** https://github.com/logtd/ComfyUI-LTXTricks
- Additional nodes for enhanced control over LTX Video
- Includes RF-Inversion, RF-Edit, FlowEdit techniques

---

## Related Links

- **Official Documentation:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
- **LTX-2 Nodes Docs:** https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
- **ComfyUI Tutorials:** https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
- **GitHub Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo/
