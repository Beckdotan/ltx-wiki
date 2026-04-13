# LTX-Video Hugging Face Model Cards, Weights, and Integration

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.1
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev

---

## Hugging Face Model Repositories

### Main Repository
- **URL:** https://huggingface.co/Lightricks/LTX-Video
- **Contains:** Links to all versions, documentation, code examples
- **License:** LTX-Video Open-Weights License

### Version-Specific Repositories

#### Early Versions (Open Access)
| Version | Repository | Status |
|---------|-----------|--------|
| 0.9.1 | `Lightricks/LTX-Video-0.9.1` | Public |
| 0.9.5 | `Lightricks/LTX-Video-0.9.5` | Public |

#### v0.9.6 (Some Gated)
| Model | Repository | Status |
|-------|-----------|--------|
| 2B dev | `Lightricks/LTX-Video-0.9.6-dev` | Gated |
| 2B distilled | `Lightricks/LTX-Video-0.9.6-distilled` | Gated |

#### v0.9.7 (Mixed Access)
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

#### v0.9.8 (Mostly Gated)
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

### Evolution of Diffusers API

#### v0.9.0 - v0.9.5 (Original API)
```python
from diffusers import LTXPipeline, LTXImageToVideoPipeline

# Text-to-Video
pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)

# Image-to-Video
pipe = LTXImageToVideoPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
```

#### v0.9.7+ (New Condition API)
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

## GitHub Repository

- **URL:** https://github.com/Lightricks/LTX-Video
- **Contains:** Inference scripts, configuration files, installation instructions
- **ComfyUI integration:** https://github.com/Lightricks/ComfyUI-LTXVideo/

### Installation
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

## Third-Party Integration

### ComfyUI
- Official node package: `ComfyUI-LTXVideo`
- Workflow files included for various configurations
- URL: https://github.com/Lightricks/ComfyUI-LTXVideo/

### Online Platforms
- **LTX-Studio:** https://app.ltx.studio/ltx-video (official)
- **Fal.ai:** https://fal.ai/models/fal-ai/ltx-video (API access)
- **Replicate:** https://replicate.com/lightricks/ltx-video (API access)

## License

- **Name:** LTX-Video Open-Weights License
- **Applies to:** v0.9.6 onwards
- **Earlier versions:** May have different license terms
- **Individual license files** available in each model repository on Hugging Face

## System Requirements

- **Python:** 3.10.5+
- **PyTorch:** >= 2.1.2
- **CUDA:** 12.2+ (recommended)
