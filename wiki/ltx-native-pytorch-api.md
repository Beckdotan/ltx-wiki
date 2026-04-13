---
title: LTX Native PyTorch API (ltx-pipelines)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
  - https://github.com/Lightricks/LTX-2
  - https://github.com/Lightricks/LTX-Video
  - https://pypi.org/project/ltx-video/
tags:
  - api
  - pytorch
  - ltx-2
  - native
  - python
  - code-example
---

# LTX Native PyTorch API (ltx-pipelines)

The LTX-2 repository provides a native PyTorch API separate from [[diffusers-pipeline-overview|HuggingFace Diffusers]]. This is the most full-featured way to use LTX-2 locally, exposing all pipeline variants and advanced configuration options.

For simpler usage, see the [[diffusers-pipeline-overview|Diffusers integration]].

## Repository Structure

The LTX-2 monorepo (`github.com/Lightricks/LTX-2`) contains three packages:

| Package | Description |
|---------|-------------|
| `ltx-core` | Model architecture, schedulers, guiders, noisers, and patchifiers |
| `ltx-pipelines` | High-level inference pipelines for all generation modes |
| `ltx-trainer` | Fine-tuning capabilities for LoRA variants |

## Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate
```

See [[python-installation-setup]] for full installation details.

## Pipeline Classes

Eight specialized pipeline implementations are available:

| Pipeline | Purpose |
|----------|---------|
| `TI2VidTwoStagesPipeline` | Text/image-to-video with dual-stage upscaling |
| `TI2VidTwoStagesRes2sPipeline` | Two-stage with res_2s second-order sampler |
| `TI2VidOneStagePipeline` | Single-stage generation without upscaling |
| `DistilledPipeline` | Fast execution using distilled checkpoint |
| `ICLoraPipeline` | Video-to-video with depth, pose, canny edge controls |
| `A2VidPipelineTwoStage` | Audio-conditioned generation |
| `RetakePipeline` | Selective temporal region regeneration |
| `KeyframeInterpolationPipeline` | Smooth transitions between keyframes |

## Usage: Distilled Pipeline

```python
from ltx_pipelines.distilled_pipeline import DistilledPipeline

pipeline = DistilledPipeline.from_config("path/to/config.yaml")
output = pipeline(
    prompt="A golden retriever running through a sunlit meadow...",
    width=768,
    height=512,
    num_frames=97,
    fps=24.0,
    seed=42,
)
```

## Usage: Two-Stage Pipeline

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline
from ltx_core.components.guiders import MultiModalGuiderParams

distilled_lora = [
    LoraPathStrengthAndSDOps(
        "/path/to/distilled_lora.safetensors", 0.6, LTXV_LORA_COMFY_RENAMING_MAP
    ),
]

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/ltx-2.3-22b-dev.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
)

video_guider_params = MultiModalGuiderParams(
    cfg_scale=3.0, stg_scale=1.0, rescale_scale=0.7,
    modality_scale=3.0, skip_step=0, stg_blocks=[29],
)

pipeline(
    prompt="A serene landscape with mountains",
    output_path="output.mp4",
    seed=42,
    height=512, width=768, num_frames=121,
    frame_rate=25.0, num_inference_steps=40,
    video_guider_params=video_guider_params,
)
```

## Usage: LTX-Video Repository (v1 Library API)

```python
from ltx_video.inference import infer, InferenceConfig

infer(
    InferenceConfig(
        pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
        prompt="A beautiful sunset over the ocean",
        height=480, width=832,
        num_frames=161,
        output_path="output.mp4",
    )
)
```

## CLI Usage (LTX-Video v1)

```bash
# Text-to-video
python inference.py --prompt "A beautiful sunset" \
    --height 480 --width 832 --num_frames 161 --seed 42 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml

# Image-to-video
python inference.py --prompt "The scene comes alive" \
    --conditioning_media_paths path/to/image.jpg \
    --conditioning_start_frames 0 \
    --height 480 --width 832 --num_frames 161 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml

# Multi-condition
python inference.py --prompt "A transition from day to night" \
    --conditioning_media_paths path/to/image1.jpg path/to/video1.mp4 \
    --conditioning_start_frames 0 80 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

## Guidance Parameters (MultiModalGuiderParams)

| Parameter | Range | Purpose |
|-----------|-------|---------|
| `cfg_scale` | 2.0-5.0 | Classifier-Free Guidance strength |
| `stg_scale` | 0.5-1.5 | Spatio-Temporal coherence control |
| `stg_blocks` | e.g., [29] | Transformer blocks for perturbation |
| `rescale_scale` | ~0.7 | Prevents over-saturation |
| `modality_scale` | 1.0-3.0 | Audio-visual synchronization |

## Quantization Options

### FP8 Cast (Broad Compatibility)

```python
from ltx_core.quantization.policy import QuantizationPolicy

pipeline = DistilledPipeline.from_config(
    "config.yaml",
    quantization=QuantizationPolicy.fp8_cast(),
)
```

### FP8 Scaled MM (Hopper GPUs Only)

```python
pipeline = DistilledPipeline.from_config(
    "config.yaml",
    quantization=QuantizationPolicy.fp8_scaled_mm(),
)
```

## Dimension Constraints

Same as Diffusers:
- **Width/Height:** Divisible by 32
- **Frame Count:** 8n + 1 pattern (1, 9, 17, 25, ..., 97, 121, 161, 257)

## Sampling Recommendations

| Model Type | Steps | CFG Scale |
|-----------|-------|-----------|
| Distilled | 4-8 | 1.0 |
| Full | 20-50 | 2.0-5.0 |

## Diffusers vs Native API

The Diffusers integration is simpler but may not expose all features available in the native `ltx-pipelines` package. The native API provides:
- More pipeline variants (retake, keyframe interpolation, audio-conditioned)
- Advanced guidance parameters (STG, modality scale)
- Direct quantization policy control
- LoRA loading with ComfyUI renaming support

## See Also

- [[diffusers-pipeline-overview]] -- the Diffusers alternative
- [[python-installation-setup]] -- installation methods
- [[ltxv-model-variants]] -- available models
