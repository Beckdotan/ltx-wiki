# LTX Video - Transformer and VAE Model Architecture

> **Source:** https://huggingface.co/docs/diffusers/api/models/ltx_video_transformer3d
> **Source:** https://huggingface.co/docs/diffusers/api/models/autoencoderkl_ltx_video
> **Source:** https://huggingface.co/Lightricks/LTX-Video

## LTXVideoTransformer3DModel

A Diffusion Transformer model for 3D video data used in the LTX Video pipeline. This is the core denoising model.

### Loading the Transformer

```python
import torch
from diffusers import LTXVideoTransformer3DModel

transformer = LTXVideoTransformer3DModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
).to("cuda")
```

Or using AutoModel:

```python
from diffusers import AutoModel

transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
).to("cuda")
```

### Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `in_channels` | `int` | `128` | Number of channels in the input |
| `out_channels` | `int` | `128` | Number of channels in the output |
| `patch_size` | `int` | `1` | Spatial patch size for patch embedding |
| `patch_size_t` | `int` | `1` | Temporal patch size for patch embedding |
| `num_attention_heads` | `int` | `32` | Number of multi-head attention heads |
| `attention_head_dim` | `int` | `64` | Channels per attention head |
| `cross_attention_dim` | `int` | `2048` | Channels for cross attention |
| `num_layers` | `int` | `28` | Number of Transformer block layers |
| `activation_fn` | `str` | `"gelu-approximate"` | Feed-forward activation function |
| `qk_norm` | `str` | `"rms_norm_across_heads"` | QK normalization layer type |

### Output

Returns `Transformer2DModelOutput` containing:
- `sample` (`torch.Tensor`) -- Hidden states output conditioned on `encoder_hidden_states`. Shape: `(batch_size, num_channels, height, width)`.

### Model Sizes

- **2B parameter model** (original): `num_layers=28`, suitable for consumer GPUs
- **13B parameter model** (0.9.7+): Larger transformer with more layers, higher quality

## AutoencoderKLLTXVideo (Video-VAE)

The 3D Variational Auto-Encoder (VAE) with KL loss used in LTX Video. This is one of the key innovations of LTX Video -- it achieves a 1:192 pixel-to-latent compression ratio, enabling much faster generation.

### Loading the VAE

```python
import torch
from diffusers import AutoencoderKLLTXVideo

vae = AutoencoderKLLTXVideo.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="vae",
    torch_dtype=torch.float32  # VAE can use float32 for better quality
).to("cuda")
```

### Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `in_channels` | `int` | `3` | Number of input channels |
| `out_channels` | `int` | `3` | Number of output channels |
| `latent_channels` | `int` | `128` | Number of latent channels |
| `block_out_channels` | `tuple[int]` | `(128, 256, 512, 512)` | Output channels per block |
| `spatio_temporal_scaling` | `tuple[bool]` | `(True, True, True, False)` | Per-block spatio-temporal downscaling |
| `layers_per_block` | `tuple[int]` | `(4, 3, 3, 3, 4)` | Layers per block |
| `patch_size` | `int` | `4` | Spatial patch size |
| `patch_size_t` | `int` | `1` | Temporal patch size |
| `resnet_norm_eps` | `float` | `1e-6` | ResNet normalization epsilon |
| `scaling_factor` | `float` | `1.0` | Latent space scaling factor |
| `encoder_causal` | `bool` | `True` | Causal encoder (future depends on past only) |
| `decoder_causal` | `bool` | `False` | Non-causal decoder by default |

### Key Feature: Timestep-Aware Decoding

The Video-VAE performs the latent-to-pixel conversion *and* the last denoising step. This is controlled by:
- `decode_timestep` -- The timestep at which video is decoded (recommended: `0.05` for v0.9.1+)
- `decode_noise_scale` -- Noise interpolation factor during decode (recommended: `0.025`)

### Methods

#### encode

```python
# Encode video frames to latent space
latents = vae.encode(video_tensor).latent_dist.sample()
```

#### decode

```python
# Decode latents back to video frames
video = vae.decode(latents).sample
```

#### enable_tiling / disable_tiling

Splits input into overlapping tiles for memory-efficient processing:

```python
vae.enable_tiling(
    tile_sample_min_height=256,     # Min height before tiling
    tile_sample_min_width=256,      # Min width before tiling
    tile_sample_stride_height=192,  # Overlap between vertical tiles
    tile_sample_stride_width=192    # Overlap between horizontal tiles
)
```

#### tiled_encode

```python
latents = vae.tiled_encode(video_tensor)
```

#### tiled_decode

```python
decoded = vae.tiled_decode(latents, return_dict=True)
video = decoded.sample
```

### VAE Compression Ratios

The LTX Video-VAE achieves a compression ratio of **1:192**, meaning:
- Each latent element represents 192 pixel elements
- Spatial compression: 32x (each spatial dimension)
- Temporal compression: Varies by block configuration

Key pipeline properties:
- `pipeline.vae_spatial_compression_ratio` -- Spatial compression factor
- `pipeline.vae_temporal_compression_ratio` -- Temporal compression factor

## Text Encoder: T5

LTX Video uses the **T5 v1.1 XXL** text encoder:

```python
from transformers import T5EncoderModel, T5TokenizerFast

text_encoder = T5EncoderModel.from_pretrained(
    "google/t5-v1_1-xxl", torch_dtype=torch.bfloat16
)
tokenizer = T5TokenizerFast.from_pretrained("google/t5-v1_1-xxl")
```

- Maximum sequence length: 128 tokens (LTXPipeline) or 256 tokens (LTXConditionPipeline)
- English language prompts recommended
- Elaborate, detailed descriptions produce the best results

## Scheduler: FlowMatchEulerDiscreteScheduler

The default scheduler used with LTX Video:

```python
from diffusers import FlowMatchEulerDiscreteScheduler

scheduler = FlowMatchEulerDiscreteScheduler()
```

Alternative scheduler for long video generation with ComfyUI parity:

```python
from diffusers import LTXEulerAncestralRFScheduler

scheduler = LTXEulerAncestralRFScheduler.from_config(pipeline.scheduler.config)
pipeline.scheduler = scheduler
```

## Latent Upsampler Model

The `LTXLatentUpsamplerModel` is a separate model for upscaling latents by 2x:

```python
from diffusers.pipelines.ltx.modeling_latent_upsampler import LTXLatentUpsamplerModel

upsampler = LTXLatentUpsamplerModel.from_pretrained(
    "a-r-r-o-w/LTX-0.9.8-Latent-Upsampler",
    torch_dtype=torch.bfloat16
)
```

Features:
- **AdaIN filtering** -- Adaptive Instance Normalization for style matching between low-res and upscaled latents
- **Tone mapping** -- Reduces dynamic range using sigmoid-based compression (recommended `compression_ratio=0.6` for 0.9.8)

## Architecture Summary

```
Text Prompt
    |
    v
T5 Text Encoder (T5-v1.1-XXL)
    |
    v
Text Embeddings (cross_attention_dim=2048)
    |
    v
LTXVideoTransformer3DModel (28+ layers, DiT)
    |    - Flow Matching denoising
    |    - Classifier-free guidance
    |    - FlowMatchEulerDiscreteScheduler
    v
Latent Video (128 channels, 1:192 compression)
    |
    v  [Optional: LTXLatentUpsamplerModel (2x)]
    |
    v
AutoencoderKLLTXVideo (Video-VAE)
    |    - Timestep-aware decoding
    |    - Final denoising step integrated
    v
Output Video (RGB, 30 FPS)
```
