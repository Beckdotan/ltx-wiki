---
title: LTX Video 0.9.7
type: release
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
  - https://huggingface.co/Lightricks/LTX-Video
  - https://www.prnewswire.com/news-releases/lightricks-launches-13b-parameters-ltx-video-model-breakthrough-rendering-approach-generates-high-quality-efficient-ai-video-30x-faster-than-comparable-models-302447660.html
tags:
  - ltx-video
  - release
  - 0.9.7
  - 13b
---

# LTX Video 0.9.7

**Release date:** May 6, 2025

Version 0.9.7 is the most significant release in the LTX Video line, scaling from 2B to [[ltxv-13b|13 billion parameters]] and introducing a rich ecosystem of model variants, upscalers, and control adapters.

## Key Introductions

- **[[ltxv-13b]] model** -- a 6.5x parameter increase from the 2B models in v0.9.6
- **[[multiscale-rendering]]** -- a layered generation process enabling 30x faster rendering than comparable models
- **[[ltxv-model-variants|Multiple model variants]]** -- dev, distilled, FP8, and mix configurations
- **[[ic-lora|IC-LoRA adapters]]** -- controlled generation via depth, pose, canny, and detailer conditioning
- **[[ltxv-spatial-upscaler]]** -- 2x latent-space spatial upscaling
- **Temporal upscaler** -- frame interpolation for smoother or longer video
- **New Diffusers API** -- `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`, and `LTXVideoCondition`

## Model Variants Released

| Variant | Description | Inference Steps | VRAM |
|---------|-------------|----------------|------|
| `ltxv-13b-0.9.7-dev` | Full quality, highest fidelity | 20-30 | 40GB+ |
| `ltxv-13b-0.9.7-distilled` | Faster, reduced quality | 7-8 | 24GB+ |
| `ltxv-13b-0.9.7-fp8` | FP8 quantized dev | ~20-30 | 16GB+ |
| `ltxv-13b-0.9.7-distilled-fp8` | Distilled + quantized | 7-8 | ~16GB |
| `ltxv-13b-0.9.7-mix` | Multi-scale dev+distilled workflow | varies | varies |
| `ltxv-13b-0.9.7-distilled-lora128` | Rank 128 LoRA adapter | 7-8 | ~1GB adapter |

## Output Specifications

- **Resolution:** 1216x704 native, best under 720x1280
- **Frame rate:** 30 FPS
- **Maximum frames:** 257 (8n+1 pattern)
- **Duration:** Up to 60 seconds

## Capabilities

- Text-to-video generation
- Image-to-video generation
- Video-to-video generation
- Multi-keyframe conditioning
- LoRA fine-tuning support
- FP8 quantization for reduced VRAM

## Hardware Requirements

- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2+

## Licensing

Free for enterprises with under $10 million annual revenue (LTX-Video Open-Weights License).

## Hugging Face Repositories

- **Dev:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
- **Distilled:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
- **Spatial Upscaler:** https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.7

## Online Deployment

- LTX Studio (image-to-video)
- Fal.ai (text-to-video and image-to-video)
- Replicate (both modalities)

## See Also

- [[ltx-video-098]] -- the successor release
- [[ltxv-13b]] -- architecture details on the 13B model
- [[multiscale-rendering]] -- the rendering approach that enables 30x speedup
- [[ltxv-model-variants]] -- detailed breakdown of all variants
