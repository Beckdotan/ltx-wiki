# Community Project: GGUF Quantizations by city96

**Source:** https://huggingface.co/city96/LTX-Video-gguf

---

## Overview

city96 created GGUF (GPT-Generated Unified Format) quantizations of LTX-Video, making the model accessible on lower-VRAM hardware through the ComfyUI-GGUF custom node system.

## Model Details

- **Creator:** city96
- **Base Model:** Lightricks/LTX-Video (2B parameters)
- **Downloads (last month):** 3,272
- **Likes:** 24
- **Community Discussions:** 7

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

## Related GGUF Quantizations

- **city96/LTX-Video-0.9.6-dev-gguf** - 1,060 downloads, newer version
- **pollockjj/ltx-video-2b-v0.9.1-gguf** - 781 downloads, alternate version
- **konakona/ltxvideo_q8** - 59 downloads, Q8 variant
