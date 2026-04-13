# LTX-Video Distilled and Optimized Variants

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
**Source:** https://huggingface.co/city96/LTX-Video-gguf
**Source:** https://huggingface.co/unsloth/LTX-2-GGUF
**Source:** https://civitai.com/models/982559/lightricks-ltxv

## Distillation Approach

LTX-Video distilled models are trained to replicate the behavior of the full "dev" model while being faster and lighter. Starting with v0.9.6, every version ships with both a dev (full-quality) and distilled variant.

### Key Properties of Distilled Models
- **15x faster inference** than non-distilled dev models
- **No classifier-free guidance (CFG) required** -- eliminates the need for dual forward passes
- **No spatio-temporal guidance (STG) required** -- further reducing computation
- **8 diffusion steps recommended** (vs 30+ for dev models), can use fewer
- **Slight quality reduction** compared to dev models -- optimized for rapid iteration
- **Real-time capable** on H100 GPUs

### Dev vs Distilled Comparison

| Property | Dev Model | Distilled Model |
|----------|-----------|-----------------|
| Inference steps | 30 | 8 (or fewer) |
| CFG required | Yes | No |
| STG required | Yes | No |
| Quality | Highest | Slightly reduced |
| Speed | 1x (baseline) | ~15x faster |
| VRAM | Higher | Lower |
| Use case | Final outputs | Rapid iteration |

## FP8 Quantized Variants

FP8 (8-bit floating point) quantization provides significant memory and storage savings:

### FP8 Benefits
- **File size reduction:** ~45% smaller (28.6 GB -> 15.7 GB for 13B)
- **VRAM reduction:** Proportional to file size reduction
- **Quality preservation:** Minimal quality loss in most cases
- **Speed:** Similar or slightly faster than bf16

### Available FP8 Models
| Model | File Size |
|-------|-----------|
| ltxv-13b-0.9.8-dev-fp8 | ~15.7 GB |
| ltxv-13b-0.9.8-distilled-fp8 | ~15.7 GB |
| ltxv-13b-0.9.7-dev-fp8 | ~15.7 GB |
| ltxv-13b-0.9.7-distilled-fp8 | ~15.7 GB |
| ltxv-2b-0.9.8-distilled-fp8 | ~4.46 GB |

## LoRA Variant

### ltxv-13b-0.9.7-distilled-lora128
- **File size:** ~1.33 GB (vs 28.6 GB for full model)
- **Rank:** 128 (significantly higher than standard LoRA rank 32)
- **Type:** Distilled model weights as LoRA adapter
- **VRAM requirement:** ~1 GB additional on top of base model
- **Use case:** Apply distilled inference behavior to dev base model

## GGUF Quantized Variants (Community)

### city96/LTX-Video-gguf
- **Source:** https://huggingface.co/city96/LTX-Video-gguf
- **Format:** GGUF (optimized for inference)
- **Usage:** ComfyUI via ComfyUI-GGUF custom node
- **Placement:** ComfyUI/models/unet/
- **Quantization levels:** Multiple (Q4, Q5, Q8, etc.)

### unsloth/LTX-2-GGUF and unsloth/LTX-2.3-GGUF
- **Source:** https://huggingface.co/unsloth/LTX-2-GGUF
- **Method:** Unsloth Dynamic 2.0 quantization methodology
- **Key feature:** Important layers upcasted to higher precision to maintain quality
- **Format:** GGUF
- **Designed for:** ComfyUI integration

### GGUF Benefits
- Further VRAM reduction beyond FP8
- Multiple quantization levels to trade quality for memory
- Enables running on consumer GPUs with 8-12 GB VRAM
- Optimized for ComfyUI workflows

## NVFP4 Quantization (Advanced)

- Created by community member Sikaworld1990
- Optimized specifically for NVIDIA Blackwell GPUs
- Even more aggressive quantization than FP8
- NVFP4 and FP8 formats can reduce VRAM usage by up to 60%
- Can accelerate generation up to 3x compared to full-precision

## IC-LoRA Models (v0.9.8+)

IC-LoRA (In-Context LoRA) is a specialized training approach where the LoRA learns to condition generation on reference inputs rather than just text:

### How IC-LoRA Differs from Standard LoRA
- Standard LoRA: Teaches the model "what this concept looks like" through captions and training clips
- IC-LoRA: Teaches "given this reference image/audio, produce a video where the subject matches the reference"
- Uses rank 128 (higher than standard rank 32) for encoding detailed identity features

### Available IC-LoRA Models
- **Depth control** -- Conditions generation on depth maps
- **Pose control** -- Conditions generation on pose skeletons
- **Canny edge control** -- Conditions generation on edge detection
- **Detailer** (LTX-Video-ICLoRA-detailer-13B-0.9.8) -- Enhances fine details
- **Union Control** -- Combined control model

## Spatial and Temporal Upscalers

### Spatial Upscaler (ltxv-spatial-upscaler-0.9.7)
- **Repository:** https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.7
- **Function:** 2x resolution upscaling of generated videos
- **Pipeline:** LTXLatentUpsamplePipeline
- **Parameters:** upscale_factor: 2.0, denoise_strength: 0.35-0.55, steps: 4-8
- **Workflow:** Generate at base resolution -> Upscale 2x -> Optional refinement

### Temporal Upscaler
- **Function:** Doubles frame rate of existing clips
- **Placement:** After spatial upscaling in pipeline

## Summary of All Available Optimization Strategies

| Strategy | VRAM Savings | Speed Impact | Quality Impact |
|----------|-------------|--------------|----------------|
| Distillation | Moderate | 15x faster | Slight reduction |
| FP8 quantization | ~45% | Similar/faster | Minimal |
| GGUF (Q8) | ~50% | Depends | Minimal |
| GGUF (Q4) | ~75% | Depends | Noticeable |
| NVFP4 | ~60% | Up to 3x | Moderate |
| LoRA adapter | ~95% (weights only) | Same as distilled | Same as distilled |
| VAE tiling | Significant | Slightly slower | None |
| Lower resolution | Proportional | Proportional | Resolution dependent |
| Fewer frames | Proportional | Proportional | Duration dependent |
