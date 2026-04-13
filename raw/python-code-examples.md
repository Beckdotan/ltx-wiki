# Python Code Examples for LTX Video

> **Sources:**
> - https://github.com/Lightricks/LTX-Video
> - https://github.com/Lightricks/LTX-2
> - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
> - https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-pipelines/README.md
> - https://pypi.org/project/ltx-video/

---

## Approach A: Using HuggingFace Diffusers

### Text-to-Video (Basic)

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

prompt = "A woman with long brown hair and light skin smiles at another woman with long blonde hair. The woman with brown hair wears a black jacket and has a small, barely noticeable mole on her right cheek. The camera angle is a close-up, focused on the woman with brown hair's face. The lighting is warm and natural, likely from the setting sun, casting a soft glow on the scene."
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

### Text-to-Video (Memory Optimized, ~10GB VRAM)

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading
from diffusers.utils import export_to_video

# Load transformer with FP8 layerwise weight-casting
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn,
    compute_dtype=torch.bfloat16
)

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    transformer=transformer,
    torch_dtype=torch.bfloat16
)

# Group offloading for memory efficiency
onload_device = torch.device("cuda")
offload_device = torch.device("cpu")
pipeline.transformer.enable_group_offload(
    onload_device=onload_device,
    offload_device=offload_device,
    offload_type="leaf_level",
    use_stream=True
)
apply_group_offloading(
    pipeline.text_encoder,
    onload_device=onload_device,
    offload_type="block_level",
    num_blocks_per_group=2
)
apply_group_offloading(
    pipeline.vae,
    onload_device=onload_device,
    offload_type="leaf_level"
)

prompt = "A woman with long brown hair and light skin smiles at another woman..."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=768,
    height=512,
    num_frames=161,
    decode_timestep=0.03,
    decode_noise_scale=0.025,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

### Image-to-Video

```python
import torch
from diffusers import LTXImageToVideoPipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXImageToVideoPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

image = load_image("path/to/your/image.png")
prompt = "A young girl stands calmly as fire rages in the background. Firefighters rush to the scene."
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

### Video-to-Video / Multi-Condition Generation

```python
import torch
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition
from diffusers.utils import export_to_video, load_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

# Load conditioning inputs
video_input = load_video("path/to/input_video.mp4")
image_input = load_image("path/to/input_image.jpg")

# Create conditions targeting specific frames
condition1 = LTXVideoCondition(image=image_input, frame_index=0)
condition2 = LTXVideoCondition(video=video_input, frame_index=80)

prompt = "A long highway stretching into the distance..."
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

### Two-Stage Upscaled Generation (0.9.7 Dev Model)

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae,
    torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipe_upsample.to("cuda")
pipeline.vae.enable_tiling()

def round_to_nearest_resolution_acceptable_by_vae(height, width):
    height = height - (height % pipeline.vae_temporal_compression_ratio)
    width = width - (width % pipeline.vae_temporal_compression_ratio)
    return height, width

# Load conditioning video
video = load_video("path/to/conditioning_video.mp4")[:21]
condition1 = LTXVideoCondition(video=video, frame_index=0)

prompt = "A winding mountain road covered in snow..."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 768, 1152
downscale_factor = 2 / 3
num_frames = 161

# Stage 1: Generate at reduced resolution
downscaled_height, downscaled_width = int(expected_height * downscale_factor), int(expected_width * downscale_factor)
downscaled_height, downscaled_width = round_to_nearest_resolution_acceptable_by_vae(downscaled_height, downscaled_width)

latents = pipeline(
    conditions=[condition1],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=num_frames,
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=5.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Stage 2: Upscale latents (2x)
upscaled_height, upscaled_width = downscaled_height * 2, downscaled_width * 2
upscaled_latents = pipe_upsample(
    latents=latents,
    output_type="latent"
).frames

# Stage 3: Refine upscaled latents
video = pipeline(
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
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=5.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

# Stage 4: Downscale to target resolution
video = [frame.resize((expected_width, expected_height)) for frame in video]
export_to_video(video, "output.mp4", fps=24)
```

### Two-Stage Distilled Model (Fastest)

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-distilled", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae,
    torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipe_upsample.to("cuda")
pipeline.vae.enable_tiling()

prompt = "A 3D render of anatomical structure, ultra quality..."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

# Stage 1: Use custom distilled timesteps
latents = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=161,
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],  # Distilled timesteps
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=1.0,       # Must be 1.0 for distilled
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Stage 2: Upscale
upscaled_latents = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    output_type="latent"
).frames

# Stage 3: Refine with upscaling timesteps
video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=161,
    denoise_strength=0.999,
    timesteps=[1000, 909, 725, 421, 0],  # Upscaling timesteps
    latents=upscaled_latents,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=1.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

### Long Video Generation (0.9.8, Multi-Prompt)

```python
import torch
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained("LTX-Video-0.9.8-13B-distilled")
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

# Multi-prompt with | separator for different temporal segments
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

# Or use prompt_segments for precise control
out = pipe(
    prompt_segments=[
        {"start_window": 0, "end_window": 2, "text": "a chimpanzee walks through a forest"},
        {"start_window": 2, "end_window": 4, "text": "a chimpanzee picks up a banana and eats it"},
    ],
    num_frames=321,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    output_type="pil",
)
```

### GGUF Quantized Model Loading

```python
import torch
from diffusers import LTXPipeline, AutoModel, GGUFQuantizationConfig
from diffusers.utils import export_to_video

transformer = AutoModel.from_single_file(
    "https://huggingface.co/city96/LTX-Video-gguf/blob/main/ltx-video-2b-v0.9-Q3_K_S.gguf",
    quantization_config=GGUFQuantizationConfig(compute_dtype=torch.bfloat16),
    torch_dtype=torch.bfloat16
)
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    transformer=transformer,
    torch_dtype=torch.bfloat16
)
```

---

## Approach B: Using the Official LTX-Video Repository

### Library API (ltx_video package)

```python
from ltx_video.inference import infer, InferenceConfig

infer(
    InferenceConfig(
        pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
        prompt="A beautiful sunset over the ocean",
        height=480,
        width=832,
        num_frames=161,
        output_path="output.mp4",
    )
)
```

### CLI: Text-to-Video

```bash
python inference.py --prompt "A beautiful sunset over the ocean" \
    --height 480 --width 832 \
    --num_frames 161 --seed 42 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### CLI: Image-to-Video

```bash
python inference.py --prompt "The scene comes alive as birds fly overhead" \
    --conditioning_media_paths path/to/image.jpg \
    --conditioning_start_frames 0 \
    --height 480 --width 832 \
    --num_frames 161 --seed 42 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### CLI: Video Extension

```bash
python inference.py --prompt "The journey continues through the valley" \
    --conditioning_media_paths path/to/video.mp4 \
    --conditioning_start_frames 0 \
    --height 480 --width 832 \
    --num_frames 257 --seed 42 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### CLI: Multi-Condition Generation

```bash
python inference.py --prompt "A transition from day to night" \
    --conditioning_media_paths path/to/image1.jpg path/to/video1.mp4 \
    --conditioning_start_frames 0 80 \
    --height 480 --width 832 \
    --num_frames 161 --seed 42 \
    --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

---

## Approach C: Using LTX-2 Pipelines (ltx-pipelines)

### Text/Image-to-Video (Two-Stage HQ Pipeline)

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline
from ltx_core.components.guiders import MultiModalGuiderParams

# Configure LoRA
distilled_lora = [
    LoraPathStrengthAndSDOps(
        "/path/to/distilled_lora.safetensors",
        0.6,
        LTXV_LORA_COMFY_RENAMING_MAP
    ),
]

# Initialize pipeline
pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/ltx-2.3-22b-dev.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=[],
)

# Configure guidance
video_guider_params = MultiModalGuiderParams(
    cfg_scale=3.0,
    stg_scale=1.0,
    rescale_scale=0.7,
    modality_scale=3.0,
    skip_step=0,
    stg_blocks=[29],
)

audio_guider_params = MultiModalGuiderParams(
    cfg_scale=7.0,
    stg_scale=1.0,
    rescale_scale=0.7,
    modality_scale=3.0,
    skip_step=0,
    stg_blocks=[29],
)

# Generate
pipeline(
    prompt="A serene landscape with mountains",
    output_path="output.mp4",
    seed=42,
    height=512,
    width=768,
    num_frames=121,
    frame_rate=25.0,
    num_inference_steps=40,
    video_guider_params=video_guider_params,
    audio_guider_params=audio_guider_params,
)
```

### Image-to-Video with LTX-2 Pipelines

```python
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline, ImageConditioningInput

pipeline(
    prompt="A serene landscape with mountains",
    output_path="output.mp4",
    seed=42,
    height=512,
    width=768,
    num_frames=121,
    frame_rate=25.0,
    num_inference_steps=40,
    video_guider_params=video_guider_params,
    audio_guider_params=audio_guider_params,
    images=[ImageConditioningInput("input_image.jpg", 0, 1.0, 33)],
)
```

### CLI (LTX-2 Pipelines)

```bash
python -m ltx_pipelines.ti2vid_two_stages \
    --checkpoint-path /path/to/checkpoint.safetensors \
    --distilled-lora /path/to/distilled_lora.safetensors 0.8 \
    --spatial-upsampler-path /path/to/upsampler.safetensors \
    --gemma-root /path/to/gemma \
    --prompt "A beautiful sunset" \
    --output-path output.mp4
```

---

## Available Model Configurations (LTX-Video Repo)

| Config File | Description |
|-------------|-------------|
| `ltxv-13b-0.9.8-dev.yaml` | Highest quality, more VRAM |
| `ltxv-13b-0.9.8-distilled.yaml` | Faster, less VRAM |
| `ltxv-2b-0.9.8-distilled.yaml` | Smallest, fastest |
| `ltxv-13b-0.9.8-dev-fp8.yaml` | Quantized full model |
| `ltxv-13b-0.9.8-distilled-fp8.yaml` | Quantized distilled |
