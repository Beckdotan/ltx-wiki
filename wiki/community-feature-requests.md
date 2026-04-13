---
title: Community Feature Requests
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-feedback-feature-requests.md
  - raw/community-feedback-discussions.md
tags:
  - feature-requests
  - community
  - diffusers
  - controlnet
  - quantization
  - apple-silicon
---
# Community Feature Requests

The [[ltx-video-overview\|LTX Video]] community has expressed a consistent set of feature requests across HuggingFace discussions for all model versions. The most requested feature by far is Diffusers support for LTX-2.3.

## Most Requested Features

### 1. Diffusers Support for LTX-2.3
- **Status:** Pending PR (huggingface/diffusers#13217)
- **Impact:** Currently blocks Python-first users who do not use [[comfyui-integration\|ComfyUI]]
- Missing `model_index.json` files prevent loading
- Most discussed issue across all LTX-2.3 discussions

### 2. ControlNet Support
- Partially addressed by IC-LoRA controls (union, canny, depth, pose)
- Community wants traditional ControlNet interface, not just LoRA-based controls

### 3. Longer Video Generation
- Current limit: [[ltx-2-overview\|LTX-2]] supports up to 20 seconds
- Users want multi-minute continuous generation
- Temporal drift occurs beyond ~20 seconds

### 4. More Quantization Variants
- Requests for INT8, NVFP4 (especially for distilled models)
- Community interest in reducing VRAM requirements further

| Request | GPU Target | Expected Benefit |
|---------|-----------|-----------------|
| INT8 distilled | Mid-range GPUs | Smaller model with fast inference |
| NVFP4 distilled | Blackwell GPUs | 200-300% speed boost |
| GGUF for LTX-2/2.3 | All GPUs | ComfyUI-GGUF compatibility |
| FP8 distilled | All NVIDIA GPUs | Already available for dev, not distilled |

### 5. Apple Silicon / MLX Support
- Swift MLX support requested for native Apple Silicon performance
- Current workaround (Torch 2.4.1) is fragile and limiting
- Would enable professional creative workflows on Mac hardware

### 6. Performance Optimization (TeaCache)
- Community wants faster inference through caching and optimization techniques
- SageAttention 2 and Flash Attention 3 already adopted by community [[huggingface-spaces-ecosystem\|Spaces]]

### 7. Better Detailer for LTX-2.3
- LTX-2 detailer works suboptimally on LTX-2.3
- Community needs updated detailer specifically trained for 2.3

### 8. Audio Effects and Control
- Users want fine-grained audio control (e.g., adding specific sound effects)
- Audio generation control is less developed than video control

### 9. End Frame Control
- Ability to specify the last frame of generated video
- Partially addressed by linoyts' "First-Last Frame" community Spaces on [[huggingface-spaces-ecosystem\|HuggingFace]]

### 10. Multi-Speaker Audio Assignment
- Known limitation in LTX-2 paper
- Model may inconsistently assign speech to characters in multi-speaker scenes
- Community wants more reliable speaker-audio mapping

## Resolved Feature Requests

| Feature | When Requested | Resolution |
|---------|---------------|------------|
| FP8 model | LTX-Video #21 | Available for LTX-2 and 2.3 |
| Image-to-Video | Early discussions | Supported since 0.9.0 |
| LoRA support | LTX-Video #12 | Available since 0.9.5 |
| NVFP4 dev model | LTX-2 #9 | Available for LTX-2 and 2.3 |
| Spatial upscaler | LTX-Video #113 | Available for 0.9.8, LTX-2, LTX-2.3 |

## References

- [[community-feedback]]
- [[github-issues-known-limitations]] -- GitHub issue tracker and known bugs
- [[adoption-metrics]]
- [[huggingface-spaces-ecosystem]]
