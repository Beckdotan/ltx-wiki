---
title: Python Integration Patterns
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/Lightricks/LTX-Video
  - https://modal.com/docs/examples/ltx
  - https://github.com/Lightricks/ComfyUI-LTXVideo
tags:
  - python
  - integration
  - fastapi
  - gradio
  - deployment
  - opencv
  - comfyui
---

# Python Integration Patterns

Patterns for integrating LTX-Video into larger Python applications, web services, and deployment platforms.

## Combining with Image Generation Models

### Generate First Frame with SDXL, Then Animate

```python
import torch
from diffusers import StableDiffusionXLPipeline, LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

# Stage 1: Generate first frame with SDXL
sdxl = StableDiffusionXLPipeline.from_pretrained(
    "stabilityai/stable-diffusion-xl-base-1.0", torch_dtype=torch.float16
).to("cuda")

first_frame = sdxl(
    prompt="A majestic dragon perched on a mountain peak",
    width=1024, height=576, num_inference_steps=30,
).images[0]

del sdxl
torch.cuda.empty_cache()

# Stage 2: Animate with LTX Video
ltx = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
).to("cuda")

first_frame_resized = first_frame.resize((768, 432))
condition = LTXVideoCondition(video=load_video(export_to_video([first_frame_resized])), frame_index=0)

video = ltx(
    conditions=[condition],
    prompt="A majestic dragon spreads its wings and takes flight",
    width=768, height=432, num_frames=97,
    num_inference_steps=30,
).frames[0]
```

The same pattern works with FLUX (`FluxPipeline`) or any image generation model.

## FastAPI Service

```python
from fastapi import FastAPI, BackgroundTasks
from pydantic import BaseModel
import torch, uuid
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

app = FastAPI()
pipe = None

@app.on_event("startup")
async def startup():
    global pipe
    pipe = LTXPipeline.from_pretrained(
        "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
    ).to("cuda")

class GenerationRequest(BaseModel):
    prompt: str
    width: int = 704
    height: int = 480
    num_frames: int = 161
    seed: int = 42

@app.post("/generate")
async def create_generation(request: GenerationRequest, background_tasks: BackgroundTasks):
    job_id = str(uuid.uuid4())
    background_tasks.add_task(generate_video, job_id, request)
    return {"job_id": job_id, "status": "processing"}
```

**Important:** Diffusers pipelines are NOT thread-safe. Use process-level parallelism or a task queue (Celery, Redis Queue) for concurrent requests.

## Gradio Web UI

```python
import torch, gradio as gr, tempfile, os
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

def generate_video(prompt, negative_prompt, width, height, num_frames, steps, seed):
    video = pipeline(
        prompt=prompt, negative_prompt=negative_prompt,
        width=width, height=height, num_frames=num_frames,
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

## OpenCV Post-Processing

```python
import cv2, numpy as np

# After generating video frames with LTX...
processed_frames = []
for frame in video:
    np_frame = np.array(frame)
    np_frame = cv2.convertScaleAbs(np_frame, alpha=1.2, beta=10)
    np_frame = cv2.GaussianBlur(np_frame, (3, 3), 0.5)
    processed_frames.append(np_frame)

height, width = processed_frames[0].shape[:2]
fourcc = cv2.VideoWriter_fourcc(*'mp4v')
out = cv2.VideoWriter('processed_output.mp4', fourcc, 24, (width, height))
for frame in processed_frames:
    out.write(cv2.cvtColor(frame, cv2.COLOR_RGB2BGR))
out.release()
```

## Adding Audio with MoviePy

```python
from moviepy.editor import VideoFileClip, AudioFileClip

video_clip = VideoFileClip("output.mp4")
audio_clip = AudioFileClip("background_music.mp3").subclip(0, video_clip.duration)
final = video_clip.set_audio(audio_clip)
final.write_videofile("output_with_audio.mp4")
```

## ComfyUI Programmatic Access

```python
import json, requests

COMFYUI_URL = "http://localhost:8188"

def queue_workflow(workflow_json: dict):
    payload = {"prompt": workflow_json}
    response = requests.post(f"{COMFYUI_URL}/prompt", json=payload)
    return response.json()

# Modify and submit workflows programmatically
with open("ltx_video_workflow.json") as f:
    workflow = json.load(f)
workflow["6"]["inputs"]["text"] = "A beautiful landscape"
result = queue_workflow(workflow)
```

## Cloud Deployment (Modal)

```python
import modal

app = modal.App("ltx-video")
image = modal.Image.debian_slim().pip_install(
    "torch", "diffusers", "transformers", "accelerate"
)

@app.cls(gpu="A10G", image=image)
class LTXVideoGenerator:
    @modal.enter()
    def setup(self):
        from diffusers import LTXPipeline
        self.pipe = LTXPipeline.from_pretrained(
            "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
        ).to("cuda")

    @modal.method()
    def generate(self, prompt: str, seed: int = 42) -> bytes:
        # Generate and return video bytes
        ...
```

## Other Cloud Options

- **Fal.ai** -- official API-based inference for LTX Video
- **Replicate** -- commercial hosting
- **RunPod** -- GPU cloud with Docker support
- **Amazon SageMaker** -- AWS managed inference

## Production Considerations

### Memory Management

```python
def generate_and_cleanup(pipeline, prompt, **kwargs):
    try:
        return pipeline(prompt=prompt, **kwargs).frames[0]
    finally:
        torch.cuda.empty_cache()
        gc.collect()
```

### Thread Safety

- Diffusers pipelines are NOT thread-safe
- Use a task queue (Celery, background tasks) for concurrent requests
- One GPU per worker for parallel processing

### Resolution Constraints

- Width and height must be divisible by 32
- Frame count must follow 8n+1 pattern
- Lower resolution + [[python-two-stage-pipeline]] upscale is the best quality/speed tradeoff

## See Also

- [[python-two-stage-pipeline]] -- Two-stage generation with latent upscaling
- [[python-batch-generation]] -- Batch processing patterns
- [[python-performance-memory]] -- Memory management for production
- [[diffusers-integration]] -- Diffusers pipeline API overview
