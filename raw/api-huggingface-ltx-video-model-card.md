# Hugging Face Model Card: LTX-Video (Lightricks/LTX-Video)

## Sources
- https://huggingface.co/Lightricks/LTX-Video
- https://huggingface.co/Lightricks/LTX-Video/blob/main/README.md
- https://huggingface.co/Lightricks

---

## Overview

LTX-Video is the first DiT (Diffusion Transformer)-based video generation model capable of generating high-quality videos in real-time. It produces 30 FPS videos at 1216x704 resolution faster than they can be watched.

- **Developer:** Lightricks
- **Model type:** Diffusion-based image-to-video and video-to-video generation model
- **Language:** English prompts
- **Repository:** https://github.com/Lightricks/LTX-Video
- **Monthly downloads:** 567,927+
- **Community discussions:** 111 active
- **Adapter models:** 24
- **Fine-tunes:** 25
- **Quantizations:** 16
- **Community spaces:** 100+

---

## Model Variants

| Model | Notes | VRAM | Speed |
|-------|-------|------|-------|
| **ltxv-13b-0.9.8-dev** | Highest quality | High | Standard |
| **ltxv-13b-0.9.8-distilled** | Balanced speed-quality | Medium | 15x faster |
| **ltxv-2b-0.9.8-distilled** | Light VRAM usage | Low | Real-time capable |
| **ltxv-13b-0.9.8-fp8** | Quantized 13B | Lower | Standard |
| **ltxv-2b-0.9.8-distilled-fp8** | Quantized 2B | Lowest | Real-time |

---

## Technical Specifications

### Architecture
- **Type:** Diffusion Transformer (DiT)
- **Key Feature:** Video-VAE with higher pixel-to-latent compression ratio (1:192)
- **VAE Decoder:** Performs latent-to-pixel conversion AND the last denoising step
- **Conditioning:** Image-to-video and video-to-video capable
- **Quantization:** FP8 available for reduced VRAM

### Resolution and Frame Requirements
- **Resolution:** Width and height must be divisible by 32
- **Frames:** Must be divisible by 8 + 1 (e.g., 9, 17, 25, ..., 97, 121, 161, 257)
- **Recommended max:** Below 720x1280 resolution, below 257 frames
- **Output FPS:** 24 FPS (standard export)
- **Native generation:** 30 FPS at 1216x704

### System Requirements
- Python 3.10.5+
- CUDA 12.2+
- PyTorch >= 2.1.2
- macOS: PyTorch 2.3.0 for MPS support

---

## Installation

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference-script]
```

---

## Key Features

- Synchronized audio+video generation
- Multi-keyframe conditioning
- Image-to-video transformation
- Video extension (forward/backward)
- Multiple resolution support
- Real-time capable with distilled models
- LoRA fine-tuning support
- IC-LoRA control models (depth, pose, canny)

---

## Usage Examples

### Command Line: Image-to-Video

```bash
python inference.py \
  --prompt "A cute little penguin takes out a book and starts reading it" \
  --input_image_path IMAGE_PATH \
  --height 480 \
  --width 832 \
  --num_frames 96 \
  --seed 0 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Python Library Usage

```python
from ltx_video.inference import infer, InferenceConfig

infer(
  InferenceConfig(
    pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
    prompt="A cute penguin reading a book",
    height=480,
    width=832,
    num_frames=96,
    output_path="output.mp4",
  )
)
```

### Diffusers: Image-to-Video (Two-Stage)

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video

# Load pipelines
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", 
    torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8", 
    vae=pipe.vae, 
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")
pipe_upsample.to("cuda")
pipe.vae.enable_tiling()

# Prepare input
image = load_image("image_url_here")
video = load_video(export_to_video([image]))
condition1 = LTXVideoCondition(video=video, frame_index=0)

prompt = "A cute little penguin takes out a book and starts reading it"
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

# Stage 1: Generate at smaller resolution
latents = pipe(
    conditions=[condition1],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=554,   # downscaled
    height=320,  # downscaled
    num_frames=96,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Stage 2: Upscale (2x)
upscaled_latents = pipe_upsample(
    latents=latents,
    output_type="latent"
).frames

# Stage 3: Denoise upscaled video
video = pipe(
    conditions=[condition1],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=1108,
    height=640,
    num_frames=96,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled_latents,
    decode_timestep=0.05,
    image_cond_noise_scale=0.025,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

# Stage 4: Downscale to target
video = [frame.resize((832, 480)) for frame in video]
export_to_video(video, "output.mp4", fps=24)
```

---

## Prompt Engineering

Detailed, descriptive English prompts work best. Structure effective prompts with:

- Clear main action opening
- Specific movement details
- Precise character/object descriptions
- Environment and background specifics
- Camera angles and movements
- Lighting and color information
- Scene transitions or sudden events
- Keep under 200 words

Enable automatic enhancement via `enhance_prompt=True` parameter.

**Example effective prompt:**
```
The turquoise waves crash against the dark, jagged rocks of the shore, 
sending white foam spraying into the air. The scene is dominated by the 
stark contrast between the bright blue water and the dark, almost black rocks. 
The water is a clear, turquoise color, and the waves are capped with white foam. 
The rocks are dark and jagged, and they are covered in patches of green moss. 
The shore is lined with lush green vegetation, including trees and bushes. 
In the background, there are rolling hills covered in dense forest. 
The sky is cloudy, and the light is dim.
```

---

## Online Demos

- [LTX-Studio 13B Mix](https://app.ltx.studio/motion-workspace?videoModel=ltxv-13b)
- [LTX-Studio 13B Distilled](https://app.ltx.studio/motion-workspace?videoModel=ltxv)
- [Fal.ai 13B Full](https://fal.ai/models/fal-ai/ltx-video-13b-dev/image-to-video)
- [Fal.ai 13B Distilled](https://fal.ai/models/fal-ai/ltx-video-13b-distilled/image-to-video)
- [Replicate](https://replicate.com/lightricks/ltx-video)

---

## Limitations

- Not designed for factual information generation
- May amplify existing societal biases
- May not perfectly match prompts in all cases
- Prompt-following heavily influenced by prompting style
- Resolutions/frames not divisible by 32/8+1 will be padded and cropped

---

## Licensing

Multiple license versions exist for different model versions, all under **LTX-Video-Open-Weights-License-0.X** variants. See individual version license files on [Hugging Face](https://huggingface.co/Lightricks/LTX-Video/tree/main).
