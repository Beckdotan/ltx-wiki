---
title: Python LoRA Loading and Management
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://huggingface.co/docs/diffusers/tutorials/using_peft_for_inference
  - https://docs.ltx.video/open-source-model/usage-guides/lo-ra
  - https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/
tags:
  - python
  - lora
  - fine-tuning
  - adapters
  - diffusers
---

# Python LoRA Loading and Management

LoRA (Low-Rank Adaptation) adapters are lightweight fine-tuned weight modifications (typically a few hundred MBs) that add new styles, effects, or capabilities to LTX-Video. Supported through both [[diffusers-integration]] and the [[sdk-ltx-2]] native API.

## Basic Loading (Diffusers)

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

# Use trigger word in prompt
prompt = "CAKEIFY a person using a knife to cut a cake shaped like a Pikachu plushie"
```

## Loading in LTX-2 Pipelines

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline

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
    loras=custom_loras,
    # ...
)
```

## Managing Multiple LoRAs

### Loading Multiple Adapters

```python
pipeline.load_lora_weights("path/to/lora1", adapter_name="style1")
pipeline.load_lora_weights("path/to/lora2", adapter_name="style2")
```

### Switching Between LoRAs

```python
pipeline.set_adapters("style1")

# Multiple LoRAs with individual weights
pipeline.set_adapters(["style1", "style2"], adapter_weights=[0.7, 0.3])
```

### Scaling LoRA Weight

```python
# Method 1: Using set_adapters
pipeline.set_adapters("cakeify", adapter_weights=0.8)

# Method 2: cross_attention_kwargs (simple cases)
video = pipeline(
    prompt="...",
    cross_attention_kwargs={"scale": 0.8},
).frames[0]
```

### Disabling, Unloading, Deleting

```python
pipeline.disable_lora()           # Disable without unloading
pipeline.unload_lora_weights()    # Remove LoRA weights from memory
pipeline.delete_adapters("style1") # Delete specific adapter

# List active adapters
active = pipeline.get_active_adapters()
adapters_by_component = pipeline.get_list_adapters()
```

## LoRA Strength Guidelines

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

## Fusing LoRAs for Performance

Permanently merges LoRA weights into the base model, removing per-step overhead:

```python
pipeline.fuse_lora(adapter_names=["cakeify"], lora_scale=1.0)
pipeline.unload_lora_weights()  # They're now part of the model

# Save the fused model
pipeline.save_pretrained("path/to/fused-pipeline")

# Unfuse to restore original (single LoRA only)
pipeline.unfuse_lora()
```

## LoRA Hotswapping

Efficiently swap LoRAs without reloading:

```python
pipeline.load_lora_weights("path/to/lora1", adapter_name="lora1")

# In-place replacement
pipeline.load_lora_weights(
    "path/to/lora2",
    hotswap=True,
    adapter_name="lora1"  # Same name as the one to replace
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
    "path/to/lora2", hotswap=True, adapter_name="lora1"
)
```

## Using LoRA with torch.compile

For maximum [[python-performance-speed]], fuse the LoRA before compiling:

```python
pipeline.load_lora_weights("path/to/lora", adapter_name="my_lora")
pipeline.set_adapters("my_lora")
pipeline.fuse_lora(adapter_names=["my_lora"], lora_scale=1.0)
pipeline.unload_lora_weights()

pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)
```

## Advanced Merge with PEFT (TIES/DARE)

```python
from peft import PeftModel

model.add_weighted_adapter(
    adapters=["lora1", "lora2"],
    combination_type="dare_linear",  # or "ties"
    weights=[1.0, 1.0],
    adapter_name="merged"
)
model.set_adapters("merged")
```

## Loading LoRA on Quantized Models

LoRAs can be loaded on top of [[python-performance-memory]] quantized models:

```python
transformer = AutoModel.from_single_file(
    "path/to/gguf-model.gguf",
    quantization_config=GGUFQuantizationConfig(compute_dtype=torch.bfloat16),
    torch_dtype=torch.bfloat16
)
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", transformer=transformer, torch_dtype=torch.bfloat16
)
pipeline.load_lora_weights("path/to/lora", adapter_name="my_lora")
```

## Available Official LoRAs

| LoRA | Trigger Word | Description |
|------|-------------|-------------|
| `Lightricks/LTX-Video-Cakeify-LoRA` | "CAKEIFY" | Transforms objects into cake versions |
| Camera Control LoRAs (7 types) | N/A | Dolly left/right/in/out, jib up/down, static |
| Control LoRAs (depth, pose, Canny) | Various | Structural control adapters |

## Compatibility Note

LTX-2 LoRAs do NOT transfer to LTX-2.3 due to VAE redesign and text connector changes.

## See Also

- [[python-ic-lora]] -- Identity-Consistent LoRA for video control
- [[python-performance-speed]] -- Fusing LoRA before torch.compile
- [[python-callbacks]] -- Dynamic LoRA scale scheduling during sampling
- [[sdk-ltx-2]] -- LTX-2 native LoRA loading API
