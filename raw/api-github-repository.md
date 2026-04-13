# LTX-Video GitHub Repository

**Source:** https://github.com/Lightricks/LTX-Video
**Date:** 2026-04-13

---

## Overview

LTX-Video is an open-source video generation framework by Lightricks. The GitHub repository contains the official inference code, configuration files, and documentation for running LTX-Video models locally.

---

## Installation

### Prerequisites
- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2+
- GPU with sufficient VRAM (see hardware requirements below)

### From Source (Inference Script)
```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e ".[inference-script]"
```

### Using Diffusers (Recommended for Python API)
```bash
pip install -U git+https://github.com/huggingface/diffusers
```

---

## CLI Usage

### Image-to-Video
```bash
python inference.py \
  --prompt "A detailed description of the desired video" \
  --input_image_path /path/to/image.png \
  --height 480 \
  --width 832 \
  --num_frames 97 \
  --seed 42 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Video-to-Video / Multi-Condition
```bash
python inference.py \
  --prompt "A detailed description of the desired video" \
  --conditioning_media_paths /path/to/image1.png /path/to/video1.mp4 \
  --conditioning_start_frames 0 20 \
  --height 480 \
  --width 832 \
  --num_frames 97 \
  --seed 42 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Key CLI Arguments
| Argument | Description |
|----------|-------------|
| `--prompt` | Text description of desired video |
| `--input_image_path` | Path to conditioning image (image-to-video) |
| `--conditioning_media_paths` | Paths to conditioning images/videos (multi-condition) |
| `--conditioning_start_frames` | Target frame indices for each conditioning media |
| `--height` | Output video height (divisible by 32) |
| `--width` | Output video width (divisible by 32) |
| `--num_frames` | Number of output frames (divisible by 8, +1) |
| `--seed` | Random seed for reproducibility |
| `--pipeline_config` | Path to YAML pipeline configuration |

---

## Pipeline Configuration Files

The repository includes YAML configuration files for different model variants:

| Config File | Model | Description |
|-------------|-------|-------------|
| `configs/ltxv-13b-0.9.8-dev.yaml` | 13B dev | Highest quality, most VRAM |
| `configs/ltxv-13b-0.9.8-distilled.yaml` | 13B distilled | Balanced speed/quality |
| `configs/ltxv-13b-0.9.8-fp8.yaml` | 13B FP8 | Reduced memory usage |
| `configs/ltxv-2b-0.9.8-distilled.yaml` | 2B distilled | Lightweight |
| `configs/ltxv-2b-0.9.8-distilled-fp8.yaml` | 2B distilled FP8 | Minimal resources |

---

## Hardware Requirements

| Model Variant | Minimum VRAM | Recommended VRAM |
|---------------|-------------|-----------------|
| 13B dev | 16GB | 24GB+ |
| 13B distilled | 16GB | 24GB+ |
| 13B FP8 | 12GB | 16GB+ |
| 2B distilled | 8GB | 12GB+ |
| 2B distilled FP8 | 6GB | 8GB+ |

---

## Repository Structure (Key Files)

```
LTX-Video/
├── inference.py              # Main inference script
├── configs/                  # Pipeline YAML configurations
│   ├── ltxv-13b-0.9.8-dev.yaml
│   ├── ltxv-13b-0.9.8-distilled.yaml
│   ├── ltxv-13b-0.9.8-fp8.yaml
│   ├── ltxv-2b-0.9.8-distilled.yaml
│   └── ltxv-2b-0.9.8-distilled-fp8.yaml
├── pyproject.toml            # Package configuration
├── src/                      # Source code
│   └── ltx_video/            # Core library
└── README.md                 # Documentation
```

---

## Related Projects

- **ComfyUI-LTXVideo:** https://github.com/Lightricks/ComfyUI-LTXVideo - Official ComfyUI custom nodes
- **Hugging Face Model Hub:** https://huggingface.co/Lightricks/LTX-Video
- **Diffusers Integration:** https://huggingface.co/docs/diffusers/main/en/api/pipelines/ltx_video
- **LTX Studio:** https://app.ltx.studio/ - Commercial web application

---

## License

LTX-Video-Open-Weights License (custom open-weights license)
- Allows research and commercial use with attribution
- See: https://huggingface.co/Lightricks/LTX-Video/blob/main/LTX-Video-Open-Weights-License-0.X.txt
