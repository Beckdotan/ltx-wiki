# ComfyUI Manager Integration for LTX Video Nodes

## Sources
- [comfyui-ltxvideo Custom Node Extension | InstaSD](https://www.instasd.com/comfyui/custom-nodes/comfyui-ltxvideo)
- [GitHub - Lightricks/ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo/)
- [Using ComfyUI with LTX-2 | LTX Documentation](https://docs.ltx.video/open-source-model/integration-tools/comfy-ui)
- [How to Install ComfyUI Custom Nodes (Plugins) | ComfyUI Wiki](https://comfyui-wiki.com/en/install/install-custom-nodes)
- [LTXV nodes - Custom Nodes Forum | ComfyUI](https://forum.comfy.org/t/ltxv-nodes/2018)
- [Unable to install ComfyUI-LTXVideo nodes | ComfyUI Forum](https://forum.comfy.org/t/unable-to-install-comfyui-ltxvideo-nodes/582)
- [ComfyUI-LTXVideo Custom Node | ComfyAI](https://comfyai.run/custom_node/ComfyUI-LTXVideo)
- [GitHub - LTX-2-desktop/ComfyUI-LTX-2.3](https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3)
- [LTX Video Workflow Step-by-Step Guide | ComfyUI Wiki](https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide)

## Overview

ComfyUI Manager is the primary package manager for ComfyUI custom nodes. It provides the easiest way to install, update, and manage the LTX Video nodes. LTX-2 is also built into ComfyUI core, meaning basic LTX support works without any custom nodes, but the ComfyUI-LTXVideo extension provides advanced features.

---

## What's Built-In vs. What Needs Installation

### Built Into ComfyUI Core (No Installation Needed)

LTX-2 has native support in ComfyUI core. Basic nodes include:
- Model loader for LTX-2 checkpoints
- Basic sampler configuration
- Standard KSampler compatibility
- VAE decode for LTX latents

These built-in nodes allow basic text-to-video and image-to-video workflows without any custom node installation.

### ComfyUI-LTXVideo Extension (Requires Installation)

The official Lightricks extension adds advanced functionality:
- IC-LoRA control nodes (depth, pose, edge, camera)
- MultimodalGuider for audio-video control
- Prompt enhancer nodes
- Low VRAM loader nodes
- Tiled sampling and VAE decode
- Conditioning save/load nodes
- GemmaAPITextEncode for API-based text encoding
- Normalizing sampler
- Spatial and temporal upscaler integration

---

## Installing via ComfyUI Manager

### Method 1: Direct Search

1. Open ComfyUI in your browser
2. Press `Ctrl+M` or click the **Manager** button in the toolbar
3. Click **"Install Custom Nodes"**
4. Search for **"LTXVideo"** in the search bar
5. Find **"ComfyUI-LTXVideo"** by Lightricks in the results
6. Click **Install**
7. Wait for installation to complete
8. Click **Restart** to restart ComfyUI
9. **Manually refresh your browser** (Ctrl+F5) to clear cache

### Method 2: Install via Git URL

1. Open ComfyUI Manager (`Ctrl+M`)
2. Click **"Install via Git URL"**
3. Enter: `https://github.com/Lightricks/ComfyUI-LTXVideo`
4. Click Install
5. Restart ComfyUI and refresh browser

### Method 3: Install Missing Custom Nodes (Workflow-Based)

1. Load a workflow JSON that uses LTX nodes
2. ComfyUI will show missing node warnings
3. Open ComfyUI Manager (`Ctrl+M`)
4. Click **"Install Missing Custom Nodes"**
5. ComfyUI Manager will automatically identify and install required nodes
6. Restart and refresh

---

## Model Downloads via ComfyUI Manager

### Automatic Model Downloads

When using the ComfyUI-LTXVideo extension, models download automatically on first use:
- Load any example workflow from the extension
- Click "Queue Prompt"
- Nodes handle downloading required models

### Manual Model Installation via Manager

For the Gemma 3 text encoder:
1. Open ComfyUI Manager
2. Navigate to model management
3. Search for Gemma 3 12B
4. Install via the Manager's model download feature

---

## Verifying Installation

After installation, verify that nodes are available:

1. Right-click on the ComfyUI canvas
2. Navigate to **Add Node** -> **LTXVideo**
3. You should see node categories including:
   - Conditioning nodes
   - Sampling nodes
   - LoRA control nodes
   - Text encoding nodes
   - Utility nodes

If nodes don't appear:
- Verify the extension is listed in ComfyUI Manager's installed nodes
- Check Python version (requires 3.10+)
- Restart ComfyUI completely (not just refresh)
- Check the ComfyUI console for error messages

---

## Installing Additional LTX-Related Extensions

### ComfyUI_LTX-2_VRAM_Memory_Management

Install via Git URL in ComfyUI Manager:
```
https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management
```

### ID-LoRA-LTX2.3-ComfyUI

Install via Git URL:
```
https://github.com/ID-LoRA/ID-LoRA-LTX2.3-ComfyUI
```

### Comfy-WaveSpeed (Acceleration)

Install via Git URL:
```
https://github.com/chengzeyi/Comfy-WaveSpeed
```

### ComfyUI-GGUF (For Quantized Models)

Required for using Kijai's GGUF-quantized LTX models:
```
https://github.com/city96/ComfyUI-GGUF
```

---

## Updating LTX Nodes

### Via ComfyUI Manager

1. Open ComfyUI Manager (`Ctrl+M`)
2. Click **"Update All"** or find ComfyUI-LTXVideo in installed nodes list
3. Click Update next to the specific extension
4. Restart ComfyUI

### Manual Update

```bash
cd ComfyUI/custom_nodes/ComfyUI-LTXVideo
git pull
pip install -r requirements.txt
```

---

## Troubleshooting Common Installation Issues

### Nodes Not Appearing After Installation

1. Verify installation through ComfyUI Manager's installed nodes list
2. Check Python version compatibility (3.10+)
3. Fully restart ComfyUI (not just browser refresh)
4. Check console output for import errors
5. Try reinstalling via ComfyUI Manager

### Workflow Errors with Missing Nodes

1. Use ComfyUI Manager's "Install Missing Custom Nodes" feature
2. Update ComfyUI to latest version
3. Update the LTXVideo extension
4. Check if workflow was made with a newer version of the extension

### Model Loading Failures

1. Verify model files are in the correct directory
2. Check file integrity (not corrupted downloads)
3. Ensure sufficient disk space (models can be 10-18+ GB)
4. For Gemma encoder: verify the entire repository is downloaded, not just individual files

### Dependencies Missing

1. ComfyUI Manager should auto-install dependencies
2. If not, manually run: `pip install -r requirements.txt` in the extension directory
3. Check for CUDA version compatibility

---

## Desktop Auto-Installer Alternative

For users who want a fully automated setup, the ComfyUI-LTX-2.3 desktop utility provides:
- One-click executable (`LTX2.3_ComfyUI.exe`)
- Automatic model downloads (~18 GB)
- SHA256 checksum verification
- Dependency installation
- No manual configuration needed

Available at: [LTX-2-desktop/ComfyUI-LTX-2.3](https://github.com/LTX-2-desktop/ComfyUI-LTX-2.3)
