---
title: LTXPipeline (Text-to-Video)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - diffusers
  - pipeline
  - text-to-video
  - python
  - code-example
---

# LTXPipeline (Text-to-Video)

`LTXPipeline` is the base Diffusers pipeline class for generating video from text prompts. It is the simplest entry point for LTX Video generation.

For the more flexible multi-condition approach (0.9.5+), see [[ltx-condition-pipeline]].

## Basic Usage

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

prompt = "A woman with long brown hair and light skin smiles at another woman with long blonde hair. The woman with brown hair wears a black jacket and has a small, barely noticeable mole on her right cheek. The camera angle is a close-up, focused on the woman with brown hair's face. The lighting is warm and natural, likely from the setting sun, casting a soft glow on the scene."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

video = pipe(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

## Call Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `prompt` | None | Text prompt(s) guiding generation |
| `negative_prompt` | None | Prompts to suppress undesired content |
| `height` | 512 | Output height (divisible by 32) |
| `width` | 704 | Output width (divisible by 32) |
| `num_frames` | 161 | Number of frames (8n+1 pattern) |
| `frame_rate` | 25 | Frames per second |
| `num_inference_steps` | 50 | Denoising steps |
| `timesteps` | None | Custom timestep schedule (overrides num_inference_steps) |
| `guidance_scale` | 3.0 | CFG scale; >1 enables classifier-free guidance |
| `guidance_rescale` | 0.0 | Rescale to mitigate overexposure |
| `num_videos_per_prompt` | 1 | Number of videos per prompt |
| `decode_timestep` | 0.0 | When to decode (0.05 for v0.9.1+) |
| `decode_noise_scale` | None | Noise interpolation at decode (0.025 for v0.9.1+) |
| `max_sequence_length` | 128 | Max tokenizer length |
| `output_type` | "pil" | "pil", "np", "pt", or "latent" |

## Memory-Optimized Text-to-Video (~10GB VRAM)

Uses FP8 layerwise weight-casting and group offloading. See [[ltxv-memory-optimization]] for details.

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading
from diffusers.utils import export_to_video

transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video", subfolder="transformer", torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn, compute_dtype=torch.bfloat16
)

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", transformer=transformer, torch_dtype=torch.bfloat16
)

onload_device = torch.device("cuda")
offload_device = torch.device("cpu")
pipeline.transformer.enable_group_offload(
    onload_device=onload_device, offload_device=offload_device,
    offload_type="leaf_level", use_stream=True
)
apply_group_offloading(
    pipeline.text_encoder, onload_device=onload_device,
    offload_type="block_level", num_blocks_per_group=2
)
apply_group_offloading(
    pipeline.vae, onload_device=onload_device, offload_type="leaf_level"
)

video = pipeline(
    prompt="A woman smiles warmly...",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=768, height=512, num_frames=161,
    decode_timestep=0.03, decode_noise_scale=0.025,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

## Text-to-Video with LTXConditionPipeline (0.9.5+)

The [[ltx-condition-pipeline|LTXConditionPipeline]] also supports text-only generation by omitting the `conditions` parameter:

```python
from diffusers import LTXConditionPipeline
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipeline.vae.enable_tiling()

video = pipeline(
    prompt="A winding mountain road covered in snow...",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=768, height=512, num_frames=161,
    num_inference_steps=30,
    decode_timestep=0.05, decode_noise_scale=0.025,
    guidance_scale=5.0, guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

## GGUF Quantized Model Loading

Load community-quantized GGUF models for reduced storage:

```python
from diffusers import LTXPipeline, AutoModel, GGUFQuantizationConfig

transformer = AutoModel.from_single_file(
    "https://huggingface.co/city96/LTX-Video-gguf/blob/main/ltx-video-2b-v0.9-Q3_K_S.gguf",
    quantization_config=GGUFQuantizationConfig(compute_dtype=torch.bfloat16),
    torch_dtype=torch.bfloat16
)
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", transformer=transformer, torch_dtype=torch.bfloat16
)
```

## See Also

- [[diffusers-pipeline-overview]] -- all pipeline classes
- [[ltx-condition-pipeline]] -- flexible multi-condition pipeline (recommended for 0.9.5+)
- [[two-stage-inference-pattern]] -- upscaled generation workflow
- [[ltxv-memory-optimization]] -- VRAM reduction techniques
- [[ltxv-schedulers]] -- scheduler configuration
