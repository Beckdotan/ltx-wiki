---
title: LTX-2 ComfyUI Integration
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-comfyui-integration.md
  - raw/dev-ltx2-comfyui-quickstart.md
tags:
  - ltx2
  - comfyui
  - integration
  - workflows
  - installation
---

# LTX-2 ComfyUI Integration

ComfyUI is the recommended way to use [[ltx2-open-source-overview|LTX-2]] locally. The ComfyUI-LTXVideo custom node pack, maintained by Lightricks, provides a visual node-based interface for all LTX-2 generation capabilities.

- GitHub: https://github.com/Lightricks/ComfyUI-LTXVideo
- Documentation: https://docs.ltx.video/open-source-model/integration-tools/comfy-ui

## Prerequisites

- ComfyUI installed and working
- CUDA GPU with 32GB+ VRAM (see [[ltx2-system-requirements]])
- 100GB+ free disk space
- Python 3.10+

## Installation

### Via ComfyUI Manager (Recommended)

1. Open ComfyUI
2. Click "Manage Extensions"
3. Search for "LTXVideo"
4. Install `ComfyUI-LTXVideo`
5. Restart ComfyUI

Dependencies are handled automatically.

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

## Model Setup

Models download automatically on first use. For manual installation, download from Hugging Face:

- **22B Distilled** -- Beginner-friendly, faster inference
- **22B Full** -- Maximum quality, slower
- **Gemma 3 text encoder** -- Install via ComfyUI Manager

## Node Categories

After installation, nodes appear under "Add Node" > "LTXVideo":

- `LTXVideo/loaders` -- Model and LoRA loading
- `LTXVideo/samplers` -- Sampling and generation
- `LTXVideo/conditioning` -- Prompt encoding and conditioning
- `LTXVideo/utils` -- Utility nodes

All nodes also appear under the "Lightricks" category.

For detailed node documentation, see [[ltx2-comfyui-nodes-reference]].

## Basic Workflow Pattern

```
Model Loader -> Text Encode -> Sampler -> VAE Decode -> Save
```

- **Image-to-video** adds initial image conditioning node
- **Upscaling** inserts an upscaler node between sampling stages

## Configuration Parameters

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
- Must follow the 8n+1 pattern

### Frame Rate

| FPS | Use Case |
|-----|----------|
| 24 | Cinematic |
| 25 | Standard default |
| 30 | Smooth motion |
| 48-60 | High-speed content |

### Sampling Parameters

| Model | Steps | CFG Scale | Sampler |
|-------|-------|-----------|---------|
| Distilled | 4-8 | 1.0 | euler |
| Full | 20-50 | 2.0-5.0 | dpmpp_2m or res_2s |

## Two-Stage Generation (Recommended)

The recommended workflow for best quality:

1. **Stage 1:** Base generation at half resolution
2. **Stage 2:** LTXVUpscale node applies 2x upscaling to refine details

Tiled decoding minimizes memory usage during final decode.

## Pre-configured Workflows

Workflow files are located at:
```
ComfyUI/custom_nodes/ComfyUI-LTXVideo/example_workflows/
```

### LTX-2 Workflows (6 included)

1. Text-to-video (basic)
2. Text-to-video with upsampling (two-stage)
3. Image-to-video
4. Audio-to-video
5. IC-LoRA control
6. Keyframe interpolation

### LTX-2.3 Workflows

- Updated workflows for the 2.3 model
- Documentation: https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
- Blog guide: https://ltx.io/model/model-blog/comfyui-workflow-guide

### Community Workflows

- **RuneXX/LTX-2-Workflows** -- Collection on Hugging Face
- **Civitai** -- Multiple community workflows
- **kombitz.com** -- Workflow resources and references

## Output Configuration

| Setting | Options |
|---------|---------|
| Format | MP4 (default), MOV, WebM |
| Codec | H.264 (compatibility), H.265 (smaller files) |
| Audio | Automatically embedded from audio decoder |

## NVIDIA Quantization Support

### NVFP8

- Available for RTX 30/40/50 Series
- 2x faster, 40% less VRAM
- Checkpoints available directly in ComfyUI

### NVFP4

- Available for RTX 50 Series only
- 3x faster, 60% less VRAM
- Announced at CES 2026

## GGUF Support

Community GGUF quantizations work through ComfyUI-GGUF nodes, enabling LTX-2 on GPUs with as little as 8-10GB VRAM. Gemma 3-12B GGUF support tracked at: https://github.com/city96/ComfyUI-GGUF/issues/398

## Community Recommendations

Reddit and community discussions heavily favor:

1. Using the "Dev" model paired with a distilled LoRA
2. Two-stage generation: base resolution + latent upscaling
3. This combination gives high-fidelity stability while accelerating render time

## LTX Desktop (Alternative)

A standalone application alternative to ComfyUI:

- Download from GitHub Releases
- Requires NVIDIA GPU with 16GB+ VRAM (Windows/Linux)
- Choose cloud text encoding (API key) or local processing
- macOS requires API key for cloud-based operation

## Troubleshooting

| Issue | Solution |
|-------|---------|
| Missing nodes | Verify installation directory; check Python version; full restart |
| Workflow errors | Install missing nodes via Manager; update LTXVideo |
| Model issues | Verify file paths; re-download if corrupted |
| Out of memory | Use distilled model; reduce resolution; enable tiled decoding |

## Community Resources

- **awesome-ltx2**: https://github.com/wildminder/awesome-ltx2 -- Curated list of models, encoders, workflows, and LoRAs

## See Also

- [[ltx2-comfyui-nodes-reference]]
- [[ltx2-text-to-video-guide]]
- [[ltx2-image-to-video-guide]]
- [[ltx2-ic-lora-guide]]
- [[ltx2-system-requirements]]
