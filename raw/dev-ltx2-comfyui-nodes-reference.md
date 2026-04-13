# LTX-2 ComfyUI Nodes Reference

**Source:** https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
**Fetched:** 2026-04-13

---

## Overview

The ComfyUI-LTXVideo repository provides specialized custom nodes for LTX-2 workflows in ComfyUI. These nodes cover text encoding, advanced guidance control, IC-LoRA integration, and quality enhancement.

---

## Gemma Text Encoding Nodes

### GemmaAPITextEncode
Offloads text encoding to Lightricks' free API endpoint, eliminating local VRAM requirements for the Gemma text encoder.

| Parameter | Type | Description |
|-----------|------|-------------|
| `api_key` | string | API key from console.ltx.video |
| `prompt` | string | Text prompt to encode |
| `ckpt_name` | string | Checkpoint name |

- **Output:** `conditioning` (encoded prompt ready for generation)
- **Benefit:** Sub-second prompt encoding vs repeated Gemma reload cycles
- **API Key:** Obtain at https://console.ltx.video

### LTXVSaveConditioning
Stores computed text conditioning to disk as `.safetensors` files for reuse across sessions.

| Parameter | Type | Description |
|-----------|------|-------------|
| `conditioning` | tensor | Computed conditioning to save |
| `filename` | string | Output filename |
| `dtype` | enum | `bfloat16` or `float16` |

- **Output Location:** `ComfyUI/models/embeddings/`
- **Use Case:** Creating reusable prompt libraries and ensuring reproducibility

### LTXVLoadConditioning
Retrieves previously saved conditioning from disk.

| Parameter | Type | Description |
|-----------|------|-------------|
| `file_name` | string | Saved conditioning file |
| `device` | enum | `cpu` or `gpu` |

- **Output:** `conditioning` ready for generation
- **Enables:** Offline workflows and batch processing with preset prompts

---

## Advanced Model Guidance Nodes

### MultimodalGuider
Enables independent control over audio and video guidance parameters beyond standard CFG.

**Three Primary Guidance Controls:**

1. **CFG Guidance (`cfg > 1`):** Controls prompt adherence per modality
2. **Spatio-Temporal Guidance (`stg > 0`):** Reduces artifacts and object breakup
3. **Cross-Modal Guidance (`modality_scale > 1`):** Manages audio-video synchronization

**Per-modality Parameters:**
- `skip_step` - Skip early steps for speed optimization
- `rescale` - Prevent oversaturation
- `perturb_attn` - Attention perturbation for diversity
- `cross_attn` - Cross-attention control

**Execution Model:** Up to four separate inference calls per diffusion step.

**Use Cases:**
- Prioritizing video quality with loose audio sync
- Achieving tight lip-sync for dialogue
- Optimizing performance through step skipping

### LTX Add Video IC-LoRA Guide Advanced
Applies IC-LoRA control adapters with granular strength adjustment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `attention_strength` | float (0.0-1.0) | Global IC-LoRA influence scaling |
| `attention_mask` | tensor (optional) | Spatial/spatiotemporal masking |

---

## Quality Enhancement Node

### LTXVNormalizingSampler
Specialized sampler applying statistical normalization to latents during denoising.

**Benefits:**
- Prevents "overbaking" (oversaturation)
- Prevents audio clipping
- Ensures consistent quality

**Mechanism:** Percentile-based latent statistics excluding extreme outliers.

**Usage:** Drop-in replacement for standard samplers. Works with all guiders, conditioning types, and LoRA workflows.

**Restrictions:**
- Use only for first sampling stage
- Avoid with inpainting, masking, or video extension workflows

---

## Standard Workflow Nodes

### Core Node Categories
Nodes appear under "Add Node" > "LTXVideo":
- `LTXVideo/loaders` - Model and LoRA loading
- `LTXVideo/samplers` - Sampling and generation
- `LTXVideo/conditioning` - Prompt encoding and conditioning
- `LTXVideo/utils` - Utility nodes

---

## Installation

### Via ComfyUI Manager
Search "ComfyUI-LTXVideo", install, restart ComfyUI.

### Manual Update
```bash
cd ComfyUI/custom_nodes/ComfyUI-LTXVideo
git pull origin master
pip install -r requirements.txt
```

---

## Troubleshooting

| Issue | Solution |
|-------|---------|
| API errors | Verify key at console.ltx.video; regenerate if needed |
| File not found | Confirm .safetensors in `models/embeddings/` |
| MultimodalGuider slow | CFG > 1 increases computation; use `skip_step: 1` |
| NormalizingSampler issues | Confirm distilled model with 8-step sigma schedule |

---

## Related Links

- **Node Reference:** https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
- **ComfyUI Integration:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
- **GitHub:** https://github.com/Lightricks/ComfyUI-LTXVideo
