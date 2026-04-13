# HF Diffusers Pipeline for LTX Video

> **Sources:**
> - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
> - https://huggingface.co/Lightricks/LTX-Video
> - https://github.com/huggingface/diffusers/blob/main/docs/source/en/api/pipelines/ltx_video.md
> - https://huggingface.co/docs/diffusers/main/api/schedulers/flow_match_euler_discrete
> - https://huggingface.co/docs/diffusers/api/models/autoencoderkl_ltx_video

---

## Overview

LTX-Video is a diffusion transformer (DiT) designed for fast, real-time generation of high-resolution videos from text and images. Its main architectural innovation is the **Video-VAE**, which achieves a high pixel-to-latent compression ratio of **1:192**, enabling more efficient video data processing and faster generation speed. To preserve fine details, the Video-VAE decoder performs both the latent-to-pixel conversion *and* the last denoising step.

All official LTX-Video checkpoints are available under the [Lightricks](https://huggingface.co/Lightricks) HuggingFace organization.

---

## Pipeline Classes in Diffusers

The HuggingFace Diffusers library provides several pipeline classes for LTX-Video:

### 1. `LTXPipeline` -- Text-to-Video

The base pipeline for text-to-video generation.

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

prompt = "A woman with long brown hair and light skin smiles at another woman with long blonde hair. The woman with brown hair wears a black jacket and has a small, barely noticeable mole on her right cheek. The camera angle is a close-up, focused on the woman with brown hair's face. The lighting is warm and natural, likely from the setting sun, casting a soft glow on the scene. The scene appears to be real-life footage"
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

**Key `__call__` Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `prompt` | None | Text prompt(s) guiding generation |
| `negative_prompt` | None | Prompts to suppress undesired content |
| `height` | 512 | Output height in pixels (divisible by 32) |
| `width` | 704 | Output width in pixels (divisible by 32) |
| `num_frames` | 161 | Number of video frames to generate |
| `frame_rate` | 25 | Frames per second |
| `num_inference_steps` | 50 | Denoising steps |
| `timesteps` | None | Custom timestep schedule (overrides num_inference_steps) |
| `guidance_scale` | 3.0 | CFG scale; >1 enables classifier-free guidance |
| `guidance_rescale` | 0.0 | Rescale to mitigate overexposure |
| `num_videos_per_prompt` | 1 | Number of videos per prompt |
| `decode_timestep` | 0.0 | Timestep at which video is decoded |
| `decode_noise_scale` | None | Noise interpolation factor during decoding |
| `max_sequence_length` | 128 | Max tokenizer length for prompt |
| `output_type` | "pil" | Output format: "pil", "np", "pt", "latent" |

### 2. `LTXImageToVideoPipeline` -- Image-to-Video

Generates video conditioned on an input image.

```python
import torch
from diffusers import LTXImageToVideoPipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXImageToVideoPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

image = load_image(
    "https://huggingface.co/datasets/a-r-r-o-w/tiny-meme-dataset-captioned/resolve/main/images/8.png"
)
prompt = "A young girl stands calmly in the foreground, looking directly at the camera, as a house fire rages in the background."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

video = pipe(
    image=image,
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

### 3. `LTXConditionPipeline` -- Multi-Condition Generation (0.9.5+)

The most flexible pipeline, supporting text, image, and video conditioning with frame-level control.

```python
import torch
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition
from diffusers.utils import export_to_video, load_video, load_image

pipe = LTXConditionPipeline.from_pretrained("Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16)
pipe.to("cuda")

# Load conditioning inputs
video = load_video("https://example.com/input_video.mp4")
image = load_image("https://example.com/input_image.jpg")

# Create conditioning objects with frame targets
condition1 = LTXVideoCondition(image=image, frame_index=0)
condition2 = LTXVideoCondition(video=video, frame_index=80)

prompt = "A scenic drive through a mountainous region..."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

generator = torch.Generator("cuda").manual_seed(0)
video = pipe(
    conditions=[condition1, condition2],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=768,
    height=512,
    num_frames=161,
    num_inference_steps=40,
    generator=generator,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

**Additional `LTXConditionPipeline` Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `conditions` | None | List of `LTXVideoCondition` objects |
| `image` | None | Input image(s) for conditioning |
| `video` | None | Input video for conditioning |
| `frame_index` | 0 | Frame index for conditioning |
| `strength` | 1.0 | Conditioning strength |
| `denoise_strength` | 1.0 | Noise strength for vid2vid editing |
| `image_cond_noise_scale` | 0.15 | Noise added to conditioning latents |

### 4. `LTXLatentUpsamplePipeline` -- Latent Upscaler (0.9.7+)

Upscales latent representations by 2x for higher resolution output.

```python
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline

pipeline = LTXConditionPipeline.from_pretrained("Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7", vae=pipeline.vae, torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipe_upsample.to("cuda")
pipeline.vae.enable_tiling()

# Stage 1: Generate at lower resolution
latents = pipeline(
    prompt=prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=161,
    num_inference_steps=30,
    output_type="latent",
).frames

# Stage 2: Upscale latents
upscaled_latents = pipe_upsample(
    latents=latents,
    output_type="latent"
).frames

# Stage 3: Refine (optional but recommended)
video = pipeline(
    prompt=prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=161,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled_latents,
    output_type="pil",
).frames[0]
```

**Upsampler Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `latents` | None | Input latent tensor to upscale |
| `adain_factor` | 0.0 | AdaIN normalization strength |
| `tone_map_compression_ratio` | 0.0 | Tone mapping compression (0.6 recommended for 0.9.8) |

Methods: `enable_vae_tiling()`, `disable_vae_tiling()`, `enable_vae_slicing()`, `disable_vae_slicing()`, `adain_filter_latent()`, `tone_map_latents()`

### 5. `LTXI2VLongMultiPromptPipeline` -- Long Video Multi-Prompt

Generates long-duration videos using temporal sliding windows and per-window prompt scheduling.

```python
import torch
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained("LTX-Video-0.9.8-13B-distilled")
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

# Multi-prompt with pipe separator
out = pipe(
    prompt="a chimpanzee walks | a chimpanzee eats",
    num_frames=161,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    output_type="pil",
    return_dict=True,
)
frames = out.frames[0]
```

**Key Long-Video Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `temporal_tile_size` | 80 | Window size in decoded frames |
| `temporal_overlap` | 24 | Overlap between windows |
| `temporal_overlap_cond_strength` | 0.5 | Strength for cross-window injection |
| `adain_factor` | 0.25 | Cross-window consistency normalization |
| `cond_image` | None | First-frame conditioning image |
| `cond_strength` | 0.5 | First-frame conditioning strength |
| `prompt_segments` | None | Per-window prompt mapping `[{"start_window", "end_window", "text"}]` |

Key features:
- Temporal sliding-window sampling (no spatial H/W sharding)
- Autoregressive fusion across windows
- Multi-prompt segmentation per window with smooth transitions
- First-frame hard conditioning via per-token mask for I2V
- VRAM control via temporal windowing and VAE tiled decoding

---

## Model Components

### LTXVideoTransformer3DModel

The core conditional Transformer architecture that denoises encoded video latents:

```python
from diffusers import AutoModel

transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
)
```

### AutoencoderKLLTXVideo

The 3D variational autoencoder (Video-VAE) with KL loss:

- **Compression ratio:** 1:192 (pixel to latent)
- **`scaling_factor`:** Component-wise standard deviation of the latent space (default 1.0)
- **`encoder_causal`:** (default True) -- future frames only depend on past frames
- **`decoder_causal`:** (default False)
- Supports `enable_tiling()` and `enable_slicing()` for memory optimization

### FlowMatchEulerDiscreteScheduler

The default scheduler for LTX-Video, based on flow-matching sampling:

```python
from diffusers import FlowMatchEulerDiscreteScheduler

scheduler = FlowMatchEulerDiscreteScheduler(
    num_train_timesteps=1000,
    shift=1.0,
    use_dynamic_shifting=False,
    base_shift=0.5,
    max_shift=1.15,
    shift_terminal=None,
    time_shift_type="exponential",
)
```

**Scheduler Configuration:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `shift` | 1.0 | Shift value for timestep schedule |
| `use_dynamic_shifting` | False | Resolution-dependent timestep shifting |
| `base_shift` | 0.5 | Stabilize generation (higher = more consistent) |
| `max_shift` | 1.15 | Variation range (higher = more stylized) |
| `shift_terminal` | None | End value for shifted schedule (LTX-specific) |
| `time_shift_type` | "exponential" | Type of dynamic shifting |
| `use_karras_sigmas` | False | Karras sigma schedule |
| `stochastic_sampling` | False | Enable stochastic sampling |

**Key Scheduler Methods:**

- `set_timesteps(num_inference_steps, device, sigmas, mu, timesteps)` -- Set discrete timesteps
- `step(model_output, timestep, sample, ...)` -- Predict sample from previous timestep
- `scale_noise(sample, timestep, noise)` -- Forward process in flow-matching
- `set_shift(shift)` -- Update the shift value
- `stretch_shift_to_terminal(t)` -- Stretch schedule to terminal value (LTX-specific)
- `time_shift(mu, sigma, t)` -- Apply time shifting to sigmas

### Text Encoder: T5

LTX-Video uses **T5EncoderModel** (`google/t5-v1_1-xxl`) for text encoding. Default `max_sequence_length` is 128 tokens (256 for `LTXConditionPipeline`).

### LTXEulerAncestralRFScheduler

Alternative scheduler for ComfyUI parity:

```python
from diffusers import LTXEulerAncestralRFScheduler
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
```

---

## Version-Specific Notes

### LTX-Video 0.9.1+
- Set `decode_timestep=0.05` and `image_cond_noise_scale=0.025` for timestep-aware VAE

### LTX-Video 0.9.5+
- Multi-condition support via `LTXConditionPipeline`
- Use similar conditioning images/videos for best results

### LTX-Video 0.9.7
- 13B parameter transformer + spatial latent upscaler
- Two-stage inference: generate low-res then upscale and refine
- **Distilled model** custom timesteps:
  - Base: `[1000, 993, 987, 981, 975, 909, 725, 0.03]`
  - Upscaling: `[1000, 909, 725, 421, 0]`
- Set `guidance_scale=1.0` for distilled, `num_inference_steps` between 4-10

### LTX-Video 0.9.8
- Very long video generation (e.g., `num_frames=361`)
- Tone mapping via `tone_map_compression_ratio` (0.6 recommended)
- 13B parameter model

### General Recommendations
- Recommended dtype: `torch.bfloat16` for all components
- VAE and text encoder also support `torch.float32` or `torch.float16`
- Resolution must be divisible by 32
- Frame count: divisible by 8 + 1 (e.g., 9, 17, 25, ..., 161, 257)
