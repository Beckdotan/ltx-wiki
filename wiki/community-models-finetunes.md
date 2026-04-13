---
title: Community Models and Finetunes
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-community-models-finetunes.md
tags:
  - community
  - models
  - finetunes
  - quantization
  - huggingface
  - ecosystem
---

# Community Models and Finetunes

The LTX Video ecosystem has spawned a significant community of model creators producing quantizations, format conversions, finetunes, and specialized variants. Over 90 community models exist on HuggingFace.

## Top Community Models by Downloads

| Creator | Model Name | Downloads | Type |
|---------|-----------|-----------|------|
| optimum-intel-internal-testing | tiny-random-ltx-video | 52,200 | Testing model |
| DeepBeepMeep | LTX_Video | 20,700 | Standard |
| Isi99999 | LTX-Video | 8,870 | 21B model |
| a-r-r-o-w | LTX-Video-0.9.7-diffusers | 6,390 | Diffusers format |
| city96 | LTX-Video-gguf | 3,270 | GGUF quantization |

## Categories of Community Contributions

### GGUF Quantizations

Enabling low-VRAM usage through llama.cpp-compatible format. See [[gguf-quantizations]] for full details.

- **city96/LTX-Video-gguf** -- 3,270 downloads, Q3-Q8 variants (985 MB to 7.69 GB)
- **city96/LTX-Video-0.9.6-dev-gguf** -- 1,060 downloads
- **pollockjj/ltx-video-2b-v0.9.1-gguf** -- 781 downloads

### FP8/INT8 Quantizations

Optimized for NVIDIA GPUs with reduced precision:

- **Symphone/ltx-video-2b-v0.9-fp8** -- FP8 variant
- **sayakpaul/q8-ltx-video** -- 25 downloads
- **konakona/ltxvideo_q8** -- 59 downloads
- **MayensGuds/ltx-video-quants** -- 16 downloads, multiple quantization methods

### Diffusers Format Conversions

Making models compatible with the HuggingFace Diffusers library:

- **a-r-r-o-w** -- Most prolific converter (multiple versions)
- **diffusers** -- Official format conversions for v0.9.0 and v0.9.1
- **magespace** -- Additional community conversion

### Precision Variants

- **ford442/LTX-Video-BF16-UNET** -- BF16 precision UNET extraction
- **PJMixers-Dev/a-r-r-o-w_LTX-Video-0.9.1-diffusers-bf16** -- BF16 variant

### Specialized Finetunes

- **spacepxl/ltx-video-0.9-vae-finetune** -- 37 downloads, VAE-specific finetune
- **Muinez/ltxvideo-2b-nsfw** -- 187 downloads, specialized variant
- Various anime and pixel art [[lora-ecosystem|LoRAs]]

### Testing and Development

- **finetrainers/dummy-ltxvideo** -- Test model for training pipelines
- **optimum-intel-internal-testing/tiny-random-ltx-video** -- For Intel optimization testing

## Aggregate Statistics

| Metric | LTX-Video | LTX-2 | LTX-2.3 |
|--------|-----------|-------|---------|
| Total finetunes | 25 | 54 | 27 |
| Total adapter models | 24 | 49 | 20 |
| Total quantizations | 16 | 8 | 14 |

**Total community models on HuggingFace:** 90+

## See Also

- [[gguf-quantizations]] -- Detailed breakdown of city96's GGUF quantizations
- [[lora-ecosystem]] -- LoRA adapters and training tools
- [[render-benchmarks]] -- Performance benchmarks for quantized variants
- [[ltx-video-overview]] -- LTX Video base model
- [[ltx-2-overview]] -- LTX-2 base model
