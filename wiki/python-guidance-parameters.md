---
title: Python Guidance Parameters
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
tags:
  - python
  - guidance
  - cfg
  - stg
  - parameters
---

# Python Guidance Parameters

Configuration for Classifier-Free Guidance (CFG), Spatio-Temporal Guidance (STG), and related parameters that control LTX-Video generation quality and prompt adherence.

## Standard Classifier-Free Guidance (Diffusers)

```python
# Higher guidance = more prompt adherence, less diversity
video = pipeline(
    prompt="...",
    guidance_scale=5.0,  # 3-7 is typical range
).frames[0]
```

## Guidance Rescale (Prevent Overexposure)

Prevents washed-out colors at high guidance values:

```python
video = pipeline(
    prompt="...",
    guidance_scale=7.0,
    guidance_rescale=0.7,  # 0.0-1.0, higher = more rescaling
).frames[0]
```

## MultiModalGuiderParams (LTX-2)

The [[sdk-ltx-2]] uses `MultiModalGuiderParams` for fine-grained control over both video and audio guidance:

```python
from ltx_core.components.guiders import MultiModalGuiderParams

video_guider_params = MultiModalGuiderParams(
    cfg_scale=3.0,         # Classifier-Free Guidance (2.0-5.0 typical; 1.0 disables)
    stg_scale=1.0,         # Spatio-Temporal Guidance (0.5-1.5 typical; 0.0 disables)
    stg_blocks=[29],       # Transformer blocks to perturb (e.g., [29] for last block)
    rescale_scale=0.7,     # Prevent over-saturation (0.5-0.7 typical; 0.0 disables)
    modality_scale=3.0,    # Audio-visual coherence (3.0 for audio-video; 1.0 to disable)
    skip_step=0,           # Skip guidance every N steps for speed
)

audio_guider_params = MultiModalGuiderParams(
    cfg_scale=7.0,
    stg_scale=1.0,
    rescale_scale=0.7,
    modality_scale=3.0,
    skip_step=0,
    stg_blocks=[29],
)
```

## Parameter Reference

| Parameter | Range | Function |
|-----------|-------|----------|
| `cfg_scale` | 2.0-5.0 | Classifier-Free Guidance; higher = more prompt adherence |
| `stg_scale` | 0.5-1.5 | Spatio-Temporal Guidance for temporal coherence |
| `stg_blocks` | e.g. `[29]` | Transformer blocks to perturb for STG |
| `rescale_scale` | 0.5-0.7 | Rescales guided prediction to prevent over-saturation |
| `modality_scale` | 1.0-3.0 | Audio-visual sync strength |
| `skip_step` | 0+ | Skip guidance every N steps for speed |

## Sampling Recommendations

| Model Type | Steps | CFG Scale |
|-----------|-------|-----------|
| Distilled | 4-8 | 1.0 (required) |
| Full (dev) | 20-50 | 2.0-5.0 |
| General recommendation | -- | 3.0-3.5 |

## See Also

- [[python-schedulers]] -- Scheduler configuration and timestep schedules
- [[python-conditioning]] -- Conditioning strength and noise scale
- [[ltx-2-pipeline-api]] -- Full LTX-2 pipeline parameter reference
