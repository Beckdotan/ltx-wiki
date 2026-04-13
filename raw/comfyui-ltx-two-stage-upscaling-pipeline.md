# LTX-2.3 Two-Stage Pipeline with Spatial and Temporal Upscaling

## Sources
- [LTX 2.3 Spatial and Temporal Upscaler: How to Use It | CrePal](https://crepal.ai/blog/aivideo/ltx-2-3-spatial-temporal-upscaler/)
- [LTX 2.3 Multi-Stage Latent Upscaling Workflow in ComfyUI | CrePal](https://crepal.ai/blog/aivideo/ltx-2-3-multi-stage-latent-upscaling-comfyui/)
- [LTX-2.3: Introducing LTX's Latest AI Video Model | LTX](https://ltx.io/model/ltx-2-3)
- [LTX 2.3 ComfyUI Workflow | RunComfy](https://www.runcomfy.com/comfyui-workflows/ltx-2-3-comfyui-workflow-high-quality-ai-video-creator)
- [LTX-2.3 ComfyUI Setup: Two-Stage Pipeline | WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-comfyui-setup-two-stage-pipeline/)
- [LTX-2.3 ComfyUI Workflow Examples | ComfyUI Docs](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [Ultimate LTX 2.3 ComfyUI Workflow Guide (2026) | LTX-2.3](https://ltx23.org/blog/ecommerce-video-guide)
- [LTX-2.3 Released: Major Upgrade | Vantage with AI](https://vantagewithai.com/ltx-2-3-released-major-upgrade-for-ai-video-generation-comfyui-workflow-guide/)
- [awesome-ltx2 | GitHub](https://github.com/wildminder/awesome-ltx2)

## Overview

The two-stage pipeline is the recommended approach for producing high-quality LTX-2.3 video output. Instead of generating directly at the target resolution (which is VRAM-intensive and often lower quality), this pipeline generates at half resolution first, then upscales in latent space before a refinement pass.

---

## Pipeline Architecture

### Stage 1: Base Generation (Half Resolution)

**Purpose:** Establish motion structure, anatomy, scene coherence at a manageable resolution.

**Process:**
1. Text encoding (Gemma 3 12B or API)
2. Generate at half target resolution (e.g., 384x512 for 768x1024 target)
3. Use full sampling steps (20-30 for dev model, 8 for distilled)
4. Focus on getting motion and composition correct

**Why Half Resolution:**
- Faster generation
- Lower VRAM usage
- Better motion coherence (model handles smaller latent spaces more reliably)
- Mistakes at half res are less costly to iterate on

### Spatial Upscale Step

**Purpose:** Double the resolution in latent space while preserving temporal consistency.

**Node:** `LTXVLatentUpsampler`

**Available Upscaler Models:**
| Model | Scale | File |
|-------|-------|------|
| 2x Spatial | 2x | `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` |
| 1.5x Spatial | 1.5x | `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` |

**Key Benefits:**
- Operates entirely in latent space (efficient)
- Adds sharpness and fine detail
- Does NOT destroy temporal consistency from Stage 1
- More VRAM-friendly than generating at full resolution directly

**Model Placement:** `ComfyUI/models/latent_upscale_models/`

### Stage 2: Refinement Pass

**Purpose:** Add fine detail, texture, and quality at the upscaled resolution.

**Process:**
1. Take upscaled latents from previous step
2. Run additional sampling steps at full resolution
3. Lower denoise value to preserve structure from Stage 1
4. Add detail without changing fundamental composition

### Decode Step

**Purpose:** Convert final latents to pixel space.

**Node:** VAEDecode (or Tiled VAE Decode for lower VRAM)

**Output:** Final video frames saved to `ComfyUI/output/`

---

## Temporal Upscaling

### Purpose

Doubles the frame rate of existing video clips for smoother playback.

**Model:** `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`

**Use Case:** Generate at 12fps then temporal upscale to 24fps, or generate at 24fps and upscale to 48fps.

**Model Placement:** `ComfyUI/models/latent_upscale_models/`

---

## Multi-Stage Combinations

### Standard Two-Stage (Spatial Only)

```
Text/Image -> Stage 1 (half res) -> Spatial 2x Upscale -> Stage 2 (full res) -> Decode
```

**Resolution Example:** 384x512 -> 768x1024
**VRAM Requirement:** 16GB+ with FP8

### Full Pipeline (Spatial + Temporal)

```
Text/Image -> Stage 1 (half res, half fps) -> Spatial 2x -> Temporal 2x -> Stage 2 -> Decode
```

**Resolution + FPS Example:** 384x512 @ 12fps -> 768x1024 @ 24fps
**VRAM Requirement:** 24GB+

### Maximum Quality Pipeline

```
Text/Image -> Stage 1 (quarter res) -> Spatial 1.5x -> Spatial 1.5x -> Temporal 2x -> Stage 2 -> Decode
```

**VRAM Requirement:** 32GB+

---

## Required Model Files

| File | Location | Purpose |
|------|----------|---------|
| `ltx-2.3-22b-dev.safetensors` | `models/checkpoints/` | Full quality model |
| `ltx-2.3-22b-distilled.safetensors` | `models/checkpoints/` | Fast iteration model |
| `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` | `models/latent_upscale_models/` | 2x spatial upscale |
| `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` | `models/latent_upscale_models/` | 1.5x spatial upscale |
| `ltx-2.3-temporal-upscaler-x2-1.0.safetensors` | `models/latent_upscale_models/` | 2x frame rate |
| `ltx-2.3-22b-distilled-lora-384.safetensors` | `models/loras/` | Distilled LoRA |
| Gemma 3 12B IT | `models/text_encoders/` | Text encoder |

---

## VRAM Requirements by Configuration

| Configuration | Target Resolution | Min VRAM | Recommended VRAM |
|---------------|------------------|----------|-----------------|
| Single-stage, half res | 480x720 | 12 GB (FP8) | 16 GB |
| Two-stage, 720p | 720x1280 | 16 GB (FP8) | 24 GB |
| Two-stage, 1080p | 1080x1920 | 24 GB | 32 GB |
| Full pipeline + temporal | 1080p @ 48fps | 32 GB | 48 GB |

**Optimization Tips for Constrained VRAM:**
- Use `--reserve-vram 5` launch flag
- Use FP8 checkpoint
- Use GemmaAPITextEncode instead of local encoder
- Use Tiled VAE Decode for the final decode step
- Use the distilled model (8 steps) for iteration, dev model for final only

---

## Workflow Configuration Tips

### Stage 1 Settings

| Parameter | Distilled Model | Dev Model |
|-----------|----------------|-----------|
| Steps | 8 | 20-30 |
| CFG | 3.0-4.0 | 3.0-5.0 |
| Sampler | euler | euler |
| Scheduler | normal | normal |
| Denoise | 1.0 | 1.0 |

### Stage 2 Settings

| Parameter | Recommendation |
|-----------|---------------|
| Steps | 10-15 |
| CFG | 2.0-3.0 |
| Denoise | 0.3-0.5 |

**Note:** Lower denoise in Stage 2 preserves the motion and composition from Stage 1 while adding detail.

---

## Practical Workflow Tips

1. **Start with Distilled Model** — Use for rapid iteration on composition and motion. Switch to dev model for final render only.

2. **Test at Low Resolution First** — Validate prompt and composition at 480x720 before committing to full two-stage pipeline.

3. **Save Stage 1 Latents** — Use LTXVSaveConditioning-like approach to save intermediate latents, allowing you to rerun Stage 2 with different settings without regenerating Stage 1.

4. **Spatial Before Temporal** — Always upscale resolution before frame rate for best results.

5. **Close Unused Workflows** — Clear cache between generations to maximize available VRAM for the pipeline.

6. **Monitor VRAM Usage** — Use task manager or nvidia-smi to ensure you have headroom, especially during the upscale step which creates large intermediate tensors.
