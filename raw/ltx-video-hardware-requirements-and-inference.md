# LTX-Video Hardware Requirements and Inference Performance

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://arxiv.org/abs/2501.00103

---

## System Requirements

### Software
- **Python:** 3.10.5+
- **PyTorch:** >= 2.1.2
- **CUDA:** 12.2+ (recommended)
- **OS:** Linux recommended for production; macOS for development

### Installation
```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference-script]
```

## VRAM Requirements by Model

| Model | Parameters | Precision | Est. VRAM | Recommended GPU |
|-------|-----------|-----------|-----------|-----------------|
| 2B dev (0.9.6) | 2B | BF16 | ~12 GB | RTX 3090, RTX 4080 |
| 2B distilled (0.9.6) | 2B | BF16 | ~8 GB | RTX 3080, RTX 4070 |
| 2B distilled (0.9.8) | 2B | BF16 | ~8-12 GB | RTX 3080, RTX 4070 |
| 2B distilled-fp8 (0.9.8) | 2B | FP8 | ~6-8 GB | RTX 3070, RTX 4060 |
| 13B dev (0.9.7/0.9.8) | 13B | BF16 | ~24-40+ GB | A100 80GB, H100 |
| 13B distilled (0.9.7/0.9.8) | 13B | BF16 | ~16-24 GB | A100 40GB, RTX 4090 |
| 13B fp8 (0.9.7/0.9.8) | 13B | FP8 | ~12-16 GB | RTX 4090, A100 40GB |
| 13B distilled-fp8 (0.9.7/0.9.8) | 13B | FP8 | ~12-16 GB | RTX 4090 |

Note: VRAM requirements scale with output resolution and frame count. Values above are estimates for typical generation settings (e.g., 480x832, 96 frames).

## Inference Speed

### Paper Benchmark (2B model, H100)
- **Configuration:** 768x512, 121 frames (5 seconds), 24 FPS, 20 steps
- **Time:** 2 seconds
- **Verdict:** Faster than real-time (2s to generate 5s of video)

### Model-Specific Speed Characteristics

| Model | Inference Steps | Speed Category | CFG/STG Required |
|-------|---------------|----------------|------------------|
| 2B dev | 20-50 | Faster-than-real-time | Yes |
| 2B distilled | ~7 | 15x faster (real-time++++) | No |
| 13B dev | 20-30 | Slower (high quality) | Yes |
| 13B distilled | 7 | Fast (comparable to 2B dev) | No |
| 13B mix | varies | Balanced | Varies by stage |

### Factors Affecting Speed
1. **Resolution:** Higher = more tokens = slower (quadratic attention)
2. **Frame count:** More frames = more tokens = slower
3. **Inference steps:** More steps = proportionally slower
4. **Model size:** 13B ~6.5x more parameters than 2B
5. **Precision:** FP8 ~2x faster than BF16 for compute-bound operations
6. **Guidance (CFG):** When used, roughly doubles compute per step
7. **Upscaling:** Additional passes add time

### Speed Optimization Strategies

#### Use Distilled Models
- 7 steps instead of 20-30 (3-4x fewer)
- No CFG needed (another ~2x)
- Combined: ~7-15x speedup

#### Use FP8 Quantization
- Roughly halves memory bandwidth requirements
- Some compute speedup on supporting hardware (H100, RTX 40 series)

#### Use Mix Mode
- Generate at lower resolution first (fast)
- Upscale with spatial upscaler
- Only refine at high resolution (fewer steps needed)

#### Use VAE Tiling
- Process video in spatial tiles for memory efficiency
- Enables generation on lower-VRAM GPUs
- Slight quality impact at tile boundaries

## GPU Recommendations

### Consumer GPUs
| GPU | VRAM | Best Model |
|-----|------|------------|
| RTX 3070 | 8 GB | 2B distilled-fp8 |
| RTX 3080 | 10 GB | 2B distilled |
| RTX 3090 | 24 GB | 2B dev, 13B distilled (tight) |
| RTX 4070 | 12 GB | 2B distilled |
| RTX 4080 | 16 GB | 2B dev, 13B fp8 (tight) |
| RTX 4090 | 24 GB | 13B distilled, 13B fp8 |

### Data Center GPUs
| GPU | VRAM | Best Model |
|-----|------|------------|
| A100 40GB | 40 GB | 13B distilled (comfortable) |
| A100 80GB | 80 GB | 13B dev (comfortable) |
| H100 80GB | 80 GB | All models, fastest inference |
| L40S | 48 GB | 13B dev |

## Resolution vs. VRAM Tradeoffs

Higher resolution and frame counts consume more VRAM. Recommended starting points:

### 2B Models (~8-12 GB VRAM)
- 480x832, 96 frames (comfortable)
- 704x480, 161 frames (moderate)
- 704x1216, 96 frames (might need tiling)

### 13B Models (~16-40+ GB VRAM)
- 480x832, 96 frames (comfortable on most)
- 704x1216, 121 frames (needs 24+ GB)
- 704x1216, 257 frames (needs 40+ GB)
