# LTX Video: Complete Version History and Model Inventory

**Source:** https://huggingface.co/Lightricks/LTX-Video (main model page)
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev (model card)

---

## Version Timeline

### v0.9 (Initial Release, Late 2024)
- **Size:** 2B parameters
- **Type:** DiT-based video generation model
- **Capabilities:** Text-to-video, image-to-video
- **Resolution:** 768x512 at 24 fps
- **Key claim:** First DiT-based model capable of real-time video generation
- **Paper:** arxiv.org/abs/2501.00103

### v0.9.1 (Early 2025)
- **Size:** 2B parameters
- **Improvements over 0.9:** Refinements to quality and stability (incremental update)
- **Hugging Face:** `Lightricks/LTX-Video` (original repo, initially pointed to 0.9.1)
- **Capabilities:** Text-to-video and image-to-video
- **Diffusers integration:** `LTXPipeline` and `LTXImageToVideoPipeline`
- **Inference steps:** ~50 steps recommended

### v0.9.5
- **Size:** 2B parameters
- **Hugging Face:** `Lightricks/LTX-Video-0.9.5`
- **Incremental improvements** over 0.9.1
- **Still used** `LTXPipeline` in diffusers

### v0.9.6
- **Introduced two variants:**
  - `ltxv-2b-0.9.6-dev` - 2B parameters, good quality, lower VRAM
  - `ltxv-2b-0.9.6-distilled` - 2B parameters, 15x faster, real-time capable, no STG/CFG required
- **Key milestone:** First distilled variant released
- **Hugging Face:** `Lightricks/LTX-Video-0.9.6-dev`, `Lightricks/LTX-Video-0.9.6-distilled`

### v0.9.7
- **Major upgrade:** Introduced 13B parameter model
- **Variants released:**
  - `ltxv-13b-0.9.7-dev` - 13B, highest quality
  - `ltxv-13b-0.9.7-distilled` - 13B, faster with slight quality reduction (7 inference steps)
  - `ltxv-13b-0.9.7-fp8` - 13B, FP8 quantized
  - `ltxv-13b-0.9.7-distilled-fp8` - 13B, distilled + FP8 quantized
  - `ltxv-13b-0.9.7-distilled-lora128` - LoRA adapter (rank 128) to convert dev model to distilled behavior
  - `ltxv-13b-0.9.7-mix` - Combines dev and distilled in multi-scale workflow
- **Specialized variants:**
  - `ltxv-13b-0.9.7-ICLoRA-depth` - Depth conditioning via ICLoRA
  - `ltxv-13b-0.9.7-ICLoRA-pose` - Pose conditioning via ICLoRA
  - `ltxv-13b-0.9.7-ICLoRA-canny` - Canny edge conditioning via ICLoRA
  - `ltxv-13b-0.9.7-ICLoRA-detailer` - Enhanced detail generation
- **Upscalers:**
  - `ltxv-spatial-upscaler-0.9.7` - 2x spatial upscaling of latent video
  - `ltxv-temporal-upscaler-0.9.7` - Frame interpolation / temporal upscaling
- **New diffusers API:** `LTXConditionPipeline` and `LTXLatentUpsamplePipeline`
- **Key features:** Multi-condition generation, LoRA fine-tuning support

### v0.9.8 (Latest as of early 2025)
- **Variants released:**
  - `ltxv-13b-0.9.8-dev` - 13B, highest quality
  - `ltxv-13b-0.9.8-mix` - 13B, balanced speed-quality with multi-scale rendering
  - `ltxv-13b-0.9.8-distilled` - 13B, faster, slight quality reduction
  - `ltxv-2b-0.9.8-distilled` - 2B, lightweight distilled variant
  - `ltxv-13b-0.9.8-fp8` - 13B, FP8 quantized
  - `ltxv-13b-0.9.8-distilled-fp8` - 13B, distilled + FP8
  - `ltxv-2b-0.9.8-distilled-fp8` - 2B, most efficient variant
- **ICLoRA variants also updated to 0.9.8**
- **Spatial and temporal upscalers also updated**
- **Note:** Many 0.9.8 model pages are gated (require authentication)

## Complete Model Inventory

| Model ID | Version | Size | Type | Key Feature |
|----------|---------|------|------|-------------|
| ltxv-2b-0.9.6-dev | 0.9.6 | 2B | Dev | Good quality, low VRAM |
| ltxv-2b-0.9.6-distilled | 0.9.6 | 2B | Distilled | 15x faster, real-time |
| ltxv-13b-0.9.7-dev | 0.9.7 | 13B | Dev | Highest quality |
| ltxv-13b-0.9.7-distilled | 0.9.7 | 13B | Distilled | Fast, 7 steps |
| ltxv-13b-0.9.7-fp8 | 0.9.7 | 13B | Quantized | Memory efficient |
| ltxv-13b-0.9.7-distilled-fp8 | 0.9.7 | 13B | Distilled+FP8 | Fast + memory efficient |
| ltxv-13b-0.9.7-mix | 0.9.7 | 13B | Mix | Multi-scale workflow |
| ltxv-13b-0.9.7-distilled-lora128 | 0.9.7 | LoRA | Adapter | Dev-to-distilled conversion |
| ltxv-13b-0.9.7-ICLoRA-depth | 0.9.7 | LoRA | Control | Depth map conditioning |
| ltxv-13b-0.9.7-ICLoRA-pose | 0.9.7 | LoRA | Control | Pose conditioning |
| ltxv-13b-0.9.7-ICLoRA-canny | 0.9.7 | LoRA | Control | Canny edge conditioning |
| ltxv-13b-0.9.7-ICLoRA-detailer | 0.9.7 | LoRA | Control | Enhanced details |
| ltxv-spatial-upscaler-0.9.7 | 0.9.7 | - | Upscaler | 2x spatial upscale |
| ltxv-temporal-upscaler-0.9.7 | 0.9.7 | - | Upscaler | Frame interpolation |
| ltxv-13b-0.9.8-dev | 0.9.8 | 13B | Dev | Latest, highest quality |
| ltxv-13b-0.9.8-mix | 0.9.8 | 13B | Mix | Balanced speed-quality |
| ltxv-13b-0.9.8-distilled | 0.9.8 | 13B | Distilled | Latest fast variant |
| ltxv-2b-0.9.8-distilled | 0.9.8 | 2B | Distilled | Lightweight |
| ltxv-13b-0.9.8-fp8 | 0.9.8 | 13B | Quantized | Better memory efficiency |
| ltxv-13b-0.9.8-distilled-fp8 | 0.9.8 | 13B | Distilled+FP8 | Combined optimizations |
| ltxv-2b-0.9.8-distilled-fp8 | 0.9.8 | 2B | Distilled+FP8 | Most efficient overall |

## Key Evolution Summary

1. **0.9 -> 0.9.1 -> 0.9.5:** Incremental quality improvements on the 2B model
2. **0.9.6:** Introduction of distilled variants (15x faster, no CFG/STG needed)
3. **0.9.7:** Major jump to 13B parameters, FP8 quantization, ICLoRA control adapters, spatial/temporal upscalers, mix mode
4. **0.9.8:** Latest iteration with both 2B and 13B distilled variants, continued refinement
