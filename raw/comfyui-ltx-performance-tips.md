# Performance Tips: VRAM Optimization for ComfyUI + LTX Video

## Sources
- [Enhancement on VRAM handling | ComfyUI GitHub Issue #11726](https://github.com/Comfy-Org/ComfyUI/issues/11726)
- [GitHub - ComfyUI_LTX-2_VRAM_Memory_Management](https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management)
- [LTX-2.3 ComfyUI Setup: Two-Stage Pipeline, VRAM Fixes | WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-comfyui-setup-two-stage-pipeline/)
- [ComfyUI Video Generation Model Workflow Guide (LTX-2.3) | LTX Blog](https://ltx.io/model/model-blog/comfyui-workflow-guide)
- [LTX Workflow in ComfyUI: The Complete Guide | ComfyUI Box](https://comfyui-box.com/advanced-tutorial/ltx-workflow/)
- [Hardware recommendations | HuggingFace Discussion](https://huggingface.co/Lightricks/LTX-Video/discussions/6)
- [Minimal VRAM requirements | HuggingFace Discussion](https://huggingface.co/Lightricks/LTX-Video/discussions/18)
- [8GB VRAM workflow | HuggingFace Discussion](https://huggingface.co/Kijai/LTXV2_comfy/discussions/32)
- [How to Run LTX-2 on Low VRAM: GGUF Workflow | AI Study Now](https://aistudynow.com/how-to-run-ltx-2-on-low-vram-the-complete-gguf-comfyui-workflow/)
- [Memory Management | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/4.3-memory-management)
- [Dynamic VRAM Discussion | ComfyUI GitHub #12699](https://github.com/Comfy-Org/ComfyUI/discussions/12699)
- [Fixing OOM Errors in ComfyUI with LTX-2.3 | ANTLATT](https://www.antlatt.com/blog/comfyui-oom-ltx-nvidia/)
- [OOM with --normalvram | ComfyUI Issue #12047](https://github.com/Comfy-Org/ComfyUI/issues/12047)
- [LTX 2.3 basic GGUF 720p workflow | Civitai](https://civitai.com/models/2295882/ltx-23-basic-gguf-720p-workflow)
- [How to Install LTX-2 GGUF Models in ComfyUI | DEV Community](https://dev.to/gary_yan_86eb77d35e0070f5/how-to-install-and-configure-ltx-2-gguf-models-in-comfyui-complete-2026-guide-1d3m)
- [LTX 2.3 Video+Audio GGUF/FP8/NVF4/BF16 | Stable Diffusion Tutorials](https://www.stablediffusiontutorials.com/2026/03/ltx-2.3-video-audio-gen.html)

## Overview

LTX Video models range from lightweight (2B parameters, ~6GB VRAM) to heavy (22B parameters, 32GB+ VRAM). Optimizing performance requires matching your hardware capabilities with the right model format, resolution settings, and memory management techniques.

---

## Hardware Requirements by Model Version

| Model | Parameters | Minimum VRAM | Recommended VRAM | Generation Speed |
|-------|-----------|-------------|-----------------|-----------------|
| LTX Video 2B (v0.9.x) | 2B | 6 GB | 8-12 GB | ~4 sec for 4s video (RTX 4090) |
| LTX Video 13B (v0.9.7) | 13B | 12 GB | 16-24 GB | Slower than 2B |
| LTX-2 (19B) | 19B | 16 GB | 24-32 GB | Varies |
| LTX-2.3 (22B) | 22B | 24 GB | 32 GB+ | Varies |
| LTX-2.3 (22B) + Upscaler | 22B | 32 GB | 48 GB | Slowest |

---

## VRAM Optimization Strategies

### 1. Model Quantization

**FP8 Weights**
- Cuts VRAM usage nearly in half compared to FP16/BF16
- Minimal quality loss
- Lets you maintain batch size 1 without OOM on 12GB cards
- Use `q8_nodes.py` from ComfyUI-LTXVideo for integrated FP8 support

**GGUF Quantized Models**
- Kijai provides GGUF versions at [Kijai/LTXV2_comfy](https://huggingface.co/Kijai/LTXV2_comfy)
- Requires ComfyUI-GGUF nodes (city96/ComfyUI-GGUF)
- Can reduce VRAM to ~3GB with `--novram` flag
- Some quality trade-off but practical for consumer hardware

### 2. ComfyUI Launch Flags

| Flag | Effect |
|------|--------|
| `--novram` | Maximum memory savings; peak VRAM as low as 3-6.74 GB |
| `--lowvram` | Moderate memory savings with better speed than novram |
| `--normalvram` | Default behavior |
| `--reserve-vram N` | Reserve N GB for system use (e.g., `--reserve-vram 5`) |
| `--cache-none` | Disable caching to free memory |
| `--disable-smart-memory` | Disable automatic memory management |
| `--use-sage-attention` | Use SAGE attention for memory efficiency |
| `--preview-method taesd` | Lighter preview method |

**Recommended combination for low VRAM:**
```
python -m main --novram --cache-none --preview-method taesd
```

**For 24GB GPUs:**
```
python -m main --reserve-vram 5
```

### 3. Low VRAM Loader Nodes

The official ComfyUI-LTXVideo extension includes dedicated low VRAM loader nodes (`low_vram_loaders.py`):
- Ensure correct execution order
- Perform model offloading automatically
- Designed to fit generation within 32GB VRAM
- Use these instead of standard CheckpointLoader for constrained systems

### 4. Tiled Decoding

- **Tiled VAE Decode** node significantly reduces VRAM during the final decode step
- Processes the output in smaller tiles rather than all at once
- Maintains quality while reducing peak memory usage
- Available in the official ComfyUI-LTXVideo extension

### 5. FeedForward Chunking

The ComfyUI_LTX-2_VRAM_Memory_Management custom node:
- Chunks FeedForward layer operations
- Reduces peak memory by up to **8x**
- Enables 800-900+ frame videos on a single 24GB GPU
- No quality loss — purely a memory optimization
- Critical for high-resolution (1920x1088) generation

### 6. Remove Prompt Enhancer

The built-in Prompt Enhancer node requires loading an extra text encoder that consumes ~8GB VRAM:
- **Alternative:** Use ChatGPT, Gemini, or Claude to rewrite prompts manually before pasting
- Saves 8GB of VRAM for the actual generation process
- No quality loss if prompts are well-written

### 7. API-Based Text Encoding

**GemmaAPITextEncode** node:
- Uses Lightricks' free API endpoint for prompt encoding
- Eliminates ALL local VRAM usage for text encoding
- Sub-second prompt encoding
- Best for consumer GPUs with limited memory
- Requires API key from console.ltx.video

### 8. Conditioning Save/Load

**LTXVSaveConditioning** + **LTXVLoadConditioning** nodes:
- Encode a prompt once, save as .safetensors
- Reuse encoding across multiple inference runs
- Saves compute, reduces latency, keeps results consistent
- Eliminates need to load text encoder for subsequent generations

---

## Resolution and Frame Optimization

### Resolution Sweet Spots

| Resolution | VRAM Impact | Use Case |
|-----------|------------|---------|
| 480x720 | Lowest | Testing and iteration |
| 512x768 | Low | Quick previews, portrait |
| 768x512 | Low | Quick previews, landscape |
| 832x480 | Low | Low VRAM systems |
| 1024x576 | Medium | Good quality balance |
| 1280x720 | High | High quality output |
| 1920x1080 | Very High | Maximum quality (requires upscaling pipeline) |

**Note:** Going straight to 1024x576 may OOM on limited VRAM. Start at 768x512 or lower.

### Frame Count Optimization

- Frame count must be multiples of 8 + 1: 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 161, 257
- Fewer frames = less VRAM usage
- Start with 41-65 frames for testing
- Maximum recommended: 257 frames for single-pass generation
- For longer videos, use segment-based generation with sequence conditioning

### Sampling Steps

- **20-30 steps** is the sweet spot for LTX outputs
- Going above 40 steps rarely improves quality and wastes time/VRAM
- Use fewer steps (10-15) for vid2vid workflows
- Distilled model can use as few as 8 steps

---

## Speed Optimization

### Inference Acceleration

**Comfy-WaveSpeed (FBCache)**
- First Block Cache reuses transformer outputs when changes are minimal
- 1.5x to 3.0x speedup with acceptable quality trade-off
- Supports LTXV in both native and non-native modes

**TeaCache**
- Caches attention block outputs that are similar to inputs
- ~2.8x speedup demonstrated
- Available through community nodes

**torch.compile**
- WaveSpeed's enhanced compile node improves over stock TorchCompileModel
- Better LoRA compatibility
- Additional optimization options

### Two-Stage Pipeline Benefits

- **Stage 1:** Generate at half resolution (faster, less VRAM)
- **Upscale:** Latent-space upscaling (efficient)
- **Stage 2:** Refine at full resolution
- Results in better quality than single-stage at full resolution
- More VRAM-friendly than generating at full resolution directly

### Model Selection for Speed

| Model | Speed Profile |
|-------|--------------|
| LTX-2.3 Distilled (8-step) | Fastest — use for iteration |
| LTX-2.3 Dev (20+ step) | Highest quality — use for final renders |
| LTX Video 2B | Very fast at lower resolutions |
| GGUF Quantized | Slower per-step but fits in less VRAM |

---

## Workflow Splitting for OOM Recovery

If experiencing OOM errors mid-generation:

1. Split the workflow between two samplers
2. Save latents to file after first sampling steps using a SaveLatent node
3. Start a new session and load the saved latents
4. Continue sampling from where you left off
5. VRAM usage hovers around 50% with this approach

---

## Dynamic VRAM (ComfyUI Feature)

As of ComfyUI Discussion #12699, dynamic VRAM management is enabled by default:
- Automatically manages model loading/unloading
- Optimizes memory allocation between model, conditioning, and latents
- Reduces manual configuration needed
- May still need `--reserve-vram` flag for stability on some systems

---

## Hardware-Specific Recommendations

### 8GB VRAM (RTX 3060, 4060)

- Use GGUF quantized models
- Use `--novram` flag
- Resolution: 832x480 or lower
- Remove prompt enhancer, use API text encoding
- Consider LTX Video 2B (v0.9.x) instead of LTX-2.3

### 12GB VRAM (RTX 3060 12GB, 4070)

- Use FP8 weights
- Use `--lowvram` or `--reserve-vram 3`
- Resolution: 768x512
- Use GemmaAPITextEncode to avoid local encoder VRAM
- 41-65 frames recommended

### 16GB VRAM (RTX 4070 Ti, 4080)

- FP8 or GGUF models work well
- Standard resolution: 768x512 to 1024x576
- Can load Gemma encoder locally with careful management
- Use tiled VAE decode

### 24GB VRAM (RTX 3090, 4090)

- Full BF16 models work
- Can run two-stage pipeline at 1024x576
- Use `--reserve-vram 5` for stability
- FeedForward chunking enables very long videos (800+ frames)
- IC-LoRA controls work smoothly

### 32GB+ VRAM (A100, dual GPU)

- Full pipeline including upscalers
- 1080p output with spatial + temporal upscaling
- No special optimization needed for standard workflows
- Low VRAM loaders ensure correct execution order
