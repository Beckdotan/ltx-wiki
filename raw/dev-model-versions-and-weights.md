# LTX-Video Model Versions & Weights

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Date:** 2026-04-13

---

## Overview

LTX-Video models are hosted on Hugging Face under the `Lightricks` organization. The main model hub page at `Lightricks/LTX-Video` contains all variants, while individual model versions also have their own repository pages.

---

## Model Version History

### v0.9.8 (Latest Major Release)

The 0.9.8 release introduced the 13B parameter model alongside updated 2B models.

#### 13B Models
| Model ID | Parameters | Type | Description |
|----------|-----------|------|-------------|
| `Lightricks/LTX-Video-0.9.8-dev` | 13B | Dev | Highest quality; full denoising pipeline |
| `ltxv-13b-0.9.8-dev` (subfolder) | 13B | Dev | Same model, accessed via subfolder |
| `ltxv-13b-0.9.8-distilled` (subfolder) | 13B | Distilled | Fewer steps needed, faster inference |
| `ltxv-13b-0.9.8-fp8` (subfolder) | 13B | FP8 Quantized | Reduced VRAM usage |

#### 2B Models
| Model ID | Parameters | Type | Description |
|----------|-----------|------|-------------|
| `ltxv-2b-0.9.8-distilled` (subfolder) | 2B | Distilled | Lightweight, fast |
| `ltxv-2b-0.9.8-distilled-fp8` (subfolder) | 2B | FP8 Quantized | Minimum resource usage |

### v0.9.6

| Model ID | Parameters | Type | Description |
|----------|-----------|------|-------------|
| `ltxv-2b-0.9.6-distilled` (subfolder) | 2B | Distilled | Real-time capable, 15x faster, no STG/CFG |

### v0.9.1 and Earlier
Earlier versions established the base 2B architecture. These are largely superseded by 0.9.6+ versions.

---

## Auxiliary Models

### Upscalers
| Model ID | Purpose |
|----------|---------|
| `Lightricks/ltxv-spatial-upscaler-0.9.8` | Spatial resolution upscaling in latent space |
| `Lightricks/ltxv-temporal-upscaler-0.9.8` | Temporal upscaling (frame interpolation) in latent space |

### ICLoRA Adapters (Controlled Generation)
| Adapter | Conditioning Type |
|---------|------------------|
| ICLoRA Depth | Depth map conditioning |
| ICLoRA Pose | Pose estimation conditioning |
| ICLoRA Canny | Canny edge detection conditioning |

These adapters are stored within the main `Lightricks/LTX-Video` repository as subfolders.

---

## How to Access Models

### Via Diffusers (Standalone Repository)
```python
from diffusers import LTXConditionPipeline

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
)
```

### Via Diffusers (Subfolder)
```python
from diffusers import LTXConditionPipeline

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="ltxv-13b-0.9.8-distilled",
    torch_dtype=torch.bfloat16
)
```

### Manual Download
```bash
# Using git lfs
git lfs install
git clone https://huggingface.co/Lightricks/LTX-Video

# Using huggingface_hub
pip install huggingface_hub
huggingface-cli download Lightricks/LTX-Video --local-dir ./ltx-video
```

---

## Model Architecture

LTX-Video uses a **DiT (Diffusion Transformer)** architecture:
- Operates in a compressed latent space
- Uses a Video VAE for encoding/decoding
- Text conditioning via text encoder (T5-based)
- Spatial and temporal attention mechanisms
- Supports variable resolution and frame counts

### Key Architecture Properties
- **VAE spatial compression ratio:** Used to calculate valid resolutions
- **Latent channels:** Compressed representation dimension
- **Frame compression:** Temporal dimension is compressed in latent space

---

## Quantization Options

### FP8 (Float 8-bit)
- Available for both 13B and 2B models
- Approximately 50% memory reduction vs BF16
- Minimal quality degradation
- Recommended for GPUs with 12-16GB VRAM

### Community Quantizations
- 16 community quantizations available on Hugging Face
- Includes GGUF, AWQ, and other formats
- Quality and compatibility varies

---

## License

**LTX-Video-Open-Weights License**
- Allows both research and commercial use
- Requires attribution to Lightricks
- Custom license (not standard Apache/MIT)
- Full text: https://huggingface.co/Lightricks/LTX-Video/blob/main/LTX-Video-Open-Weights-License-0.X.txt

---

## Community Ecosystem

As of the fetch date:
- **567,927** downloads/month
- **24** model adapters
- **25** fine-tunes
- **16** quantizations
- **100+** community Spaces
- **111+** discussion threads

---

## Related Links

- **Model Hub:** https://huggingface.co/Lightricks/LTX-Video
- **13B Dev Model:** https://huggingface.co/Lightricks/LTX-Video-0.9.8-dev
- **Spatial Upscaler:** https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.8
- **Temporal Upscaler:** https://huggingface.co/Lightricks/ltxv-temporal-upscaler-0.9.8
