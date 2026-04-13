---
title: LTX Video on Hugging Face
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-huggingface-weights-and-integration.md
  - raw/ltx-video-huggingface-model-cards.md
  - raw/dev-model-versions-and-weights.md
  - raw/model-ltx-video-versions-huggingface.md
tags:
  - ltx-video
  - huggingface
  - weights
  - diffusers
  - integration
---
# LTX Video on Hugging Face

This page documents the [[ltx-video-overview|LTX-Video]] presence on Hugging Face, including all model repositories, weight files, access status, and integration methods.

## Organization

- **Organization:** [[lightricks]] (verified on Hugging Face)
- **URL:** https://huggingface.co/Lightricks
- **Stats:** 54 team members, 37 models, 8 spaces, 3 datasets, 3 collections

## Main Repository

- **URL:** https://huggingface.co/Lightricks/LTX-Video
- **Contains:** All model weights across versions in a unified location
- **Monthly downloads:** 567,927
- **Community:** 24 adapters, 25 fine-tunes, 16 quantizations, 100+ Spaces

### Weight Files in Main Repository

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

## Version-Specific Repositories

### Early Versions (Open Access)

| Version | Repository | Status |
|---------|-----------|--------|
| 0.9.1 | `Lightricks/LTX-Video-0.9.1` | Public |
| 0.9.5 | `Lightricks/LTX-Video-0.9.5` | Public |

### v0.9.6

| Model | Repository | Status |
|-------|-----------|--------|
| 2B dev | `Lightricks/LTX-Video-0.9.6-dev` | Gated |
| 2B distilled | `Lightricks/LTX-Video-0.9.6-distilled` | Gated |

### v0.9.7

| Model | Repository | Status |
|-------|-----------|--------|
| 13B dev | `Lightricks/LTX-Video-0.9.7-dev` | Public |
| 13B distilled | `Lightricks/LTX-Video-0.9.7-distilled` | Public |
| 13B FP8 | `Lightricks/LTX-Video-0.9.7-fp8` | Gated |
| 13B distilled-fp8 | `Lightricks/LTX-Video-0.9.7-distilled-fp8` | Unknown |
| 13B mix | `Lightricks/LTX-Video-0.9.7-mix` | Unknown |
| LoRA128 | `Lightricks/LTX-Video-0.9.7-distilled-lora128` | Gated |
| ICLoRA-depth | `Lightricks/LTX-Video-0.9.7-ICLoRA-depth` | Gated |
| ICLoRA-pose | `Lightricks/LTX-Video-0.9.7-ICLoRA-pose` | Gated |
| ICLoRA-canny | `Lightricks/LTX-Video-0.9.7-ICLoRA-canny` | Gated |
| ICLoRA-detailer | `Lightricks/LTX-Video-0.9.7-ICLoRA-detailer` | Gated |
| Spatial upscaler | `Lightricks/ltxv-spatial-upscaler-0.9.7` | Public |
| Temporal upscaler | `Lightricks/ltxv-temporal-upscaler-0.9.7` | Gated |

### v0.9.8 (Mostly Gated)

| Model | Repository | Status |
|-------|-----------|--------|
| 13B dev | `Lightricks/LTX-Video-0.9.8-dev` | Gated |
| 13B mix | `Lightricks/LTX-Video-0.9.8-mix` | Gated |
| 13B distilled | `Lightricks/LTX-Video-0.9.8-distilled` | Gated |
| 2B distilled | `Lightricks/LTX-Video-2b-0.9.8-distilled` | Gated |
| 13B FP8 | `Lightricks/LTX-Video-0.9.8-fp8` | Gated |
| 13B distilled-fp8 | `Lightricks/LTX-Video-0.9.8-distilled-fp8` | Gated |
| 2B distilled-fp8 | `Lightricks/LTX-Video-2b-0.9.8-distilled-fp8` | Gated |

## Diffusers Integration

### Original API (v0.9.0 to v0.9.5)

```python
from diffusers import LTXPipeline, LTXImageToVideoPipeline

# Text-to-Video
pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)

# Image-to-Video
pipe = LTXImageToVideoPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
```

### Condition API (v0.9.7+)

```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

# Unified pipeline for T2V, I2V, V2V, and multi-condition
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev",
    torch_dtype=torch.bfloat16
)

# Upscaling pipeline
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipe.vae,
    torch_dtype=torch.bfloat16
)
```

### Loading from Single File

```python
from diffusers import LTXConditionPipeline
pipe = LTXConditionPipeline.from_single_file("path/to/ltxv-13b-0.9.8-distilled.safetensors")
```

### Loading from Subfolder

```python
from diffusers import LTXConditionPipeline
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="ltxv-13b-0.9.8-distilled",
    torch_dtype=torch.bfloat16
)
```

### Key Diffusers Parameters

- `decode_timestep` -- Controls denoising decoder behavior in [[ltx-video-vae]] (e.g., 0.03)
- `decode_noise_scale` -- Controls noise injection (e.g., 0.025)
- Supports `group_offloading` for ~10 GB VRAM usage
- Supports `fp8 layerwise weight-casting` for memory efficiency

## GitHub Repository

- **URL:** https://github.com/Lightricks/LTX-Video
- **[[comfyui-integration]]:** https://github.com/Lightricks/ComfyUI-LTXVideo/

### Local Installation

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference-script]
```

### Configuration Files

Located in `configs/` directory:
- `ltxv-13b-0.9.7-dev.yaml`
- `ltxv-13b-0.9.7-distilled.yaml`
- `ltxv-13b-0.9.8-dev.yaml`
- `ltxv-13b-0.9.8-distilled.yaml`
- `ltxv-13b-0.9.8-mix.yaml`
- `ltxv-2b-0.9.8-distilled.yaml`

### Running Inference Locally

```bash
pip install -e .[inference]
python inference.py --prompt "..." --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

## Online Inference Platforms

| Provider | Text-to-Video | Image-to-Video | URL |
|----------|:------------:|:--------------:|-----|
| [[ltx-studio]] | -- | Yes | https://app.ltx.studio/ltx-video |
| Fal.ai | Yes | Yes | https://fal.ai/models/fal-ai/ltx-video |
| Replicate | Yes | Yes | https://replicate.com/lightricks/ltx-video |

## Community Quantized Models

| Repository | Format | Notes |
|-----------|--------|-------|
| city96/LTX-Video-gguf | GGUF | Quantized original |
| city96/LTX-Video-0.9.5-gguf | GGUF | Quantized v0.9.5 |
| unsloth/LTX-2-GGUF | GGUF | Dynamic 2.0 quantization |

## Datasets Released

- Canny-Control-Dataset
- Cakeify-Dataset
- Squish-Dataset

## HuggingFace Spaces

- LTX 2.3 Distilled (featured, Running on Zero)
- LTX Video Fast
- LTX-2 Video Fast
- LTMarX (video watermarking)
- Image Aligner API

## License

- **Name:** LTX-Video Open-Weights License (v0.9.6+)
- **Earlier versions:** RAIL-M (v0.9.0), OpenRail-M (v0.9.5)
- Allows both research and commercial use
- Requires attribution to [[lightricks]]
- 13B model free for enterprises under $10M annual revenue
- Full license text: https://huggingface.co/Lightricks/LTX-Video/blob/main/LTX-Video-Open-Weights-License-0.X.txt

## System Requirements

- **Python:** >= 3.10.5
- **PyTorch:** >= 2.1.2
- **CUDA:** >= 12.2
- **Precision:** bfloat16 (default), fp8 (quantized variants)

## Limitations

- Not designed for factual information generation
- May amplify existing societal biases
- May not perfectly match prompts in all cases
- Works best below 720x1280 resolution and below 257 frames

## References

- [[ltx-video-overview]] -- Model family overview
- [[ltx-video-model-variants]] -- Complete model inventory
- [[ltx-video-architecture]] -- Technical architecture details
- [[comfyui-integration]] -- ComfyUI node integration
- [[ltx-video-to-ltx-2]] -- Evolution to LTX-2/2.3
