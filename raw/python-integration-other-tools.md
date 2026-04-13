# LTX Video - Integration with Other Tools and Pipelines

> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
> **Source:** https://huggingface.co/Lightricks/LTX-Video

## Overview

LTX Video can be integrated with other tools, models, and workflows to build more complex video generation pipelines. This document covers combining LTX Video with other components in the Python ecosystem.

## Two-Stage Generation with Latent Upscaling

The most common integration pattern: generate low-res then upscale.

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video

# Load both pipelines sharing the VAE
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8",
    vae=pipe.vae,  # Share VAE to save memory
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")
pipe_upsample.to("cuda")
pipe.vae.enable_tiling()

# Stage 1: Generate at reduced resolution
latents = pipe(
    prompt="A dramatic ocean scene",
    width=512,
    height=320,
    num_frames=97,
    num_inference_steps=30,
    output_type="latent",
    generator=torch.Generator().manual_seed(42),
).frames

# Stage 2: Upscale latents (2x)
upscaled = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    tone_map_compression_ratio=0.6,
    output_type="latent",
).frames

# Stage 3: Refine upscaled video
video = pipe(
    prompt="A dramatic ocean scene",
    width=1024,
    height=640,
    num_frames=97,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled,
    output_type="pil",
    generator=torch.Generator().manual_seed(42),
).frames[0]

export_to_video(video, "output_upscaled.mp4", fps=24)
```

## Combining with Image Generation Models

### Generate First Frame with SDXL, Then Animate

```python
import torch
from diffusers import StableDiffusionXLPipeline, LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video

# Stage 1: Generate a high-quality first frame with SDXL
sdxl = StableDiffusionXLPipeline.from_pretrained(
    "stabilityai/stable-diffusion-xl-base-1.0",
    torch_dtype=torch.float16
).to("cuda")

first_frame = sdxl(
    prompt="A majestic dragon perched on a mountain peak, fantasy art, detailed",
    negative_prompt="low quality, blurry",
    width=1024,
    height=576,
    num_inference_steps=30,
).images[0]

# Free SDXL memory
del sdxl
torch.cuda.empty_cache()

# Stage 2: Animate with LTX Video
ltx = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
).to("cuda")
ltx.vae.enable_tiling()

# Resize first frame to LTX-compatible resolution
first_frame_resized = first_frame.resize((768, 432))

from diffusers.utils import load_video
video_input = load_video(export_to_video([first_frame_resized]))
condition = LTXVideoCondition(video=video_input, frame_index=0)

video = ltx(
    conditions=[condition],
    prompt="A majestic dragon spreads its wings and takes flight from the mountain peak, cinematic motion",
    negative_prompt="worst quality, blurry, jittery",
    width=768,
    height=432,
    num_frames=97,
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=5.0,
    generator=torch.Generator().manual_seed(0),
).frames[0]

export_to_video(video, "animated_dragon.mp4", fps=24)
```

### Generate First Frame with FLUX, Then Animate

```python
import torch
from diffusers import FluxPipeline, LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_video

# Stage 1: Generate with FLUX
flux = FluxPipeline.from_pretrained(
    "black-forest-labs/FLUX.1-schnell", torch_dtype=torch.bfloat16
).to("cuda")

image = flux(
    prompt="A peaceful Japanese garden with a koi pond, cherry blossoms",
    width=1024,
    height=576,
    num_inference_steps=4,
    guidance_scale=0.0,
).images[0]

del flux
torch.cuda.empty_cache()

# Stage 2: Animate with LTX Video
# (same pattern as SDXL example above)
```

## Building a Video Generation API

### FastAPI Integration

```python
import torch
from fastapi import FastAPI, BackgroundTasks
from pydantic import BaseModel
from diffusers import LTXPipeline
from diffusers.utils import export_to_video
import uuid
import os

app = FastAPI()

# Load pipeline at startup
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

class VideoRequest(BaseModel):
    prompt: str
    negative_prompt: str = "worst quality, blurry"
    width: int = 704
    height: int = 480
    num_frames: int = 81
    num_inference_steps: int = 50
    seed: int = 42

def generate_video(request: VideoRequest, output_path: str):
    video = pipeline(
        prompt=request.prompt,
        negative_prompt=request.negative_prompt,
        width=request.width,
        height=request.height,
        num_frames=request.num_frames,
        num_inference_steps=request.num_inference_steps,
        generator=torch.Generator("cuda").manual_seed(request.seed),
    ).frames[0]
    export_to_video(video, output_path, fps=24)

@app.post("/generate")
async def create_video(request: VideoRequest, background_tasks: BackgroundTasks):
    video_id = str(uuid.uuid4())
    output_path = f"outputs/{video_id}.mp4"
    os.makedirs("outputs", exist_ok=True)
    background_tasks.add_task(generate_video, request, output_path)
    return {"video_id": video_id, "status": "generating"}
```

## Combining with Video Post-Processing

### Using OpenCV for Post-Processing

```python
import torch
import cv2
import numpy as np
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

video = pipeline(
    prompt="A bustling city street at night",
    width=704,
    height=480,
    num_frames=81,
    num_inference_steps=50,
).frames[0]

# Convert PIL frames to numpy for OpenCV processing
processed_frames = []
for frame in video:
    np_frame = np.array(frame)
    # Apply color correction
    np_frame = cv2.convertScaleAbs(np_frame, alpha=1.2, beta=10)
    # Apply slight Gaussian blur for film effect
    np_frame = cv2.GaussianBlur(np_frame, (3, 3), 0.5)
    processed_frames.append(np_frame)

# Save processed video
height, width = processed_frames[0].shape[:2]
fourcc = cv2.VideoWriter_fourcc(*'mp4v')
out = cv2.VideoWriter('processed_output.mp4', fourcc, 24, (width, height))
for frame in processed_frames:
    out.write(cv2.cvtColor(frame, cv2.COLOR_RGB2BGR))
out.release()
```

### Adding Audio with MoviePy

```python
from moviepy.editor import VideoFileClip, AudioFileClip

# Generate video first with LTX...
# Then add audio
video_clip = VideoFileClip("output.mp4")
audio_clip = AudioFileClip("background_music.mp3").subclip(0, video_clip.duration)
final = video_clip.set_audio(audio_clip)
final.write_videofile("output_with_audio.mp4")
```

## ComfyUI Integration

LTX Video is available as ComfyUI nodes. The `LTXI2VLongMultiPromptPipeline` was specifically designed for ComfyUI parity.

For Diffusers-based ComfyUI parity:

```python
from diffusers import LTXI2VLongMultiPromptPipeline, LTXEulerAncestralRFScheduler

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
# Swap to RF scheduler for ComfyUI-matching results
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
```

## Working with GGUF Quantized Models

For deployment on consumer hardware:

```python
import torch
from diffusers import LTXPipeline, AutoModel, GGUFQuantizationConfig

# Load GGUF-quantized transformer
transformer = AutoModel.from_single_file(
    "https://huggingface.co/city96/LTX-Video-gguf/blob/main/ltx-video-2b-v0.9-Q3_K_S.gguf",
    quantization_config=GGUFQuantizationConfig(compute_dtype=torch.bfloat16),
    torch_dtype=torch.bfloat16
)

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    transformer=transformer,
    torch_dtype=torch.bfloat16
).to("cuda")
```

## Gradio Web UI

```python
import torch
import gradio as gr
from diffusers import LTXPipeline
from diffusers.utils import export_to_video
import tempfile
import os

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

def generate_video(prompt, negative_prompt, width, height, num_frames, steps, seed):
    video = pipeline(
        prompt=prompt,
        negative_prompt=negative_prompt,
        width=width,
        height=height,
        num_frames=num_frames,
        num_inference_steps=steps,
        generator=torch.Generator("cuda").manual_seed(seed),
    ).frames[0]
    
    output_path = os.path.join(tempfile.gettempdir(), "ltx_output.mp4")
    export_to_video(video, output_path, fps=24)
    return output_path

demo = gr.Interface(
    fn=generate_video,
    inputs=[
        gr.Textbox(label="Prompt", lines=3),
        gr.Textbox(label="Negative Prompt", value="worst quality, blurry"),
        gr.Slider(256, 1280, 704, step=32, label="Width"),
        gr.Slider(256, 720, 480, step=32, label="Height"),
        gr.Slider(9, 257, 81, step=8, label="Frames"),
        gr.Slider(1, 100, 50, step=1, label="Steps"),
        gr.Number(value=42, label="Seed"),
    ],
    outputs=gr.Video(label="Generated Video"),
    title="LTX Video Generator",
)

demo.launch()
```

## Deployment Considerations

### Memory Management for Production

```python
import torch
import gc

def generate_and_cleanup(pipeline, prompt, **kwargs):
    """Generate video with proper memory cleanup."""
    try:
        result = pipeline(prompt=prompt, **kwargs).frames[0]
        return result
    finally:
        torch.cuda.empty_cache()
        gc.collect()
```

### Concurrent Request Handling

For serving multiple requests, consider:

1. **Queue-based processing** -- Use a task queue (Celery, Redis Queue)
2. **Model sharing** -- Load model once, process requests sequentially
3. **Multiple workers** -- One GPU per worker for parallel processing
4. **Batch optimization** -- Collect similar-size requests for batching

### Cloud Deployment Options

- **Fal.ai** -- Official API-based inference for LTX Video
- **Replicate** -- Commercial hosting
- **RunPod** -- GPU cloud with Docker support
- **Modal** -- Serverless GPU compute
- **Amazon SageMaker** -- AWS managed inference
