# LTX-Video 0.9.7 Release - First 13B Model (May 2025)

**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
**Source:** https://www.prnewswire.com/news-releases/lightricks-launches-13b-parameters-ltx-video-model-breakthrough-rendering-approach-generates-high-quality-efficient-ai-video-30x-faster-than-comparable-models-302447660.html
**Source:** https://techstartups.com/2025/05/06/lightricks-advances-ai-video-capabilities-with-new-13b-parameter-ltxv-model/

## Release Details

- **Release date:** May 6, 2025
- **Parameters:** 13 billion (massive jump from 2B)
- **Codename:** LTXV-13B
- **Licensing:** Free for enterprises with under $10 million annual revenue

## What Was Announced

Lightricks launched its 13B parameter LTX Video model, described as the "most advanced and efficient AI video generation model to date." The press release emphasized a "breakthrough rendering approach" that generates high-quality AI video 30x faster than comparable models.

## Model Variants

| Model | Parameters | Speed | Quality | VRAM | File Size |
|-------|-----------|-------|---------|------|-----------|
| ltxv-13b-0.9.7-dev | 13B | Standard | Highest | Higher | ~28.6 GB |
| ltxv-13b-0.9.7-distilled | 13B | 15x faster | Slight reduction | Lower | ~28.6 GB |
| ltxv-13b-0.9.7-dev-fp8 | 13B | Standard | Highest | Lower (quantized) | ~15.7 GB |
| ltxv-13b-0.9.7-distilled-fp8 | 13B | 15x faster | Slight reduction | Lower (quantized) | ~15.7 GB |
| ltxv-13b-0.9.7-distilled-lora128 | 13B | 15x faster | Slight reduction | Minimal (~1GB) | ~1.33 GB |

## Key Technical Breakthroughs

### Multiscale Rendering (New)
- Drafts in lower detail first to capture coarse motion
- Progressively adds structure, lighting, and micro-motion
- For 1080p output: renders at 960x540, then upscales 2x to full resolution
- Motion prioritized first, then detail infused
- Result: 30x faster rendering than comparable-size models

### 13B Architecture
- Significant parameter increase from 2B to 13B
- Enhanced prompt understanding and adherence
- Breakthrough physical understanding
- More detailed and realistic generation

### Video Length
- Supports up to 60 seconds of continuous video

## Capabilities

- Text-to-video generation
- Image-to-video generation
- Video-to-video generation
- Multi-keyframe conditioning
- LoRA fine-tuning support
- FP8 quantization for reduced VRAM

## Inference

### Text-to-Video
```bash
python inference.py \
  --prompt "PROMPT" \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.7-dev.yaml
```

### Image-to-Video
```bash
python inference.py \
  --prompt "PROMPT" \
  --input_image_path IMAGE_PATH \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.7-dev.yaml
```

### Distilled Model Parameters
- No classifier-free guidance needed
- No spatio-temporal guidance needed
- 8 diffusion steps recommended (or fewer)

### Dev Model Parameters
- 30 inference steps default
- decode_timestep: 0.05
- image_cond_noise_scale: 0.025

## Output Specifications

- **Resolution:** 1216x704 native, works best under 720x1280
- **Frame Rate:** 30 FPS
- **Maximum Frames:** 257 (8n+1 pattern)
- **Duration:** Up to 60 seconds

## Hardware Requirements

- Python 3.10.5+
- CUDA 12.2+
- PyTorch >= 2.1.2
- 13B-dev requires significantly more VRAM than 2B models
- FP8 variants reduce VRAM by roughly half

## ComfyUI Integration

New simplified flows and nodes:
- Simplified image-to-video
- Image-to-video with extension
- Image-to-video with keyframes

## Hugging Face Repositories

- **Dev:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
- **Distilled:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
- **Spatial Upscaler:** https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.7

## Online Deployment

- LTX Studio (image-to-video)
- Fal.ai (text-to-video and image-to-video)
- Replicate (both modalities)
