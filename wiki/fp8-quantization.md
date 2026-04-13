---
title: FP8 Quantization for LTX-Video
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-distilled-models-and-optimization.md
  - raw/ltx-video-distilled-optimized-variants.md
tags:
  - ltx-video
  - fp8
  - quantization
  - optimization
  - vram
---
# FP8 Quantization for LTX-Video

FP8 (8-bit floating point) quantization reduces LTX-Video model weights from bfloat16 (16-bit) to 8-bit floating point, approximately halving memory requirements with minimal quality loss. Available from v0.9.7 onward, FP8 complements [[ltx-video-distillation]] as a key optimization strategy.

## Benefits

- **File size reduction:** ~45% smaller (e.g., 28.6 GB to 15.7 GB for 13B models)
- **VRAM reduction:** Proportional to file size reduction
- **Quality preservation:** Minimal quality loss in most cases
- **Speed:** Similar or slightly faster than bfloat16

## Available FP8 Models

| Model | File Size | VRAM (est.) |
|-------|-----------|-------------|
| ltxv-13b-0.9.8-dev-fp8 | ~15.7 GB | ~16-20 GB |
| ltxv-13b-0.9.8-distilled-fp8 | ~15.7 GB | ~16 GB |
| ltxv-13b-0.9.7-dev-fp8 | ~15.7 GB | ~16-20 GB |
| ltxv-13b-0.9.7-distilled-fp8 | ~15.7 GB | ~16 GB |
| ltxv-2b-0.9.8-distilled-fp8 | ~4.46 GB | ~6 GB |

## VRAM Comparison: BF16 vs FP8

| Model | BF16 VRAM | FP8 VRAM |
|-------|-----------|----------|
| 13B dev | ~40 GB+ | ~16-20 GB |
| 13B distilled | ~24 GB+ | ~16 GB |
| 2B distilled | ~8-12 GB | ~6-8 GB |

The combination of [[ltx-video-distillation]] + FP8 is particularly powerful: the `ltxv-2b-0.9.8-distilled-fp8` model achieves an estimated ~20x+ speedup over the 2B dev baseline while requiring only ~6 GB VRAM.

## Stacking with Other Optimizations

FP8 is one layer in a hierarchy of optimization strategies available for LTX-Video:

| Strategy | VRAM Savings | Speed Impact | Quality Impact |
|----------|-------------|--------------|----------------|
| [[ltx-video-distillation]] | Moderate | 15x faster | Slight reduction |
| FP8 quantization | ~45% | Similar/faster | Minimal |
| GGUF (Q8) | ~50% | Depends | Minimal |
| GGUF (Q4) | ~75% | Depends | Noticeable |
| NVFP4 | ~60% | Up to 3x | Moderate |
| LoRA adapter | ~95% (weights only) | Same as distilled | Same as distilled |

## Community Quantization Alternatives

### GGUF (city96, unsloth)
- Multiple quantization levels (Q4, Q5, Q8) for further VRAM reduction beyond FP8
- Optimized for ComfyUI workflows via ComfyUI-GGUF custom node
- The unsloth Dynamic 2.0 methodology upcasts important layers to preserve quality

### NVFP4
- Community-created 4-bit quantization for NVIDIA Blackwell GPUs
- Up to 60% VRAM reduction, up to 3x speed improvement
- More aggressive quality tradeoff than FP8

## Relationship to Early Versions

FP8 quantization was not available during the [[ltx-video-early-versions-overview|early version era]] (0.9.0-0.9.6). The [[ltx-video-096]] distilled models were the primary optimization path for the 2B architecture. FP8 became significant starting with the 13B models in v0.9.7, where memory savings were essential to making the larger models practical on consumer hardware.

## References

- [Hugging Face: Lightricks/LTX-Video](https://huggingface.co/Lightricks/LTX-Video)
- [city96/LTX-Video-gguf](https://huggingface.co/city96/LTX-Video-gguf)
- [unsloth/LTX-2-GGUF](https://huggingface.co/unsloth/LTX-2-GGUF)
