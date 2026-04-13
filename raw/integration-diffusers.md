# LTX Video HuggingFace Diffusers Integration

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-2.3/discussions/4

---

## Overview

HuggingFace Diffusers is the primary Python library integration for LTX Video. Support varies by model version.

## LTX-Video (Original) - Full Diffusers Support

The original LTX-Video has full Diffusers support via `LTXConditionPipeline` and `LTXLatentUpsamplePipeline`.

### Example Usage
```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", 
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")

video = pipe(
    conditions=[condition],
    prompt="Your detailed prompt here",
    negative_prompt="worst quality, inconsistent motion, blurry",
    width=832,
    height=480,
    num_frames=96,
    num_inference_steps=30,
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

### Community Diffusers Format Conversions
- **a-r-r-o-w/LTX-Video-diffusers** - 151 downloads, diffusers format
- **a-r-r-o-w/LTX-Video-0.9.7-diffusers** - 6,390 downloads
- **a-r-r-o-w/LTX-Video-0.9.1-diffusers** - 96 downloads
- **diffusers/LTX-Video-0.9.0** - Official diffusers conversion
- **magespace/LTX-Video-diffusers** - Community conversion

## LTX-2 - Full Diffusers Support

LTX-2 has Diffusers support via `LTX2Pipeline`.

### Example Usage
```python
import torch
from diffusers import FlowMatchEulerDiscreteScheduler
from diffusers.pipelines.ltx2 import LTX2Pipeline

pipe = LTX2Pipeline.from_pretrained(
    "Lightricks/LTX-2", torch_dtype=torch.bfloat16
)
pipe.enable_sequential_cpu_offload(device="cuda:0")

video_latent, audio_latent = pipe(
    prompt="A beautiful sunset over the ocean",
    width=768,
    height=512,
    num_frames=121,
    frame_rate=24.0,
    num_inference_steps=40,
    guidance_scale=4.0,
    output_type="latent",
    return_dict=False,
)
```

## LTX-2.3 - Diffusers Support Pending

As of April 2026, LTX-2.3 does **not yet have official Diffusers support**.

### Community Discussion (Discussion #4)
- **tintwotin** (Mar 5, 2026): Asked if there would be an official Diffusers release
- **cupra-ai** (Mar 27, 2026): Confirmed Diffusers does NOT officially support LTX-2.3 yet
- Pending PR: https://github.com/huggingface/diffusers/pull/13217
- PR remains unmerged

### Key Finding from Discussion
- LTX-2.0 trained LoRAs work significantly better on LTX-2.3
- LoRAs that were unusable for further training on 2.0 become usable on 2.3
- Inference requires reduced strength values when using 2.0 LoRAs on 2.3

### Missing `model_index.json`
Multiple users reported that LTX-2.3 fails to load with Diffusers due to missing `model_index.json` files (Discussions #20, #31). The community has requested these files be added for compatibility.

## Swift MLX Support Request

Discussion #22 on LTX-2.3 requests Swift MLX support for running on Apple Silicon devices, indicating community interest in non-Python/non-CUDA platforms.
