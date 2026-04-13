---
title: LTX-2 HuggingFace Ecosystem
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx2-huggingface-ecosystem.md
tags:
  - ltx-2
  - huggingface
  - diffusers
  - spaces
  - ecosystem
---

# LTX-2 HuggingFace Ecosystem

HuggingFace repositories, spaces, and integrations for the [[ltx-2-overview|LTX-2]] model family.

## Official Model Repositories

### LTX-2 (19B)
- **URL:** https://huggingface.co/Lightricks/LTX-2
- **Downloads:** 84,353 in December 2025; 796,000 monthly at peak
- Passed **5 million total downloads** before [[ltx-2.3-model|LTX-2.3]] shipped

### LTX-2.3 (22B)
- **URL:** https://huggingface.co/Lightricks/LTX-2.3
- **Downloads:** 1.66M monthly (as of April 2026)
- Contains: dev model (bf16), distilled model, fp8 variants, spatial/temporal upscalers

### Collection
- **URL:** https://huggingface.co/collections/Lightricks/ltx-2
- Contains: Base models, LoRAs, IC-LoRAs, upscalers

## Official IC-LoRA Adapters

| Adapter | URL |
|---------|-----|
| LTX-2-19b-IC-LoRA-Union-Control | https://huggingface.co/Lightricks/LTX-2-19b-IC-LoRA-Union-Control |
| LTX-2.3-22b-IC-LoRA-Union-Control | https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Union-Control |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | https://huggingface.co/Lightricks/LTX-2.3-22b-IC-LoRA-Motion-Track-Control |

See [[ltx-2-lora-training]] for training and usage details.

## Official Spaces (Demos)

| Space | URL | Description |
|-------|-----|-------------|
| LTX-2 Full Demo | https://huggingface.co/spaces/Lightricks/ltx-2 | Text prompt + optional image input |
| LTX-2 Distilled (Fast) | https://huggingface.co/spaces/Lightricks/ltx-2-distilled | Fast generation using distilled model |
| LTX-2.3 Distilled | https://huggingface.co/spaces/Lightricks/LTX-2-3 | Latest model demo |

## Diffusers Integration

### Documentation
- **Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx2
- **Latest:** https://huggingface.co/docs/diffusers/main/api/pipelines/ltx2

### Pipeline Classes

- `LTX2Pipeline` -- Main text-to-video pipeline
- `LTX2LatentUpsamplePipeline` -- Latent space upsampling (stage 2)
- `LTX2LatentUpsamplerModel` -- Upsampler model
- `AutoencoderKLLTX2Audio` -- Audio VAE encoder/decoder

### Two-Stage Pipeline Usage

```python
import torch
from diffusers.pipelines.ltx2 import LTX2Pipeline, LTX2LatentUpsamplePipeline

# Stage 1: Generate at target resolution
pipe = LTX2Pipeline.from_pretrained("Lightricks/LTX-2", torch_dtype=torch.bfloat16)
pipe.enable_sequential_cpu_offload()

video = pipe(
    prompt="A beautiful sunset over the ocean",
    width=768, height=512,
    num_inference_steps=30,
).videos[0]

# Stage 2: Upsample 2x
upsampler = LTX2LatentUpsamplePipeline.from_pretrained(...)
video_hq = upsampler(video).videos[0]
```

## Community Contributions on HuggingFace

### Quantizations
- **unsloth/LTX-2-GGUF** -- Multiple quantization levels
- **vantagewithai/LTX-2-GGUF** -- Alternative GGUF set

See [[ltx-2-model-variants]] for full quantization details.

### ComfyUI Weights
- **GitMylo/LTX-2-comfy_gemma_fp8_e4m3fn** -- FP8 Gemma text encoder
- **Kijai/LTXV2_comfy** -- Community ComfyUI-optimized weights (includes audio VAE)

### Other Community Resources
- **CalamitousFelicitousness/LTX-2.3-distilled-Diffusers** -- Diffusers-format conversion
- **RuneXX/LTX-2-Workflows** -- ComfyUI workflow collection

## Paper Page
- **URL:** https://huggingface.co/papers/2601.03233
- Paper: "LTX-2: Efficient Joint Audio-Visual Foundation Model"

## Notable Tracked Issues
- Image-to-video stability issues (discussions/34)
- Distilled checkpoint support in Diffusers (Issue #12925)
- Condition pipeline support (Issue #12926)

## Related Pages

- [[ltx-2-overview]] -- Model overview
- [[ltx-2-huggingface]] -- LTX-2 model card
- [[ltx-2-3-huggingface]] -- LTX-2.3 model card
- [[ltx-2-model-variants]] -- All weight variants
- [[ltx-2-lora-training]] -- LoRA adapters
- [[ltx-2-community-reception]] -- Community engagement
