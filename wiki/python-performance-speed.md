---
title: Python Speed Optimization
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/optimization/fp16
  - https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
  - https://deepwiki.com/Lightricks/LTX-Video/4.2-performance-optimization
  - https://github.com/Lightricks/LTX-Video-Q8-Kernels
tags:
  - python
  - performance
  - speed
  - torch-compile
  - distilled
  - optimization
---

# Python Speed Optimization

Speed optimizations for LTX-Video inference in Python. For memory-focused optimizations, see [[python-performance-memory]].

## torch.compile

Compiling the transformer provides the largest speed improvement. The first call is slow (compilation), but subsequent calls are significantly faster.

```python
import torch
from diffusers import LTXPipeline

# Enable compiler optimizations
torch._inductor.config.conv_1x1_as_mm = True
torch._inductor.config.coordinate_descent_tuning = True
torch._inductor.config.epilogue_fusion = False
torch._inductor.config.coordinate_descent_check_all_directions = True

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)

pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)
```

**Notes:**
- Shows small gains for short 8-16 frame tests; compile time can outweigh savings
- Use `mode="max-autotune"` with `fullgraph=True` for best performance
- `torch.channels_last` memory format can improve GPU utilization
- Best combined with FP8 and caching

### Regional Compilation (Faster Compile Time)

Compile only the repeated transformer blocks instead of the full model (8-10x faster compilation):

```python
pipeline.transformer.compile_repeated_blocks(fullgraph=True)
```

### Dynamic Shape Compilation

Avoid recompilation when generating at different resolutions:

```python
torch.fx.experimental._config.use_duck_shape = False
pipeline.transformer = torch.compile(
    pipeline.transformer, fullgraph=True, dynamic=True
)
```

## Distilled Models

Distilled models require 4-10 inference steps instead of 50, providing 5-12x speedup:

```python
pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-13B-distilled", torch_dtype=torch.bfloat16
).to("cuda")

video = pipeline(
    prompt="A cinematic scene of a sunset over the ocean",
    timesteps=[1000, 993, 987, 981, 975, 909, 725, 0.03],
    guidance_scale=1.0,  # Must be 1.0 for distilled
).frames[0]
```

See [[python-schedulers]] for custom timestep schedules.

## Mixed Precision (BFloat16)

Always use `torch.bfloat16` for LTX-Video:

```python
pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")
```

## TF32 for Ampere GPUs

Enable TensorFloat-32 for faster matrix operations on NVIDIA Ampere+ GPUs (RTX 30xx+):

```python
torch.backends.cuda.matmul.allow_tf32 = True
```

## Attention Backends

### Native PyTorch SDPA (Recommended)

Automatically enabled with PyTorch 2.0+. No code changes needed.

```python
# Explicitly set if needed:
from diffusers.models.attention_processor import AttnProcessor2_0
pipeline.transformer.set_attn_processor(AttnProcessor2_0())
```

### xFormers

```python
pipeline.enable_xformers_memory_efficient_attention()
```

### Flash Attention 3 (Hopper GPUs)

For H100 GPUs:

```bash
pip install flash-attn --no-build-isolation
```

**Tip:** If your setup offers a toggle between SDPA and xFormers, try native SDPA first on recent PyTorch -- it is reportedly more stable on Windows.

## Fused QKV Projections

Combine Q, K, V projection matrices for larger, faster matrix multiplications:

```python
pipeline.fuse_qkv_projections()
```

## FP8 Kernels (Ada Architecture)

Custom FP8 kernels optimized for LTX-Video on Ada Lovelace GPUs (RTX 40xx+):

```bash
git clone https://github.com/Lightricks/LTX-Video-Q8-Kernels.git
cd LTX-Video-Q8-Kernels
pip install -e .
```

Requires CUDA Toolkit 12.8+.

## Precision Comparison

| Precision | VRAM Usage | Quality | Speed | GPU Support |
|-----------|-----------|---------|-------|-------------|
| BF16 | Highest | Best | Baseline | Ampere+ (RTX 30xx+) |
| FP16 | High | Very Good | Slightly faster | All modern GPUs |
| FP8 (e4m3fn) | ~30% less | Good | Faster | Ada Lovelace+ (RTX 40xx+) |
| GGUF (Q3-Q8) | Lowest | Variable | Variable | All modern GPUs |

## High VRAM (~24GB+) Maximum Speed Recipe

```python
import torch
from diffusers import LTXPipeline

torch._inductor.config.conv_1x1_as_mm = True
torch._inductor.config.coordinate_descent_tuning = True
torch._inductor.config.epilogue_fusion = False
torch.backends.cuda.matmul.allow_tf32 = True

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
).to("cuda")

pipeline.transformer.to(memory_format=torch.channels_last)
pipeline.transformer = torch.compile(
    pipeline.transformer, mode="max-autotune", fullgraph=True
)
pipeline.fuse_qkv_projections()
```

## Environment Variables

```bash
# Recommended for FP8 memory management
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True

# Better throughput
export CUDA_LAUNCH_BLOCKING=0
```

## Recommended Settings by Model

| Model | VRAM | Recommended Optimizations |
|-------|------|--------------------------|
| 2B base | 24 GB | BFloat16 + torch.compile |
| 2B distilled | 8-10 GB | FP8 + group offloading + 8 steps |
| 13B dev | 40 GB | BFloat16 + torch.compile |
| 13B distilled | 24 GB | FP8 + group offloading + 8 steps |

## See Also

- [[python-performance-memory]] -- Memory optimization techniques
- [[python-schedulers]] -- Custom timesteps for distilled models
- [[python-lora-loading]] -- Fusing LoRA before torch.compile
