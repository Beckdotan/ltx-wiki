# ComfyUI-LTXVideo Plugin

## Overview

ComfyUI-LTXVideo is a collection of custom nodes that extends ComfyUI's capabilities for working with the LTX-2 video generation model. LTX-2 is also built into ComfyUI core, making it readily accessible to all ComfyUI users. The plugin provides specialized nodes for advanced video generation workflows.

**GitHub:** https://github.com/Lightricks/ComfyUI-LTXVideo

## Installation

### Via ComfyUI Manager (Recommended)
1. Search for "LTXVideo" in ComfyUI Manager
2. Click Install
3. Restart ComfyUI
4. Nodes appear under the "LTXVideo" category

### Manual Installation
1. Clone the repository into `ComfyUI/custom_nodes/`
2. Run `pip install -r requirements.txt`

Required models download automatically on first use.

## Core Capabilities

### Video Generation Modes
- **Text-to-Video (T2V)** - Generate videos from text prompts
- **Image-to-Video (I2V)** - Create videos from static images
- **Video-to-Video (V2V)** - Enhance and detail existing footage
- **Video Extension** - Extend videos forward or backward

### Control Features
- **IC-LoRA Control** - Apply depth maps, edge detection, human pose, and motion tracking
- **Union IC-LoRA** - Unified control model combining depth and edge (canny) conditions with downsampled latent processing for efficiency
- **Frame Conditioning** - For smooth transitions
- **Sequence Conditioning** - Motion interpolation to extend videos seamlessly

### Enhancement
- **Prompt Enhancer Node** - Optimizes text prompts for the LTXV model
- **Two-Stage Upscaling** - Higher resolution outputs
- **Improved artifact handling**

## Node Categories

The plugin provides Python modules for:
- Conditioning (loading/saving)
- Sampling and easy samplers
- Latent processing and normalization
- Mask operations
- Text encoding (with Gemma support)
- VAE operations with tiling
- LoRA integration and attention mechanisms
- Prompt enhancement
- Low-VRAM loaders for memory-constrained systems

## Workflow Structure

All workflows follow: **Model Loading -> Prompt Encoding -> Conditioning Setup -> Sampling -> Decoding**

## Supported Models

- LTX-2.3 checkpoints (full and distilled versions)
- Spatial upscalers (x2 and x1.5 variants)
- Temporal upscaler
- Various LoRAs for control
- Gemma-3 text encoder

## System Requirements

- CUDA-compatible GPU with 32GB+ VRAM
- 100GB+ disk space
- For low-VRAM systems: dedicated loader nodes from `low_vram_loaders.py` manage model offloading within 32GB constraints

## Example Workflows

Included in `ComfyUI/custom_nodes/ComfyUI-LTXVideo/example_workflows/`:
- Text-to-video workflows
- Image-to-video workflows
- IC-LoRA control workflows
- Union IC-LoRA workflows
- Two-stage upscaling workflows

## Sources

- [ComfyUI-LTXVideo GitHub](https://github.com/Lightricks/ComfyUI-LTXVideo/)
- [Using ComfyUI with LTX-2 Documentation](https://docs.ltx.video/open-source-model/integration-tools/comfy-ui)
- [ComfyUI LTX-2.3 Workflow Guide](https://ltx.io/model/model-blog/comfyui-workflow-guide)
- [ComfyUI-LTXVideo DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo)
- [ComfyUI LTX-2.3 Workflow Examples](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [ComfyUI LTX Node Details - Floyo](https://www.floyo.ai/all-comfyui-nodes/comfyui-ltxvideo-lightricks)
