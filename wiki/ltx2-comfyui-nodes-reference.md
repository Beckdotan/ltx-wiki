---
title: LTX-2 ComfyUI Nodes Reference
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-ltx2-comfyui-nodes-reference.md
  - raw/ltx2-comfyui-integration.md
tags:
  - ltx2
  - comfyui
  - nodes
  - reference
  - api
---

# LTX-2 ComfyUI Nodes Reference

Detailed reference for the custom nodes in the ComfyUI-LTXVideo node pack. These nodes are used within [[ltx2-comfyui-integration|ComfyUI workflows]] for LTX-2 generation.

- Node reference docs: https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
- GitHub: https://github.com/Lightricks/ComfyUI-LTXVideo

## Gemma Text Encoding Nodes

The Gemma 3-12B text encoder has a large memory footprint, requiring frequent VRAM load/unload cycles that slow prompt iteration on consumer GPUs. These nodes address that bottleneck.

### GemmaAPITextEncode (Text Encode API)

Offloads text encoding to Lightricks' free API endpoint, eliminating local VRAM requirements for the Gemma text encoder.

| Parameter | Type | Description |
|-----------|------|-------------|
| `api_key` | string | API key from console.ltx.video |
| `prompt` | string | Text prompt to encode |
| `ckpt_name` | string | Checkpoint name |

- Output: `conditioning` (encoded prompt ready for generation)
- Benefit: Sub-second prompt encoding vs repeated Gemma reload cycles
- API key: Obtain at https://console.ltx.video

### LTXVSaveConditioning (Save Text Conditioning)

Stores computed text conditioning to disk as `.safetensors` files for reuse across sessions.

| Parameter | Type | Description |
|-----------|------|-------------|
| `conditioning` | tensor | Computed conditioning to save |
| `filename` | string | Output filename |
| `dtype` | enum | `bfloat16` or `float16` |

- Output location: `ComfyUI/models/embeddings/`
- Use case: Creating reusable prompt libraries and ensuring reproducibility

### LTXVLoadConditioning (Load Text Conditioning)

Retrieves previously saved conditioning from disk.

| Parameter | Type | Description |
|-----------|------|-------------|
| `file_name` | string | Saved conditioning file |
| `device` | enum | `cpu` or `gpu` |

- Output: `conditioning` ready for generation
- Enables offline workflows and batch processing with preset prompts

## Advanced Model Guidance Nodes

### MultimodalGuider

Decouples audio and video guidance controls, solving the problem where standard guidance treats audio and video as a single unit.

**Three Primary Guidance Controls:**

1. **CFG Guidance (`cfg > 1`)** -- Controls prompt adherence per modality
2. **Spatio-Temporal Guidance (`stg > 0`)** -- Reduces artifacts and object breakup
3. **Cross-Modal Guidance (`modality_scale > 1`)** -- Manages audio-video synchronization

**Per-modality Parameters:**

| Parameter | Description |
|-----------|-------------|
| `skip_step` | Skip early steps for speed optimization |
| `rescale` | Prevent oversaturation |
| `perturb_attn` | Attention perturbation for diversity |
| `cross_attn` | Cross-attention control |

Execution model: Up to four separate inference calls per diffusion step.

**Use Cases:**

- Prioritizing video quality with loose audio sync
- Achieving tight lip-sync for dialogue
- Optimizing performance through step skipping
- Dialing up motion fluidity without overfitting to the prompt

### LTXVAddVideoICLoRAGuideAdvanced (IC-LoRA Control)

Applies [[ltx2-ic-lora-guide|IC-LoRA]] control adapters with granular strength adjustment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `attention_strength` | float (0.0-1.0) | Global IC-LoRA influence scaling |
| `attention_mask` | tensor (optional) | Spatial/spatiotemporal masking |

Three control modes: Canny (edge preservation), Depth (camera/spatial geometry), Pose (human motion transfer).

## Quality Enhancement Node

### LTXVNormalizingSampler (Advanced Sampler)

Drop-in replacement for standard samplers that applies statistical normalization to latents during denoising.

**Benefits:**

- Prevents "overbaking" (oversaturation)
- Prevents audio clipping
- Ensures consistent quality, especially with high guidance values

**Mechanism:** Percentile-based latent statistics excluding extreme outliers.

**Compatibility:** Works with all guider nodes (including MultimodalGuider), text/image conditioning, LoRA, and IC-LoRA workflows.

**Restrictions:**

- Use only for the first sampling stage
- Avoid with inpainting, masking, or video extension workflows

## Additional Specialized Nodes

### LTXVTiledSampler (Tiled Sampler)

For generating larger videos by tiling. Manages VRAM for high-resolution generation.

### Looping Sampler

Creates seamlessly looping video clips. Documentation at: `ComfyUI-LTXVideo/looping_sampler.md`

## Standard Workflow Nodes

Core node categories under "Add Node" > "LTXVideo":

| Category | Purpose |
|----------|---------|
| `LTXVideo/loaders` | Model and LoRA loading |
| `LTXVideo/samplers` | Sampling and generation |
| `LTXVideo/conditioning` | Prompt encoding and conditioning |
| `LTXVideo/utils` | Utility nodes |

## Troubleshooting

| Issue | Solution |
|-------|---------|
| API errors | Verify key at console.ltx.video; regenerate if needed |
| File not found | Confirm .safetensors in `models/embeddings/` |
| MultimodalGuider slow | CFG > 1 increases computation; use `skip_step: 1` |
| NormalizingSampler issues | Confirm distilled model with 8-step sigma schedule |

## See Also

- [[ltx2-comfyui-integration]]
- [[ltx2-ic-lora-guide]]
- [[ltx2-text-to-video-guide]]
- [[ltx2-image-to-video-guide]]
