---
title: Python Batch Video Generation
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
tags:
  - python
  - batch
  - generation
  - production
---

# Python Batch Video Generation

Patterns for generating multiple videos efficiently using LTX-Video in Python.

## Simple Batch Loop (Diffusers)

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

prompts = [
    "A sunset over the ocean with waves crashing on rocks",
    "A cat playing with a ball of yarn on a wooden floor",
    "A time-lapse of clouds moving over a mountain range",
]

for i, prompt in enumerate(prompts):
    video = pipe(
        prompt=prompt,
        negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
        width=704, height=480, num_frames=161,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(42),
    ).frames[0]
    export_to_video(video, f"output_{i}.mp4", fps=24)
    torch.cuda.empty_cache()  # Free memory between generations
```

## Multiple Seeds from Same Prompt

```python
prompt = "A butterfly landing on a flower"
for i in range(5):
    video = pipeline(
        prompt=prompt,
        negative_prompt="worst quality, blurry",
        width=704, height=480, num_frames=81,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(i),
    ).frames[0]
    export_to_video(video, f"output_{i}.mp4", fps=24)
```

## Pre-computed Prompt Embeddings

For repeated generation with the same prompt, pre-compute embeddings to skip redundant text encoding:

```python
prompt_embeds, prompt_attention_mask, negative_prompt_embeds, negative_prompt_attention_mask = pipeline.encode_prompt(
    prompt="A beautiful sunset scene",
    negative_prompt="worst quality",
    do_classifier_free_guidance=True,
    device="cuda",
    dtype=torch.bfloat16,
)

for seed in range(10):
    video = pipeline(
        prompt_embeds=prompt_embeds,
        prompt_attention_mask=prompt_attention_mask,
        negative_prompt_embeds=negative_prompt_embeds,
        negative_prompt_attention_mask=negative_prompt_attention_mask,
        width=704, height=480, num_frames=81,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(seed),
    ).frames[0]
    export_to_video(video, f"precomputed_{seed}.mp4", fps=24)
```

## Batch with DistilledPipeline (LTX-2)

For fastest batch processing using the [[sdk-ltx-2]] native pipeline:

```python
from ltx_pipelines.distilled import DistilledPipeline

pipeline = DistilledPipeline(
    checkpoint_path="/path/to/ltx-2.3-22b-distilled.safetensors",
    gemma_root="/path/to/gemma",
)

prompts = ["prompt 1", "prompt 2", "prompt 3"]
for i, prompt in enumerate(prompts):
    pipeline(
        prompt=prompt,
        output_path=f"output_{i}.mp4",
        seed=42 + i,
        height=512, width=768, num_frames=121,
    )
```

## Batch with FP8 for Memory Efficiency

Combine batch processing with [[python-performance-memory]] optimizations:

```python
from ltx_core.quantization import QuantizationPolicy

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
    quantization=QuantizationPolicy.fp8_cast(),
)
```

## Structured Batch Processor

A reusable pattern for production batch processing:

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
    def __init__(self, model_id="Lightricks/LTX-Video", output_dir="./outputs"):
        self.pipe = LTXPipeline.from_pretrained(
            model_id, torch_dtype=torch.bfloat16
        ).to("cuda")
        self.output_dir = Path(output_dir)
        self.output_dir.mkdir(exist_ok=True)

    def process_jobs(self, jobs: list[VideoJob]) -> list[dict]:
        results = []
        for i, job in enumerate(jobs):
            try:
                video = self.pipe(
                    prompt=job.prompt,
                    negative_prompt="worst quality, inconsistent motion, blurry",
                    width=job.width, height=job.height,
                    num_frames=job.num_frames, num_inference_steps=50,
                    generator=torch.Generator("cuda").manual_seed(job.seed),
                ).frames[0]
                output_path = self.output_dir / f"{job.output_name}.mp4"
                export_to_video(video, str(output_path), fps=24)
                results.append({"job": job.output_name, "status": "success"})
            except Exception as e:
                results.append({"job": job.output_name, "status": "error", "error": str(e)})
            finally:
                torch.cuda.empty_cache()
        return results
```

## See Also

- [[python-performance-speed]] -- Speed optimizations for batch workloads
- [[python-performance-memory]] -- Memory management between generations
- [[python-callbacks]] -- Progress monitoring during batch runs
- [[python-integration-patterns]] -- REST API and cloud deployment for batch processing
