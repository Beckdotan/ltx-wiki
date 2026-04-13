---
title: LTX-2.3 Two-Stage Upscaling Pipeline
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://crepal.ai/blog/aivideo/ltx-2-3-spatial-temporal-upscaler/
  - https://crepal.ai/blog/aivideo/ltx-2-3-multi-stage-latent-upscaling-comfyui/
  - https://ltx.io/model/ltx-2-3
  - https://www.runcomfy.com/comfyui-workflows/ltx-2-3-comfyui-workflow-high-quality-ai-video-creator
  - https://wavespeed.ai/blog/posts/ltx-2-3-comfyui-setup-two-stage-pipeline/
  - https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
  - https://github.com/wildminder/awesome-ltx2
tags:
  - comfyui
  - ltx-video
  - upscaling
  - pipeline
  - two-stage
---

# LTX-2.3 Two-Stage Upscaling Pipeline

The two-stage pipeline is the recommended approach for producing high-quality [[ltx-video-overview|LTX-2.3]] video output in ComfyUI. Instead of generating directly at target resolution, it generates at half resolution first, upscales in latent space, then refines.

## Why Two-Stage

Generating directly at full resolution is VRAM-intensive and often produces lower quality. The two-stage approach provides:
- Better motion coherence (model handles smaller latent spaces more reliably)
- Lower VRAM usage per step
- Sharper fine detail after upscaling
- Faster iteration on composition before committing to full resolution

## Pipeline Architecture

### Stage 1: Base Generation (Half Resolution)

Establish motion structure, anatomy, and scene coherence at manageable resolution.

1. Text encoding (Gemma 3 12B or [[comfyui-ltx-node-reference|GemmaAPITextEncode]])
2. Generate at half target resolution (e.g., 384x512 for 768x1024 target)
3. Full sampling steps (20-30 for dev model, 8 for distilled)
4. Focus on motion and composition correctness

### Spatial Upscale Step

Double resolution in latent space while preserving temporal consistency.

**Node:** [[comfyui-ltx-node-reference|LTXVLatentUpsampler]]

| Model | Scale | File |
|-------|-------|------|
| 2x Spatial | 2x | `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` |
| 1.5x Spatial | 1.5x | `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` |

Operates entirely in latent space. Does not destroy temporal consistency from Stage 1. Place models in `ComfyUI/models/latent_upscale_models/`.

### Stage 2: Refinement Pass

Add fine detail, texture, and quality at the upscaled resolution.

1. Take upscaled latents from previous step
2. Run additional sampling at full resolution
3. Lower denoise to preserve Stage 1 structure
4. Add detail without changing fundamental composition

### Decode Step

Convert final latents to pixel space via VAEDecode (or Tiled VAE Decode for lower VRAM). Output to `ComfyUI/output/`.

## Temporal Upscaling

Doubles frame rate for smoother playback.

**Model:** `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`

Use cases: Generate at 12fps then upscale to 24fps, or 24fps to 48fps.

## Pipeline Combinations

### Standard Two-Stage (Spatial Only)

```
Text/Image -> Stage 1 (half res) -> Spatial 2x -> Stage 2 (full res) -> Decode
```
Resolution: 384x512 -> 768x1024. VRAM: 16GB+ with FP8.

### Full Pipeline (Spatial + Temporal)

```
Text/Image -> Stage 1 (half res, half fps) -> Spatial 2x -> Temporal 2x -> Stage 2 -> Decode
```
Resolution + FPS: 384x512 @ 12fps -> 768x1024 @ 24fps. VRAM: 24GB+.

### Maximum Quality Pipeline

```
Text/Image -> Stage 1 (quarter res) -> Spatial 1.5x -> Spatial 1.5x -> Temporal 2x -> Stage 2 -> Decode
```
VRAM: 32GB+.

## Recommended Settings

### Stage 1

| Parameter | Distilled Model | Dev Model |
|-----------|----------------|-----------|
| Steps | 8 | 20-30 |
| CFG | 3.0-4.0 | 3.0-5.0 |
| Sampler | euler | euler |
| Scheduler | normal | normal |
| Denoise | 1.0 | 1.0 |

### Stage 2

| Parameter | Recommendation |
|-----------|---------------|
| Steps | 10-15 |
| CFG | 2.0-3.0 |
| Denoise | 0.3-0.5 |

Lower denoise in Stage 2 preserves motion and composition from Stage 1 while adding detail.

## VRAM Requirements

| Configuration | Target Resolution | Min VRAM | Recommended VRAM |
|---------------|------------------|----------|-----------------|
| Single-stage, half res | 480x720 | 12 GB (FP8) | 16 GB |
| Two-stage, 720p | 720x1280 | 16 GB (FP8) | 24 GB |
| Two-stage, 1080p | 1080x1920 | 24 GB | 32 GB |
| Full pipeline + temporal | 1080p @ 48fps | 32 GB | 48 GB |

See [[comfyui-ltx-performance-tips]] for VRAM optimization strategies.

## Practical Tips

1. **Start with distilled model** for rapid iteration. Switch to dev model for final render only.
2. **Test at low resolution first** (480x720) before committing to full two-stage pipeline.
3. **Save Stage 1 latents** to rerun Stage 2 with different settings without regenerating Stage 1.
4. **Spatial before temporal** -- always upscale resolution before frame rate.
5. **Close unused workflows** and clear cache between generations to maximize VRAM.
6. **Monitor VRAM** via nvidia-smi, especially during upscale step which creates large intermediate tensors.

## Required Model Files

| File | Location | Purpose |
|------|----------|---------|
| `ltx-2.3-22b-dev.safetensors` | `models/checkpoints/` | Full quality model |
| `ltx-2.3-22b-distilled.safetensors` | `models/checkpoints/` | Fast iteration model |
| `ltx-2.3-spatial-upscaler-x2-1.0.safetensors` | `models/latent_upscale_models/` | 2x spatial |
| `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors` | `models/latent_upscale_models/` | 1.5x spatial |
| `ltx-2.3-temporal-upscaler-x2-1.0.safetensors` | `models/latent_upscale_models/` | 2x frame rate |
| `ltx-2.3-22b-distilled-lora-384.safetensors` | `models/loras/` | Distilled LoRA |
| Gemma 3 12B IT | `models/text_encoders/` | Text encoder |

## Related Pages

- [[comfyui-ltx-integration-overview]] -- Integration overview
- [[comfyui-ltx-workflows]] -- Workflow types
- [[comfyui-ltx-node-reference]] -- Node reference (LTXVLatentUpsampler, Tiled VAE Decode)
- [[comfyui-ltx-performance-tips]] -- VRAM optimization
