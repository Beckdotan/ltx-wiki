# LTX Video Integration in Larger Python Pipelines

> **Sources:**
> - https://github.com/Lightricks/LTX-2
> - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
> - https://github.com/Lightricks/ComfyUI-LTXVideo
> - https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
> - https://modal.com/docs/examples/ltx
> - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video

---

## LTX-2 Monorepo Architecture

The LTX-2 repository is organized as a monorepo with three packages that can be used independently or together:

```
LTX-2/
  packages/
    ltx-core/       # Core model implementation, inference stack, utilities
    ltx-pipelines/  # High-level pipeline implementations
    ltx-trainer/    # Training and fine-tuning tools
```

### ltx-core

The foundational package containing building blocks:
- **Schedulers** -- Noise scheduling algorithms
- **Guiders** -- Guidance mechanisms (CFG, STG, multimodal)
- **Noisers** -- Noise injection utilities
- **Patchifiers** -- Token patchification for the transformer
- **Quantization** -- FP8 quantization policies
- **Loader** -- Model loading and LoRA management

### ltx-pipelines

High-level pipeline implementations that compose ltx-core components:

| Pipeline | Description | Use Case |
|----------|-------------|----------|
| `TI2VidTwoStagesPipeline` | Two-stage text/image-to-video with 2x upsampling | Production use |
| `TI2VidTwoStagesHQPipeline` | Two-stage with res_2s second-order sampler | Higher quality |
| `TI2VidOneStagePipeline` | Single-stage, no upsampling | Quick prototyping |
| `DistilledPipeline` | 8 predefined sigmas, fastest inference | Batch processing |
| `ICLoraPipeline` | IC-LoRA support for vid2vid/img2vid | Style transfer |
| `KeyframeInterpolationPipeline` | Interpolate between keyframes | Animation |
| `A2VidPipelineTwoStage` | Audio-to-video generation | Audio sync |
| `RetakePipeline` | Region-specific video regeneration | Editing |

### ltx-trainer

Training and fine-tuning tools for LoRA, full fine-tuning, and IC-LoRA.

---

## Integration Pattern 1: Diffusers in a Python Application

### Basic Integration

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video
from pathlib import Path


class VideoGenerator:
    """Wrapper for LTX Video generation in a larger application."""

    def __init__(self, model_id="Lightricks/LTX-Video", device="cuda"):
        self.pipe = LTXPipeline.from_pretrained(
            model_id, torch_dtype=torch.bfloat16
        )
        self.pipe.to(device)
        self.device = device

    def generate(
        self,
        prompt: str,
        output_path: str,
        width: int = 704,
        height: int = 480,
        num_frames: int = 161,
        num_inference_steps: int = 50,
        seed: int = 42,
        negative_prompt: str = "worst quality, inconsistent motion, blurry, jittery, distorted",
    ) -> str:
        generator = torch.Generator(self.device).manual_seed(seed)
        video = self.pipe(
            prompt=prompt,
            negative_prompt=negative_prompt,
            width=width,
            height=height,
            num_frames=num_frames,
            num_inference_steps=num_inference_steps,
            generator=generator,
        ).frames[0]
        export_to_video(video, output_path, fps=24)
        return output_path

    def cleanup(self):
        torch.cuda.empty_cache()


# Usage
gen = VideoGenerator()
gen.generate("A sunset over the ocean", "output.mp4")
gen.cleanup()
```

### Multi-Condition Integration

```python
import torch
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video
from PIL import Image
from typing import Optional


class AdvancedVideoGenerator:
    """Video generator with image/video conditioning support."""

    def __init__(self, model_id="Lightricks/LTX-Video-0.9.5"):
        self.pipe = LTXConditionPipeline.from_pretrained(
            model_id, torch_dtype=torch.bfloat16
        ).to("cuda")

    def text_to_video(self, prompt: str, **kwargs) -> list:
        return self.pipe(prompt=prompt, **kwargs).frames[0]

    def image_to_video(
        self,
        prompt: str,
        image_path: str,
        frame_index: int = 0,
        **kwargs
    ) -> list:
        image = load_image(image_path)
        condition = LTXVideoCondition(image=image, frame_index=frame_index)
        return self.pipe(
            conditions=[condition],
            prompt=prompt,
            **kwargs
        ).frames[0]

    def video_to_video(
        self,
        prompt: str,
        video_path: str,
        denoise_strength: float = 0.6,
        **kwargs
    ) -> list:
        video = load_video(video_path)
        condition = LTXVideoCondition(video=video, frame_index=0)
        return self.pipe(
            conditions=[condition],
            prompt=prompt,
            denoise_strength=denoise_strength,
            **kwargs
        ).frames[0]

    def interpolate(
        self,
        prompt: str,
        start_image_path: str,
        end_image_path: str,
        num_frames: int = 161,
        **kwargs
    ) -> list:
        start_img = load_image(start_image_path)
        end_img = load_image(end_image_path)
        condition1 = LTXVideoCondition(image=start_img, frame_index=0)
        condition2 = LTXVideoCondition(image=end_img, frame_index=num_frames - 1)
        return self.pipe(
            conditions=[condition1, condition2],
            prompt=prompt,
            num_frames=num_frames,
            **kwargs
        ).frames[0]
```

---

## Integration Pattern 2: LTX-2 Pipeline API

### Using the LTX-2 Python API Directly

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline, ImageConditioningInput
from ltx_core.components.guiders import MultiModalGuiderParams
from ltx_core.quantization import QuantizationPolicy


class LTX2VideoGenerator:
    """Production-grade video generator using LTX-2 pipeline."""

    def __init__(
        self,
        checkpoint_path: str,
        gemma_root: str,
        upsampler_path: str,
        distilled_lora_path: str,
        use_fp8: bool = False,
    ):
        distilled_lora = [
            LoraPathStrengthAndSDOps(
                distilled_lora_path, 0.6, LTXV_LORA_COMFY_RENAMING_MAP
            ),
        ]

        quantization = QuantizationPolicy.fp8_cast() if use_fp8 else None

        self.pipeline = TI2VidTwoStagesPipeline(
            checkpoint_path=checkpoint_path,
            distilled_lora=distilled_lora,
            spatial_upsampler_path=upsampler_path,
            gemma_root=gemma_root,
            loras=[],
            quantization=quantization,
        )

        self.default_video_guider = MultiModalGuiderParams(
            cfg_scale=3.0,
            stg_scale=1.0,
            rescale_scale=0.7,
            modality_scale=3.0,
            skip_step=0,
            stg_blocks=[29],
        )

        self.default_audio_guider = MultiModalGuiderParams(
            cfg_scale=7.0,
            stg_scale=1.0,
            rescale_scale=0.7,
            modality_scale=3.0,
            skip_step=0,
            stg_blocks=[29],
        )

    def generate(
        self,
        prompt: str,
        output_path: str,
        seed: int = 42,
        height: int = 512,
        width: int = 768,
        num_frames: int = 121,
        num_inference_steps: int = 40,
        images: list = None,
    ):
        self.pipeline(
            prompt=prompt,
            output_path=output_path,
            seed=seed,
            height=height,
            width=width,
            num_frames=num_frames,
            frame_rate=25.0,
            num_inference_steps=num_inference_steps,
            video_guider_params=self.default_video_guider,
            audio_guider_params=self.default_audio_guider,
            images=images or [],
        )
        return output_path
```

---

## Integration Pattern 3: Batch Processing Pipeline

```python
import json
import torch
from pathlib import Path
from dataclasses import dataclass
from diffusers import LTXPipeline
from diffusers.utils import export_to_video


@dataclass
class VideoJob:
    prompt: str
    output_name: str
    width: int = 704
    height: int = 480
    num_frames: int = 161
    seed: int = 42


class BatchVideoProcessor:
    """Process multiple video generation jobs efficiently."""

    def __init__(self, model_id="Lightricks/LTX-Video", output_dir="./outputs"):
        self.pipe = LTXPipeline.from_pretrained(
            model_id, torch_dtype=torch.bfloat16
        ).to("cuda")
        self.output_dir = Path(output_dir)
        self.output_dir.mkdir(exist_ok=True)

    def process_jobs(self, jobs: list[VideoJob]) -> list[dict]:
        results = []
        for i, job in enumerate(jobs):
            print(f"Processing job {i+1}/{len(jobs)}: {job.output_name}")
            try:
                generator = torch.Generator("cuda").manual_seed(job.seed)
                video = self.pipe(
                    prompt=job.prompt,
                    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
                    width=job.width,
                    height=job.height,
                    num_frames=job.num_frames,
                    num_inference_steps=50,
                    generator=generator,
                ).frames[0]

                output_path = self.output_dir / f"{job.output_name}.mp4"
                export_to_video(video, str(output_path), fps=24)
                results.append({"job": job.output_name, "status": "success", "path": str(output_path)})
            except Exception as e:
                results.append({"job": job.output_name, "status": "error", "error": str(e)})
            finally:
                torch.cuda.empty_cache()

        return results

    def process_from_json(self, json_path: str) -> list[dict]:
        with open(json_path) as f:
            job_data = json.load(f)
        jobs = [VideoJob(**j) for j in job_data]
        return self.process_jobs(jobs)


# Usage
processor = BatchVideoProcessor()
jobs = [
    VideoJob(prompt="A sunset over the ocean", output_name="sunset"),
    VideoJob(prompt="A cat playing with yarn", output_name="cat"),
    VideoJob(prompt="A time-lapse of clouds", output_name="clouds"),
]
results = processor.process_jobs(jobs)
```

---

## Integration Pattern 4: REST API Service

```python
"""Example FastAPI service wrapping LTX Video generation."""
from fastapi import FastAPI, BackgroundTasks
from pydantic import BaseModel
import torch
import uuid
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

app = FastAPI()

# Initialize pipeline once at startup
pipe = None

@app.on_event("startup")
async def startup():
    global pipe
    pipe = LTXPipeline.from_pretrained(
        "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
    ).to("cuda")


class GenerationRequest(BaseModel):
    prompt: str
    negative_prompt: str = "worst quality, inconsistent motion, blurry, jittery, distorted"
    width: int = 704
    height: int = 480
    num_frames: int = 161
    num_inference_steps: int = 50
    seed: int = 42


class GenerationResponse(BaseModel):
    job_id: str
    status: str


def generate_video(job_id: str, request: GenerationRequest):
    generator = torch.Generator("cuda").manual_seed(request.seed)
    video = pipe(
        prompt=request.prompt,
        negative_prompt=request.negative_prompt,
        width=request.width,
        height=request.height,
        num_frames=request.num_frames,
        num_inference_steps=request.num_inference_steps,
        generator=generator,
    ).frames[0]
    export_to_video(video, f"outputs/{job_id}.mp4", fps=24)
    torch.cuda.empty_cache()


@app.post("/generate", response_model=GenerationResponse)
async def create_generation(request: GenerationRequest, background_tasks: BackgroundTasks):
    job_id = str(uuid.uuid4())
    background_tasks.add_task(generate_video, job_id, request)
    return GenerationResponse(job_id=job_id, status="processing")
```

---

## Integration Pattern 5: ComfyUI Programmatic Access

LTX-2 is built into ComfyUI core and can be accessed programmatically:

```python
"""Execute ComfyUI workflows programmatically via the API."""
import json
import requests

COMFYUI_URL = "http://localhost:8188"

def queue_workflow(workflow_json: dict):
    """Submit a workflow to ComfyUI for execution."""
    payload = {"prompt": workflow_json}
    response = requests.post(f"{COMFYUI_URL}/prompt", json=payload)
    return response.json()

def get_history(prompt_id: str):
    """Get the output history for a completed workflow."""
    response = requests.get(f"{COMFYUI_URL}/history/{prompt_id}")
    return response.json()

# Load and execute an LTX-Video workflow
with open("ltx_video_workflow.json") as f:
    workflow = json.load(f)

# Modify prompts programmatically
workflow["6"]["inputs"]["text"] = "A beautiful landscape"

result = queue_workflow(workflow)
```

**ComfyUI LTX nodes available:**
- Built-in ComfyUI core nodes for LTX-2
- `ComfyUI-LTXVideo` custom nodes for advanced features
- `LTXICLoRALoaderModelOnly` for IC-LoRA loading
- `LTXAddVideoICLoRAGuide` for IC-LoRA guidance

---

## Integration Pattern 6: Cloud Deployment (Modal)

```python
"""Deploy LTX Video generation as a serverless function on Modal."""
import modal

app = modal.App("ltx-video")

image = modal.Image.debian_slim().pip_install(
    "torch", "diffusers", "transformers", "accelerate"
)

@app.cls(gpu="A10G", image=image)
class LTXVideoGenerator:
    @modal.enter()
    def setup(self):
        import torch
        from diffusers import LTXPipeline

        self.pipe = LTXPipeline.from_pretrained(
            "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
        ).to("cuda")

    @modal.method()
    def generate(self, prompt: str, seed: int = 42) -> bytes:
        import torch
        from diffusers.utils import export_to_video
        import tempfile

        generator = torch.Generator("cuda").manual_seed(seed)
        video = self.pipe(
            prompt=prompt,
            negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
            width=704,
            height=480,
            num_frames=161,
            num_inference_steps=50,
            generator=generator,
        ).frames[0]

        with tempfile.NamedTemporaryFile(suffix=".mp4") as f:
            export_to_video(video, f.name, fps=24)
            return open(f.name, "rb").read()
```

---

## Key Integration Considerations

### Memory Management
- Call `torch.cuda.empty_cache()` between generations in batch processing
- Use FP8 quantization or group offloading for limited VRAM
- Enable VAE tiling for high resolutions

### Thread Safety
- Diffusers pipelines are NOT thread-safe; use process-level parallelism or a queue
- For web services, use a task queue (Celery, background tasks) rather than concurrent calls

### Resolution and Frame Constraints
- Width and height must be divisible by 32
- Frame count must follow 8n+1 pattern (9, 17, 25, ..., 161, 257)
- Lower resolution + upscale is often the best quality/speed tradeoff

### Model Versioning
- LTX-2 LoRAs do NOT transfer to LTX-2.3 (VAE redesign + text connector changes)
- Distilled models require `guidance_scale=1.0`
- Check version-specific parameters (decode_timestep, image_cond_noise_scale)
