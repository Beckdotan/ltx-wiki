# Use Case: Hardware Accessibility and Low-End GPU Support

**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions/112, https://huggingface.co/Lightricks/LTX-Video/discussions/26, https://huggingface.co/Lightricks/LTX-Video/discussions/6

---

## Running LTX Video on Consumer Hardware

### GTX 1650 (4GB VRAM) - hashtagg1

**Achievement:** Successfully ran LTX-Video on a laptop GPU with only 4GB VRAM.

**System Specs:**
- GPU: NVIDIA GTX 1650 (4GB VRAM)
- CPU: Intel i5-12450H
- RAM: 20GB
- OS: Windows 11 with SSD

**Performance:**
- Model: ltx-video-2b-v0.9.5.safetensors
- VRAM usage: 2.7-3.5 GB
- 640x480, 73 frames: 3-9 minutes
- 768x512: ~27 minutes
- Steps: 30, FPS: 24

**User Rating:** 9.5/10 - "Highly recommended"

### MacBook Pro M3/M4 (Apple Silicon)

**Key Findings:**
- Torch 2.5+ causes noise output on Apple Silicon
- **Solution:** Downgrade to Torch 2.4.1
- M3 MacBook Pro: ~5 minutes per video
- M1 MacBook Pro (16GB): ~15 minutes per video
- Limit video to 4-6 seconds for stable output

**Community Resource:**
- YouTube tutorial by monk05: https://www.youtube.com/watch?v=lvD9l4b2pzQ

### Minimum VRAM Requirements

| Hardware | Model | Resolution | Status |
|----------|-------|-----------|--------|
| 4GB VRAM (GTX 1650) | 2B | 640x480 | Working |
| 6GB VRAM | 2B (quantized) | 512x512 | Working with optimizations |
| 8GB VRAM (RTX 4060) | 2B | Various | Working |
| 24GB VRAM (RTX 4090) | 13B | 768x512 | Working |
| 40GB VRAM (A100) | 13B+ | Full resolution | Recommended |

### Optimization Techniques for Low VRAM

1. **Quantize CLIP/T5 encoder** to fp16 or fp8
2. **Use GGUF quantizations** (Q4_K_M for best quality/size ratio)
3. **Reduce resolution and frame count**
4. **Enable CPU offloading** via PYTORCH_HIP_ALLOC_CONF
5. **NVIDIA Control Panel:** Set CUDA Sysmem Fallback Policy to "Prefer Sysmem Fallback"
6. **Install optimization packages:** xformers, Flash Attention 2, SageAttention 2

### High-End Benchmarks

| Hardware | Config | Performance |
|----------|--------|-------------|
| RTX 4090 | 512x512, 121 frames | 11 seconds |
| H100 (fal.ai) | 512x768, 121 frames | 4 seconds |
| RTX 5090 | 1280x720, 121 frames | 80-95 seconds (LTX-2) |
| RTX 5090 | 1920x1088, 241 frames | 222 seconds (LTX-2.3) |
| RTX 5090 | 1280x720, 481 frames | 82 seconds (FP8, 8 steps) |

## Significance

LTX-Video's 2B parameter model is uniquely accessible among video generation models, running on hardware as low as a GTX 1650 laptop GPU. This accessibility, combined with GGUF quantizations and the distilled model variants, makes it one of the most democratized video generation models available.
