# LTX-Video Distilled Models and Optimization Variants

**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
**Source:** https://huggingface.co/Lightricks/LTX-Video (main page)

---

## Distillation Overview

Starting with v0.9.6, Lightricks introduced distilled variants of LTX-Video models that dramatically reduce inference time while maintaining good quality.

## 2B Distilled (v0.9.6)

### Key Characteristics
- **Speed:** 15x faster than the dev model
- **Real-time capable:** Can generate video faster than playback
- **No STG/CFG required:** Greatly simplifies inference pipeline
- **Fewer inference steps:** Significantly reduced step count

### Significance
- First distilled variant released
- Demonstrated that LTX-Video architecture is amenable to distillation
- Made real-time video generation accessible on consumer hardware

## 13B Distilled (v0.9.7)

### Key Characteristics
- **Inference steps:** 7 steps (vs. 20-30 for dev)
- **Quality:** "Slight quality reduction compared to 13B dev"
- **Use case:** "Ideal for rapid iterations"
- **VRAM:** Less than 13B dev, estimated 24GB+

### Distillation Approach
The exact methodology is not publicly documented, but evidence suggests:
- A **LoRA128 adapter** (`ltxv-13b-0.9.7-distilled-lora128`) exists that converts the dev model to behave like distilled
- This implies knowledge distillation or fine-tuning techniques
- The LoRA rank of 128 captures the distillation transformation

### Configuration
```yaml
# ltxv-13b-0.9.7-distilled.yaml
```

### ComfyUI Workflow
```json
# ltxv-13b-dist-i2v-base.json
```

## FP8 Quantization

### What It Is
- Float8 (FP8) quantization reduces model weights from bfloat16 to 8-bit floating point
- Approximately halves memory requirements
- Available for both dev and distilled variants

### Available FP8 Models
- `ltxv-13b-0.9.7-fp8` - Full quality model, quantized
- `ltxv-13b-0.9.7-distilled-fp8` - Distilled + quantized
- `ltxv-13b-0.9.8-fp8` - Latest full quality, quantized
- `ltxv-13b-0.9.8-distilled-fp8` - Latest distilled + quantized
- `ltxv-2b-0.9.8-distilled-fp8` - Smallest, most efficient

### VRAM Savings
| Model | BF16 VRAM | FP8 VRAM |
|-------|-----------|----------|
| 13B dev | ~40GB+ | ~16-20GB |
| 13B distilled | ~24GB+ | ~16GB |
| 2B distilled | ~8-12GB | ~6-8GB |

## Mix Mode

### Concept
The "mix" variant (`ltxv-13b-0.9.7-mix`, `ltxv-13b-0.9.8-mix`) combines dev and distilled models in a multi-scale rendering workflow:

1. **First pass:** Distilled model generates at lower resolution (fast)
2. **Upscale:** Spatial upscaler increases resolution
3. **Second pass:** Dev model refines at higher resolution (quality)

### Benefits
- Balanced speed vs. quality tradeoff
- Takes advantage of both model strengths
- Practical workflow for production use

## LoRA Adapters

### Distilled LoRA128
- **Model:** `ltxv-13b-0.9.7-distilled-lora128`
- **Rank:** 128
- **Purpose:** Apply to dev model to get distilled-like behavior
- **Use case:** Fine-tuning starting from dev weights while maintaining distillation benefits

### ICLoRA Conditioning Adapters
- **Depth:** Depth map conditioning
- **Pose:** Pose keypoint conditioning
- **Canny:** Edge detection conditioning
- **Detailer:** Enhanced fine detail generation
- Available for both 0.9.7 and 0.9.8

## Upscaling Models

### Spatial Upscaler (v0.9.7+)
- **Type:** Latent diffusion-based spatial upscaler
- **Function:** 2x upscaling of height and width in latent space
- **Integration:** Works with `LTXLatentUpsamplePipeline`
- **Workflow:**
  1. Generate video at lower resolution → get latents
  2. Upscale latents with spatial upscaler
  3. Optionally denoise upscaled latents (recommended, denoise_strength=0.4, 10 steps)
  4. Decode to pixels and downscale to expected resolution

### Temporal Upscaler (v0.9.7+)
- Frame interpolation model
- Increases temporal resolution (more frames between existing ones)
- Works in latent space for efficiency

## Efficiency Comparison

| Model | Params | Precision | Steps | Relative Speed | VRAM |
|-------|--------|-----------|-------|----------------|------|
| 2B dev (0.9.6) | 2B | BF16 | 20-50 | 1x (baseline) | ~12GB |
| 2B distilled (0.9.6) | 2B | BF16 | ~7 | ~15x | ~8GB |
| 13B dev (0.9.7) | 13B | BF16 | 20-30 | ~0.3x | ~40GB |
| 13B distilled (0.9.7) | 13B | BF16 | 7 | ~1x | ~24GB |
| 13B FP8 (0.9.7) | 13B | FP8 | 20-30 | ~0.5x | ~16GB |
| 13B distilled-fp8 (0.9.7) | 13B | FP8 | 7 | ~2x | ~16GB |
| 2B distilled-fp8 (0.9.8) | 2B | FP8 | ~7 | ~20x+ | ~6GB |

Note: Speed values are rough estimates based on model characteristics, not official benchmarks.
