# Community Models, Finetunes, and Quantizations

**Source:** https://huggingface.co/models?search=ltx+video&sort=downloads, https://huggingface.co/models?search=ltx+video+lora

---

## Overview

The LTX Video ecosystem has spawned a significant community of model creators producing quantizations, format conversions, finetunes, and specialized variants. Over 90 community models exist on HuggingFace.

## Top Community Models by Downloads

| Creator | Model Name | Downloads | Type |
|---------|-----------|-----------|------|
| optimum-intel-internal-testing | tiny-random-ltx-video | 52,200 | Testing model |
| DeepBeepMeep | LTX_Video | 20,700 | Standard |
| Isi99999 | LTX-Video | 8,870 | 21B model |
| a-r-r-o-w | LTX-Video-0.9.7-diffusers | 6,390 | Diffusers format |
| city96 | LTX-Video-gguf | 3,270 | GGUF quantization |
| city96 | LTX-Video-0.9.6-dev-gguf | 1,060 | GGUF quantization |
| oxide-lab | LTX-Video-0.9.8-2B-distilled | 968 | 5B distilled |
| pollockjj | ltx-video-2b-v0.9.1-gguf | 781 | GGUF quantization |
| Muinez | ltxvideo-2b-nsfw | 187 | Specialized variant |
| jbilcke-hf | LTX-Video-0.9.1-HFIE | 150 | Custom implementation |
| a-r-r-o-w | LTX-Video-0.9.1-diffusers | 96 | Diffusers format |

## Categories of Community Contributions

### GGUF Quantizations
Enabling low-VRAM usage through llama.cpp-compatible format:
- **city96/LTX-Video-gguf** - 3,270 downloads, 24 likes, Q3-Q8 variants
- **city96/LTX-Video-0.9.6-dev-gguf** - 1,060 downloads
- **pollockjj/ltx-video-2b-v0.9.1-gguf** - 781 downloads
- File sizes range from 985 MB (Q3) to 7.69 GB (F32)

### FP8/INT8 Quantizations
Optimized for NVIDIA GPUs with reduced precision:
- **Symphone/ltx-video-2b-v0.9-fp8** - FP8 variant
- **sayakpaul/q8-ltx-video** - 25 downloads, 7 likes
- **konakona/ltxvideo_q8** - 59 downloads, 7 likes
- **MayensGuds/ltx-video-quants** - 16 downloads, multiple quantization methods

### Diffusers Format Conversions
Making models compatible with the HuggingFace Diffusers library:
- **a-r-r-o-w** - Most prolific converter (multiple versions)
- **diffusers** - Official format conversions for 0.9.0 and 0.9.1
- **magespace** - Additional community conversion

### Precision Variants
- **ford442/LTX-Video-BF16-UNET** - BF16 precision UNET extraction
- **PJMixers-Dev/a-r-r-o-w_LTX-Video-0.9.1-diffusers-bf16** - BF16 variant

### Specialized Finetunes
- **spacepxl/ltx-video-0.9-vae-finetune** - 37 downloads, VAE-specific finetune
- **Muinez/ltxvideo-2b-nsfw** - 187 downloads, NSFW-specialized variant
- Various anime and pixel art LoRAs

### Testing/Development
- **finetrainers/dummy-ltxvideo** - Test model for training pipelines
- **optimum-intel-internal-testing/tiny-random-ltx-video** - For Intel optimization testing

## Aggregate Statistics

- **Total community models:** 90+ on HuggingFace
- **Total finetunes (LTX-Video):** 25
- **Total finetunes (LTX-2):** 54
- **Total finetunes (LTX-2.3):** 27
- **Total adapter models (LTX-Video):** 24
- **Total adapter models (LTX-2):** 49
- **Total adapter models (LTX-2.3):** 20
- **Total quantizations (LTX-Video):** 16
- **Total quantizations (LTX-2):** 8
- **Total quantizations (LTX-2.3):** 14
