# LTX-2 on Hugging Face: Models, Spaces, and Ecosystem

## Official Model Repositories

### LTX-2 (19B)
- **URL:** https://huggingface.co/Lightricks/LTX-2
- **Downloads:** 84,353 in December 2025; 796,000 monthly by March 2026
- **File tree:** https://huggingface.co/Lightricks/LTX-2/tree/main
- **Discussions:** https://huggingface.co/Lightricks/LTX-2/discussions

### LTX-2.3 (22B)
- **URL:** https://huggingface.co/Lightricks/LTX-2.3
- **File tree:** https://huggingface.co/Lightricks/LTX-2.3/tree/main
- Contains: dev model (bf16), distilled model, fp8 variants, spatial/temporal upscalers

### Hugging Face Collection
- **URL:** https://huggingface.co/collections/Lightricks/ltx-2
- Contains: Base models, LoRAs, IC-LoRAs, upscalers

## Official IC-LoRA Adapters on Hugging Face

| Adapter | URL |
|---------|-----|
| LTX-2-19b-IC-LoRA-Union-Control | https://huggingface.co/Lightricks/LTX-2-19b-IC-LoRA-Union-Control |
| LTX-2.3-22b-IC-LoRA-Union-Control | https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Union-Control |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Motion-Track-Control |

## Official Hugging Face Spaces (Demos)

### LTX-2 Full Demo
- **URL:** https://huggingface.co/spaces/Lightricks/ltx-2
- Provides text prompt input, optional image input
- Adjustable length and generation settings
- Generates short video with matching audio

### LTX-2 Video Fast (Distilled)
- **URL:** https://huggingface.co/spaces/Lightricks/ltx-2-distilled
- Fast generation using distilled model
- Text description + optional image input
- Generates short video clips quickly

### LTX 2.3 Distilled
- **URL:** https://huggingface.co/spaces/Lightricks/LTX-2-3
- Latest model demo
- Short video clips with sound from text descriptions

## Diffusers Integration

### Documentation
- **Main docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx2
- **Latest:** https://huggingface.co/docs/diffusers/main/api/pipelines/ltx2

### Pipeline Classes
- `LTX2Pipeline` -- Main text-to-video pipeline
- `LTX2LatentUpsamplePipeline` -- Latent space upsampling (stage 2)
- `LTX2LatentUpsamplerModel` -- Upsampler model
- `AutoencoderKLLTX2Audio` -- Audio VAE encoder/decoder

### Basic Usage (Two-Stage Pipeline)
```python
import torch
from diffusers import FlowMatchEulerDiscreteScheduler
from diffusers.pipelines.ltx2 import LTX2Pipeline, LTX2LatentUpsamplePipeline
from diffusers.pipelines.ltx2.latent_upsampler import LTX2LatentUpsamplerModel

# Stage 1: Generate at target resolution
pipe = LTX2Pipeline.from_pretrained("Lightricks/LTX-2", torch_dtype=torch.bfloat16)
pipe.enable_sequential_cpu_offload()

video = pipe(
    prompt="A beautiful sunset over the ocean",
    width=768,
    height=512,
    num_inference_steps=30,
).videos[0]

# Stage 2: Upsample 2x with distilled LoRA
upsampler = LTX2LatentUpsamplePipeline.from_pretrained(...)
video_hq = upsampler(video).videos[0]
```

### Condition Pipeline
- `pipeline_ltx_condition.py` -- Supports passing conditions for flexible generation
- Available at: `diffusers/src/diffusers/pipelines/ltx/pipeline_ltx_condition.py`

## Community Contributions on Hugging Face

### GGUF Quantizations
- **unsloth/LTX-2-GGUF** -- Multiple quantization levels
- **vantagewithai/LTX-2-GGUF** -- Alternative GGUF set

### FP8 Text Encoder
- **GitMylo/LTX-2-comfy_gemma_fp8_e4m3fn** -- FP8 Gemma for ComfyUI

### ComfyUI Weights
- **Kijai/LTXV2_comfy** -- Community ComfyUI-optimized weights (includes audio VAE)

### Diffusers Conversions
- **CalamitousFelicitousness/LTX-2.3-distilled-Diffusers** -- Community Diffusers-format conversion

### Workflow Packs
- **RuneXX/LTX-2-Workflows** -- ComfyUI workflow collection

## Paper Page
- **URL:** https://huggingface.co/papers/2601.03233
- Paper: "LTX-2: Efficient Joint Audio-Visual Foundation Model"

## Download Statistics (as of early 2026)
- LTX-2 passed 5 million total downloads before LTX-2.3 shipped (March 2026)
- 796,000 monthly downloads on HuggingFace (at peak)
- Reddit launch thread: 700+ upvotes

## GitHub Issues Tracking on HF
Notable tracked issues:
- Image-to-video stability issues (discussions/34)
- Distilled checkpoint support in Diffusers (Issue #12925)
- Condition pipeline support (Issue #12926)

## Sources

- [LTX-2 HuggingFace](https://huggingface.co/Lightricks/LTX-2)
- [LTX-2.3 HuggingFace](https://huggingface.co/Lightricks/LTX-2.3)
- [Diffusers LTX-2 Docs](https://huggingface.co/docs/diffusers/api/pipelines/ltx2)
- [LTX-2 Collection](https://huggingface.co/collections/Lightricks/ltx-2)
- [LTX-2 Paper Page](https://huggingface.co/papers/2601.03233)
- [LTX-2 Download Guide -- WaveSpeedAI](https://wavespeed.ai/blog/posts/blog-ltx-2-download-huggingface/)
