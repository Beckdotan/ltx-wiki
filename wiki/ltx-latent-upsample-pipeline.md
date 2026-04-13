---
title: LTXLatentUpsamplePipeline
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.7
tags:
  - diffusers
  - pipeline
  - upscaler
  - latent-upsampling
  - python
  - code-example
---

# LTXLatentUpsamplePipeline

`LTXLatentUpsamplePipeline` performs 2x spatial upscaling in latent space, introduced in v0.9.7. It is the key component of the [[two-stage-inference-pattern]].

## Available Upscaler Models

| Model ID | Version | Notes |
|----------|---------|-------|
| `Lightricks/ltxv-spatial-upscaler-0.9.7` | 0.9.7 | Standard upscaler |
| `Lightricks/ltxv-spatial-upscaler-0.9.8` | 0.9.8 | Latest upscaler |
| `a-r-r-o-w/LTX-0.9.8-Latent-Upsampler` | 0.9.8 | Alternative upsampler model |

## Basic Usage

The upscaler shares the VAE with the main generation pipeline:

```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae,
    torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipe_upsample.to("cuda")
pipeline.vae.enable_tiling()
```

## Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `latents` | None | Input latent tensor to upscale |
| `adain_factor` | 0.0 | AdaIN normalization strength (1.0 for distilled models) |
| `tone_map_compression_ratio` | 0.0 | Tone mapping compression (0.6 recommended for 0.9.8) |
| `output_type` | "latent" | Usually "latent" for further refinement |

## Loading with LTXLatentUpsamplerModel (0.9.8)

For 0.9.8, the upsampler model can be loaded separately:

```python
from diffusers.pipelines.ltx.modeling_latent_upsampler import LTXLatentUpsamplerModel

upsampler = LTXLatentUpsamplerModel.from_pretrained(
    "a-r-r-o-w/LTX-0.9.8-Latent-Upsampler", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline(
    vae=pipeline.vae, latent_upsampler=upsampler
).to(torch.bfloat16)
```

## Methods

- `enable_vae_tiling()` / `disable_vae_tiling()` -- memory-efficient tiled VAE processing
- `enable_vae_slicing()` / `disable_vae_slicing()` -- batch-sliced VAE processing
- `adain_filter_latent()` -- apply AdaIN normalization
- `tone_map_latents()` -- apply tone mapping compression

## Distilled vs Full Model Settings

| Setting | Full Model | Distilled Model |
|---------|-----------|----------------|
| `adain_factor` | 0.0 | 1.0 |
| `tone_map_compression_ratio` | 0.0 | 0.6 (0.9.8 only) |

## See Also

- [[two-stage-inference-pattern]] -- the recommended workflow using this pipeline
- [[ltx-condition-pipeline]] -- the main generation pipeline
- [[ltxv-video-vae]] -- the shared VAE component
- [[diffusers-pipeline-overview]] -- all pipeline classes
