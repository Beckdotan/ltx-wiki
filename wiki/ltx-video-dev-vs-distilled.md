---
title: LTX-Video Dev vs Distilled Comparison
type: comparison
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-096-distillation-milestone.md
  - raw/ltx-video-096-release.md
  - raw/ltx-video-distilled-models-and-optimization.md
  - raw/ltx-video-distilled-optimized-variants.md
tags:
  - ltx-video
  - comparison
  - dev-model
  - distilled-model
  - inference
---
# LTX-Video Dev vs Distilled Comparison

Starting with [[ltx-video-096]], every LTX-Video release ships with two model variants: **dev** (full-quality) and **distilled** (speed-optimized). This page compares the two approaches.

## Head-to-Head Comparison

| Property | Dev Model | Distilled Model |
|----------|-----------|-----------------|
| Inference steps | 20-50 | 8 (or fewer) |
| [[classifier-free-guidance]] (CFG) | Required | Not required |
| Spatio-temporal guidance (STG) | Required | Not required |
| Forward passes per step | 2 (with guidance) | 1 |
| Quality | Highest available | Slightly reduced |
| Relative speed | 1x (baseline) | ~15x faster |
| VRAM (2B, BF16) | ~12 GB | ~8 GB |
| VRAM (13B, BF16) | ~40 GB+ | ~24 GB+ |
| Best use case | Final production outputs | Rapid iteration, previews |
| Real-time capable | No (at full quality settings) | Yes (on H100) |

## Why the Speed Difference Is So Large

The 15x speedup is not from a single optimization but from three compounding factors:

1. **Step reduction (~4-6x):** Going from 30-50 steps to 8 steps
2. **Guidance elimination (~2x):** Removing the need for dual forward passes (CFG/STG)
3. **Internalized guidance:** The [[ltx-video-distillation]] process teaches the model to produce guided-quality outputs without explicit guidance signals

Rough calculation: 4x fewer steps * 2x from no guidance * some efficiency gains = ~15x total speedup.

## When to Use Each

### Use the Dev Model When:
- Generating final, production-quality video
- Quality is the top priority
- Sufficient compute is available (12+ GB VRAM for 2B, 40+ GB for 13B)
- Willing to wait longer for results

### Use the Distilled Model When:
- Iterating on prompts and compositions
- Real-time or near-real-time generation is needed
- Running on consumer-grade GPUs with limited VRAM
- Building interactive applications
- Previewing before committing to a full-quality render

## Mix Mode: Best of Both

The "mix" variant (available from v0.9.7) combines both models in a multi-scale workflow:

1. **First pass:** Distilled model generates at lower resolution (fast)
2. **Upscale:** Spatial upscaler increases resolution
3. **Second pass:** Dev model refines at higher resolution (quality)

This provides a balanced speed vs. quality tradeoff suitable for production pipelines.

## Efficiency Across the Full Model Range

| Model | Params | Precision | Steps | Relative Speed | VRAM |
|-------|--------|-----------|-------|----------------|------|
| 2B dev ([[ltx-video-096]]) | 2B | BF16 | 20-50 | 1x (baseline) | ~12 GB |
| 2B distilled ([[ltx-video-096]]) | 2B | BF16 | ~8 | ~15x | ~8 GB |
| 13B dev (v0.9.7) | 13B | BF16 | 20-30 | ~0.3x | ~40 GB |
| 13B distilled (v0.9.7) | 13B | BF16 | 7 | ~1x | ~24 GB |
| 13B [[fp8-quantization|FP8]] (v0.9.7) | 13B | FP8 | 20-30 | ~0.5x | ~16 GB |
| 13B distilled-FP8 (v0.9.7) | 13B | FP8 | 7 | ~2x | ~16 GB |
| 2B distilled-FP8 (v0.9.8) | 2B | FP8 | ~7 | ~20x+ | ~6 GB |

Note: Speed values are rough estimates based on model characteristics, not official benchmarks.

## LoRA Bridge Between Variants

The `ltxv-13b-0.9.7-distilled-lora128` adapter (~1.33 GB, rank 128) can convert the dev model into distilled-like behavior. This is useful for:
- Fine-tuning starting from dev weights while preserving [[ltx-video-distillation]] benefits
- Exploring the boundary between dev and distilled quality/speed tradeoffs

## References

- See [[ltx-video-distillation]] for the underlying technique
- See [[ltx-video-096]] for the first version with both variants
