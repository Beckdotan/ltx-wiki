---
title: LTXConditionPipeline (Multi-Condition)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
  - https://github.com/huggingface/diffusers/blob/main/src/diffusers/pipelines/ltx/pipeline_ltx_condition.py
  - https://huggingface.co/Lightricks/LTX-Video
tags:
  - diffusers
  - pipeline
  - multi-condition
  - image-to-video
  - video-to-video
  - python
  - code-example
---

# LTXConditionPipeline (Multi-Condition)

`LTXConditionPipeline` is the most flexible pipeline for LTX Video generation, introduced in v0.9.5. It supports text, image, and video conditioning with frame-level control via `LTXVideoCondition` objects. It also supports text-only generation without passing `conditions`.

This pipeline is the recommended choice for all use cases starting from v0.9.5.

## LTXVideoCondition

The conditioning object that wraps images or videos with frame targeting:

```python
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

# Condition on an image at frame 0
condition = LTXVideoCondition(image=image, frame_index=0)

# Condition on a video starting at frame 80
condition = LTXVideoCondition(video=video, frame_index=80)
```

## Image-to-Video

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8", vae=pipe.vae, torch_dtype=torch.bfloat16
)
pipe.to("cuda")
pipe_upsample.to("cuda")
pipe.vae.enable_tiling()

# Wrap image as a single-frame video for conditioning
image = load_image("penguin.png")
video = load_video(export_to_video([image]))
condition1 = LTXVideoCondition(video=video, frame_index=0)

latents = pipe(
    conditions=[condition1],
    prompt="A cute little penguin takes out a book and starts reading it",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=832, height=480, num_frames=96,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Upscale and denoise (see [[two-stage-inference-pattern]])
upscaled_latents = pipe_upsample(latents=latents, output_type="latent").frames
video = pipe(
    conditions=[condition1],
    prompt="A cute little penguin takes out a book and starts reading it",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=1664, height=960, num_frames=96,
    denoise_strength=0.4, num_inference_steps=10,
    latents=upscaled_latents,
    decode_timestep=0.05, image_cond_noise_scale=0.025,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

## Multi-Condition Generation (Image + Video)

Combine multiple conditioning inputs at different frame positions:

```python
import torch
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition
from diffusers.utils import export_to_video, load_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

video_input = load_video("input_video.mp4")
image = load_image("input_image.jpg")

condition1 = LTXVideoCondition(image=image, frame_index=0)
condition2 = LTXVideoCondition(video=video_input, frame_index=80)

generator = torch.Generator("cuda").manual_seed(0)
video = pipe(
    conditions=[condition1, condition2],
    prompt="A long highway stretching into the distance...",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=768, height=512, num_frames=161,
    num_inference_steps=40,
    generator=generator,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

## Video-to-Video Editing

Use `denoise_strength` to control how much the output deviates from the input. Lower values stay closer to the original; higher values allow more creative freedom.

```python
source_video = load_video("input_video.mp4")
condition = LTXVideoCondition(video=source_video, frame_index=0)

video = pipeline(
    conditions=[condition],
    prompt="Transform the scene into a winter wonderland with falling snow",
    negative_prompt="worst quality, blurry, distorted",
    width=768, height=512,
    num_frames=len(source_video),
    denoise_strength=0.6,
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=5.0,
    generator=torch.Generator().manual_seed(42),
    output_type="pil",
).frames[0]
```

## Additional Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `conditions` | None | List of `LTXVideoCondition` objects |
| `image` | None | Input image(s) for conditioning |
| `video` | None | Input video for conditioning |
| `frame_index` | 0 | Frame index for conditioning |
| `strength` | 1.0 | Conditioning strength |
| `denoise_strength` | 1.0 | Noise strength for vid2vid editing |
| `image_cond_noise_scale` | 0.15 | Noise added to conditioning latents |
| `max_sequence_length` | 256 | Max tokenizer length (higher than LTXPipeline) |

## See Also

- [[diffusers-pipeline-overview]] -- all pipeline classes
- [[ltx-image-to-video-pipeline]] -- simpler dedicated image-to-video pipeline
- [[two-stage-inference-pattern]] -- recommended upscaled generation workflow
- [[ltx-latent-upsample-pipeline]] -- the upscaler used in two-stage workflows
- [[ltxv-model-variants]] -- which model to use
