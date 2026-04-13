# LTX-Video Hugging Face Model Card

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Fetched:** 2026-04-13

---

## Model Description

**LTX-Video** is the first DiT (Diffusion Transformer)-based video generation model capable of generating high-quality videos in real-time. It produces **30 FPS videos at 1216x704 resolution faster than real-time playback**.

- **Developer:** Lightricks
- **Model Type:** Diffusion-based image-to-video generation model
- **Language:** English
- **Repository:** [GitHub](https://github.com/Lightricks/LTX-Video)

---

## Model Versions & Architecture

| Version | Size | Key Features | Notes |
|---------|------|--------------|-------|
| **ltxv-13b-0.9.8-dev** | 13B | Highest quality | Requires more VRAM |
| **ltxv-13b-0.9.8-distilled** | 13B | Balanced speed-quality | Faster than dev |
| **ltxv-13b-0.9.8-fp8** | 13B | Quantized version | Reduced memory footprint |
| **ltxv-2b-0.9.8-distilled** | 2B | Lightweight | Ideal for limited VRAM |
| **ltxv-2b-0.9.8-distilled-fp8** | 2B | Quantized, lightweight | Minimal resources |
| **ltxv-2b-0.9.6-distilled** | 2B | Real-time capable | 15x faster, no STG/CFG required |
| **ICLoRA variants** | - | Conditional generation | Depth, Pose, Canny edge control |
| **Upscalers** | - | Quality enhancement | Spatial and temporal upscaling |

---

## Key Capabilities

### Supported Tasks
- Image-to-Video Generation
- Video-to-Video Generation (with conditioning)
- Multi-condition Generation (multiple images/videos at specific frames)
- Controlled Generation (depth, pose, canny edge conditioning via ICLoRA)

### Technical Specifications
- **Resolution:** Up to 720x1280 (model works best under this)
- **Frame Rate:** 30 FPS
- **Maximum Frames:** Up to 257 frames (divisible by 8+1)
- **Resolution Requirements:** Divisible by 32 (auto-padded if not)
- **Frame Requirements:** Divisible by 8+1 (e.g., 257 frames = 8x32+1)

---

## Installation & Setup

### Local Installation
```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video

# Create environment
python -m venv env
source env/bin/activate
python -m pip install -e .[inference-script]
```

### Diffusers Installation
```bash
pip install -U git+https://github.com/huggingface/diffusers
```

---

## Usage Instructions

### 1. Command Line Inference

**Image-to-Video:**
```bash
python inference.py \
  --prompt "PROMPT" \
  --input_image_path IMAGE_PATH \
  --height HEIGHT \
  --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

**Video-to-Video or Multi-Condition:**
```bash
python inference.py \
  --prompt "PROMPT" \
  --conditioning_media_paths IMAGE_OR_VIDEO_PATH_1 IMAGE_OR_VIDEO_PATH_2 \
  --conditioning_start_frames TARGET_FRAME_1 TARGET_FRAME_2 \
  --height HEIGHT \
  --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### 2. Python API - Image-to-Video

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video

# Load models
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

def round_to_nearest_resolution_acceptable_by_vae(height, width):
    height = height - (height % pipe.vae_spatial_compression_ratio)
    width = width - (width % pipe.vae_spatial_compression_ratio)
    return height, width

# Load and prepare image
image = load_image("https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/penguin.png")
video = load_video(export_to_video([image]))
condition1 = LTXVideoCondition(video=video, frame_index=0)

prompt = "A cute little penguin takes out a book and starts reading it"
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 480, 832
downscale_factor = 2 / 3
num_frames = 96

# Part 1: Generate at lower resolution
downscaled_height, downscaled_width = int(expected_height * downscale_factor), int(expected_width * downscale_factor)
downscaled_height, downscaled_width = round_to_nearest_resolution_acceptable_by_vae(downscaled_height, downscaled_width)

latents = pipe(
    conditions=[condition1],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=num_frames,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Part 2: Upscale using latent upsampler
upscaled_height, upscaled_width = downscaled_height * 2, downscaled_width * 2
upscaled_latents = pipe_upsample(
    latents=latents,
    output_type="latent"
).frames

# Part 3: Denoise upscaled video (optional but recommended)
video = pipe(
    conditions=[condition1],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=num_frames,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled_latents,
    decode_timestep=0.05,
    image_cond_noise_scale=0.025,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

# Part 4: Downscale to expected resolution
video = [frame.resize((expected_width, expected_height)) for frame in video]

export_to_video(video, "output.mp4", fps=24)
```

### 3. Python API - Video-to-Video

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_video

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

# Load video (use first 21 frames as conditioning)
video = load_video(
    "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/cosmos/cosmos-video2world-input-vid.mp4"
)[:21]
condition1 = LTXVideoCondition(video=video, frame_index=0)

prompt = "The video depicts a winding mountain road covered in snow..."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 768, 1152
downscale_factor = 2 / 3
num_frames = 161

# Follow same generation pipeline as image-to-video (Parts 1-4 identical)
```

---

## Prompt Guidelines

**Optimal Format:** Detailed, descriptive English prompts work best.

**Example:**
```
"The turquoise waves crash against the dark, jagged rocks of the shore, 
sending white foam spraying into the air. The scene is dominated by the stark 
contrast between the bright blue water and the dark, almost black rocks. 
The water is a clear, turquoise color, and the waves are capped with white foam. 
The rocks are dark and jagged, and they are covered in patches of green moss. 
The shore is lined with lush green vegetation, including trees and bushes. 
In the background, there are rolling hills covered in dense forest. 
The sky is cloudy, and the light is dim."
```

---

## Online Demo Platforms

- **LTX-Studio** (13B-mix): https://app.ltx.studio/motion-workspace?videoModel=ltxv-13b
- **LTX-Studio** (13B distilled): https://app.ltx.studio/motion-workspace?videoModel=ltxv
- **Fal.ai** (13B full): https://fal.ai/models/fal-ai/ltx-video-13b-dev/image-to-video
- **Fal.ai** (13B distilled): https://fal.ai/models/fal-ai/ltx-video-13b-distilled/image-to-video
- **Replicate**: https://replicate.com/lightricks/ltx-video

---

## ComfyUI Integration

Follow instructions at: [ComfyUI-LTXVideo GitHub](https://github.com/Lightricks/ComfyUI-LTXVideo/)

**Recommended Workflows:**
- `ltxv-13b-i2v-base.json` (13B full)
- `ltxv-13b-i2v-mixed-multiscale.json` (balanced speed-quality)
- `ltxv-13b-dist-i2v-base.json` (13B distilled)
- `ltxv-13b-i2v-base-fp8.json` (quantized)

---

## License

All model versions are available under the **LTX-Video-Open-Weights License**.
- [License for v0.9.8+](https://huggingface.co/Lightricks/LTX-Video/blob/main/LTX-Video-Open-Weights-License-0.X.txt)

---

## Model Performance & Limitations

### Capabilities
- Real-time video generation at 30 FPS
- High-quality video outputs
- Multi-condition video synthesis
- Minimal token requirements

### Limitations
- Not intended for factual information generation
- May amplify existing societal biases
- Perfect prompt matching not guaranteed
- Prompt following influenced by prompt style
- Works best below 720x1280 resolution
- Works best with fewer than 257 frames

### Hardware Requirements
- **Recommended:** GPU with 16GB+ VRAM (for 13B models)
- **Lightweight:** 8GB+ VRAM (for 2B distilled models)
- **Minimum:** 6GB VRAM (for 2B distilled-fp8)
- **Framework:** PyTorch >= 2.1.2
- **CUDA:** Version 12.2+
- **Python:** 3.10.5+

---

## Ecosystem & Community

- **Downloads Last Month:** 567,927
- **Model Adapters:** 24
- **Fine-tunes:** 25
- **Quantizations:** 16
- **Community Spaces:** 100+
- **Discussions:** 111+ active threads

---

## Documentation & Resources

- **Official Docs:** https://huggingface.co/docs/diffusers/main/en/api/pipelines/ltx_video
- **GitHub Repository:** https://github.com/Lightricks/LTX-Video
- **ComfyUI Integration:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Hugging Face Model Card:** https://huggingface.co/Lightricks/LTX-Video
