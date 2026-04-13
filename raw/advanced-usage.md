# LTX Video Advanced Usage

> **Sources:**
> - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
> - https://docs.ltx.video/open-source-model/usage-guides/lo-ra
> - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
> - https://huggingface.co/docs/diffusers/main/api/schedulers/flow_match_euler_discrete
> - https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/
> - https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Motion-Track-Control

---

## Conditioning Types

LTX-Video supports three distinct conditioning mechanisms in the LTX-2 pipeline:

### 1. Replacing Latents (Strongest Control)

Directly substitutes latent values at specific frames. Provides the strongest frame-level control.

```python
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
).to("cuda")

# Image at frame 0 with full strength
condition = LTXVideoCondition(image=image, frame_index=0, strength=1.0)

video = pipe(
    conditions=[condition],
    prompt="...",
    num_frames=161,
    num_inference_steps=40,
).frames[0]
```

### 2. Guiding Latents (Smoother Interpolation)

Additively conditions on images for smoother interpolation between keyframes:

```python
# Multiple conditions at different frames
condition1 = LTXVideoCondition(image=start_image, frame_index=0)
condition2 = LTXVideoCondition(image=end_image, frame_index=160)

video = pipe(
    conditions=[condition1, condition2],
    prompt="A smooth transition from day to night",
    num_frames=161,
).frames[0]
```

### 3. Video Conditioning (Vid2Vid)

Full reference video guidance for video-to-video transformations:

```python
from diffusers.utils import load_video

input_video = load_video("path/to/reference.mp4")
condition = LTXVideoCondition(video=input_video, frame_index=0)

video = pipe(
    conditions=[condition],
    prompt="Transform to anime style...",
    denoise_strength=0.6,  # Lower = closer to original
    num_frames=161,
).frames[0]
```

### Conditioning Noise Scale

The `image_cond_noise_scale` parameter adds timestep-dependent noise to conditioning latents, which helps with motion continuity (especially when conditioned on a single frame):

```python
video = pipe(
    conditions=[condition],
    prompt="...",
    image_cond_noise_scale=0.15,  # Default; 0.0 for 0.9.7+ with upscaler
    decode_timestep=0.05,
    decode_noise_scale=0.025,
).frames[0]
```

---

## LoRA Loading

### In Diffusers

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.utils import export_to_video, load_image

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)

# Load LoRA weights
pipeline.load_lora_weights("Lightricks/LTX-Video-Cakeify-LoRA", adapter_name="cakeify")
pipeline.set_adapters("cakeify")

# Use the LoRA trigger word in the prompt
prompt = "CAKEIFY a person using a knife to cut a cake shaped like a Pikachu plushie"
image = load_image("path/to/pikachu.png")

video = pipeline(
    prompt=prompt,
    image=image,
    width=576,
    height=576,
    num_frames=161,
    decode_timestep=0.03,
    decode_noise_scale=0.025,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=26)
```

### Multiple LoRAs

```python
# Load multiple adapters
pipeline.load_lora_weights("Lightricks/LTX-Video-Cakeify-LoRA", adapter_name="cakeify")
pipeline.load_lora_weights("path/to/another-lora", adapter_name="style_lora")

# Set adapters with individual weights
pipeline.set_adapters(["cakeify", "style_lora"], adapter_weights=[1.0, 0.8])
```

### In LTX-2 Pipelines

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline

# Configure LoRAs
distilled_lora = [
    LoraPathStrengthAndSDOps(
        "/path/to/distilled_lora.safetensors",
        0.6,
        LTXV_LORA_COMFY_RENAMING_MAP
    ),
]

custom_loras = [
    LoraPathStrengthAndSDOps(
        "/path/to/custom_style_lora.safetensors",
        1.0,
        LTXV_LORA_COMFY_RENAMING_MAP
    ),
]

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=custom_loras,
)
```

### LoRA Strength Guidelines

| Strength Range | Effect | Use Case |
|---------------|--------|----------|
| 0.0 | No effect | Disabled |
| 0.9 - 1.1 | Subtle | Preserving base model characteristics |
| 1.2 - 1.4 | Balanced | Recommended for most applications |
| 1.5 - 1.6 | Strong | Maximum style transfer |

**Best Practices:**
- Begin effect LoRAs at strength 1.0
- Set IC-LoRA control models to full strength (designed for 1.0)
- Keep combined total strength under 2.0
- Test incrementally with different resolutions and aspect ratios
- Avoid mixing multiple IC-LoRA control types simultaneously

### Available Camera Control LoRAs

Seven official camera movement LoRAs from Lightricks:
- Dolly left / right / in / out
- Jib up / down
- Static camera

---

## IC-LoRA (Identity-Consistent LoRA)

IC-LoRA enables conditioning video generation on reference video frames at inference time, allowing fine-grained video-to-video control.

### IC-LoRA Types
- **Depth Map Control** -- Conditions on depth maps
- **Human Pose Control** -- Conditions on pose skeletons
- **Canny Edge Control** -- Conditions on edge detection
- **Motion Track Control** -- Conditions on motion tracking

### Usage with LTX-2 Pipelines

```python
from ltx_pipelines.ic_lora import ICLoraPipeline

pipeline = ICLoraPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    ic_lora_path="/path/to/ic_lora.safetensors",
)

# Condition on reference video
pipeline(
    prompt="A person dancing in the rain",
    reference_video="path/to/reference.mp4",
    output_path="output.mp4",
    seed=42,
    height=512,
    width=768,
    num_frames=121,
)
```

---

## Scheduler Configuration

### Default: FlowMatchEulerDiscreteScheduler

```python
from diffusers import FlowMatchEulerDiscreteScheduler

# Custom scheduler configuration
scheduler = FlowMatchEulerDiscreteScheduler(
    num_train_timesteps=1000,
    shift=1.0,
    use_dynamic_shifting=True,   # Resolution-dependent shifting
    base_shift=0.5,
    max_shift=1.15,
    time_shift_type="exponential",
)

pipe.scheduler = scheduler
```

### Custom Timesteps for Distilled Models

```python
# Distilled model base inference timesteps
video = pipe(
    prompt=prompt,
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],
    guidance_scale=1.0,
    output_type="latent",
).frames

# Distilled model upscaling timesteps
video = pipe(
    prompt=prompt,
    timesteps=[1000, 909, 725, 421, 0],
    denoise_strength=0.999,
    guidance_scale=1.0,
    latents=upscaled_latents,
).frames[0]
```

### Alternative: LTXEulerAncestralRFScheduler

For ComfyUI parity with the long-video pipeline:

```python
from diffusers import LTXEulerAncestralRFScheduler

pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
```

---

## MultiModal Guidance Parameters (LTX-2)

The `MultiModalGuiderParams` dataclass controls generation behavior in LTX-2 pipelines:

```python
from ltx_core.components.guiders import MultiModalGuiderParams

video_guider_params = MultiModalGuiderParams(
    cfg_scale=3.0,         # Classifier-Free Guidance (2.0-5.0 typical; 1.0 disables)
    stg_scale=1.0,         # Spatio-Temporal Guidance (0.5-1.5 typical; 0.0 disables)
    stg_blocks=[29],       # Transformer blocks to perturb (e.g., [29] for last block)
    rescale_scale=0.7,     # Prevent over-saturation (0.5-0.7 typical; 0.0 disables)
    modality_scale=3.0,    # Audio-visual coherence (3.0 for audio-video; 1.0 to disable)
    skip_step=0,           # Skip guidance every N steps for speed
)

audio_guider_params = MultiModalGuiderParams(
    cfg_scale=7.0,
    stg_scale=1.0,
    rescale_scale=0.7,
    modality_scale=3.0,
    skip_step=0,
    stg_blocks=[29],
)
```

---

## Batch Generation

### Simple Batch Loop (Diffusers)

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
        width=704,
        height=480,
        num_frames=161,
        num_inference_steps=50,
        generator=torch.Generator("cuda").manual_seed(42),
    ).frames[0]
    export_to_video(video, f"output_{i}.mp4", fps=24)
    torch.cuda.empty_cache()  # Free memory between generations
```

### Batch with DistilledPipeline (LTX-2)

For fastest batch processing, use the DistilledPipeline with predefined sigmas:

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
        height=512,
        width=768,
        num_frames=121,
    )
```

### Batch with FP8 for Memory Efficiency

```python
from ltx_core.quantization import QuantizationPolicy

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
    quantization=QuantizationPolicy.fp8_cast(),  # Enable FP8 for batch processing
)
```

---

## Prompt Enhancement

LTX-Video supports automatic prompt enhancement to improve generation quality:

```python
# With the official LTX-Video repo
from ltx_video.inference import infer, InferenceConfig

infer(
    InferenceConfig(
        pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
        prompt="A sunset",
        enhance_prompt=True,  # Automatically enriches the prompt
        output_path="output.mp4",
    )
)
```

### Prompting Guidelines

Effective prompts should be approximately 200 words with detailed, chronological descriptions:

1. Primary action statement
2. Specific movement details
3. Character/object appearance descriptions
4. Environmental context
5. Camera angles and movements
6. Lighting and color specifications
7. Scene changes or events

---

## Gradient Estimation for Faster Inference (LTX-2)

Reduce inference steps from 40 to 20-30 while maintaining quality:

```python
from ltx_pipelines.utils import gradient_estimating_euler_denoising_loop

def denoising_loop(sigmas, video_state, audio_state, stepper):
    return gradient_estimating_euler_denoising_loop(
        sigmas=sigmas,
        video_state=video_state,
        audio_state=audio_state,
        stepper=stepper,
        denoise_fn=your_denoise_function,
        ge_gamma=2.0,
    )
```

---

## Callback on Step End

Monitor denoising progress or implement custom logic during generation:

```python
def callback_fn(pipe, step, timestep, callback_kwargs):
    latents = callback_kwargs["latents"]
    print(f"Step {step}, timestep {timestep}, latent shape: {latents.shape}")
    # Optionally modify latents
    return callback_kwargs

video = pipe(
    prompt=prompt,
    num_inference_steps=50,
    callback_on_step_end=callback_fn,
    callback_on_step_end_tensor_inputs=["latents"],
).frames[0]
```
