# LTX-Video Hugging Face Model Cards and Weights

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.1
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.5
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled

## Main Repository

**URL:** https://huggingface.co/Lightricks/LTX-Video

The main repository contains all model weights across versions in a unified location. As of v0.9.8, this includes:

### Files Available
```
ltxv-13b-0.9.8-dev.safetensors          (~28.6 GB)
ltxv-13b-0.9.8-dev-fp8.safetensors      (~15.7 GB)
ltxv-13b-0.9.8-distilled.safetensors     (~28.6 GB)
ltxv-13b-0.9.8-distilled-fp8.safetensors (~15.7 GB)
ltxv-13b-0.9.7-dev.safetensors          (~28.6 GB)
ltxv-13b-0.9.7-dev-fp8.safetensors      (~15.7 GB)
ltxv-13b-0.9.7-distilled.safetensors     (~28.6 GB)
ltxv-13b-0.9.7-distilled-fp8.safetensors (~15.7 GB)
ltxv-13b-0.9.7-distilled-lora128.safetensors (~1.33 GB)
ltxv-2b-0.9.8-distilled.safetensors      (~6.34 GB)
ltxv-2b-0.9.8-distilled-fp8.safetensors  (~4.46 GB)
ltxv-2b-0.9.6-dev-04-25.safetensors     (~6.34 GB)
ltxv-2b-0.9.6-distilled-04-25.safetensors (~6.34 GB)
```

### Diffusers Integration
```python
# From single file
from diffusers import LTXConditionPipeline
pipe = LTXConditionPipeline.from_single_file("path/to/ltxv-13b-0.9.8-distilled.safetensors")

# From pretrained
pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")
```

## Version-Specific Repositories

### LTX-Video-0.9.1
- **URL:** https://huggingface.co/Lightricks/LTX-Video-0.9.1
- **Model type:** Diffusion-based text-to-video and image-to-video
- **Parameters:** 2B
- **License:** Version-specific

### LTX-Video-0.9.5
- **URL:** https://huggingface.co/Lightricks/LTX-Video-0.9.5
- **Model type:** Diffusion-based text-to-video and image-to-video
- **Parameters:** 2B
- **License:** OpenRail-M (commercial use allowed)
- **Downloads (reported):** ~1,414/month

### LTX-Video-0.9.7-dev
- **URL:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
- **Model type:** DiT-based text-to-video, image-to-video, video-to-video
- **Parameters:** 13B
- **Key config:** configs/ltxv-13b-0.9.7-dev.yaml

### LTX-Video-0.9.7-distilled
- **URL:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
- **Model type:** Distilled variant, 15x faster
- **Parameters:** 13B
- **Steps:** 8 recommended, no CFG/STG

### LTX-Video-0.9.8-13B-distilled
- **URL:** https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled
- **Pipeline:** LTXConditionPipeline
- **Parameters:** 13B
- **Key features:** IC-LoRA support, 60-second videos

## Auxiliary Model Repositories

### Spatial Upscaler
- **URL:** https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.7
- **Purpose:** 2x spatial resolution upscaling of generated videos
- **Pipeline:** LTXLatentUpsamplePipeline

### IC-LoRA Models
- Depth control LoRA
- Pose control LoRA
- Canny edge control LoRA
- Detailer LoRA: LTX-Video-ICLoRA-detailer-13B-0.9.8

## Community Quantized Repositories

### GGUF Formats
- **city96/LTX-Video-gguf:** https://huggingface.co/city96/LTX-Video-gguf
- **city96/LTX-Video-0.9.5-gguf:** GGUF quantized version of 0.9.5
- **unsloth/LTX-2-GGUF:** GGUF quantized LTX-2 using Unsloth Dynamic 2.0

### Third-Party Distilled
- **linoyts/LTX-Video-0.9.8-13B-distilled:** Community-hosted mirror

## Model Card Common Fields

All LTX-Video model cards share these common specifications:

- **Model type:** Diffusion-based video generation
- **Language:** English
- **Prompt style:** Elaborate, descriptive prompts perform best
- **Resolution requirements:** Height and width divisible by 32
- **Frame requirements:** Divisible by 8 + 1
- **Limitations:** Not intended for factual information; may amplify societal biases; imperfect prompt matching

## Loading Models

### Native Inference
```bash
pip install -e .[inference]
python inference.py --prompt "..." --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Diffusers
```bash
pip install -U git+https://github.com/huggingface/diffusers
```

### ComfyUI
Via ComfyUI-LTXVideo custom node: https://github.com/Lightricks/ComfyUI-LTXVideo

## Online Inference Providers

| Provider | Text-to-Video | Image-to-Video |
|----------|--------------|----------------|
| LTX Studio | -- | Yes |
| Fal.ai | Yes | Yes |
| Replicate | Yes | Yes |
