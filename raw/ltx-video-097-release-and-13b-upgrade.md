# LTX-Video 0.9.7: 13B Model Release and New Features

**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
**Source:** https://huggingface.co/Lightricks/LTX-Video (main page)

---

## What's New in 0.9.7

Version 0.9.7 represents the biggest upgrade in LTX-Video's history, scaling from 2B to 13B parameters and introducing a rich ecosystem of variants and tools.

### 1. 13B Parameter Model
- Massive scale-up from 2B to 13B parameters
- Significantly improved quality and coherence
- Better prompt following and motion consistency
- Higher VRAM requirements (20-40GB+ depending on variant)

### 2. Model Variants

| Variant | Description | Inference Steps | VRAM |
|---------|-------------|----------------|------|
| `ltxv-13b-0.9.7-dev` | Full quality, highest fidelity | 20-30 | 40GB+ |
| `ltxv-13b-0.9.7-distilled` | Faster, slightly reduced quality | 7 | 24GB+ |
| `ltxv-13b-0.9.7-fp8` | FP8 quantized for memory | ~20-30 | 16GB+ |
| `ltxv-13b-0.9.7-distilled-fp8` | Distilled + quantized | 7 | ~16GB |
| `ltxv-13b-0.9.7-mix` | Multi-scale dev+distilled workflow | varies | varies |

### 3. Mix Mode
- Combines dev and distilled models in a multi-scale rendering workflow
- Uses distilled model for initial low-resolution pass
- Uses dev model for high-resolution refinement
- Balances speed and quality

### 4. LoRA Support
- `ltxv-13b-0.9.7-distilled-lora128` - Rank 128 LoRA adapter
- Converts the dev model to behave like the distilled version
- Enables fine-tuning workflows
- Suggests knowledge distillation approach was used

### 5. ICLoRA Conditioning Adapters
New LoRA-based adapters for controlled generation:
- **Depth:** Condition on depth maps for 3D-aware generation
- **Pose:** Condition on pose keypoints for human motion control
- **Canny:** Condition on edge maps for structure preservation
- **Detailer:** Enhanced fine detail generation

### 6. Upscaling Models
- **Spatial Upscaler:** 2x spatial resolution increase in latent space
- **Temporal Upscaler:** Frame interpolation for smoother/longer video
- Both work in the latent space for efficiency

### 7. New Diffusers API
- `LTXConditionPipeline` replaces `LTXPipeline` for more flexible conditioning
- `LTXLatentUpsamplePipeline` for upscaling workflows
- `LTXVideoCondition` class for specifying multi-condition inputs

## Distilled Model Details

### Speed
- 7 inference steps (vs. 20-30 for dev)
- No STG/CFG required (significant simplification)
- "Ideal for rapid iterations"

### Quality Tradeoff
- "Slight quality reduction compared to 13B dev"
- Still produces high-quality output
- The LoRA128 adapter approach suggests the distillation preserved most knowledge

### Configuration
- Config file: `ltxv-13b-0.9.7-distilled.yaml`
- ComfyUI workflow: `ltxv-13b-dist-i2v-base.json`

## FP8 Quantization

- Reduces model precision from bfloat16 to float8
- Significant VRAM reduction
- Available for both dev and distilled variants
- Maintains good quality despite lower precision

## Hardware Requirements (0.9.7)

| Variant | Estimated VRAM | Recommended GPU |
|---------|---------------|-----------------|
| 13B dev | 40GB+ | A100 80GB, H100 |
| 13B distilled | 24GB+ | A100 40GB, RTX 4090 |
| 13B FP8 | 16GB+ | RTX 4090, A100 40GB |
| 13B distilled-fp8 | ~16GB | RTX 4090 |
| 2B dev (0.9.6) | ~12GB | RTX 3090, RTX 4080 |
| 2B distilled (0.9.6) | ~8GB | RTX 3080, RTX 4070 |

### System Requirements
- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2+

## Usage Example (0.9.7 Dev, Image-to-Video)

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", 
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")

image = load_image("penguin.png")
video = load_video(export_to_video([image]))
condition = LTXVideoCondition(video=video, frame_index=0)

output = pipe(
    conditions=[condition],
    prompt="A cute penguin reading a book",
    negative_prompt="worst quality, blurry",
    width=832,
    height=480,
    num_frames=96,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(0),
).frames

export_to_video(output, "output.mp4", fps=24)
```

## CLI Usage (0.9.7)

```bash
# Text-to-Video
python inference.py --prompt "PROMPT" --height 704 --width 1216 \
  --num_frames 121 --seed 42 \
  --pipeline_config configs/ltxv-13b-0.9.7-dev.yaml

# Image-to-Video
python inference.py --prompt "PROMPT" --input_image_path IMAGE.png \
  --height 480 --width 832 --num_frames 96 --seed 0 \
  --pipeline_config configs/ltxv-13b-0.9.7-distilled.yaml
```
