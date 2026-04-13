# LTX Video - LoRA Loading and Usage Guide

> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
> **Source:** https://huggingface.co/docs/diffusers/tutorials/using_peft_for_inference

## Overview

LTX-Video supports LoRA (Low-Rank Adaptation) adapters through the Diffusers library. LoRAs are lightweight fine-tuned weight modifications (typically a few hundred MBs) that can be loaded on top of the base model to add new styles, effects, or capabilities.

## Loading a LoRA

### Basic LoRA Loading with LTXConditionPipeline

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.utils import export_to_video, load_image

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)

pipeline.load_lora_weights(
    "Lightricks/LTX-Video-Cakeify-LoRA",
    weight_name="ltxv_095_cakeify_lora.safetensors",
    adapter_name="cakeify"
)
pipeline.set_adapters("cakeify")

# Use "CAKEIFY" trigger word to activate the LoRA
prompt = "CAKEIFY a person using a knife to cut a cake shaped like a Pikachu plushie"
image = load_image(
    "https://huggingface.co/Lightricks/LTX-Video-Cakeify-LoRA/resolve/main/assets/images/pikachu.png"
)

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

### Loading LoRA from Older Pipeline (LTXPipeline)

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)

# Load LoRA weights
pipeline.load_lora_weights(
    "Lightricks/LTX-Video-Cakeify-LoRA",
    adapter_name="cakeify"
)
pipeline.set_adapters("cakeify")
pipeline.to("cuda")

video = pipeline(
    prompt="CAKEIFY a cute cat sitting on a table",
    negative_prompt="worst quality, blurry",
    width=576,
    height=576,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

## Available Official LoRAs

Lightricks provides several official LoRAs for LTX Video:

| LoRA | Trigger Word | Description |
|------|-------------|-------------|
| `Lightricks/LTX-Video-Cakeify-LoRA` | "CAKEIFY" | Transforms objects into cake versions |
| Control LoRAs (depth, pose, Canny) | Various | Structural control adapters |

## Managing Multiple LoRAs

### Loading Multiple LoRAs

```python
pipeline.load_lora_weights(
    "path/to/lora1",
    weight_name="lora1.safetensors",
    adapter_name="style1"
)
pipeline.load_lora_weights(
    "path/to/lora2",
    weight_name="lora2.safetensors",
    adapter_name="style2"
)
```

### Switching Between LoRAs

```python
# Activate a specific LoRA
pipeline.set_adapters("style1")

# Activate multiple LoRAs with different weights
pipeline.set_adapters(["style1", "style2"], adapter_weights=[0.7, 0.3])
```

### Scaling LoRA Weight

```python
# Method 1: Using set_adapters with weights
pipeline.set_adapters("cakeify", adapter_weights=0.8)

# Method 2: Using cross_attention_kwargs (simple cases)
video = pipeline(
    prompt="...",
    cross_attention_kwargs={"scale": 0.8},
    # ... other params
).frames[0]
```

### LoRA Scale Scheduling

Dynamically adjust LoRA scale during sampling for better control:

```python
import torch

num_inference_steps = 30
lora_steps = 20
# Start strong, gradually decay
lora_scales = torch.linspace(1.5, 0.7, lora_steps).tolist()
lora_scales += [0.2] * (num_inference_steps - lora_steps + 1)

pipeline.set_adapters("my_lora", lora_scales[0])

def callback(pipeline, step, timestep, callback_kwargs):
    pipeline.set_adapters("my_lora", lora_scales[step + 1])
    return callback_kwargs

video = pipeline(
    prompt="...",
    num_inference_steps=num_inference_steps,
    callback_on_step_end=callback,
    # ... other params
).frames[0]
```

### Disabling a LoRA (Without Unloading)

```python
pipeline.disable_lora()
```

### Unloading LoRA Weights

```python
pipeline.unload_lora_weights()
```

### Deleting a LoRA

```python
pipeline.delete_adapters("style1")
```

### Listing Active Adapters

```python
# Get active LoRAs
active = pipeline.get_active_adapters()
print(active)  # ["cakeify", "style1"]

# Get adapters per component
adapters_by_component = pipeline.get_list_adapters()
print(adapters_by_component)  # {"transformer": ["cakeify"]}
```

## Fusing LoRAs for Performance

Fusing permanently merges LoRA weights into the base model, removing per-step overhead:

```python
# Fuse LoRA into base model
pipeline.fuse_lora(adapter_names=["cakeify"], lora_scale=1.0)

# Unload the separate LoRA weights (they're now part of the model)
pipeline.unload_lora_weights()

# Save the fused model
pipeline.save_pretrained("path/to/fused-pipeline")

# Optional: unfuse to restore original weights (only for single LoRA)
pipeline.unfuse_lora()
```

## LoRA Hotswapping

Efficiently swap LoRAs without reloading:

```python
# Load first LoRA
pipeline.load_lora_weights(
    "path/to/lora1",
    adapter_name="lora1"
)

# Hotswap to second LoRA (in-place replacement)
pipeline.load_lora_weights(
    "path/to/lora2",
    hotswap=True,
    adapter_name="lora1"  # same adapter name as the one to replace
)
```

### Hotswapping with Compiled Models

```python
# 1. Enable hotswap BEFORE loading first LoRA
pipeline.enable_lora_hotswap(target_rank="max_rank")

# 2. Load first LoRA
pipeline.load_lora_weights("path/to/lora1", adapter_name="lora1")

# 3. Compile AFTER loading first LoRA
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="reduce-overhead", fullgraph=True
)

# 4. Hotswap without recompilation
pipeline.load_lora_weights(
    "path/to/lora2",
    hotswap=True,
    adapter_name="lora1"
)
```

## Using LoRA with torch.compile

For maximum performance, fuse the LoRA before compiling:

```python
# Load and fuse
pipeline.load_lora_weights("path/to/lora", adapter_name="my_lora")
pipeline.set_adapters("my_lora")
pipeline.fuse_lora(adapter_names=["my_lora"], lora_scale=1.0)
pipeline.unload_lora_weights()

# Then compile
pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)
```

## Merging Multiple LoRAs

### Using set_adapters (Simple Merge)

```python
pipeline.load_lora_weights("lora1_path", adapter_name="style1")
pipeline.load_lora_weights("lora2_path", adapter_name="style2")
pipeline.set_adapters(["style1", "style2"], adapter_weights=[0.7, 0.8])
```

### Using fuse_lora (Permanent Merge)

```python
pipeline.load_lora_weights("lora1_path", adapter_name="style1")
pipeline.load_lora_weights("lora2_path", adapter_name="style2")
pipeline.set_adapters(["style1", "style2"], adapter_weights=[0.7, 0.8])
pipeline.fuse_lora(adapter_names=["style1", "style2"], lora_scale=1.0)
pipeline.unload_lora_weights()
```

### Advanced Merge with PEFT (TIES/DARE)

For more sophisticated merging methods:

```python
from peft import PeftModel
import copy

# Create PeftModel from LoRA checkpoints
# Then use add_weighted_adapter with combination_type
model.add_weighted_adapter(
    adapters=["lora1", "lora2"],
    combination_type="dare_linear",  # or "ties"
    weights=[1.0, 1.0],
    adapter_name="merged"
)
model.set_adapters("merged")
```

## Loading GGUF Quantized Models with LoRA

```python
import torch
from diffusers import LTXPipeline, AutoModel, GGUFQuantizationConfig

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

# LoRA can then be loaded on top
pipeline.load_lora_weights("path/to/lora", adapter_name="my_lora")
```
