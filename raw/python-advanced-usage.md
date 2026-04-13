# LTX Video - Advanced Python Usage

> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
> **Source:** https://huggingface.co/Lightricks/LTX-Video

## Multi-Condition Video Generation

### Conditioning with Multiple Images/Videos at Specific Frames

The `LTXConditionPipeline` (v0.9.5+) supports placing conditioning inputs at arbitrary frame positions:

```python
import torch
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition
from diffusers.utils import export_to_video, load_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

# Condition at frame 0 with an image
image = load_image("start_frame.png")
condition1 = LTXVideoCondition(image=image, frame_index=0)

# Condition at frame 80 with a video clip
video_clip = load_video("reference.mp4")
condition2 = LTXVideoCondition(video=video_clip, frame_index=80)

# Condition at frame 160 with another image
end_image = load_image("end_frame.png")
condition3 = LTXVideoCondition(image=end_image, frame_index=160)

video = pipe(
    conditions=[condition1, condition2, condition3],
    prompt="A smooth transition between scenes",
    negative_prompt="worst quality, blurry, jittery",
    width=768,
    height=512,
    num_frames=161,
    num_inference_steps=40,
    generator=torch.Generator("cuda").manual_seed(0),
).frames[0]

export_to_video(video, "multi_condition.mp4", fps=24)
```

### Conditioning Strength Control

```python
# Strong conditioning (preserves input closely)
condition = LTXVideoCondition(image=image, frame_index=0, strength=1.0)

# Weak conditioning (more creative freedom)
condition = LTXVideoCondition(image=image, frame_index=0, strength=0.5)
```

### Image Conditioning Noise Scale

Controls motion continuity when conditioned on a single frame:

```python
video = pipe(
    conditions=[condition],
    prompt="...",
    image_cond_noise_scale=0.025,  # Lower = closer to conditioning image
    # ...
).frames[0]
```

## Custom Schedulers

### Using FlowMatchEulerDiscreteScheduler (Default)

```python
from diffusers import FlowMatchEulerDiscreteScheduler

scheduler = FlowMatchEulerDiscreteScheduler()
pipeline.scheduler = scheduler
```

### Using LTXEulerAncestralRFScheduler (ComfyUI Parity)

```python
from diffusers import LTXEulerAncestralRFScheduler

pipeline.scheduler = LTXEulerAncestralRFScheduler.from_config(
    pipeline.scheduler.config
)
```

### Custom Timestep Schedules

For distilled models, specific timestep schedules provide optimal results:

```python
# Base model generation timesteps (distilled)
base_timesteps = [1000, 993, 987, 981, 975, 909, 725, 0.03]

# Upscaling refinement timesteps (distilled)
upsample_timesteps = [1000, 909, 725, 421, 0]

video = pipeline(
    prompt="...",
    timesteps=base_timesteps,
    guidance_scale=1.0,  # Must be 1.0 for distilled
    # ...
).frames[0]
```

## Batch Video Generation

### Generating Multiple Videos from Same Prompt

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

prompt = "A butterfly landing on a flower"
negative_prompt = "worst quality, blurry"

# Generate multiple videos with different seeds
for i in range(5):
    video = pipeline(
        prompt=prompt,
        negative_prompt=negative_prompt,
        width=704,
        height=480,
        num_frames=81,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(i),
    ).frames[0]
    export_to_video(video, f"output_{i}.mp4", fps=24)
```

### Batch Processing Multiple Prompts

```python
prompts = [
    "A sunset over the ocean with waves crashing",
    "A bird flying through a forest",
    "Rain drops falling on a window",
]

for idx, prompt in enumerate(prompts):
    video = pipeline(
        prompt=prompt,
        negative_prompt="worst quality, blurry",
        width=704,
        height=480,
        num_frames=81,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(42),
    ).frames[0]
    export_to_video(video, f"batch_{idx}.mp4", fps=24)
```

## Pre-computed Prompt Embeddings

For repeated generation with the same prompt, pre-compute embeddings to avoid redundant text encoding:

```python
import torch
from diffusers import LTXPipeline

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

# Pre-compute prompt embeddings
prompt_embeds, prompt_attention_mask, negative_prompt_embeds, negative_prompt_attention_mask = pipeline.encode_prompt(
    prompt="A beautiful sunset scene",
    negative_prompt="worst quality",
    do_classifier_free_guidance=True,
    device="cuda",
    dtype=torch.bfloat16,
)

# Generate multiple videos using pre-computed embeddings
for seed in range(10):
    video = pipeline(
        prompt_embeds=prompt_embeds,
        prompt_attention_mask=prompt_attention_mask,
        negative_prompt_embeds=negative_prompt_embeds,
        negative_prompt_attention_mask=negative_prompt_attention_mask,
        width=704,
        height=480,
        num_frames=81,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(seed),
    ).frames[0]
    export_to_video(video, f"precomputed_{seed}.mp4", fps=24)
```

## Callback Functions

### Progress Monitoring

```python
def progress_callback(pipeline, step, timestep, callback_kwargs):
    total_steps = pipeline._num_inference_steps  
    print(f"Step {step}/{total_steps}, timestep: {timestep:.2f}")
    return callback_kwargs

video = pipeline(
    prompt="...",
    num_inference_steps=50,
    callback_on_step_end=progress_callback,
    # ...
).frames[0]
```

### Intermediate Latent Inspection

```python
intermediates = []

def save_intermediates(pipeline, step, timestep, callback_kwargs):
    latents = callback_kwargs["latents"]
    intermediates.append(latents.clone())
    return callback_kwargs

video = pipeline(
    prompt="...",
    num_inference_steps=50,
    callback_on_step_end=save_intermediates,
    callback_on_step_end_tensor_inputs=["latents"],
    # ...
).frames[0]

print(f"Captured {len(intermediates)} intermediate latent states")
```

### Early Stopping

```python
def early_stop_callback(pipeline, step, timestep, callback_kwargs):
    if step >= 30:  # Stop after 30 steps
        # Return the current latents for decoding
        raise StopIteration()
    return callback_kwargs
```

## Guidance Scale and Rescale

### Standard Classifier-Free Guidance

```python
# Higher guidance = more prompt adherence, less diversity
video = pipeline(
    prompt="...",
    guidance_scale=5.0,  # 3-7 is typical range
    # ...
).frames[0]
```

### Guidance Rescale (Prevent Overexposure)

```python
# Use guidance_rescale to prevent washed-out colors at high guidance
video = pipeline(
    prompt="...",
    guidance_scale=7.0,
    guidance_rescale=0.7,  # 0.0-1.0, higher = more rescaling
    # ...
).frames[0]
```

## Video-to-Video Editing with denoise_strength

Control how much the generation deviates from the input:

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
).to("cuda")

source_video = load_video("input.mp4")
condition = LTXVideoCondition(video=source_video, frame_index=0)

# Low denoise = subtle changes (style transfer)
video_subtle = pipeline(
    conditions=[condition],
    prompt="Oil painting style scene",
    denoise_strength=0.3,
    num_inference_steps=30,
    # ...
).frames[0]

# High denoise = major changes (creative reinterpretation)
video_creative = pipeline(
    conditions=[condition],
    prompt="Fantasy world with dragons",
    denoise_strength=0.8,
    num_inference_steps=30,
    # ...
).frames[0]
```

## Long Video Generation with Temporal Tiling

### Using LTXI2VLongMultiPromptPipeline

For videos exceeding the model's native frame limit:

```python
import torch
from diffusers import LTXI2VLongMultiPromptPipeline, LTXEulerAncestralRFScheduler
from diffusers.utils import export_to_video, load_image

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

# First frame conditioning
cond_image = load_image("first_frame.png")

# Multi-prompt with | separator for scene changes
out = pipe(
    prompt="A person walks through a city | They enter a park | They sit on a bench and read",
    cond_image=cond_image,
    cond_strength=0.5,
    num_frames=257,
    height=512,
    width=704,
    temporal_tile_size=80,      # Window size in frames
    temporal_overlap=24,         # Overlap between windows
    temporal_overlap_cond_strength=0.5,  # Cross-window conditioning
    adain_factor=0.25,          # Cross-window consistency
    num_inference_steps=8,
    guidance_scale=1.0,
    seed=42,
    output_type="pil",
)

export_to_video(out.frames[0], "long_video.mp4", fps=25)
```

### Prompt Segments for Fine Control

```python
out = pipe(
    prompt_segments=[
        {"start_window": 0, "end_window": 2, "text": "A cat sleeping peacefully"},
        {"start_window": 2, "end_window": 4, "text": "The cat wakes up and stretches"},
        {"start_window": 4, "end_window": 6, "text": "The cat walks to its food bowl"},
    ],
    num_frames=257,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    output_type="pil",
)
```

## Resolution Handling

### Ensuring VAE-Compatible Resolutions

```python
def round_to_nearest_resolution_acceptable_by_vae(pipeline, height, width):
    """Ensure dimensions are compatible with VAE compression ratio."""
    height = height - (height % pipeline.vae_spatial_compression_ratio)
    width = width - (width % pipeline.vae_spatial_compression_ratio)
    return height, width

# Usage
height, width = round_to_nearest_resolution_acceptable_by_vae(pipeline, 720, 1280)
print(f"Adjusted resolution: {width}x{height}")
```

### Frame Count Requirements

```python
def get_valid_frame_count(desired_frames):
    """Return the nearest valid frame count (8n+1 pattern)."""
    # Valid: 9, 17, 25, 33, ..., 161, 169, ..., 257
    n = round((desired_frames - 1) / 8)
    return 8 * n + 1

# Usage
frames = get_valid_frame_count(100)  # Returns 97 (8*12+1)
```

## Working with Output Formats

### PIL Images (Default)

```python
result = pipeline(prompt="...", output_type="pil").frames[0]
# result is a list of PIL.Image.Image

# Save individual frames
for i, frame in enumerate(result):
    frame.save(f"frame_{i:04d}.png")
```

### NumPy Arrays

```python
result = pipeline(prompt="...", output_type="np").frames
# result is np.ndarray of shape (batch, frames, height, width, channels)
```

### PyTorch Tensors

```python
result = pipeline(prompt="...", output_type="pt").frames
# result is torch.Tensor of shape (batch, frames, channels, height, width)
```

### Latent Output (for Further Processing)

```python
latents = pipeline(prompt="...", output_type="latent").frames
# latents is torch.Tensor in normalized latent space
# Can be passed to upsampler or for further processing
```
