---
title: ComfyUI LTX Video Performance and VRAM Optimization
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Comfy-Org/ComfyUI/issues/11726
  - https://github.com/RandomInternetPreson/ComfyUI_LTX-2_VRAM_Memory_Management
  - https://wavespeed.ai/blog/posts/ltx-2-3-comfyui-setup-two-stage-pipeline/
  - https://ltx.io/model/model-blog/comfyui-workflow-guide
  - https://comfyui-box.com/advanced-tutorial/ltx-workflow/
  - https://huggingface.co/Lightricks/LTX-Video/discussions/6
  - https://huggingface.co/Lightricks/LTX-Video/discussions/18
  - https://huggingface.co/Kijai/LTXV2_comfy/discussions/32
  - https://aistudynow.com/how-to-run-ltx-2-on-low-vram-the-complete-gguf-comfyui-workflow/
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/4.3-memory-management
  - https://github.com/Comfy-Org/ComfyUI/discussions/12699
  - https://www.antlatt.com/blog/comfyui-oom-ltx-nvidia/
  - https://github.com/chengzeyi/Comfy-WaveSpeed
  - https://civitai.com/models/2295882/ltx-23-basic-gguf-720p-workflow
  - https://www.stablediffusiontutorials.com/2026/03/ltx-2.3-video-audio-gen.html
tags:
  - comfyui
  - ltx-video
  - performance
  - vram
  - optimization
---

# ComfyUI LTX Video Performance and VRAM Optimization

[[ltx-video-overview|LTX Video]] models range from 2B parameters (~6GB VRAM) to 22B parameters (32GB+ VRAM). Optimizing performance requires matching hardware with the right model format, resolution, and memory management.

## Hardware Requirements by Model

| Model | Parameters | Min VRAM | Recommended | Speed (RTX 4090) |
|-------|-----------|----------|-------------|-------------------|
| LTX Video 2B (v0.9.x) | 2B | 6 GB | 8-12 GB | ~4s for 4s video |
| LTX Video 13B (v0.9.7) | 13B | 12 GB | 16-24 GB | Slower |
| LTX-2 (19B) | 19B | 16 GB | 24-32 GB | Varies |
| LTX-2.3 (22B) | 22B | 24 GB | 32 GB+ | Varies |
| LTX-2.3 + Upscaler | 22B | 32 GB | 48 GB | Slowest |

## VRAM Optimization Strategies

### 1. Model Quantization

**FP8 Weights:**
- Cuts VRAM nearly in half vs FP16/BF16
- Minimal quality loss
- Use `q8_nodes.py` from [[comfyui-ltxvideo-official-nodes|ComfyUI-LTXVideo]] for integrated FP8

**GGUF Quantized Models:**
- Kijai provides GGUF at [Kijai/LTXV2_comfy](https://huggingface.co/Kijai/LTXV2_comfy)
- Requires ComfyUI-GGUF nodes (city96/ComfyUI-GGUF)
- Can reduce VRAM to ~3GB with `--novram`
- Some quality trade-off but practical for consumer hardware

### 2. ComfyUI Launch Flags

| Flag | Effect |
|------|--------|
| `--novram` | Maximum memory savings; peak VRAM 3-6.74 GB |
| `--lowvram` | Moderate savings, better speed than novram |
| `--normalvram` | Default behavior |
| `--reserve-vram N` | Reserve N GB for system (e.g., `--reserve-vram 5`) |
| `--cache-none` | Disable caching to free memory |
| `--disable-smart-memory` | Disable automatic memory management |
| `--use-sage-attention` | SAGE attention for memory efficiency |
| `--preview-method taesd` | Lighter preview method |

**Low VRAM combo:**
```
python -m main --novram --cache-none --preview-method taesd
```

**24GB GPUs:**
```
python -m main --reserve-vram 5
```

### 3. Low VRAM Loader Nodes

The [[comfyui-ltxvideo-official-nodes|official extension]] includes dedicated low VRAM loaders (`low_vram_loaders.py`):
- Ensure correct execution order and model offloading
- Designed to fit within 32GB VRAM
- Use instead of standard CheckpointLoader on constrained systems

### 4. Tiled Decoding

[[comfyui-ltx-node-reference|Tiled VAE Decode]] processes output in smaller tiles. Maintains quality while reducing peak memory during final decode.

### 5. FeedForward Chunking

The [[comfyui-ltx-community-nodes|ComfyUI_LTX-2_VRAM_Memory_Management]] node pack:
- Chunks FeedForward layer operations
- Reduces peak memory by up to **8x**
- Enables 800-900+ frame videos on 24GB GPU
- No quality loss
- Critical for 1920x1088 generation

### 6. Remove Prompt Enhancer

The built-in Prompt Enhancer requires ~8GB VRAM for its extra text encoder. Alternative: use ChatGPT/Gemini/Claude to rewrite prompts externally before pasting. Saves 8GB with no quality loss if prompts are well-written.

### 7. API-Based Text Encoding

[[comfyui-ltx-node-reference|GemmaAPITextEncode]] uses Lightricks' free API endpoint:
- Eliminates all local VRAM for text encoding
- Sub-second encoding
- Requires API key from console.ltx.video

### 8. Conditioning Save/Load

LTXVSaveConditioning + LTXVLoadConditioning:
- Encode once, save as .safetensors, reuse across runs
- Eliminates need to load text encoder for subsequent generations
- Guaranteed reproducibility

## Resolution and Frame Optimization

### Resolution Sweet Spots

| Resolution | VRAM Impact | Use Case |
|-----------|------------|---------|
| 480x720 | Lowest | Testing and iteration |
| 512x768 / 768x512 | Low | Quick previews |
| 832x480 | Low | Low VRAM systems |
| 1024x576 | Medium | Good quality balance |
| 1280x720 | High | High quality |
| 1920x1080 | Very High | Maximum (requires [[comfyui-ltx-two-stage-pipeline|upscaling pipeline]]) |

### Frame Count

- Must be multiples of 8 + 1: 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 161, 257
- Start with 41-65 frames for testing
- Maximum recommended: 257 for single-pass
- For longer videos: segment-based generation with sequence conditioning

### Sampling Steps

- 20-30 steps is the sweet spot
- Above 40 steps rarely improves quality
- 10-15 steps for vid2vid workflows
- Distilled model: as few as 8 steps

## Speed Optimization

### Inference Acceleration

**[[comfyui-ltx-community-nodes|Comfy-WaveSpeed]] (FBCache):**
- First Block Cache reuses transformer outputs when changes are minimal
- 1.5x to 3.0x speedup
- Acceptable quality trade-off

**TeaCache:**
- Caches attention block outputs similar to inputs
- ~2.8x speedup demonstrated

**torch.compile:**
- WaveSpeed's enhanced compile improves over stock TorchCompileModel
- Better LoRA compatibility

### Model Selection for Speed

| Model | Speed Profile |
|-------|--------------|
| LTX-2.3 Distilled (8-step) | Fastest -- use for iteration |
| LTX-2.3 Dev (20+ step) | Highest quality -- final renders |
| LTX Video 2B | Very fast at lower resolutions |
| GGUF Quantized | Slower per-step but fits less VRAM |

### Two-Stage Pipeline Benefits

The [[comfyui-ltx-two-stage-pipeline|two-stage pipeline]] generates at half resolution then upscales, producing better quality than single-stage at full resolution while being more VRAM-friendly.

## OOM Recovery: Workflow Splitting

If experiencing out-of-memory errors mid-generation:
1. Split workflow between two samplers
2. Save latents to file after first sampling via SaveLatent node
3. Start new session, load saved latents
4. Continue sampling from checkpoint
5. VRAM usage hovers around 50% with this approach

## Dynamic VRAM Management

ComfyUI includes automatic dynamic VRAM management (Discussion #12699):
- Automatically manages model loading/unloading
- Optimizes allocation between model, conditioning, and latents
- Reduces manual configuration
- May still benefit from `--reserve-vram` on some systems

## Hardware-Specific Recommendations

### 8GB VRAM (RTX 3060, 4060)
- GGUF quantized models + `--novram`
- Resolution: 832x480 or lower
- Remove prompt enhancer, use API text encoding
- Consider LTX Video 2B instead of LTX-2.3

### 12GB VRAM (RTX 3060 12GB, 4070)
- FP8 weights + `--lowvram` or `--reserve-vram 3`
- Resolution: 768x512
- GemmaAPITextEncode for text encoding
- 41-65 frames

### 16GB VRAM (RTX 4070 Ti, 4080)
- FP8 or GGUF models
- Resolution: 768x512 to 1024x576
- Can load Gemma locally with careful management
- Tiled VAE decode recommended

### 24GB VRAM (RTX 3090, 4090)
- Full BF16 models
- Two-stage pipeline at 1024x576
- `--reserve-vram 5` for stability
- FeedForward chunking for 800+ frame videos
- IC-LoRA controls work smoothly

### 32GB+ VRAM (A100, dual GPU)
- Full pipeline including all upscalers
- 1080p output with spatial + temporal upscaling
- No special optimization needed for standard workflows

## Related Pages

- [[comfyui-ltx-integration-overview]] -- Integration overview
- [[comfyui-ltx-two-stage-pipeline]] -- Upscaling pipeline
- [[comfyui-ltx-community-nodes]] -- VRAM management and acceleration nodes
- [[comfyui-ltx-node-reference]] -- Node reference (GemmaAPITextEncode, Tiled VAE Decode, etc.)
- [[comfyui-ltx-model-comparison]] -- VRAM budget guide across models
