# Hugging Face Model Card: LTX-2 (Lightricks/LTX-2)

## Sources
- https://huggingface.co/Lightricks/LTX-2
- https://github.com/Lightricks/LTX-2
- https://arxiv.org/abs/2601.03233

---

## Overview

LTX-2 is a DiT-based audio-video foundation model designed to generate synchronized video and audio within a single model. It brings together modern video generation building blocks with open weights and a focus on practical, local execution.

- **Developer:** Lightricks
- **Model Type:** Diffusion-based audio-video foundation model
- **Language:** English
- **Paper:** [LTX-2: Efficient Joint Audio-Visual Foundation Model](https://arxiv.org/abs/2601.03233)
- **License:** LTX-2 Community License Agreement
- **GitHub:** https://github.com/Lightricks/LTX-2
- **Monthly downloads:** 907,185+
- **Community discussions:** 52

---

## Model Checkpoints

| Checkpoint | Details |
|-----------|---------|
| `ltx-2-19b-dev` | Full model, flexible and trainable in bf16 |
| `ltx-2-19b-dev-fp8` | Full model in fp8 quantization |
| `ltx-2-19b-dev-fp4` | Full model in nvfp4 quantization |
| `ltx-2-19b-distilled` | Distilled version, 8 steps, CFG=1 |
| `ltx-2-19b-distilled-lora-384` | LoRA version applicable to full model |
| `ltx-2-spatial-upscaler-x2-1.0` | x2 spatial upscaler for latents |
| `ltx-2-temporal-upscaler-x2-1.0` | x2 temporal upscaler for higher FPS |

---

## Technical Specifications

- **Python Version:** >= 3.12
- **CUDA Version:** > 12.7
- **PyTorch:** ~2.7
- **Parameters:** 19B

### Supported Pipelines
- Text-to-video
- Image-to-video
- Video-to-video
- Audio-to-video
- Text-to-audio
- And combinations thereof

### Resolution and Frame Requirements
- Width and height: Must be divisible by 32
- Frame count: Must be divisible by 8 + 1 (e.g., 121 frames)
- Padding with -1 if constraints not met

---

## Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync
source .venv/bin/activate
```

For attention optimization: `uv sync --extra xformers` or Flash Attention 3 for Hopper GPUs.

---

## Diffusers Usage: Two-Stage Text-to-Video

```python
import torch
from diffusers import FlowMatchEulerDiscreteScheduler
from diffusers.pipelines.ltx2 import LTX2Pipeline, LTX2LatentUpsamplePipeline
from diffusers.pipelines.ltx2.latent_upsampler import LTX2LatentUpsamplerModel
from diffusers.pipelines.ltx2.utils import STAGE_2_DISTILLED_SIGMA_VALUES
from diffusers.pipelines.ltx2.export_utils import encode_video

device = "cuda:0"

# Stage 1: Base generation
pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2", torch_dtype=torch.bfloat16
)
pipe.enable_sequential_cpu_offload(device=device)

prompt = "A beautiful sunset over the ocean"
negative_prompt = "shaky, glitchy, low quality, worst quality, deformed, distorted"

video_latent, audio_latent = pipe(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=768,
    height=512,
    num_frames=121,
    frame_rate=24.0,
    num_inference_steps=40,
    guidance_scale=4.0,
    output_type="latent",
    return_dict=False,
)

# Stage 2: Upsampling
latent_upsampler = LTX2LatentUpsamplerModel.from_pretrained(
    "Lightricks/LTX-2",
    subfolder="latent_upsampler",
    torch_dtype=torch.bfloat16,
)
upsample_pipe = LTX2LatentUpsamplePipeline(
    vae=pipe.vae, latent_upsampler=latent_upsampler
)
upsample_pipe.enable_model_cpu_offload(device=device)
upscaled_video_latent = upsample_pipe(
    latents=video_latent,
    output_type="latent",
    return_dict=False,
)[0]

# Stage 3: Refine with distilled LoRA
pipe.load_lora_weights(
    "Lightricks/LTX-2", adapter_name="stage_2_distilled",
    weight_name="ltx-2-19b-distilled-lora-384.safetensors"
)
pipe.set_adapters("stage_2_distilled", 1.0)
pipe.vae.enable_tiling()

new_scheduler = FlowMatchEulerDiscreteScheduler.from_config(
    pipe.scheduler.config, use_dynamic_shifting=False, shift_terminal=None
)
pipe.scheduler = new_scheduler

video, audio = pipe(
    latents=upscaled_video_latent,
    audio_latents=audio_latent,
    prompt=prompt,
    negative_prompt=negative_prompt,
    num_inference_steps=3,
    noise_scale=STAGE_2_DISTILLED_SIGMA_VALUES[0],
    sigmas=STAGE_2_DISTILLED_SIGMA_VALUES,
    guidance_scale=1.0,
    output_type="np",
    return_dict=False,
)

encode_video(
    video[0],
    fps=24.0,
    audio=audio[0].float().cpu(),
    audio_sample_rate=pipe.vocoder.config.output_sampling_rate,
    output_path="ltx2_output.mp4",
)
```

---

## Training and Fine-tuning

The base dev model is fully trainable.

### LoRA Training
- Motion, style, or likeness (sound+appearance) LoRAs can train in under 1 hour
- Easy reproduction using the [LTX-2 Trainer](https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md)

---

## Online Demos

- **Text-to-Video:** https://app.ltx.studio/ltx-2-playground/t2v
- **Image-to-Video:** https://app.ltx.studio/ltx-2-playground/i2v

---

## Limitations

- Not designed for factual information generation
- May amplify existing societal biases
- May not perfectly match prompts
- May generate inappropriate or offensive content
- Audio-only generation may produce lower quality audio

---

## Citation

```bibtex
@article{hacohen2025ltx2,
  title={LTX-2: Efficient Joint Audio-Visual Foundation Model},
  author={HaCohen, Yoav and Brazowski, Benny and Chiprut, Nisan and others},
  journal={arXiv preprint arXiv:2601.03233},
  year={2025}
}
```
