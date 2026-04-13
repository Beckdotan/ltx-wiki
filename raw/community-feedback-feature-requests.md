# Community Feature Requests and Wishlists

**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions, https://huggingface.co/Lightricks/LTX-2/discussions, https://huggingface.co/Lightricks/LTX-2.3/discussions

---

## Most Requested Features

### 1. Diffusers Support for LTX-2.3
- **Discussions:** LTX-2.3 #4, #20, #27, #31
- **Status:** Pending PR (https://github.com/huggingface/diffusers/pull/13217)
- **Impact:** Currently blocks Python-first users who don't use ComfyUI
- Missing `model_index.json` files prevent loading
- Most discussed issue across all LTX-2.3 discussions

### 2. ControlNet Support
- **Discussion:** LTX-Video #81 ("Give us Controlnet, Pretty Please")
- **Status:** Partially addressed by IC-LoRA controls (union, canny, depth, pose)
- Community wants traditional ControlNet interface, not just LoRA-based controls

### 3. Longer Video Generation
- **Discussion:** LTX-Video #25
- **Current limit:** LTX-2 supports up to 20 seconds
- Users want multi-minute continuous generation
- Temporal drift occurs beyond ~20 seconds

### 4. More Quantization Variants
- **Discussions:** LTX-Video #21, #47; LTX-2 #9, #23
- Requests for INT8, NVFP4 (especially for distilled models)
- NVFP4 distilled not yet supported (Discussion #9 on LTX-2)
- Community interest in reducing VRAM requirements further

### 5. Apple Silicon / MLX Support
- **Discussion:** LTX-2.3 #22
- Swift MLX support requested for native Apple Silicon performance
- Current workaround (Torch 2.4.1) is fragile and limiting
- Would enable professional creative workflows on Mac hardware

### 6. Performance Optimization
- **Discussion:** LTX-Video #87 (TeaCache request)
- Community wants faster inference through caching and optimization techniques
- SageAttention 2 and Flash Attention 3 already adopted by community Spaces

### 7. Better Detailer for LTX-2.3
- **Discussion:** LTX-2.3 #11
- LTX-2 detailer works suboptimally on LTX-2.3
- Community needs updated detailer specifically trained for 2.3

### 8. Audio Effects and Control
- **Discussion:** LTX-2.3 #38
- Users want fine-grained audio control (e.g., adding beep sound effects for censored words)
- Audio generation control is less developed than video control

### 9. End Frame Control
- **Discussion:** LTX-Video #37
- Ability to specify the last frame of generated video
- Partially addressed by linoyts' "First-Last Frame" community Spaces

### 10. Multi-Speaker Audio Assignment
- Known limitation in LTX-2 paper
- Model may inconsistently assign speech to characters in multi-speaker scenes
- Community wants more reliable speaker-audio mapping

## Quantization Feature Request Detail

| Request | GPU Target | Expected Benefit |
|---------|-----------|-----------------|
| INT8 distilled | Mid-range GPUs | Smaller model with fast inference |
| NVFP4 distilled | Blackwell GPUs | 200-300% speed boost |
| GGUF for LTX-2/2.3 | All GPUs | ComfyUI-GGUF compatibility |
| FP8 distilled | All NVIDIA GPUs | Already available for dev, not distilled |

## Resolved Feature Requests

| Feature | Requested | Resolved |
|---------|-----------|----------|
| FP8 model | LTX-Video #21 | Available for LTX-2 and 2.3 |
| Image-to-Video | Early discussions | Supported since 0.9.0 |
| LoRA support | LTX-Video #12 | Available since 0.9.5 |
| NVFP4 dev model | LTX-2 #9 | Available for LTX-2 and 2.3 |
| Spatial upscaler | LTX-Video #113 | Available for 0.9.8, LTX-2, LTX-2.3 |
