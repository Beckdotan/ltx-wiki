---
title: ComfyUI Manager Setup for LTX Video
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://www.instasd.com/comfyui/custom-nodes/comfyui-ltxvideo
  - https://github.com/Lightricks/ComfyUI-LTXVideo
  - https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
  - https://comfyui-wiki.com/en/install/install-custom-nodes
  - https://forum.comfy.org/t/ltxv-nodes/2018
  - https://forum.comfy.org/t/unable-to-install-comfyui-ltxvideo-nodes/582
  - https://comfyai.run/custom_node/ComfyUI-LTXVideo
  - https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3
  - https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide
tags:
  - comfyui
  - ltx-video
  - installation
  - comfyui-manager
  - setup
---

# ComfyUI Manager Setup for LTX Video

ComfyUI Manager is the primary package manager for installing, updating, and managing [[comfyui-ltx-integration-overview|LTX Video nodes in ComfyUI]].

## What Needs Installation vs What Is Built-In

### Built Into ComfyUI Core (No Install Needed)

[[ltx-2-overview|LTX-2]] has native support in ComfyUI core:
- Model loader for LTX-2 checkpoints
- Basic sampler configuration
- Standard KSampler compatibility
- VAE decode for LTX latents

These enable basic text-to-video and image-to-video without any custom nodes.

### ComfyUI-LTXVideo Extension (Requires Install)

The [[comfyui-ltxvideo-official-nodes|official Lightricks extension]] adds:
- IC-LoRA control nodes
- MultimodalGuider for audio-video
- Prompt enhancer nodes
- Low VRAM loaders
- Tiled sampling and VAE decode
- Conditioning save/load
- GemmaAPITextEncode
- Normalizing sampler
- Spatial/temporal upscaler integration

## Installation Methods

### Method 1: Direct Search (Recommended)

1. Open ComfyUI in browser
2. Press `Ctrl+M` or click **Manager** button
3. Click **"Install Custom Nodes"**
4. Search for **"LTXVideo"**
5. Find **"ComfyUI-LTXVideo"** by Lightricks
6. Click **Install**
7. Click **Restart** to restart ComfyUI
8. **Refresh browser** (`Ctrl+F5`) to clear cache

### Method 2: Git URL

1. Open ComfyUI Manager (`Ctrl+M`)
2. Click **"Install via Git URL"**
3. Enter: `https://github.com/Lightricks/ComfyUI-LTXVideo`
4. Click Install, restart, refresh

### Method 3: Install Missing Nodes (Workflow-Based)

1. Load a workflow JSON that uses LTX nodes
2. ComfyUI shows missing node warnings
3. Open Manager (`Ctrl+M`)
4. Click **"Install Missing Custom Nodes"**
5. Manager auto-identifies and installs required nodes
6. Restart and refresh

### Method 4: Manual

```
cd ComfyUI/custom_nodes/
git clone https://github.com/Lightricks/ComfyUI-LTXVideo
cd ComfyUI-LTXVideo
pip install -r requirements.txt
```

## Model Downloads

### Automatic Downloads

Models download automatically on first use:
1. Load any example workflow from the extension
2. Click "Queue Prompt"
3. Nodes handle downloading required models

### Gemma 3 Text Encoder

Install via ComfyUI Manager's model management, or download manually from HuggingFace and place in `models/text_encoders/`.

## Verification

After installation, right-click the canvas and navigate to **Add Node -> LTXVideo**. You should see:
- Conditioning nodes
- Sampling nodes
- LoRA control nodes
- Text encoding nodes
- Utility nodes

## Installing Additional LTX Extensions

### Via Git URL in ComfyUI Manager

| Extension | URL |
|-----------|-----|
| [[comfyui-ltx-community-nodes\|VRAM Memory Management]] | `https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management` |
| [[comfyui-ltx-community-nodes\|ID-LoRA]] | `https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI` |
| [[comfyui-ltx-community-nodes\|Comfy-WaveSpeed]] | `https://github.com/chengzeyi/Comfy-WaveSpeed` |
| ComfyUI-GGUF (for quantized models) | `https://github.com/city96/ComfyUI-GGUF` |

## Updating

### Via Manager

1. Open ComfyUI Manager (`Ctrl+M`)
2. Click **"Update All"** or find ComfyUI-LTXVideo in installed list
3. Click Update, restart

### Manual

```
cd ComfyUI/custom_nodes/ComfyUI-LTXVideo
git pull
pip install -r requirements.txt
```

## Troubleshooting

### Nodes Not Appearing

1. Verify installation in Manager's installed nodes list
2. Check Python version (requires 3.10+)
3. Fully restart ComfyUI (not just browser refresh)
4. Check console for import errors
5. Reinstall via Manager

### Workflow Errors with Missing Nodes

1. Use Manager's "Install Missing Custom Nodes"
2. Update ComfyUI to latest version
3. Update LTXVideo extension
4. Check workflow version compatibility

### Model Loading Failures

1. Verify files in correct directories
2. Check download integrity (no corruption)
3. Ensure sufficient disk space (models 10-18+ GB)
4. For Gemma: entire repository must be downloaded, not just individual files

### Dependencies Missing

1. Manager should auto-install dependencies
2. Manual fallback: `pip install -r requirements.txt` in extension directory
3. Verify CUDA version compatibility

## Desktop Auto-Installer

For fully automated setup, the [[comfyui-ltx-community-nodes|ComfyUI-LTX-2.3 desktop utility]] provides:
- One-click `LTX2.3_ComfyUI.exe`
- Automatic ~18 GB model downloads
- SHA256 checksum verification
- Zero manual configuration

Available at: https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3

## Related Pages

- [[comfyui-ltx-integration-overview]] -- Integration overview
- [[comfyui-ltxvideo-official-nodes]] -- What the extension provides
- [[comfyui-ltx-community-nodes]] -- Third-party extensions
- [[comfyui-ltx-workflow-tutorials]] -- Tutorials for getting started
