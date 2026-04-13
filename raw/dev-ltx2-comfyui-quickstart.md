# LTX-2 ComfyUI Quick Start & Integration Guide

**Source:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui, https://docs.ltx.video/open-source-model/getting-started/quick-start
**Fetched:** 2026-04-13

---

## Overview

ComfyUI is the recommended way to use LTX-2 locally. The ComfyUI-LTXVideo custom nodes provide a visual, node-based interface for all LTX-2 generation capabilities.

---

## Installation

### Via ComfyUI Manager (Recommended)
1. Open ComfyUI
2. Click "Manage Extensions"
3. Search for "LTXVideo"
4. Install `ComfyUI-LTXVideo`
5. Restart ComfyUI

System handles dependencies automatically.

### Manual Installation
```bash
cd ComfyUI/custom_nodes
git clone https://github.com/Lightricks/ComfyUI-LTXVideo.git
cd ComfyUI-LTXVideo
uv sync --frozen
source .venv/bin/activate
```

For portable ComfyUI installations:
```bash
.\python_embeded\python.exe -m pip install -r .\ComfyUI\custom_nodes\ComfyUI-LTXVideo\requirements.txt
```

---

## Prerequisites
- ComfyUI installed
- CUDA GPU with 32GB+ VRAM
- 100GB+ free disk space
- Python 3.10+

---

## Model Setup

Models download automatically on first use. For manual installation, download from Hugging Face:
- **22B Distilled** (beginner option, faster)
- **22B Full** (maximum quality, slower)
- **Gemma 3 text encoder** (install via ComfyUI Manager)

---

## Node Categories

After installation, nodes appear under "Add Node" > "LTXVideo":
- `LTXVideo/loaders` - Model and LoRA loading
- `LTXVideo/samplers` - Sampling and generation
- `LTXVideo/conditioning` - Prompt encoding and conditioning
- `LTXVideo/utils` - Utility nodes

---

## Basic Workflow Pattern

```
Model Loader -> Text Encode -> Sampler -> VAE Decode -> Save
```

**Image-to-video** adds initial image conditioning node.
**Upscaling** inserts an upscaler node between sampling stages.

---

## Example Workflows

Load from `custom_nodes/ComfyUI-LTXVideo/example_workflows/`:

### Text-to-Video Workflow
1. Load model (distilled or full checkpoint)
2. Encode prompt with Gemma text encoder
3. Configure sampler (steps, resolution, frames)
4. Decode VAE
5. Save output

### Image-to-Video Workflow
Same as text-to-video, but adds image loading and conditioning nodes.

---

## Key Configuration Parameters

### Resolution Options
| Resolution | Aspect Ratio | Notes |
|-----------|-------------|-------|
| 1920x1080 | 16:9 | Full HD landscape |
| 1080x1920 | 9:16 | Portrait |
| 1280x720 | 16:9 | HD, lower VRAM |
| 768x512 | 3:2 | Fast iteration |
| 704x512 | 4:3 | Standard |
| 640x640 | 1:1 | Square |

All dimensions must be divisible by 32.

### Frame Count
- Maximum: 257 frames (~10 seconds at 25fps)
- Recommended: 121-161 frames
- Fast iteration: 65-97 frames
- Must follow 8n+1 pattern

### Frame Rate
- 24 fps - Cinematic
- 25 fps - Standard default
- 30 fps - Smooth motion
- 48-60 fps - High-speed content

### Sampling
| Model | Steps | CFG Scale | Sampler |
|-------|-------|-----------|---------|
| Distilled | 4-8 | 1.0 | euler |
| Full | 20-50 | 2.0-5.0 | dpmpp_2m or res_2s |

---

## Two-Stage Generation

Recommended workflow for best quality:
1. **Stage 1:** Base generation at half resolution
2. **Stage 2:** LTXVUpscale node applies 2x upscaling

Tiled decoding minimizes memory usage during final decode.

---

## Output Configuration
- **Formats:** MP4 (default), MOV, WebM
- **Codecs:** H.264 (compatibility) or H.265 (smaller files)
- **Audio:** Automatically embedded from audio decoder

---

## LTX Desktop (Alternative)

A standalone application alternative to ComfyUI:
- Download from GitHub Releases
- Requires NVIDIA GPU with 16GB+ VRAM (Windows/Linux)
- Choose cloud text encoding (API key) or local processing
- macOS requires API key for cloud-based operation

---

## Troubleshooting

| Issue | Solution |
|-------|---------|
| Missing nodes | Verify installation directory; check Python version; full restart |
| Workflow errors | Install missing nodes via Manager; update LTXVideo |
| Model issues | Verify file paths; re-download if corrupted |
| Out of memory | Use distilled model; reduce resolution; enable tiled decoding |

---

## Related Links

- **ComfyUI Guide:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
- **Quick Start:** https://docs.ltx.video/open-source-model/getting-started/quick-start
- **Node Reference:** https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
- **GitHub:** https://github.com/Lightricks/ComfyUI-LTXVideo
