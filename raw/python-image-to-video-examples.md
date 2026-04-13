# LTX Video - Image-to-Video Python Code Examples

> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
> **Source:** https://huggingface.co/Lightricks/LTX-Video

## Basic Image-to-Video with LTXImageToVideoPipeline

The simplest approach using the dedicated image-to-video pipeline:

```python
import torch
from diffusers import LTXImageToVideoPipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXImageToVideoPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

image = load_image(
    "https://huggingface.co/datasets/a-r-r-o-w/tiny-meme-dataset-captioned/resolve/main/images/8.png"
)
prompt = "A young girl stands calmly in the foreground, looking directly at the camera, as a house fire rages in the background. Flames engulf the structure, with smoke billowing into the air. Firefighters in protective gear rush to the scene, a fire truck labeled '38' visible behind them. The girl's neutral expression contrasts sharply with the chaos of the fire, creating a poignant and emotionally charged scene."
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

## Image-to-Video with LTXConditionPipeline (Recommended for 0.9.5+)

Using the more flexible condition pipeline with LTXVideoCondition:

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video

# Load pipelines
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", 
    torch_dtype=torch.bfloat16
)
pipe_upsample = LTXLatentUpsamplePipeline.from_pretrained(
    "Lightricks/ltxv-spatial-upscaler-0.9.8", 
    vae=pipe.vae, 
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")
pipe_upsample.to("cuda")
pipe.vae.enable_tiling()

# Prepare condition from an image
# The image must be wrapped as a single-frame video
image = load_image("penguin.png")
video = load_video(export_to_video([image]))
condition1 = LTXVideoCondition(video=video, frame_index=0)

# Generate at downscaled resolution
latents = pipe(
    conditions=[condition1],
    prompt="A cute little penguin takes out a book and starts reading it",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=832,
    height=480,
    num_frames=96,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(0),
    output_type="latent",
).frames

# Upscale and denoise
upscaled_latents = pipe_upsample(latents=latents, output_type="latent").frames
video = pipe(
    conditions=[condition1],
    prompt="A cute little penguin takes out a book and starts reading it",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=1664,
    height=960,
    num_frames=96,
    denoise_strength=0.4,
    num_inference_steps=10,
    latents=upscaled_latents,
    decode_timestep=0.05,
    image_cond_noise_scale=0.025,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

## Multi-Condition Generation (Image + Video)

Combining an image and a video as conditioning inputs at different frame positions:

```python
import torch
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXConditionPipeline, LTXVideoCondition
from diffusers.utils import export_to_video, load_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)
pipe.to("cuda")

# Load input image and video
video_input = load_video(
    "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/cosmos/cosmos-video2world-input-vid.mp4"
)
image = load_image(
    "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/cosmos/cosmos-video2world-input.jpg"
)

# Create conditioning objects at different frame positions
condition1 = LTXVideoCondition(
    image=image,
    frame_index=0,   # Condition the first frame
)
condition2 = LTXVideoCondition(
    video=video_input,
    frame_index=80,  # Condition at frame 80
)

prompt = "The video depicts a long, straight highway stretching into the distance, flanked by metal guardrails. The road is divided into multiple lanes, with a few vehicles visible in the far distance. The surrounding landscape features dry, grassy fields on one side and rolling hills on the other. The sky is mostly clear with a few scattered clouds, suggesting a bright, sunny day. And then the camera switch to a winding mountain road covered in snow, with a single vehicle traveling along it."
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

# Generate video
generator = torch.Generator("cuda").manual_seed(0)
# Text-only conditioning is also supported without passing `conditions`
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

## Video-to-Video with LTXConditionPipeline

Using a full video as conditioning input for style transfer or editing:

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipeline.vae.enable_tiling()

# Load source video (first 21 frames for conditioning)
video = load_video(
    "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/diffusers/cosmos/cosmos-video2world-input-vid.mp4"
)[:21]  # only use first 21 frames as conditioning

condition1 = LTXVideoCondition(video=video, frame_index=0)

prompt = """
The video depicts a winding mountain road covered in snow, with a single vehicle 
traveling along it. The road is flanked by steep, rocky cliffs and sparse vegetation. 
"""
negative_prompt = "worst quality, inconsistent motion, blurry, jittery, distorted"

video_output = pipeline(
    conditions=[condition1],
    prompt=prompt,
    negative_prompt=negative_prompt,
    width=768,
    height=512,
    num_frames=161,
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    image_cond_noise_scale=0.0,
    guidance_scale=5.0,
    guidance_rescale=0.7,
    generator=torch.Generator().manual_seed(0),
    output_type="pil",
).frames[0]

export_to_video(video_output, "output.mp4", fps=24)
```

## Video-to-Video Editing with denoise_strength

Using `denoise_strength` to control how much the output differs from the input:

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.7-dev", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")
pipeline.vae.enable_tiling()

# Load source video
source_video = load_video("input_video.mp4")
condition = LTXVideoCondition(video=source_video, frame_index=0)

# Lower denoise_strength = closer to original
# Higher denoise_strength = more creative freedom
video = pipeline(
    conditions=[condition],
    prompt="Transform the scene into a winter wonderland with falling snow",
    negative_prompt="worst quality, blurry, distorted",
    width=768,
    height=512,
    num_frames=len(source_video),
    denoise_strength=0.6,  # Controls how much to change the original
    num_inference_steps=30,
    decode_timestep=0.05,
    decode_noise_scale=0.025,
    guidance_scale=5.0,
    generator=torch.Generator().manual_seed(42),
    output_type="pil",
).frames[0]

export_to_video(video, "edited_output.mp4", fps=24)
```

## Image-to-Video with Long Multi-Prompt Pipeline

Using LTXI2VLongMultiPromptPipeline for long videos with scene transitions:

```python
import torch
from diffusers import LTXEulerAncestralRFScheduler, LTXI2VLongMultiPromptPipeline
from diffusers.utils import load_image

pipe = LTXI2VLongMultiPromptPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled"
)
pipe.scheduler = LTXEulerAncestralRFScheduler.from_config(pipe.scheduler.config)
pipe = pipe.to("cuda").to(dtype=torch.bfloat16)

# Load conditioning image
cond_image = load_image("first_frame.png")

out = pipe(
    prompt="a person walks through a forest | the person arrives at a clearing | sunset over the clearing",
    cond_image=cond_image,
    cond_strength=0.5,
    num_frames=257,
    height=512,
    width=704,
    temporal_tile_size=80,
    temporal_overlap=24,
    num_inference_steps=8,
    guidance_scale=1.0,
    seed=42,
    output_type="pil",
    return_dict=True,
)
frames = out.frames[0]

from diffusers.utils import export_to_video
export_to_video(frames, "long_video.mp4", fps=25)
```
