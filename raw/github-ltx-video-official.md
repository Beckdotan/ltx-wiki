# Lightricks/LTX-Video -- Official Repository

> **Source:** <https://github.com/Lightricks/LTX-Video>
> **Paper:** <https://arxiv.org/abs/2501.00103>
> **HuggingFace:** <https://huggingface.co/Lightricks/LTX-Video>
> **Documentation:** <https://docs.ltx.video>

## Repository Stats

| Metric | Value |
|--------|-------|
| Stars | ~10,000 |
| Forks | ~960 |
| Open Issues | 78 |
| Primary Language | Python |
| License | OpenRail-M (commercial use since v0.9.5) |
| Formal GitHub Releases | None (no packaged releases; users clone the repo directly) |

## Description

LTX-Video is described as "the first DiT-based video generation model that contains all core capabilities of modern video generation in one model." It generates videos up to 50 FPS at native 4K resolution with synchronized audio in a single pass. Supports image-to-video, multi-keyframe conditioning, keyframe-based animation, video extension (forward and backward), and video-to-video transformations.

## Model Versions (Available Checkpoints)

| Model | Notes | Config |
|-------|-------|--------|
| ltxv-13b-0.9.8-dev | Highest quality, more VRAM needed | ltxv-13b-0.9.8-dev.yaml |
| ltxv-13b-0.9.8-distilled | Faster inference, balanced quality | ltxv-13b-0.9.8-distilled.yaml |
| ltxv-2b-0.9.8-distilled | Smaller model, light VRAM usage | ltxv-2b-0.9.8-distilled.yaml |
| FP8 Quantized Versions | Reduced precision for efficiency | Various FP8 configs |
| ltxv-2b-0.9.6 & Distilled | Previous generation, still available | Legacy configs |

## Release Timeline

- **December 2024**: Initial release (v0.9.0), paper published (arXiv 2501.00103)
- **March 2025**: v0.9.5 -- switched to commercial-use OpenRail-M license
- **April 2025**: Enhanced checkpoints with improved prompt understanding
- **May 2025**: v0.9.7 -- 13B model with improved quality
- **July 2025**: v0.9.8 -- distilled models supporting up to 60 seconds of video
- **October 2025**: LTX-2 announced as successor model

## Installation

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference]
```

**System Requirements:**
- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2 (GPU) or MPS on macOS (PyTorch 2.3.0 or >= 2.6)

## Repository Structure

```
LTX-Video/
├── .github/workflows/        # CI/CD
├── configs/                   # Model configuration YAMLs
├── docs/_static/             # Example GIFs and documentation assets
├── ltx_video/                # Core Python package
│   ├── models/               # Model definitions
│   ├── pipelines/            # Processing pipeline implementations
│   ├── schedulers/           # Scheduler components
│   ├── utils/                # Utility functions
│   └── inference.py          # Main inference API
├── tests/                    # Test suite
├── inference.py              # Standalone inference script (CLI)
├── pyproject.toml            # Package configuration
├── LICENSE
└── README.md
```

## Key Features

- **Multimodal Conditioning:** Image-to-video, video extension, multi-keyframe support
- **High Fidelity:** Native 4K, up to 50 FPS, realistic motion
- **Synchronized Audio:** Audio+video generation together (LTX-2 lineage)
- **Control Models:** IC-LoRA implementations for depth, pose, and canny edge control
- **Distilled Variants:** 15x faster inference with minimal quality loss
- **LoRA Support:** Both standard and IC-LoRA fine-tuning capabilities
- **Quantization:** FP8 options for reduced VRAM requirements
- **Up to 60 seconds** of video generation (v0.9.8)

## Usage Examples

**CLI Inference:**
```bash
python inference.py --prompt "PROMPT" \
  --conditioning_media_paths IMAGE_PATH \
  --conditioning_start_frames 0 \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

**Python Library:**
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

## Integrations

- **ComfyUI:** Official workflows at github.com/Lightricks/ComfyUI-LTXVideo
- **Diffusers:** Native support via HuggingFace `LTXVideoPipeline`
- **Cloud platforms:** LTX-Studio (app.ltx.studio), Fal.ai, Replicate

## Prompt Engineering Guidelines

- Start with main action in opening sentence
- Include specific movement and gesture details
- Describe character/object appearances precisely
- Detail background and environmental context
- Specify camera angles and movements
- Describe lighting and color palette
- Stay within 200 words, presented chronologically

## Technical Architecture

- **Model Type:** Diffusion Transformer (DiT)
- **Default Resolution:** 1216 x 704 @ 30 FPS
- **Compression Ratio:** 1:192 (32x32x8 pixels per token)
- **Latent Depth:** 128 channels
- **VAE Innovation:** Timestep-conditioned decoder that performs both latent-to-pixel conversion and final denoising simultaneously
- **Guidance Methods:** Classifier-Free Guidance (CFG) and Spatiotemporal Guidance (STG)

## Research Paper (arXiv 2501.00103)

**Title:** "LTX-Video: Realtime Video Latent Diffusion"
**Authors:** Yoav HaCohen, Nisan Chiprut, Benny Brazowski, Daniel Shalem, Dudu Moshe, Eitan Richardson, Eran Levin, Guy Shiran, Nir Zabari, Ori Gordon, Poriya Panet, Sapir Weissbuch, Victor Kulikov, Yaki Bitterman, Zeev Melumian, and Ofir Bibi
**Submitted:** December 30, 2024
**Category:** Computer Vision and Pattern Recognition (cs.CV)

**Key contributions:**
1. Unified architecture integrating Video-VAE and denoising transformer as interdependent components
2. 1:192 compression ratio with spatiotemporal downscaling of 32x32x8 pixels per token
3. Patchifying operation relocated from transformer input to VAE input
4. VAE decoder performs both latent-to-pixel conversion and final denoising step simultaneously
5. Faster-than-real-time: 5 seconds of 24fps video at 768x512 in just 2 seconds on H100 GPU

## Links

- Discord Community: <https://discord.gg/ltxplatform>
- Training Tools: <https://github.com/Lightricks/LTX-Video-Trainer>
- ComfyUI Extension: <https://github.com/Lightricks/ComfyUI-LTXVideo>
