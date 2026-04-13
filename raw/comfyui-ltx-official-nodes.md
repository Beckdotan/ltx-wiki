# ComfyUI-LTXVideo: Official Nodes for LTX Video

## Sources
- [GitHub - Lightricks/ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo/)
- [Using ComfyUI with LTX-2 | LTX Documentation](https://docs.ltx.video/open-source-model/integration-tools/comfy-ui)
- [LTX-2 ComfyUI Nodes | LTX Documentation](https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes)
- [ComfyUI-LTXVideo detailed guide | RunComfy](https://www.runcomfy.com/comfyui-nodes/ComfyUI-LTXVideo)
- [NEW VIDEO MODEL: LTXV day-1 Native Support in ComfyUI](https://blog.comfy.org/p/ltxv-day-1-comfyui)
- [LTX-Video 0.9.5 Day-1 Support in ComfyUI](https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui)
- [ComfyUI-LTXVideo Custom Node | ComfyAI](https://comfyai.run/custom_node/ComfyUI-LTXVideo)

## Overview

ComfyUI-LTXVideo is the official custom node extension developed by Lightricks for integrating the LTX Video generation model into ComfyUI. LTX Video received day-1 native support in ComfyUI, meaning from the model's initial release, ComfyUI users could immediately start using it through visual node-based workflows.

LTX-2 is now built into ComfyUI core, making it readily accessible to all ComfyUI users. The ComfyUI-LTXVideo extension adds advanced functionality beyond the built-in nodes.

## Installation

### Method 1: ComfyUI Manager (Recommended)

1. Open ComfyUI
2. Press `Ctrl+M` or click the Manager button
3. Select "Install Custom Nodes"
4. Search for "LTXVideo"
5. Click Install
6. Wait for completion
7. Click the Restart button to restart ComfyUI
8. Manually refresh your browser to clear cache and access updated node list

### Method 2: Install via Git URL

1. Click the "Install via Git URL" button in ComfyUI Manager
2. Enter: `https://github.com/Lightricks/ComfyUI-LTXVideo`
3. Click Install

### Method 3: Manual Installation

```bash
cd ComfyUI/custom_nodes/
git clone https://github.com/Lightricks/ComfyUI-LTXVideo
cd ComfyUI-LTXVideo
pip install -r requirements.txt
```

### Verification

Right-click the canvas and navigate to Add Node -> LTXVideo to confirm node categories appear.

## Required Model Files

### Main Checkpoints (choose one, place in `models/checkpoints/`)

- `ltx-2.3-22b-dev.safetensors` — Full 22B model for maximum quality
- `ltx-2.3-22b-distilled.safetensors` — Distilled 22B model for faster iteration

### Spatial Upscalers (place in `models/latent_upscale_models/`)

- `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` — 2x spatial upscaler
- `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` — 1.5x spatial upscaler

### Temporal Upscaler (place in `models/latent_upscale_models/`)

- `ltx-2.3-temporal-upscaler-x2-1.0.safetensors` — 2x frame rate doubler

### Distilled LoRA (place in `models/loras/`)

- `ltx-2.3-22b-distilled-lora-384.safetensors`

### Text Encoder (place in `models/text_encoders/`)

- Gemma 3 12B IT (complete repository) — Can be installed via ComfyUI Manager or downloaded manually from HuggingFace

### Additional LoRAs (place in `models/loras/`)

- Union control LoRA (Canny + Depth + Pose combined)
- Motion tracking LoRA
- Detailer LoRA
- Pose control LoRA
- Multiple camera control variants (dolly, jib movements)

**Note:** Models download automatically upon first use. Simply load a workflow and click "Queue Prompt" and the nodes will handle downloading.

## Complete Node List

The extension provides nodes organized across multiple categories:

### Conditioning Nodes
- **LTXVSaveConditioning** — Preserves computed text conditioning as .safetensors files for reuse
- **LTXVLoadConditioning** — Retrieves previously saved conditioning from disk
- **LTXVKeyFrameConditioning** — Keyframe-based conditional control
- **LTXVSequenceConditioning** — Sequence-based conditional control (extend video from first or last position)
- **Dynamic Conditioning nodes** — Dynamic prompt handling

### Text Encoding Nodes
- **GemmaAPITextEncode** — Encodes text using Lightricks' free API endpoint, bypassing local Gemma loading. Inputs: api_key, prompt, ckpt_name. Eliminates local VRAM for text encoding, enables sub-second prompt encoding.
- **LTXVGemmaEnhancePrompt** — Gemma-based prompt enhancement
- **Text Embeddings Connectors** — Bridge nodes for embedding compatibility

### Sampling Nodes
- **LTXVNormalizingSampler** — Applies statistical normalization to latents during generation to prevent overbaking and audio clipping. Drop-in replacement for standard samplers. Optimized for Distilled model with 8-step schedule.
- **Easy Samplers** — Simplified sampling nodes for common use cases
- **Looping Sampler** — For iterative/looping video generation
- **Tiled Sampler** — Tiled sampling for reduced VRAM usage

### Guidance Nodes
- **MultimodalGuider** — Independent per-modality control over guidance parameters for audio and video. Supports three independent guidance controls:
  - CFG Guidance (cfg > 1) — Controls prompt adherence and semantic accuracy
  - Spatio-Temporal Guidance (stg > 0) — Reduces structural artifacts, prevents object breakup
  - Cross-Modal Guidance (modality_scale > 1) — Manages audio-video synchronization
- **LTX Add Video IC-LoRA Guide Advanced** — Applies IC-LoRA control with granular strength and optional spatial masking

### LoRA Nodes
- **IC-LoRA** nodes — In-Context LoRA for structural control (depth, pose, edge)
- **IC-LoRA Attention** nodes — Attention-level LoRA control

### Processing Nodes
- **Latent Norm** — Latent normalization
- **Tiled VAE Decode** — Significantly reduces VRAM during decoding while maintaining quality
- **VAE Patcher** — VAE modification utilities
- **Decoder Noise** — Noise management for decoding

### Utility Nodes
- **Prompt Enhancer** — Automates prompt enhancement using LLMs and image captioning
- **Masks** — Mask manipulation utilities
- **Sparse Tracks** — Sparse point trajectory tracking
- **Guide** — Visual guidance nodes
- **Vanish Nodes** — Object removal/vanishing effects
- **Q8 Nodes** — 8-bit quantization nodes for reduced VRAM

### Model Loading
- **Low VRAM Loaders** — Specialized model loaders ensuring correct execution order and model offloading for 32GB VRAM systems

## Key Features and Capabilities

1. **Text-to-Video Generation** — Transform descriptive text prompts into video sequences
2. **Image-to-Video Generation** — Convert static images into dynamic video
3. **Video Enhancement and Detailing** — Refine and improve existing video
4. **Motion Tracking** — IC-LoRA-based motion tracking from reference videos
5. **Camera Control** — Dolly, jib, and other camera movement controls via LoRA
6. **Pose-Guided Generation** — Generate video guided by human pose data
7. **Audio-Video Generation** — Synchronized audio and video in a single model (LTX-2.3)
8. **Multi-Stage Processing** — Spatial and temporal upsampling pipelines
9. **Frame Interpolation** — Smooth transitions between keyframes
10. **Sequence Conditioning** — Extend videos from first or last frame position

## Example Workflows

Workflows are located in: `ComfyUI/custom_nodes/ComfyUI-LTXVideo/example_workflows/`

### LTX-2.3 Workflows
- Single-stage text/image-to-video (full and distilled models)
- Two-stage text/image-to-video with spatial upsampling (distilled)
- IC-LoRA union control with depth, pose, and edge guidance
- IC-LoRA motion tracking for image-to-video

### Older LTX-2.0 Workflows
- Text-to-video (full and distilled variants)
- Image-to-video (full and distilled variants)
- Video-to-video detailer
- IC-LoRA multi-control workflows
- IC-LoRA with downscaled reference latents

## Hardware Requirements

- **GPU:** CUDA-compatible with 32GB+ VRAM (recommended)
- **Disk Space:** 100GB+ free for models
- **Python:** 3.10 or later
- **CUDA:** 12.2+
- **PyTorch:** 2.1.2+

## Version History

- **LTX Video 2B (v0.9)** — Original release, 2 billion parameters, 768x512 @ 24fps
- **LTX Video 2B (v0.9.1)** — Improved version with low VRAM support
- **LTX Video 0.9.5** — Multi-keyframe control, longer videos, improved quality
- **LTX Video 13B (v0.9.7)** — 13 billion parameters, better details and prompt adherence
- **LTX-2** — Major architecture update, native ComfyUI core support
- **LTX-2.3** — Latest version with audio generation, Gemma 3 encoder, spatial/temporal upscalers
