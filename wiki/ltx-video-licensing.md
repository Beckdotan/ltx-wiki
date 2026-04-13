---
title: LTX Video Licensing
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/Lightricks/LTX-Video
  - https://huggingface.co/Lightricks/LTX-Video/blob/main/LTX-Video-Open-Weights-License-0.X.txt
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
  - https://arxiv.org/abs/2501.00103
tags:
  - licensing
  - open-source
  - ltx-video
  - legal
---

# LTX Video Licensing

The LTX Video models from [[lightricks-company]] use a multi-tier licensing structure depending on the component.

## Application Code: Apache 2.0

All open-source application code in the [[ltx-ecosystem]] uses the **Apache 2.0** license:

- [[ltx-desktop]] (full application)
- LTX-2 (model code, pipelines, trainer)
- LTX-Video (original model code)
- ComfyUI-LTXVideo (plugin nodes)
- LTX-Video-Trainer (fine-tuning toolkit)

## Model Weights: LTX-Video-Open-Weights License

Model weights are released under the **LTX-Video-Open-Weights License**, a custom open-weights license created by Lightricks. This is not a standard open-source license (not Apache 2.0, MIT, or GPL).

- **Full license text:** https://huggingface.co/Lightricks/LTX-Video/blob/main/LTX-Video-Open-Weights-License-0.X.txt

### Key Terms

- Allows both research and commercial use
- Requires attribution to Lightricks
- Free for companies under $10M annual revenue
- Companies above $10M must contact Lightricks for commercial terms
- Applies to all model variants (2B, 13B, 22B, upscalers, IC-LoRA adapters)

### Version Coverage

- v0.9.8+ models: LTX-Video-Open-Weights-License
- v0.9.6 models: Same license family
- Earlier versions (v0.9, v0.9.1, v0.9.5) may have different terms; check individual repositories

## Intended Uses

- Creative video generation
- Research and development
- Commercial applications (with attribution)
- Educational purposes

## Restrictions

- Not intended for factual/informational content (model may hallucinate)
- Not for creating deceptive or misleading content
- Must not violate applicable laws

## Comparison with Other Video Model Licenses

| Model | License | Commercial Use | Open Weights |
|-------|---------|---------------|--------------|
| LTX-Video | LTX-Video-Open-Weights | Yes (with attribution) | Yes |
| Stable Video Diffusion | Stability AI License | Limited | Yes |
| CogVideo | Apache 2.0 | Yes | Yes |
| Runway Gen-3 | Proprietary | Via API only | No |
| Sora | Proprietary | Via product only | No |

## Deployment Options

LTX Video models can be deployed via multiple channels:

1. **Local inference** - Clone repo, install dependencies, run inference.py
2. **HuggingFace Diffusers** - `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`
3. **ComfyUI** - Custom node installation (see [[ltx-integration-projects]])
4. **[[ltx-studio]]** - Official web UI
5. **fal.ai** - Cloud API hosting
6. **Replicate** - Simple API access, Docker-based deployment
7. **[[ltx-desktop]]** - Desktop application with local inference

## Responsible Use

- May amplify existing societal biases present in training data
- Clear guidelines and disclaimers included in documentation
- Designed to run on consumer-grade GPUs to democratize access
- Open-source approach fosters innovation and collaboration
