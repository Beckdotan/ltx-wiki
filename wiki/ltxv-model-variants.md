---
title: LTX Video Model Variants
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/Lightricks/LTX-Video
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
  - https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
tags:
  - ltx-video
  - model-variants
  - distilled
  - fp8
  - quantization
  - model-selection
---

# LTX Video Model Variants

Starting with [[ltx-video-097|v0.9.7]], LTX Video established a consistent release pattern of multiple model variants to serve different hardware and workflow needs. This pattern continued in [[ltx-video-098|v0.9.8]] with the addition of 2B distilled models.

## Variant Types

### Dev (Full Quality)

The dev variant is the highest-quality model, intended for production use where quality is the priority.

- 20-30 inference steps (default 30)
- Requires classifier-free guidance (CFG) and spatio-temporal guidance (STG)
- Highest VRAM requirements
- `decode_timestep`: 0.05
- `image_cond_noise_scale`: 0.025

### Distilled (Fast Inference)

The distilled variant trades a small amount of quality for dramatically faster inference.

- 7-8 inference steps (vs 20-30 for dev)
- **No CFG or STG required** -- a significant simplification
- Created via knowledge distillation from the dev model
- Approximately 15x faster than dev
- The `distilled-lora128` adapter (rank 128, ~1.33 GB) converts the dev model to behave like the distilled version, enabling fine-tuning workflows

### FP8 (Quantized)

FP8 variants reduce model precision from bfloat16 to float8 for significant VRAM savings.

- Roughly halves VRAM requirements
- Roughly halves file size (~15.7 GB vs ~28.6 GB for 13B)
- Available for both dev and distilled variants
- Maintains good quality despite lower precision

### Mix (Multi-Scale)

The mix variant implements the [[multiscale-rendering]] workflow by combining dev and distilled models.

- Uses the distilled model for initial low-resolution pass
- Uses the dev model for high-resolution refinement
- Balances speed and quality
- Variable VRAM and step requirements depending on configuration

## Complete Variant Matrix

### v0.9.7 (13B Only)

| Variant | Steps | VRAM | File Size |
|---------|-------|------|-----------|
| `ltxv-13b-0.9.7-dev` | 20-30 | 40GB+ | ~28.6 GB |
| `ltxv-13b-0.9.7-distilled` | 7-8 | 24GB+ | ~28.6 GB |
| `ltxv-13b-0.9.7-fp8` | 20-30 | 16GB+ | ~15.7 GB |
| `ltxv-13b-0.9.7-distilled-fp8` | 7-8 | ~16GB | ~15.7 GB |
| `ltxv-13b-0.9.7-mix` | varies | varies | N/A |
| `ltxv-13b-0.9.7-distilled-lora128` | 7-8 | ~1GB adapter | ~1.33 GB |

### v0.9.8 (13B + 2B)

| Variant | Steps | VRAM | File Size |
|---------|-------|------|-----------|
| `ltxv-13b-0.9.8-dev` | 30 | ~24+ GB | ~28.6 GB |
| `ltxv-13b-0.9.8-distilled` | 7-8 | ~16-20 GB | ~28.6 GB |
| `ltxv-13b-0.9.8-fp8` | 30 | ~12-16 GB | ~15.7 GB |
| `ltxv-13b-0.9.8-distilled-fp8` | 7-8 | varies | ~15.7 GB |
| `ltxv-13b-0.9.8-mix` | varies | varies | N/A |
| `ltxv-2b-0.9.8-distilled` | few | ~8-12 GB | ~6.34 GB |
| `ltxv-2b-0.9.8-distilled-fp8` | few | ~6-8 GB | ~4.46 GB |

## GPU Recommendations

| Variant Class | Recommended GPUs |
|---------------|-----------------|
| 13B dev (bfloat16) | A100 80GB, H100 |
| 13B distilled (bfloat16) | A100 40GB, RTX 4090 |
| 13B FP8 (any) | RTX 4090, A100 40GB |
| 2B distilled | RTX 3090, RTX 4080 |
| 2B distilled-fp8 | RTX 3080, RTX 4070 |

## Configuration Files

Each variant has a corresponding YAML config file:

- `configs/ltxv-13b-0.9.7-dev.yaml`
- `configs/ltxv-13b-0.9.7-distilled.yaml`
- `configs/ltxv-13b-0.9.8-dev.yaml`
- `configs/ltxv-13b-0.9.8-distilled.yaml`
- `configs/ltxv-13b-0.9.8-mix.yaml`
- `configs/ltxv-2b-0.9.8-distilled.yaml`

## Choosing a Variant

- **Maximum quality:** Use the dev variant with full inference steps
- **Fast iteration:** Use the distilled variant (7-8 steps, no CFG/STG)
- **Limited VRAM:** Use FP8 variants or the 2B distilled model
- **Balanced workflow:** Use the mix variant for multiscale rendering
- **Fine-tuning:** Use the distilled-lora128 adapter on top of the dev model

## HuggingFace Model Identifiers

| Model | HuggingFace ID |
|-------|----------------|
| Base 2B | `Lightricks/LTX-Video` |
| 2B with multi-condition | `Lightricks/LTX-Video-0.9.5` |
| 2B 0.9.8 distilled | `Lightricks/ltxv-2b-0.9.8-distilled` |
| 13B 0.9.7 dev | `Lightricks/LTX-Video-0.9.7-dev` |
| 13B 0.9.7 distilled | `Lightricks/LTX-Video-0.9.7-distilled` |
| 13B 0.9.8 dev | `Lightricks/LTX-Video-0.9.8-dev` |
| 13B 0.9.8 distilled | `Lightricks/LTX-Video-0.9.8-13B-distilled` |
| Spatial Upscaler 0.9.7 | `Lightricks/ltxv-spatial-upscaler-0.9.7` |
| Spatial Upscaler 0.9.8 | `Lightricks/ltxv-spatial-upscaler-0.9.8` |
| GGUF Quantized | `city96/LTX-Video-gguf` |

## Auxiliary Models

| Model | HuggingFace ID | Purpose |
|-------|----------------|---------|
| Spatial Upscaler 0.9.7 | `Lightricks/ltxv-spatial-upscaler-0.9.7` | 2x latent upscaling via [[ltx-latent-upsample-pipeline]] |
| Spatial Upscaler 0.9.8 | `Lightricks/ltxv-spatial-upscaler-0.9.8` | 2x latent upscaling |
| Latent Upsampler 0.9.8 | `a-r-r-o-w/LTX-0.9.8-Latent-Upsampler` | Alternative upsampler |
| Cakeify LoRA | `Lightricks/LTX-Video-Cakeify-LoRA` | Object-to-cake LoRA (trigger: "CAKEIFY") |

## Generation Modes by Model

| Mode | Supported Models | Pipeline |
|------|-----------------|----------|
| Text-to-video | All | [[ltx-pipeline-text-to-video\|LTXPipeline]] or [[ltx-condition-pipeline\|LTXConditionPipeline]] |
| Image-to-video | All | [[ltx-image-to-video-pipeline\|LTXImageToVideoPipeline]] or [[ltx-condition-pipeline\|LTXConditionPipeline]] |
| Video-to-video | v0.9.5+ | [[ltx-condition-pipeline\|LTXConditionPipeline]] |
| Multi-condition | v0.9.5+ | [[ltx-condition-pipeline\|LTXConditionPipeline]] |
| Long video + multi-prompt | v0.9.8 distilled | [[ltx-long-multi-prompt-pipeline\|LTXI2VLongMultiPromptPipeline]] |

## Technical Specifications

- **Resolution:** Divisible by 32; best at 480x704, 512x768, or 576x1024
- **Frame count:** 8n+1 pattern (9, 17, 25, ..., 161, 257, 361)
- **Frame rate:** 24-30 FPS
- **Compression:** [[ltxv-video-vae|Video-VAE]] with 1:192 pixel-to-latent ratio
- **Text encoder:** T5 v1.1 XXL (English prompts only)
- **Dtype:** `torch.bfloat16` recommended for all components

## Limitations

- English prompts only; elaborate descriptions produce best results
- Not designed for factual information generation
- May amplify societal biases present in training data

## See Also

- [[ltxv-13b]] -- architecture details for the 13B model
- [[ltx-video-097]] -- the release establishing this variant pattern
- [[ltx-video-098]] -- the release adding 2B variants
- [[multiscale-rendering]] -- the technique behind the mix variant
- [[python-installation-setup]] -- how to download and install models
- [[diffusers-pipeline-overview]] -- pipeline class reference
- [[ltxv-memory-optimization]] -- reducing VRAM requirements
