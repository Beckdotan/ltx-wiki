---
title: ComfyUI-LTXVideo Official Extension
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/ComfyUI-LTXVideo
  - https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
  - https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
  - https://www.runcomfy.com/comfyui-nodes/ComfyUI-LTXVideo
  - https://blog.comfy.org/p/ltxv-day-1-comfyui
  - https://comfyai.run/custom_node/ComfyUI-LTXVideo
tags:
  - comfyui
  - ltx-video
  - nodes
  - official
---

# ComfyUI-LTXVideo Official Extension

ComfyUI-LTXVideo is the official custom node extension developed by Lightricks for the [[comfyui-ltx-integration-overview|ComfyUI LTX Video integration]]. While [[ltx-2-overview|LTX-2]] is built into ComfyUI core for basic workflows, this extension provides advanced capabilities.

## Installation

### Via ComfyUI Manager (Recommended)

1. Open ComfyUI and press `Ctrl+M` (or click the Manager button)
2. Select "Install Custom Nodes"
3. Search for "LTXVideo"
4. Click Install, then Restart ComfyUI
5. Refresh browser (`Ctrl+F5`) to clear cache

See [[comfyui-manager-ltx-setup]] for alternative installation methods and troubleshooting.

### Manual Installation

```
cd ComfyUI/custom_nodes/
git clone https://github.com/Lightricks/ComfyUI-LTXVideo
cd ComfyUI-LTXVideo
pip install -r requirements.txt
```

### Verification

Right-click the canvas and navigate to Add Node -> LTXVideo to confirm node categories appear.

## Required Model Files

### Checkpoints (`models/checkpoints/`)

| File | Purpose |
|------|---------|
| `ltx-2.3-22b-dev.safetensors` | Full 22B model for maximum quality |
| `ltx-2.3-22b-distilled.safetensors` | Distilled 22B model for faster iteration |

### Upscalers (`models/latent_upscale_models/`)

| File | Purpose |
|------|---------|
| `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` | 2x spatial upscaler |
| `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` | 1.5x spatial upscaler |
| `ltx-2.3-temporal-upscaler-x2-1.0.safetensors` | 2x frame rate doubler |

### Text Encoder (`models/text_encoders/`)

- Gemma 3 12B IT (complete repository) -- installable via ComfyUI Manager or manual HuggingFace download

### LoRAs (`models/loras/`)

- `ltx-2.3-22b-distilled-lora-384.safetensors` (distilled LoRA)
- Union control LoRA (Canny + Depth + Pose)
- Motion tracking LoRA
- Detailer LoRA
- Pose control LoRA
- Camera control variants (dolly, jib movements)

See [[comfyui-ltx-lora-training-control]] for full LoRA documentation.

Models download automatically on first use when you queue a prompt.

## Node Categories

The extension provides nodes organized across these categories. See [[comfyui-ltx-node-reference]] for detailed per-node documentation.

### Conditioning

- **LTXVSaveConditioning** -- Save computed text conditioning as .safetensors
- **LTXVLoadConditioning** -- Load previously saved conditioning
- **LTXVKeyFrameConditioning** -- Keyframe-based conditional control
- **LTXVSequenceConditioning** -- Sequence extension (first or last position)
- Dynamic conditioning nodes

### Text Encoding

- **GemmaAPITextEncode** -- Free API-based prompt encoding (no local VRAM needed)
- **LTXVGemmaEnhancePrompt** -- Gemma-based prompt enhancement
- **LTXVPromptEnhancerLoader** / **LTXVPromptEnhancer** -- LLM-based prompt enhancement

### Sampling

- **LTXVNormalizingSampler** -- Drop-in sampler with latent normalization
- Easy samplers, looping sampler, tiled sampler

### Guidance

- **MultimodalGuider** -- Independent audio/video guidance control (CFG, STG, cross-modal)
- **LTX Add Video IC-LoRA Guide Advanced** -- IC-LoRA control with strength and masking

### LoRA and Control

- IC-LoRA nodes (depth, pose, edge)
- IC-LoRA Attention nodes
- Camera control integration

### Processing

- **LTXVLatentUpsampler** -- Spatial upscaling in latent space
- **Temporal Upscaler** -- Frame rate doubling
- **Tiled VAE Decode** -- Low-VRAM decoding
- VAE Patcher, Decoder Noise

### Utilities

- Prompt Enhancer, Masks, Sparse Tracks, Guide, Vanish Nodes, Q8 Nodes

### Model Loading

- **Low VRAM Loaders** -- Specialized loaders for 32GB VRAM systems

## Example Workflows

Located in `ComfyUI/custom_nodes/ComfyUI-LTXVideo/example_workflows/`:

### LTX-2.3

- Single-stage text/image-to-video (full and distilled)
- [[comfyui-ltx-two-stage-pipeline|Two-stage with spatial upsampling]]
- IC-LoRA union control (depth, pose, edge)
- IC-LoRA motion tracking for image-to-video

### LTX-2.0 (Legacy)

- Text-to-video and image-to-video (full and distilled)
- Video-to-video detailer
- IC-LoRA multi-control workflows

## Pre-Built Workflow Files

| File | Model | Use Case |
|------|-------|----------|
| `ltxv-13b-i2v-base.json` | 13B Dev | Full quality image-to-video |
| `ltxv-13b-i2v-mixed-multiscale.json` | 13B Mixed | Balanced speed/quality with upscaling |
| `ltxv-13b-dist-i2v-base.json` | 13B Distilled | Fast image-to-video |
| `ltxv-13b-i2v-base-fp8.json` | 13B FP8 | Low-VRAM image-to-video |
| `ltxv-2b-dist-i2v-base.json` | 2B Distilled | Lightweight image-to-video |

## Key Capabilities

1. Text-to-Video, Image-to-Video, Video-to-Video generation
2. Audio-Video synchronized generation (LTX-2.3)
3. IC-LoRA structural control (depth, pose, edge, motion tracking)
4. Camera control (dolly, jib movements)
5. Multi-stage processing with spatial and temporal upsampling
6. Frame interpolation and sequence conditioning
7. Prompt enhancement via LLM or Gemma

## Hardware Requirements

| Component | Requirement |
|-----------|-------------|
| GPU | CUDA-compatible, 32GB+ VRAM recommended |
| Disk | 100GB+ free |
| Python | 3.10+ |
| CUDA | 12.2+ |
| PyTorch | 2.1.2+ |

## Related Pages

- [[comfyui-ltx-integration-overview]] -- Integration overview
- [[comfyui-ltx-node-reference]] -- Detailed node reference
- [[comfyui-manager-ltx-setup]] -- Installation and manager details
- [[comfyui-ltx-community-nodes]] -- Third-party extensions
