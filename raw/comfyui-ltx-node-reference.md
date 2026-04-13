# ComfyUI LTX Video Node Reference

## Sources
- [LTX-2 ComfyUI Nodes | LTX Documentation](https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes)
- [GitHub - Lightricks/ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo/)
- [LTXVPromptEnhancerLoader Node Documentation | ComfyAI](https://comfyai.run/documentation/LTXVPromptEnhancerLoader)
- [LTXVPromptEnhancer Node Documentation | ComfyAI](https://comfyai.run/documentation/LTXVPromptEnhancer)
- [LTXVGemmaEnhancePrompt Node Documentation | ComfyAI](https://comfyai.run/documentation/LTXVGemmaEnhancePrompt)
- [Prompt Enhancement | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/3.2-prompt-enhancement)
- [IC-LoRA Workflows | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.4-ic-lora-workflows)
- [Camera Control | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.6-camera-control)
- [Memory Management | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/4.3-memory-management)

## Overview

This document provides a detailed reference for all known ComfyUI nodes in the LTX Video ecosystem, including both the official ComfyUI-LTXVideo extension and relevant core ComfyUI nodes.

---

## Text Encoding Nodes

### GemmaAPITextEncode

**Purpose:** Encodes text prompts using Lightricks' free API endpoint, bypassing the need to load Gemma locally.

| Input | Type | Description |
|-------|------|-------------|
| `api_key` | STRING | Authentication credential from console.ltx.video |
| `prompt` | STRING | Text to be encoded |
| `ckpt_name` | COMBO | LTX-2 checkpoint filename for model ID extraction |

| Output | Type | Description |
|--------|------|-------------|
| `conditioning` | CONDITIONING | Encoded prompt ready for generation |

**Key Benefit:** Eliminates local VRAM requirements for text encoding. Enables sub-second prompt encoding.

**Best For:** Consumer GPUs with limited memory; workflows requiring multiple prompt iterations.

---

### LTXVGemmaEnhancePrompt

**Purpose:** Uses the Gemma model to enhance and enrich user prompts before encoding.

**Key Feature:** Automatically transforms simple prompts into detailed, descriptive text optimized for video generation.

---

### LTXVPromptEnhancerLoader

**Purpose:** Downloads and initializes LLMs and image captioning models from HuggingFace for prompt enhancement.

| Parameter | Default | Description |
|-----------|---------|-------------|
| `llm_name` | (configurable) | Large language model to use for enhancement |
| `image_captioner_name` | (configurable) | Image captioning model for context |

**Note:** Loading the prompt enhancer requires ~8GB additional VRAM for the extra text encoder.

---

### LTXVPromptEnhancer

**Purpose:** Takes a basic prompt and transforms it into a detailed, rich description optimized for LTX Video.

| Parameter | Default | Description |
|-----------|---------|-------------|
| `max_resulting_tokens` | 256 | Controls detail level of enhanced prompt |

**Output:** Connect to CLIPTextEncode -> LTXVConditioning for the video generation pipeline.

---

## Conditioning Nodes

### LTXVSaveConditioning

**Purpose:** Preserves computed text conditioning as .safetensors files for future reuse.

| Input | Type | Description |
|-------|------|-------------|
| `conditioning` | CONDITIONING | Output from any text encoder |
| `filename` | STRING | Base filename identifier |
| `dtype` | COMBO | Numerical precision ("bfloat16" or "float16") |

**Storage Location:** `ComfyUI/models/embeddings/`

**Ideal For:** Locking validated prompt encodings; batch generation workflows; offline usage.

---

### LTXVLoadConditioning

**Purpose:** Retrieves previously saved conditioning from disk, bypassing re-encoding.

| Input | Type | Description |
|-------|------|-------------|
| `file_name` | COMBO | .safetensors file identifier |
| `device` | COMBO | Target location ("cpu" or "gpu") |

| Output | Type | Description |
|--------|------|-------------|
| `conditioning` | CONDITIONING | Ready for generation pipeline |

**Device Notes:** GPU loading offers faster generation start; CPU loading works universally.

**Use Case:** Batch workflows; guaranteed reproducibility; offline scenarios.

---

### LTXVKeyFrameConditioning

**Purpose:** Applies conditioning at specific keyframes within the video generation, enabling frame-specific prompt control.

**Replaces:** Legacy interpolation nodes from ComfyUI-LTXTricks.

---

### LTXVSequenceConditioning

**Purpose:** Specifies how to extend a video sequence — either from the first or last position.

**Replaces:** Legacy frame setting nodes from ComfyUI-LTXTricks.

---

## Guidance Nodes

### MultimodalGuider

**Purpose:** Enables independent, per-modality control over guidance parameters for audio and video.

| Input | Type | Description |
|-------|------|-------------|
| `model` | MODEL | LTX-2 model instance |
| `positive` | CONDITIONING | Positive prompt conditioning |
| `negative` | CONDITIONING | Negative prompt conditioning |
| `parameters` | GUIDER_PARAMETERS | Configuration object |
| `skip_blocks` | LIST | Transformer blocks to exclude from STG |

**Three Independent Guidance Controls:**

1. **CFG Guidance** (`cfg > 1`) — Controls prompt adherence and semantic accuracy
2. **Spatio-Temporal Guidance** (`stg > 0`) — Reduces structural artifacts; prevents object breakup
3. **Cross-Modal Guidance** (`modality_scale > 1`) — Manages audio-video synchronization tightness

**Per-Modality Parameters:**

| Parameter | Range | Description |
|-----------|-------|-------------|
| `skip_step` | 0-2 | Periodically skip diffusion steps |
| `rescale` | 0-1 | Post-guidance normalization |
| `perturb_attn` | Boolean | Controls STG perturbation |
| `cross_attn` | Boolean | Controls cross-modal attention |

| Output | Type | Description |
|--------|------|-------------|
| `guider` | GUIDER | Configured for sampling stage |

**Applications:** Quality prioritization over sync; dialogue lip-sync; performance optimization.

---

### LTX Add Video IC-LoRA Guide Advanced

**Purpose:** Applies IC-LoRA control adapter with granular strength adjustment and optional spatial masking.

| Input | Type | Description |
|-------|------|-------------|
| `attention_strength` | FLOAT | Global scaling factor (0.0-1.0, default 1.0) |
| `attention_mask` | MASK | Optional spatial (HxW) or spatiotemporal (TxHxW) mask |

**Advantage:** Enables softer control; region-specific application; blended generation approaches.

---

## Sampling Nodes

### LTXVNormalizingSampler

**Purpose:** Applies statistical normalization to latents during generation to prevent overbaking and audio clipping.

**Mechanism:** Monitors latent statistics via percentile-based analysis; maintains optimal value ranges throughout denoising.

**Problems Addressed:**
- Oversaturated visual outputs
- Audio clipping and distortion
- Inconsistent quality across generations

**Compatibility:** Drop-in replacement for standard samplers. Works with all guiders, conditioning, and LoRA workflows.

**Limitations:**
- Use only for initial sampling stage
- Incompatible with inpainting or masked workflows
- Optimized for Distilled model with 8-step schedule

---

### Tiled Sampler

**Purpose:** Performs sampling in tiles to reduce VRAM usage during generation.

**Best For:** High-resolution generation on limited VRAM systems.

---

### Looping Sampler

**Purpose:** Supports iterative/looping video generation for extended sequences.

---

## LoRA and Control Nodes

### IC-LoRA Union Control

**Purpose:** Unified control combining Canny + Depth + Pose signals in a single LoRA.

**Control Modes:**
- **Canny** — Edge preservation from reference
- **Depth** — Camera and spatial geometry preservation
- **Pose** — Human motion transfer

**Important:** Camera control LoRAs cannot be combined with IC-LoRA controls in a single generation. Only one LoRA type can be applied at a time.

---

### IC-LoRA Motion Tracking

**Purpose:** Guides object motion using sparse point trajectories from reference video.

**Use Case:** Transfer specific motion patterns to new video content.

---

### Camera Control LoRAs

**Purpose:** Apply specific camera movements to generated video.

**Available Movements:** Dolly, jib, and other camera movement patterns.

**Note:** Mutually exclusive with IC-LoRA Union Control in a single generation pass.

---

### LTXVReferenceAudio (Upstream)

**Purpose:** Reference-audio speaker identity transfer for ID-LoRA workflows.

**Status:** Merged into upstream ComfyUI (PR #13111, March 2026).

---

## Processing Nodes

### LTXVLatentUpsampler

**Purpose:** Performs spatial upscaling directly in latent space.

**Available Models:**
- 2x spatial upscaler (`ltx-2.3-spatial-upscaler-x2-1.0.safetensors`)
- 1.5x spatial upscaler (`ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors`)

**Key Benefit:** Adds sharpness and fine detail without destroying temporal consistency.

---

### Temporal Upscaler

**Purpose:** Doubles the frame rate of existing video clips.

**Model:** `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`

---

### Tiled VAE Decode

**Purpose:** Decodes latents to pixel space in smaller tiles.

**Benefit:** Significantly reduces VRAM during final decode while maintaining quality.

---

### LTXVPreprocess

**Purpose:** Prepares input images for the LTX pipeline (resizing, normalization).

**Used In:** Image-to-video workflows.

---

### LTXVAddGuide

**Purpose:** Conditions generation on one or more reference images.

**Chaining:** Multiple `LoadImage -> LTXVPreprocess -> LTXVAddGuide` chains can be used for multi-keyframe control.

---

## Model Loading Nodes

### LTXVLoader

**Purpose:** Primary model loader for LTX Video checkpoints.

---

### Low VRAM Loaders

**Purpose:** Specialized loaders from `low_vram_loaders.py` that ensure correct execution order and model offloading.

**Target:** Systems with 32GB VRAM that need careful memory management.

---

### Q8 Nodes

**Purpose:** 8-bit quantization nodes for reduced VRAM consumption.

**Source:** `q8_nodes.py` in the ComfyUI-LTXVideo extension.

---

## Utility Nodes

### Sparse Tracks

**Purpose:** Sparse point trajectory tracking for motion control.

### Vanish Nodes

**Purpose:** Object removal and vanishing effects in generated video.

### Masks

**Purpose:** Mask manipulation utilities for region-specific control.

### Guide

**Purpose:** Visual guidance display nodes for workflow development.
