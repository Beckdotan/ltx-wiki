# LTX Video Installation and Setup

> **Sources:**
> - https://github.com/Lightricks/LTX-Video
> - https://github.com/Lightricks/LTX-2
> - https://pypi.org/project/ltx-video/
> - https://deepwiki.com/Lightricks/LTX-Video/1.1-installation-and-setup
> - https://apatero.com/blog/ltx-2-standalone-gradio-installation-guide-2025
> - https://github.com/Lightricks/LTX-Video-Q8-Kernels

---

## System Requirements

### LTX-Video (Original)

| Requirement | Version |
|-------------|---------|
| Python | 3.10.5+ |
| CUDA | 12.2+ |
| PyTorch | >= 2.1.2 |
| macOS MPS | PyTorch 2.3.0 or >= 2.6 |

### LTX-2 / LTX-2.3

| Requirement | Version |
|-------------|---------|
| Python | >= 3.12 |
| CUDA | > 12.7 |
| PyTorch | ~= 2.7 |

### FP8 Quantized Models

| Requirement | Version |
|-------------|---------|
| CUDA Toolkit | 12.8+ |
| GPU Architecture | Ada Lovelace (RTX 40xx) or newer |

---

## VRAM Requirements

| Model / Mode | Minimum VRAM | Notes |
|--------------|-------------|-------|
| LTX-Video 2B (basic) | ~6-8 GB | With FP8 quantization |
| LTX-Video 2B (FP16) | ~10 GB | With group offloading |
| LTX-Video 13B (FP8) | ~12-16 GB | With quantization |
| LTX-Video 13B (BF16) | ~24-30 GB | Full precision |
| LTX-2 (FP8, optimized) | ~8 GB | 480p, limited frames |
| LTX-2 (BF16) | ~24+ GB | Full precision |
| LTX-2 13B training | 80+ GB | H100 recommended |

### Resolution vs VRAM (8GB GPU, FP8)

| Resolution | Max Frames |
|-----------|------------|
| 1280x704 | ~257 frames |
| 1920x1088 | ~121 frames |
| 3840x2176 | ~25 frames |

---

## Installation Methods

### Method 1: HuggingFace Diffusers (Recommended for Quick Start)

```bash
# Install PyTorch with CUDA support first
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

# Install diffusers from main branch (recommended for latest LTX features)
pip install -U git+https://github.com/huggingface/diffusers

# Or install stable release
pip install diffusers[torch] transformers accelerate
```

**Verify installation:**

```python
import torch
import diffusers
print(f"PyTorch: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
print(f"Diffusers: {diffusers.__version__}")
```

### Method 2: LTX-Video Repository (Full Feature Set)

```bash
# Clone the repository
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video

# Create and activate virtual environment
python -m venv env
source env/bin/activate  # Linux/macOS
# env\Scripts\activate   # Windows

# Install with inference dependencies
python -m pip install -e .[inference]
```

### Method 3: LTX-2 Monorepo (Latest Generation)

```bash
# Clone the repository
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2

# Install using uv (recommended)
uv sync --frozen
source .venv/bin/activate

# Or install with pip
pip install -e packages/ltx-pipelines
```

### Method 4: PyPI Package

```bash
pip install ltx-video
```

Note: The PyPI package (`ltx-video`) was released July 2025 and provides the core LTX-Video model package.

---

## Optional: FP8 Kernel Installation

FP8 kernels provide a significant performance boost on Ada architecture GPUs (RTX 40xx series and newer):

```bash
git clone https://github.com/Lightricks/LTX-Video-Q8-Kernels.git
cd LTX-Video-Q8-Kernels
pip install -e .
```

**Requirements:**
- CUDA Toolkit 12.8 or later
- Ada Lovelace architecture GPU or newer

---

## Optional: Attention Backend Installation

### xFormers

```bash
pip install xformers
```

### Flash Attention 3

```bash
# For compatible GPUs (Hopper architecture)
pip install flash-attn --no-build-isolation
```

---

## Model Downloads

### For Diffusers (Automatic Download)

Models are downloaded automatically when using `from_pretrained()`:

```python
from diffusers import LTXPipeline
pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
```

**Available model identifiers:**

| Model ID | Description |
|----------|-------------|
| `Lightricks/LTX-Video` | Base 2B model |
| `Lightricks/LTX-Video-0.9.5` | With multi-condition support |
| `Lightricks/LTX-Video-0.9.7-dev` | 13B dev model + upscaler |
| `Lightricks/LTX-Video-0.9.7-distilled` | 13B distilled (fast) |
| `Lightricks/LTX-Video-0.9.8-13B-distilled` | Latest distilled 13B |
| `Lightricks/ltxv-spatial-upscaler-0.9.7` | Latent upscaler |
| `Lightricks/LTX-2` | LTX-2 base |
| `Lightricks/LTX-2.3-22b-IC-LoRA-Motion-Track-Control` | IC-LoRA variant |

### For LTX-2 (Manual Download Required)

Download from the [LTX-2.3 HuggingFace repository](https://huggingface.co/Lightricks/LTX-2):

**Main Checkpoints:**
- `ltx-2.3-22b-dev.safetensors`
- `ltx-2.3-22b-distilled.safetensors`

**Supporting Models:**
- Spatial upscalers (x2 or x1.5)
- Temporal upscaler (x2)
- Distilled LoRA
- Gemma 3 text encoder

```bash
# Using huggingface-cli
huggingface-cli download Lightricks/LTX-2 --local-dir ./models/ltx-2
```

---

## Configuration Files (LTX-Video Repo)

Available pipeline configurations in the `configs/` directory:

| File | Model Size | Type | Notes |
|------|-----------|------|-------|
| `ltxv-13b-0.9.8-dev.yaml` | 13B | Dev | Highest quality |
| `ltxv-13b-0.9.8-distilled.yaml` | 13B | Distilled | Fast inference |
| `ltxv-2b-0.9.8-distilled.yaml` | 2B | Distilled | Fastest, lowest VRAM |
| `ltxv-13b-0.9.8-dev-fp8.yaml` | 13B | Dev + FP8 | Quantized full |
| `ltxv-13b-0.9.8-distilled-fp8.yaml` | 13B | Distilled + FP8 | Quantized fast |

---

## Environment Variables

```bash
# Recommended for memory management with FP8
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True

# For better CUDA memory allocation
export CUDA_LAUNCH_BLOCKING=0
```

---

## Verification Script

```python
import torch
print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
if torch.cuda.is_available():
    print(f"CUDA version: {torch.version.cuda}")
    print(f"GPU: {torch.cuda.get_device_name(0)}")
    print(f"GPU Memory: {torch.cuda.get_device_properties(0).total_mem / 1024**3:.1f} GB")

try:
    from diffusers import LTXPipeline
    print("Diffusers LTXPipeline: Available")
except ImportError:
    print("Diffusers LTXPipeline: Not installed")

try:
    from ltx_video.inference import infer
    print("ltx-video package: Available")
except ImportError:
    print("ltx-video package: Not installed")
```

---

## Troubleshooting

### Common Issues

1. **CUDA Out of Memory**: Use FP8 quantization, enable group offloading, or reduce resolution/frame count
2. **PyTorch version mismatch**: Ensure PyTorch is installed with matching CUDA version
3. **Slow first run**: `torch.compile` has a warm-up period; subsequent runs will be faster
4. **Resolution errors**: Width and height must be divisible by 32
5. **Frame count errors**: Number of frames must follow the `8n + 1` pattern (e.g., 9, 17, 25, ..., 161, 257)
6. **macOS MPS issues**: Use PyTorch 2.3.0 or >= 2.6 specifically
