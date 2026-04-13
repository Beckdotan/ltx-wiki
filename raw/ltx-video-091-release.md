# LTX-Video 0.9.1 Release (December 2024)

**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.1
**Source:** https://x.com/LTXStudio/status/1870099463416803594
**Source:** https://deepnewz.com/ai-modeling/ltx-video-ai-model-version-0-9-1-released-enhanced-image-to-video-quality-faster-a247473d

## Release Details

- **Release date:** December 2024
- **Parameters:** 2B
- **Announced by:** LTX Studio on X (Twitter)

## What Changed from 0.9.0

Version 0.9.1 was positioned as "a big update to LTX Video" with the following improvements:

### Motion Quality
- **Smoother motion** -- more natural and fluid video movement
- **Improved physics** -- better physical plausibility in generated scenes
- **Cleaner visuals** -- reduced artifacts and visual noise

### Image-to-Video Quality
- Significant boost in image-to-video generation quality
- Enhanced fidelity when conditioning on input images

### Speed
- Maintained the faster-than-real-time generation speeds from 0.9.0
- "Stunning results without compromising on speed"

### Video Enhancement
- First training-free generated video enhancement capability
- Ability to improve generated video quality post-generation

## Output Specifications (Unchanged from 0.9.0)

- **Resolution:** 1216x704 native, supports resolutions divisible by 32
- **Frame Rate:** 30 FPS
- **Maximum Frames:** 257 (must be divisible by 8+1)
- **Best under:** 720x1280 resolution, 257 frames

## Inference Details

```python
# Diffusers integration example
from diffusers import LTXPipeline
import torch

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

video = pipe(
    prompt=prompt,
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
```

## Hardware Requirements

- Python 3.10.5+
- CUDA 12.2
- PyTorch >= 2.1.2

## Hugging Face

- **Repository:** https://huggingface.co/Lightricks/LTX-Video-0.9.1
- **Model type:** Diffusion-based text-to-video and image-to-video
- **Language:** English prompts only
