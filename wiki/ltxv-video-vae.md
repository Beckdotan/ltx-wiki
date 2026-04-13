---
title: LTX Video-VAE (AutoencoderKLLTXVideo)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/models/autoencoderkl_ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - architecture
  - vae
  - video-vae
  - compression
  - model
---

# LTX Video-VAE (AutoencoderKLLTXVideo)

The `AutoencoderKLLTXVideo` is the 3D Variational Auto-Encoder with KL loss used across all LTX Video pipelines. It is one of the key architectural innovations of LTX Video, achieving a **1:192 pixel-to-latent compression ratio** that enables much faster generation compared to other video diffusion models.

A distinctive feature of the Video-VAE is that the decoder performs both the latent-to-pixel conversion **and** the last denoising step.

## Loading the VAE

```python
import torch
from diffusers import AutoencoderKLLTXVideo

vae = AutoencoderKLLTXVideo.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="vae",
    torch_dtype=torch.float32  # VAE can use float32 for better quality
).to("cuda")
```

## Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `in_channels` | int | 3 | Number of input channels |
| `out_channels` | int | 3 | Number of output channels |
| `latent_channels` | int | 128 | Number of latent channels |
| `block_out_channels` | tuple[int] | (128, 256, 512, 512) | Output channels per block |
| `spatio_temporal_scaling` | tuple[bool] | (True, True, True, False) | Per-block spatio-temporal downscaling |
| `layers_per_block` | tuple[int] | (4, 3, 3, 3, 4) | Layers per block |
| `patch_size` | int | 4 | Spatial patch size |
| `patch_size_t` | int | 1 | Temporal patch size |
| `resnet_norm_eps` | float | 1e-6 | ResNet normalization epsilon |
| `scaling_factor` | float | 1.0 | Latent space scaling factor |
| `encoder_causal` | bool | True | Causal encoder (future depends on past only) |
| `decoder_causal` | bool | False | Non-causal decoder by default |

## Compression Ratios

The Video-VAE achieves a compression ratio of **1:192**:
- Each latent element represents 192 pixel elements
- **Spatial compression:** 32x per dimension
- **Temporal compression:** Varies by block configuration

Key pipeline properties:
- `pipeline.vae_spatial_compression_ratio` -- spatial compression factor
- `pipeline.vae_temporal_compression_ratio` -- temporal compression factor

## Timestep-Aware Decoding (v0.9.1+)

The VAE performs the last denoising step during decode. This is controlled by two parameters passed to the pipeline call:

| Parameter | Recommended Value | Description |
|-----------|-------------------|-------------|
| `decode_timestep` | 0.05 | When to decode (closer to 0 = cleaner) |
| `decode_noise_scale` | 0.025 | Noise interpolation at decode time |

```python
video = pipeline(
    prompt="...",
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    # ...
).frames[0]
```

## Methods

### encode / decode

```python
# Encode video frames to latent space
latents = vae.encode(video_tensor).latent_dist.sample()

# Decode latents back to video frames
video = vae.decode(latents).sample
```

### enable_tiling / disable_tiling

Splits input into overlapping tiles for memory-efficient processing of large resolutions:

```python
vae.enable_tiling(
    tile_sample_min_height=256,
    tile_sample_min_width=256,
    tile_sample_stride_height=192,
    tile_sample_stride_width=192
)
```

### tiled_encode / tiled_decode

```python
latents = vae.tiled_encode(video_tensor)
decoded = vae.tiled_decode(latents, return_dict=True)
video = decoded.sample
```

## Usage in Pipelines

The VAE is shared between the main generation pipeline and the [[ltx-latent-upsample-pipeline|latent upsampler]]:

```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline

pipeline = LTXConditionPipeline.from_pretrained("Lightricks/LTX-Video-0.9.7-dev", ...)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae,  # Shared VAE
    ...
)
pipeline.vae.enable_tiling()  # Enable for both pipelines
```

## See Also

- [[video-vae]] -- concept-level overview of the Video-VAE design and innovations
- [[ltxv-transformer-architecture]] -- the denoising transformer
- [[ltxv-schedulers]] -- noise scheduling
- [[ltx-latent-upsample-pipeline]] -- latent upsampler that shares the VAE
- [[ltxv-memory-optimization]] -- VAE tiling and slicing for memory savings
- [[diffusers-pipeline-overview]] -- pipeline architecture
