---
title: LTX-Video Distillation
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-096-distillation-milestone.md
  - raw/ltx-video-096-release.md
  - raw/ltx-video-distilled-models-and-optimization.md
  - raw/ltx-video-distilled-optimized-variants.md
tags:
  - ltx-video
  - distillation
  - optimization
  - knowledge-distillation
  - inference-speed
---
# LTX-Video Distillation

Distillation is the core optimization technique that enabled LTX-Video to achieve real-time video generation on consumer hardware. Introduced in [[ltx-video-096]], distilled variants replicate the behavior of the full "dev" model while being dramatically faster and lighter.

## How Distillation Works in LTX-Video

The distilled model is trained to reproduce the outputs of the dev (teacher) model while requiring far fewer computational resources at inference time. The exact methodology is not publicly documented, but evidence from the model ecosystem reveals the approach:

### Evidence from LoRA Adapters
A **LoRA128 adapter** (`ltxv-13b-0.9.7-distilled-lora128`, ~1.33 GB) exists that converts the dev model to behave like the distilled model. This implies the distillation transformation can be captured as a relatively compact weight delta, suggesting fine-tuning-based knowledge distillation rather than architectural changes.

### Three Compounding Speed Gains

1. **Reduced inference steps:** Distilled models use ~8 steps vs 30-50 for dev models
2. **Elimination of [[classifier-free-guidance]]:** Dev models require CFG (and often STG), which roughly doubles computation per step. Distilled models internalize guidance, requiring only a single forward pass per step
3. **Knowledge compression:** The distillation process captures the teacher model's behavior in a form optimized for fewer steps

Together, these produce a **15x speedup** over the dev model.

## Distilled Model Characteristics

All LTX-Video distilled variants share these properties:

- **15x faster inference** than corresponding dev models
- **No CFG or STG required** -- single forward pass per step
- **8 diffusion steps recommended** (can use fewer)
- **Slight quality reduction** compared to dev -- optimized for rapid iteration
- **Real-time capable** on H100 GPUs
- **Lower VRAM** than dev counterparts

## Distilled Models by Version

| Version | Model | Params | VRAM | Steps |
|---------|-------|--------|------|-------|
| [[ltx-video-096]] | ltxv-2b-0.9.6-distilled | 2B | ~8 GB | 8 |
| v0.9.7 | ltxv-13b-0.9.7-distilled | 13B | ~24 GB | 7 |
| v0.9.8 | ltxv-13b-0.9.8-distilled | 13B | ~24 GB | 7 |
| v0.9.8 | ltxv-2b-0.9.8-distilled | 2B | ~8 GB | ~7 |

The [[ltx-video-096]] release was the first to include a distilled variant, proving the concept. Every subsequent version has shipped with both dev and distilled variants as standard (see [[ltx-video-dev-vs-distilled]]).

## Combining Distillation with Other Optimizations

Distillation can be stacked with other optimization techniques for further efficiency:

- **[[fp8-quantization]]:** Reduces memory by ~45% on top of distillation. The `ltxv-2b-0.9.8-distilled-fp8` model needs only ~6 GB VRAM and achieves ~20x+ speed over the 2B dev baseline.
- **GGUF quantization:** Community-created quantized versions (Q4, Q5, Q8) provide further VRAM reduction beyond FP8.
- **Mix mode:** Combines distilled (fast first pass) + dev (quality refinement pass) in a multi-scale rendering workflow.

## Practical Impact

Distillation transformed LTX-Video's accessibility:

- **Before distillation:** Required ~12 GB VRAM, ~50 inference steps, with guidance overhead
- **After distillation:** Accessible on consumer GPUs with ~8 GB VRAM (2B) or ~16 GB (13B FP8)
- **Interactive use cases** became possible due to real-time generation speeds
- **The dev/distilled pattern** became the standard release strategy for all subsequent versions

## References

- See [[ltx-video-096]] for the first distilled release
- See [[ltx-video-dev-vs-distilled]] for a detailed comparison
- See [[fp8-quantization]] for complementary optimization
