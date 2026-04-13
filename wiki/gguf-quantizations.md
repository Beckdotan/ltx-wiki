---
title: GGUF Quantizations
type: community-project
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-gguf-quantizations.md
  - raw/community-project-community-models-finetunes.md
tags:
  - community
  - quantization
  - gguf
  - low-vram
  - comfyui
  - city96
---

# GGUF Quantizations

Community member **city96** created GGUF (GPT-Generated Unified Format) quantizations of [[ltx-video-overview|LTX Video]], making the model accessible on lower-VRAM hardware through the [[comfyui-integration|ComfyUI]]-GGUF custom node system.

## Key Details

- **Creator:** city96
- **Primary repository:** https://huggingface.co/city96/LTX-Video-gguf
- **Downloads:** 3,272/month
- **Likes:** 24

## Quantization Variants

| Bit-Depth | Type | File Size |
|-----------|------|-----------|
| 3-bit | Q3_K_S | 985 MB |
| 3-bit | Q3_K_M | 1.08 GB |
| 4-bit | Q4_K_S | 1.30 GB |
| 4-bit | Q4_0 | 1.26 GB |
| 4-bit | Q4_1 | 1.35 GB |
| 4-bit | Q4_K_M | 1.42 GB |
| 5-bit | Q5_K_S | 1.47 GB |
| 5-bit | Q5_0 | 1.50 GB |
| 5-bit | Q5_1 | 1.59 GB |
| 5-bit | Q5_K_M | 1.56 GB |
| 6-bit | Q6_K | 1.72 GB |
| 8-bit | Q8_0 | 2.17 GB |
| 16-bit | BF16/F16 | 3.94 GB |
| 32-bit | F32 | 7.69 GB |

## Usage

1. Install the [ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF) custom node
2. Place model files in `ComfyUI/models/unet`
3. Use standard ComfyUI LTX-Video workflows

## Significance

- Enables running LTX-Video on GPUs with as little as 3-4 GB VRAM
- The Q4_K_M variant is commonly used in community benchmarks
- Part of a broader ecosystem of GGUF quantizations for diffusion models

## Additional GGUF Variants

Other community members have also published GGUF quantizations:

- **city96/LTX-Video-0.9.6-dev-gguf** -- 1,060 downloads, updated for v0.9.6
- **pollockjj/ltx-video-2b-v0.9.1-gguf** -- 781 downloads, alternate v0.9.1 version
- **konakona/ltxvideo_q8** -- 59 downloads, Q8-specific variant

See [[community-models-finetunes]] for the full landscape of community quantizations and model variants.

## See Also

- [[community-models-finetunes]] -- Overview of all community models, quantizations, and finetunes
- [[render-benchmarks]] -- Performance benchmarks including quantized model comparisons
- [[ltx-video-overview]] -- LTX Video base model
