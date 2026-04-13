---
title: LTXVideoTransformer3DModel
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/models/ltx_video_transformer3d
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - architecture
  - transformer
  - dit
  - diffusion
  - model
---

# LTXVideoTransformer3DModel

The `LTXVideoTransformer3DModel` is the core conditional Diffusion Transformer (DiT) architecture that denoises encoded video latents in the LTX Video pipeline. It is one of five components in every [[diffusers-pipeline-overview|LTX Video pipeline]].

## Architecture

The transformer operates on latent video representations from the [[ltxv-video-vae|Video-VAE]], conditioned on text embeddings from the T5 text encoder.

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
Output Video (RGB, 24-30 FPS)
```

## Loading the Transformer

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

## Constructor Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `in_channels` | int | 128 | Number of input channels |
| `out_channels` | int | 128 | Number of output channels |
| `patch_size` | int | 1 | Spatial patch size for patch embedding |
| `patch_size_t` | int | 1 | Temporal patch size for patch embedding |
| `num_attention_heads` | int | 32 | Number of multi-head attention heads |
| `attention_head_dim` | int | 64 | Channels per attention head |
| `cross_attention_dim` | int | 2048 | Channels for cross attention (matches T5 output) |
| `num_layers` | int | 28 | Number of Transformer block layers |
| `activation_fn` | str | "gelu-approximate" | Feed-forward activation function |
| `qk_norm` | str | "rms_norm_across_heads" | QK normalization layer type |

## Output

Returns `Transformer2DModelOutput` containing:
- `sample` (`torch.Tensor`) -- Hidden states conditioned on `encoder_hidden_states`. Shape: `(batch_size, num_channels, height, width)`.

## Model Sizes

- **2B parameter model** (original, v0.9-0.9.5): `num_layers=28`, suitable for consumer GPUs
- **13B parameter model** (v0.9.7+): Larger transformer with more layers, higher quality output

## Text Encoder: T5

LTX Video uses the **T5 v1.1 XXL** text encoder:

```python
from transformers import T5EncoderModel, T5TokenizerFast

text_encoder = T5EncoderModel.from_pretrained(
    "google/t5-v1_1-xxl", torch_dtype=torch.bfloat16
)
tokenizer = T5TokenizerFast.from_pretrained("google/t5-v1_1-xxl")
```

- Maximum sequence length: 128 tokens ([[ltx-pipeline-text-to-video|LTXPipeline]]) or 256 tokens ([[ltx-condition-pipeline|LTXConditionPipeline]])
- English language prompts recommended
- Elaborate, detailed descriptions produce the best results

## Latent Upsampler Model

The `LTXLatentUpsamplerModel` is a separate model for upscaling latents by 2x, used in the [[two-stage-inference-pattern]]:

```python
from diffusers.pipelines.ltx.modeling_latent_upsampler import LTXLatentUpsamplerModel

upsampler = LTXLatentUpsamplerModel.from_pretrained(
    "a-r-r-o-w/LTX-0.9.8-Latent-Upsampler", torch_dtype=torch.bfloat16
)
```

Features:
- **AdaIN filtering** -- Adaptive Instance Normalization for style matching
- **Tone mapping** -- Sigmoid-based compression (recommended `compression_ratio=0.6` for 0.9.8)

## See Also

- [[ltxv-video-vae]] -- the Video-VAE decoder
- [[ltxv-schedulers]] -- scheduler configuration
- [[diffusers-pipeline-overview]] -- pipeline architecture
- [[ltxv-model-variants]] -- 2B vs 13B models
