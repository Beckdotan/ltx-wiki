# LTX Video - Text-to-Video Python Code Examples

> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
> **Source:** https://huggingface.co/Lightricks/LTX-Video

## Basic Text-to-Video (LTXPipeline)

The simplest way to generate video from text using the original 2B model:

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

## Memory-Optimized Text-to-Video (~10GB VRAM)

Uses FP8 layerwise weight-casting and group offloading for low VRAM usage:

```python
import torch
from diffusers import LTXPipeline, AutoModel
from diffusers.hooks import apply_group_offloading
from diffusers.utils import export_to_video

# fp8 layerwise weight-casting
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video",
    subfolder="transformer",
    torch_dtype=torch.bfloat16
)
transformer.enable_layerwise_casting(
    storage_dtype=torch.float8_e4m3fn, compute_dtype=torch.bfloat16
)

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", transformer=transformer, torch_dtype=torch.bfloat16
)

# group-offloading
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

prompt = """
A woman with long brown hair and light skin smiles at another woman with long blonde hair.
The woman with brown hair wears a black jacket and has a small, barely noticeable mole on her right cheek.
The camera angle is a close-up, focused on the woman with brown hair's face. The lighting is warm and 
natural, likely from the setting sun, casting a soft glow on the scene. The scene appears to be real-life footage
"""
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

## Text-to-Video with LTXConditionPipeline (0.9.5+)

The `LTXConditionPipeline` supports text-only conditioning without passing `conditions`:

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipeline.vae.enable_tiling()

prompt = """
The video depicts a winding mountain road covered in snow, with a single vehicle 
traveling along it. The road is flanked by steep, rocky cliffs and sparse vegetation. 
The landscape is characterized by rugged terrain and a river visible in the distance. 
The scene captures the solitude and beauty of a winter drive through a mountainous region.
"""
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

# Text-only conditioning is supported without passing `conditions`
video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=768,
    height=512,
    num_frames=161,
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=5.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

## Text-to-Video with Latent Upscaling (0.9.7+)

Two-stage generation: low-res first, then upscale:

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipeline_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.7",
    vae=pipeline.vae,
    torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipeline_upsample.to("cuda")
pipeline.vae.enable_tiling()

def round_to_nearest_resolution_acceptable_by_vae(height, width):
    height = height - (height % pipeline.vae_temporal_compression_ratio)
    width = width - (width % pipeline.vae_temporal_compression_ratio)
    return height, width

prompt = """
artistic anatomical 3d render, ultra quality, human half full male body with transparent 
skin revealing structure instead of organs, muscular, intricate creative patterns, 
monochromatic with backlighting, lightning mesh, scientific concept art
"""
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 768, 1152
downscale_factor = 2 / 3
num_frames = 161

# 1. Generate video at smaller resolution
downscaled_height, downscaled_width = int(expected_height * downscale_factor), int(expected_width * downscale_factor)
downscaled_height, downscaled_width = round_to_nearest_resolution_acceptable_by_vae(downscaled_height, downscaled_width)

latents = pipeline(
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

# 2. Upscale generated video (2x height/width)
upscaled_height, upscaled_width = downscaled_height * 2, downscaled_width * 2
upscaled_latents = pipeline_upsample(
    latents=latents,
    output_type="latent"
).frames

# 3. Denoise the upscaled video to improve texture (optional but recommended)
video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=num_frames,
    denoise_strength=0.4,  # Effectively, 4 inference steps out of 10
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

# 4. Downscale to expected resolution
video = [frame.resize((expected_width, expected_height)) for frame in video]

export_to_video(video, "output.mp4", fps=24)
```

## Text-to-Video with Distilled Model (Fast, 4-10 Steps)

Uses the distilled model with custom timesteps for rapid generation:

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

def round_to_nearest_resolution_acceptable_by_vae(height, width):
    height = height - (height % pipeline.vae_spatial_compression_ratio)
    width = width - (width % pipeline.vae_spatial_compression_ratio)
    return height, width

prompt = "A cinematic shot of a futuristic city at sunset with flying cars"
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 768, 1152
downscale_factor = 2 / 3
num_frames = 161

# 1. Generate at smaller resolution with custom distilled timesteps
downscaled_height, downscaled_width = int(expected_height * downscale_factor), int(expected_width * downscale_factor)
downscaled_height, downscaled_width = round_to_nearest_resolution_acceptable_by_vae(downscaled_height, downscaled_width)

latents = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=num_frames,
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],  # Recommended for base
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=1.0,  # Must be 1.0 for distilled models
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# 2. Upscale
upscaled_height, upscaled_width = downscaled_height * 2, downscaled_width * 2
upscaled_latents = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    output_type="latent"
).frames

# 3. Denoise upscaled video with custom upscaling timesteps
video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=num_frames,
    denoise_strength=0.999,  # Effectively, 4 steps out of 5
    timesteps=[1000, 909, 725, 421, 0],  # Recommended for upscaling
    latents=upscaled_latents,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=1.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

# 4. Downscale to expected resolution
video = [frame.resize((expected_width, expected_height)) for frame in video]

export_to_video(video, "output.mp4", fps=24)
```

## Long Text-to-Video with 0.9.8 Distilled (Up to 361 Frames)

Uses the 0.9.8 model with `tone_map_compression_ratio` for improved quality:

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.pipelines.ltx.modeling_latent_upsampler import LTXLatentUpsamplerModel
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled", torch_dtype=torch.bfloat16
)
upsampler = LTXLatentUpsamplerModel.from_pretrained(
    "a-r-r-o-w/LTX-0.9.8-Latent-Upsampler", torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline(
    vae=pipeline.vae, latent_upsampler=upsampler
).to(torch.bfloat16)
pipeline.to("cuda")
pipe_upsample.to("cuda")
pipeline.vae.enable_tiling()

def round_to_nearest_resolution_acceptable_by_vae(height, width):
    height = height - (height % pipeline.vae_spatial_compression_ratio)
    width = width - (width % pipeline.vae_spatial_compression_ratio)
    return height, width

prompt = """The camera pans over a snow-covered mountain range, revealing a vast expanse of 
snow-capped peaks and valleys. The mountains are covered in a thick layer of snow, with some 
areas appearing almost white while others have a slightly darker, almost grayish hue."""
negative_prompt = "bright colors, symbols, graffiti, watermarks, worst quality, inconsistent motion, blurry, jittery, distorted"
expected_height, expected_width = 480, 832
downscale_factor = 2 / 3
num_frames = 361  # Long video support!

# 1. Generate at smaller resolution
downscaled_height, downscaled_width = int(expected_height * downscale_factor), int(expected_width * downscale_factor)
downscaled_height, downscaled_width = round_to_nearest_resolution_acceptable_by_vae(downscaled_height, downscaled_width)

latents = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=downscaled_width,
    height=downscaled_height,
    num_frames=num_frames,
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=1.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# 2. Upscale with tone mapping
upscaled_height, upscaled_width = downscaled_height * 2, downscaled_width * 2
upscaled_latents = pipe_upsample(
    latents=latents,
    adain_factor=1.0,
    tone_map_compression_ratio=0.6,  # New in 0.9.8 -- improves quality
    output_type="latent"
).frames

# 3. Denoise
video = pipeline(
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=upscaled_width,
    height=upscaled_height,
    num_frames=num_frames,
    denoise_strength=0.999,
    timesteps=[1000, 909, 725, 421, 0],
    latents=upscaled_latents,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=1.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

# 4. Downscale
video = [frame.resize((expected_width, expected_height)) for frame in video]

export_to_video(video, "output.mp4", fps=24)
```

## Long Multi-Prompt Video Generation

Using the LTXI2VLongMultiPromptPipeline for scene transitions:

```python
import torch
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
# For ComfyUI parity, swap in the RF scheduler
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

# Use | to separate prompts for different temporal windows
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
frames = out.frames[0]  # list of PIL.Image.Image

# Or use latent output for later decoding
out_latent = pipe(
    prompt="a chimpanzee walking",
    output_type="latent",
    return_dict=True
).frames
frames = pipe.vae_decode_tiled(out_latent, output_type="pil")[0]
```
