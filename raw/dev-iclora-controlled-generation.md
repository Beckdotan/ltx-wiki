# LTX-Video ICLoRA Controlled Generation

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Date:** 2026-04-13

---

## Overview

ICLoRA (Image-Conditioned LoRA) adapters enable controlled video generation with LTX-Video. These adapters allow conditioning on structural signals like depth maps, pose estimation, and edge detection, giving users precise control over the spatial layout and motion of generated videos.

---

## Available ICLoRA Adapters

| Adapter | Conditioning Type | Input | Use Case |
|---------|------------------|-------|----------|
| Depth ICLoRA | Depth maps | Monocular depth estimation | Scene layout, 3D structure |
| Pose ICLoRA | Pose estimation | OpenPose keypoints | Character animation, motion |
| Canny ICLoRA | Edge detection | Canny edge maps | Structural guidance, outlines |

---

## How ICLoRA Works

1. **Extract conditioning signal** from input image/video (e.g., run depth estimation)
2. **Load the ICLoRA adapter** on top of the base LTX-Video model
3. **Pass conditioning signal** alongside the text prompt
4. **Generate video** that follows both the structural conditioning and text description

The adapter modifies attention layers in the DiT transformer to attend to the structural conditioning signal while maintaining the generative quality of the base model.

---

## Usage with Diffusers

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

# Load base model
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
).to("cuda")

# Load ICLoRA adapter (example: depth)
pipe.load_lora_weights(
    "Lightricks/LTX-Video",
    subfolder="iclora-depth",  # or iclora-pose, iclora-canny
    adapter_name="depth_adapter"
)

# Prepare depth-conditioned input
# (depth map should be extracted from your input video/image)
depth_condition = LTXVideoCondition(
    video=depth_map_frames,
    frame_index=0
)

result = pipe(
    conditions=[depth_condition],
    prompt="A person walking through a garden",
    negative_prompt="worst quality, blurry",
    width=768,
    height=512,
    num_frames=97,
    num_inference_steps=30,
).frames[0]
```

---

## Usage with ComfyUI

The ComfyUI-LTXVideo custom nodes include an `LTXVICLoRA` node for loading adapters:

1. Load base model with `LTXVLoader`
2. Load ICLoRA adapter with `LTXVICLoRA` node
3. Provide conditioning signal (depth/pose/canny map)
4. Connect to `LTXVSampler` and generate

---

## Generating Conditioning Signals

### Depth Maps
```python
from transformers import pipeline

depth_estimator = pipeline("depth-estimation", model="Intel/dpt-large")
depth_map = depth_estimator("input_image.jpg")
```

### Pose Estimation
Use OpenPose or similar pose estimation tools to extract keypoints from reference images/videos.

### Canny Edge Detection
```python
import cv2
import numpy as np

image = cv2.imread("input_image.jpg")
edges = cv2.Canny(image, 100, 200)
```

---

## Applications

- **Character re-animation** - Apply new motions to characters using pose conditioning
- **Scene reconstruction** - Generate new views of scenes using depth conditioning
- **Style transfer with structure** - Maintain structural layout while changing visual style
- **Storyboard-to-video** - Convert sketches/outlines to full video using edge conditioning

---

## Related Links

- **LTX-Video Model Hub:** https://huggingface.co/Lightricks/LTX-Video
- **ComfyUI-LTXVideo:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Diffusers LoRA Docs:** https://huggingface.co/docs/diffusers/main/en/training/lora
