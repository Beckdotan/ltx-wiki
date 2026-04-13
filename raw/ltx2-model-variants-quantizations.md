# LTX-2 Model Variants, Distilled Versions, and Quantizations

## Official Model Variants

### LTX-2 (19B) -- January 2026

#### Performance Tiers
- **LTX-2 Fast** -- Optimized for responsiveness and iteration; 2x faster inference
- **LTX-2 Pro** -- Balanced for efficiency and polish; best fidelity

#### Weight Checkpoints (Hugging Face)
| Checkpoint | Precision | Description |
|-----------|-----------|-------------|
| ltx-2-dev | BF16 | Full-precision dev model (~38GB) |
| ltx-2-fp8 | FP8 | Quantized for lower VRAM (~18-20GB) |
| ltx-2-distilled | BF16 | 8-step distilled, CFG=1, faster inference |
| ltx-2-fp8-distilled | FP8 | Distilled + quantized |

#### IC-LoRA Adapters
- **LTX-2-19b-IC-LoRA-Union-Control** -- Supports depth, pose, and edges automatically

### LTX-2.3 (22B) -- March 2026

#### Performance Tiers
- **LTX-2.3 Fast** -- Up to 20-second clips, 2x inference speed, 1/10 compute cost
- **LTX-2.3 Pro** -- Best quality, supports all endpoints (audio-to-video, retake, extend), up to 10-second clips

#### Weight Checkpoints (Hugging Face)
| Checkpoint | Precision | Description |
|-----------|-----------|-------------|
| ltx-2.3-22b-dev.safetensors | BF16 | Full-precision model (~42GB on disk) |
| ltx-2.3-22b-distilled.safetensors | BF16 | 8-step distilled for faster inference |
| ltx-2.3-fp8 | FP8 | Quantized for lower VRAM setups |
| Spatial upscaler (1.5x) | -- | Latent-space 1.5x spatial upscaling |
| Spatial upscaler (2x) | -- | Latent-space 2x spatial upscaling |
| Temporal upscaler | -- | Frame rate doubling in latent space |

#### IC-LoRA Adapters
- **LTX-2.3-22b-IC-LoRA-Motion-Track-Control** -- Motion tracking control
- **LTX-2.3-22b-IC-LoRA-Union-Control** -- Union control (depth, pose, edges)

## Spatial and Temporal Upscalers

### Spatial Upscaler
- Operates entirely in latent space (before VAE decode)
- Two versions: 1.5x and 2x resolution
- Example: Generate at 512x768, upscale to 1024x1536
- Used in the recommended two-stage pipeline for production quality

### Temporal Upscaler
- Doubles frame rate in latent space
- Generate at 24 FPS, get 48 FPS output
- New in LTX-2.3

## Community Quantizations

### GGUF Quantizations
Multiple community-created GGUF quantizations are available for running on consumer GPUs:

| Provider | Variants | Notes |
|---------|----------|-------|
| unsloth/LTX-2-GGUF | Multiple quant levels | Popular community option |
| vantagewithai/LTX-2-GGUF | Multiple quant levels | Alternative GGUF set |

#### GGUF Performance Examples
- **Q4_K_S** on RTX 3080 (10GB VRAM): 960x544, 5-second clip with audio in ~2-3 minutes
- **Q8_0** on RTX 4070: Better quality, moderate VRAM usage

### NVIDIA Quantizations

#### NVFP8 (RTX 30/40/50 Series)
- 2x faster inference
- 40% less VRAM
- ~30% model size reduction
- Preserves full architecture and capabilities

#### NVFP4 (RTX 50 Series Only)
- 3x faster inference
- 60% less VRAM
- Available through ComfyUI integration
- Announced at CES 2026

### FP8 Quantization Details
- Full-quality LTX-2 model quantized to FP8 precision
- Base optimized package: ~18-20GB (includes diffusion weights, VAE, text encoders)
- Preserves complete architecture while reducing memory

### Community FP8 Variants
- **GitMylo/LTX-2-comfy_gemma_fp8_e4m3fn** -- FP8 Gemma text encoder for ComfyUI
- **Kijai/LTXV2_comfy** -- Community ComfyUI-optimized weights

## VRAM Requirements by Variant

| Variant | VRAM Required | GPU Examples |
|---------|---------------|-------------|
| Full BF16 (dev) | 32GB+ | A100, H100 |
| FP8 Standard | 16-20GB | RTX 4070 Ti, RTX 3090 |
| FP8 Distilled | 14-16GB | RTX 4060 Ti 16GB |
| GGUF Q8_0 | ~14-16GB | RTX 4070 |
| GGUF Q4_K_S | 8-10GB | RTX 3080, RTX 3070 |
| NVFP4 (RTX 50) | ~13GB | RTX 5070+ |
| Minimum viable | 12GB | RTX 3060 (with offloading) |

## LoRA Compatibility Warning

LTX-2 LoRAs are NOT compatible with LTX-2.3 due to architecture changes (new VAE, expanded text connector, parameter count changes). LoRAs must be retrained for each major version.

## Distilled Model Details

### How Distillation Works
- Trained to replicate the behavior of the full model
- Uses 8 predefined diffusion steps (vs 30-36 for full model)
- CFG (classifier-free guidance) set to 1 (no guidance needed)
- Significantly faster inference at cost of some quality

### When to Use Distilled
- Rapid prototyping and iteration
- Interactive workflows where speed matters
- Lower-VRAM setups
- Hugging Face Spaces demos use distilled variants

## Sources

- [LTX-2 HuggingFace](https://huggingface.co/Lightricks/LTX-2)
- [LTX-2.3 HuggingFace](https://huggingface.co/Lightricks/LTX-2.3)
- [unsloth/LTX-2-GGUF](https://huggingface.co/unsloth/LTX-2-GGUF)
- [LTX-2 GGUF Guide -- Civitai](https://civitai.com/models/2291679/ltx-2-19b-all-you-need-is-here)
- [NVIDIA RTX Accelerates LTX-2](https://blogs.nvidia.com/blog/rtx-ai-garage-ces-2026-open-models-video-generation/)
- [NVFP8 vs NVFP4 for LTX-2 -- CrePal](https://crepal.ai/blog/aivideo/blog-nvfp8-vs-nvfp4-ltx-2-comfyui/)
- [LTX-2 GGUF Install Guide -- DEV Community](https://dev.to/gary_yan_86eb77d35e0070f5/how-to-install-and-configure-ltx-2-gguf-models-in-comfyui-complete-2026-guide-1d3m)
- [LTX-2 Video+Audio Gen Locally](https://www.stablediffusiontutorials.com/2026/01/ltx2-video-generation.html)
- [LTX-2.3 Download Guide -- WaveSpeedAI](https://wavespeed.ai/blog/posts/blog-ltx-2-download-huggingface/)
