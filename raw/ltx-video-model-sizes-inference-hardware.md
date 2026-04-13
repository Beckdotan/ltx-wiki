# LTX-Video Model Sizes, Inference Speed, and Hardware Requirements

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions/6
**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions/18
**Source:** https://docs.ltx.video/open-source-model/getting-started/system-requirements
**Source:** https://apatero.com/blog/ltx-video-13b-real-time-low-vram-generation-guide-2025

## Model File Sizes

### 13B Parameter Models
| File | Size | Precision |
|------|------|-----------|
| ltxv-13b-0.9.8-dev.safetensors | ~28.6 GB | bf16 |
| ltxv-13b-0.9.8-distilled.safetensors | ~28.6 GB | bf16 |
| ltxv-13b-0.9.8-dev-fp8.safetensors | ~15.7 GB | fp8 |
| ltxv-13b-0.9.8-distilled-fp8.safetensors | ~15.7 GB | fp8 |
| ltxv-13b-0.9.7-dev.safetensors | ~28.6 GB | bf16 |
| ltxv-13b-0.9.7-distilled.safetensors | ~28.6 GB | bf16 |
| ltxv-13b-0.9.7-dev-fp8.safetensors | ~15.7 GB | fp8 |
| ltxv-13b-0.9.7-distilled-fp8.safetensors | ~15.7 GB | fp8 |
| ltxv-13b-0.9.7-distilled-lora128.safetensors | ~1.33 GB | LoRA weights |

### 2B Parameter Models
| File | Size | Precision |
|------|------|-----------|
| ltxv-2b-0.9.8-distilled.safetensors | ~6.34 GB | bf16 |
| ltxv-2b-0.9.8-distilled-fp8.safetensors | ~4.46 GB | fp8 |
| ltxv-2b-0.9.6-dev-04-25.safetensors | ~6.34 GB | bf16 |
| ltxv-2b-0.9.6-distilled-04-25.safetensors | ~6.34 GB | bf16 |

## Inference Speed Benchmarks

### H100 GPU
- **Original 2B (v0.9.0):** 5 seconds of 24fps video at 768x512 in 2 seconds
- **13B distilled:** Real-time generation at 1216x704 / 30fps
- **Distilled models:** 15x faster than dev models

### RTX 4090
- **121 frames generated in ~11 seconds** (community benchmark)
- Best consumer GPU option for LTX-Video

### Fal.ai (H100 cloud)
- 512x768, 121 frames in ~4 seconds

### RTX 3070 Ti (Laptop)
- ~300 seconds for standard presets (significantly slower)

### General Rule
- Dev models: ~30 inference steps needed
- Distilled models: 8 steps (or fewer), no CFG/STG

## VRAM Requirements

### Minimum VRAM by Model Variant

| Model | Min VRAM | Comfortable VRAM | Notes |
|-------|----------|-------------------|-------|
| 2B bf16 | ~8 GB | 12 GB | With VAE tiling |
| 2B fp8 | ~6 GB | 8 GB | Quantized |
| 13B bf16 | ~24 GB | 32+ GB | Full precision |
| 13B fp8 | ~16 GB | 24 GB | Quantized |
| 13B LoRA | ~1 GB extra | -- | On top of base model |

### Resolution vs VRAM Tradeoffs

| VRAM | Achievable Resolution | Frame Count | Notes |
|------|----------------------|-------------|-------|
| 6 GB | 512x512 | ~50 frames | With tricks (quantized CLIP, etc.) |
| 8 GB | 512x640 | ~80 frames | FP8 quantization recommended |
| 12 GB | 640x960 | ~120 frames | Practical baseline |
| 16 GB | 1080p | ~161 frames | Comfortable generation |
| 24 GB | 1216x704 | 257 frames | Full native resolution |
| 32+ GB | 720x1280 | 257 frames | Maximum quality |

### Performance by VRAM (Approximate)

| VRAM | Resolution | Speed | FPS |
|------|-----------|-------|-----|
| 8 GB | 512x512 | Slow | -- |
| 12 GB | 640px range | Moderate | -- |
| 16 GB | 1080p | 6-10 seconds | 16-24 fps |
| 24 GB | 1080p | 12-20 seconds | 24 fps |

## Software Requirements

| Requirement | Minimum | Tested |
|-------------|---------|--------|
| Python | 3.10.5+ | 3.10.5 |
| CUDA | 12.2+ | 12.2 |
| PyTorch | >= 2.1.2 | 2.1.2+ |
| macOS (MPS) | PyTorch 2.3.0 | PyTorch == 2.3 or >= 2.6 |

## System Requirements (Official)

- **GPU VRAM:** 32GB+ recommended (official), 8GB+ with quantization (community)
- **System RAM:** 32 GB recommended
- **Storage:** 100 GB free space (for model files)
- **OS:** Windows 11, macOS 13+ (Ventura), Linux (Ubuntu 20.04+)

## Memory Optimization Techniques

### VAE Tiling
```python
pipe.vae.enable_tiling()
```
Allows processing larger resolutions by tiling VAE operations.

### FP8 Quantization
- Reduces model file size by ~45% (28.6 GB -> 15.7 GB for 13B)
- Reduces VRAM usage proportionally
- Minimal quality loss in most cases

### LoRA Variants
- LoRA weights only ~1.33 GB (vs 28.6 GB for full model)
- Require base model loaded, but dramatically reduce additional VRAM for distilled inference

### GGUF Quantization (Community)
- Available from city96 and Unsloth on Hugging Face
- Various quantization levels (Q4, Q5, Q8, etc.)
- Designed for ComfyUI integration
- Further VRAM reduction beyond FP8

## GPU Recommendations by Use Case

| Use Case | Recommended GPU | VRAM |
|----------|----------------|------|
| Quick prototyping (2B) | RTX 3060/4060 | 8-12 GB |
| Quality generation (2B) | RTX 3070/4070 | 12 GB |
| 13B development | RTX 3090/4090 | 24 GB |
| 13B production | A100/H100 | 40-80 GB |
| Real-time 13B | H100 | 80 GB |
| Low-budget 13B | RTX 4060 Ti 16GB | 16 GB (with FP8) |
