# LTX Video - Installation and Setup Guide

> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
> **Source:** https://huggingface.co/Lightricks/LTX-Video

## Prerequisites

### Hardware Requirements

- **GPU:** NVIDIA GPU with CUDA support
- **VRAM Requirements by Configuration:**
  - ~10 GB VRAM: FP8 layerwise casting + group offloading (2B model)
  - ~16 GB VRAM: BFloat16 with model offloading (2B model)
  - ~24 GB VRAM: Full BFloat16 (2B model, no offloading)
  - ~40+ GB VRAM: 13B model without offloading
  - 2B distilled model recommended for resource-constrained environments
- **System RAM:** At least 2x model size when using CPU offloading with streams
- **CUDA:** CUDA 11.8 or higher recommended (for PyTorch 2.0+)

### Software Requirements

- Python 3.8+
- PyTorch 2.0+ (for SDPA/FlashAttention and torch.compile support)
- CUDA Toolkit 11.8+ (matching PyTorch build)

## Installation

### Basic Installation via pip

Install the latest stable Diffusers:

```bash
pip install -U diffusers transformers accelerate torch
```

For the latest features (may be required for newest LTX Video pipelines), install from source:

```bash
pip install -U git+https://github.com/huggingface/diffusers
```

### Full Installation with All Dependencies

```bash
# Core dependencies
pip install -U diffusers transformers accelerate torch torchvision

# For LoRA support
pip install -U peft

# For video export
pip install imageio imageio-ffmpeg

# For GGUF quantized model support
pip install -U gguf

# For xFormers memory-efficient attention (optional)
pip install xformers

# For TorchAO quantization (optional)
pip install torchao
```

### PyTorch Installation

If PyTorch is not installed or you need a specific CUDA version:

```bash
# CUDA 11.8
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# CUDA 12.1
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121

# CUDA 12.4
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu124
```

### Verify Installation

```python
import torch
import diffusers

print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
print(f"CUDA version: {torch.version.cuda}")
print(f"GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'N/A'}")
print(f"Diffusers version: {diffusers.__version__}")

# Test import of LTX Video pipelines
from diffusers import LTXPipeline, LTXImageToVideoPipeline
print("LTX Video pipelines imported successfully!")

# Check for optional features
try:
    from diffusers import LTXConditionPipeline
    print("LTXConditionPipeline available")
except ImportError:
    print("LTXConditionPipeline not available - update diffusers")

try:
    from diffusers import LTXLatentUpsamplePipeline
    print("LTXLatentUpsamplePipeline available")
except ImportError:
    print("LTXLatentUpsamplePipeline not available - update diffusers")
```

## Model Download

### Automatic Download (First Run)

Models are automatically downloaded from Hugging Face Hub on first use:

```python
import torch
from diffusers import LTXPipeline

# This will download the model (~4GB for 2B, ~26GB for 13B)
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video",
    torch_dtype=torch.bfloat16
)
```

### Pre-download Models

```bash
# Using huggingface-cli
pip install huggingface_hub
huggingface-cli download Lightricks/LTX-Video

# For specific model variant
huggingface-cli download Lightricks/LTX-Video-0.9.8-dev
huggingface-cli download Lightricks/ltxv-spatial-upscaler-0.9.7
```

### Available Models and Sizes

| Model ID | Size | Description |
|----------|------|-------------|
| `Lightricks/LTX-Video` | ~4 GB | Original 2B parameter model |
| `Lightricks/LTX-Video-0.9.5` | ~4 GB | 2B with multi-condition support |
| `Lightricks/LTX-Video-0.9.7-dev` | ~26 GB | 13B parameter model |
| `Lightricks/LTX-Video-0.9.7-distilled` | ~26 GB | 13B distilled (fast) |
| `Lightricks/LTX-Video-0.9.8-dev` | ~26 GB | Latest 13B model |
| `Lightricks/LTX-Video-0.9.8-13B-distilled` | ~26 GB | Latest 13B distilled |
| `Lightricks/ltxv-2b-0.9.8-distilled` | ~4 GB | Latest 2B distilled |
| `Lightricks/ltxv-spatial-upscaler-0.9.7` | ~1 GB | Latent space upscaler |

## Key Dependencies

| Package | Purpose | Minimum Version |
|---------|---------|----------------|
| `diffusers` | Pipeline framework | 0.30+ (latest recommended) |
| `transformers` | T5 text encoder | 4.30+ |
| `accelerate` | Device management, offloading | 0.20+ |
| `torch` | Deep learning framework | 2.0+ |
| `torchvision` | Image processing | Matching torch |
| `safetensors` | Efficient model loading | 0.3+ |
| `imageio` | Video export | 2.9+ |
| `imageio-ffmpeg` | FFmpeg backend for video | 0.4+ |
| `peft` | LoRA adapter support | 0.6+ (optional) |
| `xformers` | Memory-efficient attention | Latest (optional) |

## Component Architecture

An LTX Video pipeline consists of these components:

1. **Transformer** (`LTXVideoTransformer3DModel`) -- The core diffusion model
2. **VAE** (`AutoencoderKLLTXVideo`) -- Video encoder/decoder with 1:192 compression
3. **Text Encoder** (`T5EncoderModel`) -- Google T5 v1.1 XXL for text understanding
4. **Tokenizer** (`T5TokenizerFast`) -- Text tokenizer
5. **Scheduler** (`FlowMatchEulerDiscreteScheduler`) -- Noise scheduling

Individual components can be loaded separately:

```python
from diffusers import AutoModel, AutoencoderKLLTXVideo, LTXVideoTransformer3DModel
import torch

# Load transformer separately
transformer = LTXVideoTransformer3DModel.from_pretrained(
    "Lightricks/LTX-Video", subfolder="transformer", torch_dtype=torch.bfloat16
)

# Or use AutoModel
transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video", subfolder="transformer", torch_dtype=torch.bfloat16
)

# Load VAE separately
vae = AutoencoderKLLTXVideo.from_pretrained(
    "Lightricks/LTX-Video", subfolder="vae", torch_dtype=torch.float32
)
```

## Quick Start

### Minimal Text-to-Video

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

video = pipe(
    prompt="A cat playing with a ball of yarn",
    negative_prompt="worst quality, blurry, distorted",
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

### Minimal Image-to-Video

```python
import torch
from diffusers import LTXImageToVideoPipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXImageToVideoPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

image = load_image("input.png")
video = pipe(
    image=image,
    prompt="The scene comes to life with gentle motion",
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]

export_to_video(video, "output.mp4", fps=24)
```

## Troubleshooting

### Common Issues

**CUDA Out of Memory:**
- Use `torch.bfloat16` dtype
- Enable group offloading or model offloading
- Use FP8 layerwise casting
- Enable VAE tiling: `pipeline.vae.enable_tiling()`
- Reduce resolution or frame count

**Slow Generation:**
- Use distilled models (4-10 steps vs 50)
- Enable `torch.compile`
- Use `torch.bfloat16` instead of `float32`

**Import Errors for New Pipelines:**
- Install latest diffusers from source: `pip install -U git+https://github.com/huggingface/diffusers`

**Poor Quality Output:**
- Use detailed, elaborate prompts in English
- For non-distilled models, use `guidance_scale=5.0`
- For distilled models, use `guidance_scale=1.0`
- Set `decode_timestep=0.05` and `decode_noise_scale=0.025` for v0.9.1+
