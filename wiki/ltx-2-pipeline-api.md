---
title: LTX-2 Pipeline API
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
  - https://github.com/Lightricks/LTX-2
tags:
  - python
  - ltx-2
  - pipeline
  - api
  - architecture
---

# LTX-2 Pipeline API

The `ltx-pipelines` package within the [[sdk-ltx-2]] monorepo provides high-level pipeline implementations that compose `ltx-core` components. These are distinct from the [[diffusers-integration]] pipelines.

## Available Pipelines

| Pipeline | Description | Use Case | Steps |
|----------|-------------|----------|-------|
| `TI2VidTwoStagesPipeline` | Two-stage text/image-to-video with 2x upsampling | Production use | 40 + 8 |
| `TI2VidTwoStagesHQPipeline` | Two-stage with res_2s second-order sampler | Higher quality | Variable |
| `TI2VidOneStagePipeline` | Single-stage, no upsampling | Quick prototyping | 20-50 |
| `DistilledPipeline` | 8 predefined sigmas, fastest inference | Batch processing | 8 + 4 |
| `ICLoraPipeline` | IC-LoRA support for vid2vid/img2vid | Style transfer | Variable |
| `KeyframeInterpolationPipeline` | Interpolate between keyframes | Animation | Variable |
| `A2VidPipelineTwoStage` | Audio-to-video generation | Audio sync | Variable |
| `RetakePipeline` | Region-specific video regeneration | Editing | Variable |

## Production Pipeline (TI2VidTwoStagesPipeline)

The recommended pipeline for production use. See [[python-two-stage-pipeline]] for full usage.

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline
from ltx_core.components.guiders import MultiModalGuiderParams
from ltx_core.quantization import QuantizationPolicy

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
    quantization=QuantizationPolicy.fp8_cast(),
)

pipeline(
    prompt="A dramatic ocean scene",
    output_path="output.mp4",
    seed=42,
    height=512,
    width=768,
    num_frames=121,
    frame_rate=25.0,
    num_inference_steps=40,
    video_guider_params=video_guider_params,
    audio_guider_params=audio_guider_params,
    images=[],
)
```

## Distilled Pipeline

Uses 8 predefined sigmas for fastest inference. See [[python-performance-speed]].

```python
from ltx_pipelines.distilled import DistilledPipeline

pipeline = DistilledPipeline(
    checkpoint_path="/path/to/ltx-2.3-22b-distilled.safetensors",
    gemma_root="/path/to/gemma",
)

pipeline(
    prompt="...",
    output_path="output.mp4",
    seed=42,
    height=512,
    width=768,
    num_frames=121,
)
```

## IC-LoRA Pipeline

For video/image-to-video transformations with structural control. See [[python-ic-lora]].

```python
from ltx_pipelines.ic_lora import ICLoraPipeline

pipeline = ICLoraPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    ic_lora_path="/path/to/ic_lora.safetensors",
)
```

## Guidance Configuration

All LTX-2 pipelines accept [[python-guidance-parameters]] via `MultiModalGuiderParams`:

```python
from ltx_core.components.guiders import MultiModalGuiderParams

video_guider_params = MultiModalGuiderParams(
    cfg_scale=3.0,
    stg_scale=1.0,
    stg_blocks=[29],
    rescale_scale=0.7,
    modality_scale=3.0,
    skip_step=0,
)
```

## LoRA Configuration

LoRAs are configured at pipeline construction time:

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps

loras = [
    LoraPathStrengthAndSDOps(
        "/path/to/lora.safetensors",
        1.0,
        LTXV_LORA_COMFY_RENAMING_MAP
    ),
]
```

See [[python-lora-loading]] for full LoRA management details.

## See Also

- [[sdk-ltx-2]] -- LTX-2 monorepo overview and installation
- [[python-two-stage-pipeline]] -- Two-stage generation in detail
- [[python-guidance-parameters]] -- MultiModalGuiderParams reference
- [[python-ic-lora]] -- IC-LoRA pipeline details
- [[diffusers-integration]] -- Alternative Diffusers-based pipeline API
