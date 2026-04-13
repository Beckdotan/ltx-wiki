---
title: LoRA Community Ecosystem
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/social-lora-training-community-ecosystem.md
  - raw/community-project-lora-training-tools.md
  - raw/third-party-lora-training-services.md
tags:
  - community
  - lora
  - ecosystem
  - civitai
  - huggingface
---
# LoRA Community Ecosystem

An active community has formed around [[lora-training]] for [[ltx-video-overview|LTX-Video]] models, sharing trained LoRAs, tools, and knowledge across multiple platforms. See [[lora-ecosystem]] for the full inventory of official and community LoRA adapters.

## Community Sentiment

- Very positive about training ecosystem maturity
- Community values the official trainer being open source
- Multiple cloud training options reduce barrier to entry
- [[ic-lora|IC-LoRA]] seen as innovative and unique capability
- Concern about LoRA compatibility across model versions

## Community-Shared LoRAs

### Civitai

Multiple community-trained LoRAs are shared on Civitai:

- **LTX-2 IC-LoRA Detailer** -- enhances fine visual details and clarity
- **SSX LTX2 T2V** -- text-to-video LoRA trained with the official trainer
- Various style, NSFW, and image-to-video adapter LoRAs

### HuggingFace

| Model Family | Adapter Models | Community Fine-tunes |
|-------------|----------------|---------------------|
| LTX-Video | 24 | 25 |
| LTX-2 | 49 | 54 |
| LTX-2.3 | 20 | 27 |

Notable community contributions on HuggingFace:
- **Phr00t/LTX2-Rapid-Merges** -- community model merges
- **Kijai/LTXV2_comfy**, **Kijai/LTX2.3_comfy** -- widely used quantized variants

## Notable Community LoRA Authors

- **Burgstall** -- Multiple expression LoRAs (amgery, smile, surprise, headbanger)
- **svjack** -- Style LoRAs (anime landscape, pixel art)
- **Pierre-Jean** -- Camera trajectory LoRA
- **Sameric934** -- General LTX-2 video LoRA

## Cross-Version LoRA Transfer

A key community discovery from HuggingFace Discussion #4 on LTX-2.3:

- LTX-2.0 trained LoRAs work **significantly better** on LTX-2.3 than on 2.0 itself
- LoRAs that were unusable for further training on 2.0 become usable on 2.3
- Inference requires reduced strength values when applying 2.0 LoRAs to 2.3

## Training on Quantized Models

From HuggingFace Discussion #92 on LTX-Video:
- LoRA fine-tuning confirmed to work on FP8 quantized models
- Enables training on lower-VRAM hardware (under 24GB)

## Community Resource Hub

**awesome-ltx2** (github.com/wildminder/awesome-ltx2):
- Community-maintained collection of all available resources
- LTX-2 models, encoders, workflows, LoRAs for [[comfyui-integration|ComfyUI]]
- Central aggregation point for the community

## Uploading LoRAs

### To HuggingFace Hub

```python
from huggingface_hub import HfApi

api = HfApi()
api.upload_folder(
    folder_path="output/ltx-video-lora",
    repo_id="your-username/my-ltx-video-lora",
    repo_type="model",
)
```

## References

- [[lora-training]]
- [[ltx-video-trainer]]
- [[third-party-training-services]]
- [[ic-lora]]
