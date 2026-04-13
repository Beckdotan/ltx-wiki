---
title: ComfyUI LTX Video Node Reference
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
  - https://github.com/Lightricks/ComfyUI-LTXVideo
  - https://comfyai.run/documentation/LTXVPromptEnhancerLoader
  - https://comfyai.run/documentation/LTXVPromptEnhancer
  - https://comfyai.run/documentation/LTXVGemmaEnhancePrompt
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/3.2-prompt-enhancement
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.4-ic-lora-workflows
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.6-camera-control
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/4.3-memory-management
tags:
  - comfyui
  - ltx-video
  - nodes
  - reference
---

# ComfyUI LTX Video Node Reference

Detailed reference for all nodes in the [[comfyui-ltxvideo-official-nodes|ComfyUI-LTXVideo extension]] and relevant core ComfyUI nodes for [[ltx-video-overview|LTX Video]] workflows.

## Text Encoding Nodes

### GemmaAPITextEncode

Encodes text prompts using Lightricks' free API endpoint, bypassing local Gemma loading entirely.

| Input | Type | Description |
|-------|------|-------------|
| `api_key` | STRING | Credential from console.ltx.video |
| `prompt` | STRING | Text to encode |
| `ckpt_name` | COMBO | LTX-2 checkpoint for model ID extraction |

**Output:** `conditioning` (CONDITIONING)

Key benefit: eliminates all local VRAM for text encoding. Sub-second prompt encoding. Best for consumer GPUs and multi-prompt iteration workflows.

### LTXVGemmaEnhancePrompt

Uses the Gemma model to automatically transform simple prompts into detailed descriptions optimized for video generation.

### LTXVPromptEnhancerLoader

Downloads and initializes LLMs and image captioning models from HuggingFace for prompt enhancement.

| Parameter | Description |
|-----------|-------------|
| `llm_name` | Large language model for enhancement |
| `image_captioner_name` | Image captioning model for context |

Requires ~8GB additional VRAM for the extra text encoder. See [[comfyui-ltx-performance-tips]] for alternatives.

### LTXVPromptEnhancer

Transforms basic prompts into rich, detailed descriptions.

| Parameter | Default | Description |
|-----------|---------|-------------|
| `max_resulting_tokens` | 256 | Controls detail level |

Connect output to CLIPTextEncode -> LTXVConditioning in the pipeline.

## Conditioning Nodes

### LTXVSaveConditioning

Preserves computed text conditioning as .safetensors files for reuse across sessions.

| Input | Type | Description |
|-------|------|-------------|
| `conditioning` | CONDITIONING | From any text encoder |
| `filename` | STRING | Base filename |
| `dtype` | COMBO | "bfloat16" or "float16" |

Storage: `ComfyUI/models/embeddings/`. Ideal for batch workflows, reproducibility, and offline usage.

### LTXVLoadConditioning

Loads previously saved conditioning from disk.

| Input | Type | Description |
|-------|------|-------------|
| `file_name` | COMBO | .safetensors file |
| `device` | COMBO | "cpu" or "gpu" |

GPU loading offers faster start; CPU loading works universally.

### LTXVKeyFrameConditioning

Applies conditioning at specific keyframes for frame-specific prompt control. Replaces legacy interpolation nodes from [[comfyui-ltxtricks|ComfyUI-LTXTricks]].

### LTXVSequenceConditioning

Specifies how to extend a video sequence (from first or last position). Replaces legacy frame setting nodes from [[comfyui-ltxtricks|ComfyUI-LTXTricks]].

## Guidance Nodes

### MultimodalGuider

Independent per-modality control over guidance for audio and video.

| Input | Type | Description |
|-------|------|-------------|
| `model` | MODEL | LTX-2 model instance |
| `positive` | CONDITIONING | Positive prompt |
| `negative` | CONDITIONING | Negative prompt |
| `parameters` | GUIDER_PARAMETERS | Configuration |
| `skip_blocks` | LIST | Transformer blocks to exclude from STG |

**Three Guidance Controls:**

1. **CFG Guidance** (`cfg > 1`) -- Prompt adherence and semantic accuracy
2. **Spatio-Temporal Guidance** (`stg > 0`) -- Reduces structural artifacts, prevents object breakup
3. **Cross-Modal Guidance** (`modality_scale > 1`) -- Audio-video synchronization tightness

**Per-Modality Parameters:**

| Parameter | Range | Description |
|-----------|-------|-------------|
| `skip_step` | 0-2 | Periodically skip diffusion steps |
| `rescale` | 0-1 | Post-guidance normalization |
| `perturb_attn` | Boolean | STG perturbation toggle |
| `cross_attn` | Boolean | Cross-modal attention toggle |

### LTX Add Video IC-LoRA Guide Advanced

Applies [[comfyui-ltx-lora-training-control|IC-LoRA]] control with granular strength and optional spatial masking.

| Input | Type | Description |
|-------|------|-------------|
| `attention_strength` | FLOAT | Global scaling 0.0-1.0 (default 1.0) |
| `attention_mask` | MASK | Optional spatial (HxW) or spatiotemporal (TxHxW) mask |

Enables softer control, region-specific application, and blended generation.

## Sampling Nodes

### LTXVNormalizingSampler

Drop-in sampler replacement that applies statistical normalization to latents during generation.

**Problems Addressed:** Oversaturated visuals, audio clipping, inconsistent quality across generations.

**Mechanism:** Monitors latent statistics via percentile-based analysis; maintains optimal value ranges throughout denoising.

**Limitations:** Use only for initial sampling stage. Incompatible with inpainting/masked workflows. Optimized for distilled model with 8-step schedule.

### Tiled Sampler

Performs sampling in tiles to reduce VRAM usage. Best for high-resolution generation on limited VRAM.

### Looping Sampler

Supports iterative/looping video generation for extended sequences.

## LoRA and Control Nodes

### IC-LoRA Union Control

Unified control combining Canny + Depth + Pose in a single LoRA. See [[comfyui-ltx-lora-training-control]] for details.

**Important:** Camera control LoRAs cannot be combined with IC-LoRA controls in a single generation pass.

### IC-LoRA Motion Tracking

Guides object motion using sparse point trajectories from reference video.

### Camera Control LoRAs

Apply specific camera movements (dolly, jib). Mutually exclusive with IC-LoRA Union Control in a single pass.

### LTXVReferenceAudio

Reference-audio speaker identity transfer for ID-LoRA workflows. Merged into upstream ComfyUI (PR #13111, March 2026).

## Processing Nodes

### LTXVLatentUpsampler

Spatial upscaling in latent space. See [[comfyui-ltx-two-stage-pipeline]] for pipeline details.

| Model | Scale |
|-------|-------|
| `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` | 2x |
| `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` | 1.5x |

Adds sharpness and detail without destroying temporal consistency.

### Temporal Upscaler

Doubles frame rate. Model: `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`

### Tiled VAE Decode

Decodes latents to pixel space in smaller tiles. Significantly reduces VRAM during decode while maintaining quality.

### LTXVPreprocess

Prepares input images for the pipeline (resizing, normalization). Used in image-to-video workflows.

### LTXVAddGuide

Conditions generation on reference images. Multiple chains can be used for multi-keyframe control: `LoadImage -> LTXVPreprocess -> LTXVAddGuide`

## Model Loading Nodes

### LTXVLoader

Primary model loader for LTX Video checkpoints.

### Low VRAM Loaders

Specialized loaders from `low_vram_loaders.py` ensuring correct execution order and model offloading. Targets 32GB VRAM systems.

### Q8 Nodes

8-bit quantization nodes from `q8_nodes.py` for reduced VRAM consumption.

## Utility Nodes

| Node | Purpose |
|------|---------|
| Sparse Tracks | Point trajectory tracking for motion control |
| Vanish Nodes | Object removal/vanishing effects |
| Masks | Mask manipulation for region-specific control |
| Guide | Visual guidance display for development |

## Related Pages

- [[comfyui-ltxvideo-official-nodes]] -- Extension overview and installation
- [[comfyui-ltx-workflows]] -- Workflow types using these nodes
- [[comfyui-ltx-lora-training-control]] -- LoRA and IC-LoRA details
- [[comfyui-ltx-performance-tips]] -- VRAM optimization strategies
