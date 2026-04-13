---
title: LTX-Video Code Structure
type: technical
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/github-code-structure-architecture.md
  - raw/github-ltx-video-official.md
tags:
  - github
  - code-structure
  - ltx-video
  - architecture
  - python
---

# LTX-Video Code Structure

The [[github-official-repositories|Lightricks/LTX-Video]] repository is a standard Python package organized around a core library (`ltx_video/`) with inference scripts, configuration files, and a test suite.

## Directory Layout

```
LTX-Video/
├── .github/workflows/          # CI/CD pipelines
├── configs/                     # Model configuration YAMLs (per model variant)
├── docs/_static/               # Example GIFs and documentation assets
├── ltx_video/                  # Core Python package
│   ├── __init__.py
│   ├── inference.py            # Main inference API
│   ├── models/                 # Core model definitions (DiT, VAE)
│   ├── pipelines/              # Processing pipeline implementations
│   ├── schedulers/             # Noise scheduling components
│   └── utils/                  # Shared helper functions
├── tests/                      # Test suite
├── inference.py                # CLI inference entry point (standalone)
├── pyproject.toml              # Package & dependency configuration
├── LICENSE                     # OpenRail-M
└── README.md
```

## Key Components

### `ltx_video/` (Core Package)

- **`inference.py`** -- Main inference API, exposes `infer()` and `InferenceConfig` for programmatic use
- **`models/`** -- [[ltx-video-architecture|DiT (Diffusion Transformer)]] and [[video-vae|Video-VAE]] definitions
- **`pipelines/`** -- Processing pipeline implementations for various generation modes
- **`schedulers/`** -- [[python-schedulers|Noise scheduling]] components (FlowMatchEulerDiscreteScheduler)
- **`utils/`** -- Shared helper functions

### `configs/` (Model Configurations)

YAML configuration files for each model variant:

| Config | Model |
|--------|-------|
| ltxv-13b-0.9.8-dev.yaml | Highest quality, more VRAM |
| ltxv-13b-0.9.8-distilled.yaml | Faster inference, balanced quality |
| ltxv-2b-0.9.8-distilled.yaml | Smallest, lightest VRAM |
| Various FP8 configs | [[fp8-quantization|Reduced precision]] variants |
| Legacy configs | v0.9.6 and earlier |

### `inference.py` (CLI Entry Point)

Standalone inference script for command-line usage:

```bash
python inference.py --prompt "PROMPT" \
  --conditioning_media_paths IMAGE_PATH \
  --conditioning_start_frames 0 \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Python Library Usage

```python
from ltx_video.inference import infer, InferenceConfig

infer(InferenceConfig(
    pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
    prompt=PROMPT,
    height=HEIGHT,
    width=WIDTH,
    num_frames=NUM_FRAMES,
    output_path="output.mp4"
))
```

## Dependency Stack

- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2 (or MPS on macOS)
- transformers, diffusers, mediapy
- Optional: [[fp8-quantization|LTX-Video-Q8-Kernels]] for FP8

## Related Pages

- [[github-official-repositories]] -- All official repositories
- [[ltx-2-code-structure]] -- LTX-2 monorepo layout (successor)
- [[ltx-video-architecture]] -- Core technical architecture
- [[python-installation-setup]] -- Installation guide
- [[diffusers-integration]] -- HuggingFace Diffusers integration
