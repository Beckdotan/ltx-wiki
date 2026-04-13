---
title: GitHub Issues and Known Limitations
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/github-issues-discussions.md
tags:
  - github
  - issues
  - bugs
  - limitations
  - troubleshooting
  - known-issues
---

# GitHub Issues and Known Limitations

The [[github-official-repositories|Lightricks/LTX-Video]] GitHub issue tracker documents bugs, feature requests, and technical limitations reported by the community. As of the research date, there are 78 open issues out of approximately 276+ total filed since the initial release in late 2024.

## Issue Statistics

| Tracker | Open Issues | Activity Period |
|---------|-------------|-----------------|
| Lightricks/LTX-Video | 78 (of ~276+ total) | Late 2024 -- March 2026 |
| Lightricks/ComfyUI-LTXVideo | Active | Ongoing |
| Lightricks/LTX-Video-Q8-Kernels | Active | Ongoing |

## Recent Issues (2026)

| # | Title | Date | Category |
|---|-------|------|----------|
| 276 | NaN outputs when saving video on 8x Tesla P40 | Mar 10, 2026 | Bug |
| 275 | LoRA silently ignored with TI2VidTwoStagesPipeline | Mar 7, 2026 | Bug |
| 274 | LTX 2.3 First frame last frame | Mar 6, 2026 | Question |
| 268 | Looking for help / freelancer to finalize LTX-Video setup | Feb 2, 2026 | Support |
| 267 | LTX2 Efficient node / GGUF problem | Feb 2, 2026 | Bug |
| 264 | Increase to 48fps breaks video coherency and quality | Jan 14, 2026 | Bug/Limitation |
| 263 | Audio breaks export when using 4K | Jan 14, 2026 | Bug |
| 262 | LTX2 GGUF got artifacts noise result without errors | Jan 14, 2026 | Bug |
| 260 | TypeError when running image to video generation | Jan 4, 2026 | Bug |
| 259 | IC-LoRA inference in diffusers pipeline not working | Jan 3, 2026 | Bug |
| 257 | Full model weights and tooling release delayed | Nov 30, 2025 | Meta |

## Issue Categories

### Technical Bugs

- **NaN outputs** on multi-GPU setups (Tesla P40) -- #276
- **FP8 compatibility:** `FP8Linear.forward()` argument mismatch -- #231
- **[[gguf-quantizations|GGUF]] artifacts:** Noise results without error messages -- #262
- **Video output fails to play** from image-to-video inference -- #55
- **Generation stuck at 0%** -- #66
- **UnboundLocalError** in self_attn_func -- #233

### Quality Concerns

- **48fps breaks coherency** and quality -- #264
- **Plastic skin rendering** in 0.9.8 distilled -- #230
- **"How to generate good videos?"** -- #15 (early guidance request)

See also [[community-feedback]] for broader quality discussion.

### Feature Requests

- **Multi-GPU support** for inference -- #247
- **Chinese language support** for LTX2 -- #253
- **Sketch to video** capability -- #245

See also [[community-feature-requests]] for the full feature request landscape.

### Integration Issues

- **[[lora-ecosystem|LoRA]] silently ignored** in programmatic pipeline use (missing sd_ops renaming map) -- #275
- **[[ic-lora|IC-LoRA]] not working** with [[diffusers-integration|diffusers]] pipeline -- #259
- **ComfyUI missing nodes** even with ComfyUI-LTXVideo installed -- #240
- **Diffusers config mapping** issues -- #241
- **No CLIP/text encoder weights** in checkpoint -- #219

### Setup / Support

- Freelancer requests for setup assistance -- #268
- Various "how to use FP8" questions -- #227

## Known Limitations (Documented)

These are confirmed architectural or design limitations, not bugs:

1. **Resolution constraints:** Model works best on resolutions under 720x1280 and frame counts below 257
2. **Diffusers suboptimality:** The default [[diffusers-integration|Diffusers]] inference pipeline does not include STG or other inference-time tricks; the official LTX-Video repo inference script is recommended for best quality
3. **Frame count requirements:** Input videos must contain frames in multiples of 8 plus 1 (9, 17, 25, etc.) due to [[video-vae|VAE temporal compression]]
4. **Audio at 4K:** Audio breaks export when using 4K resolution -- #263
5. **High FPS quality degradation:** Increasing to 48fps can break video coherency -- #264
6. **VRAM requirements:** 16GB minimum for LTX-Video, 32GB+ recommended for ComfyUI workflows
7. **Quantized model artifacts:** [[gguf-quantizations|GGUF quantized]] versions can produce noise artifacts without error messages
8. **LoRA compatibility:** Some [[lora-ecosystem|LoRA]] configurations silently fail in certain pipeline modes

## ComfyUI-LTXVideo Issues

Separate issue tracker at https://github.com/Lightricks/ComfyUI-LTXVideo/issues. Common themes:

- Node compatibility issues
- Model download/loading problems
- Workflow configuration questions
- VRAM optimization requests

See [[comfyui-ltx-integration-overview]] for setup guidance.

## Troubleshooting Resources

- **LTX-Video-Trainer troubleshooting guide:** https://github.com/Lightricks/LTX-Video-Trainer/blob/main/docs/troubleshooting.md
- **Discord community:** https://discord.gg/ltxplatform (primary real-time support channel)
- **ComfyUI Manager:** For automated dependency resolution

## Pull Requests

Active PRs at https://github.com/Lightricks/LTX-Video/pulls. The project accepts community contributions but primary development is driven by the [[lightricks-company|Lightricks]] team.

## Related Pages

- [[github-official-repositories]] -- Official repositories
- [[community-feedback]] -- Broader community sentiment
- [[community-feature-requests]] -- Feature request landscape
- [[fp8-quantization]] -- FP8 quantization details
- [[installation-quickstart]] -- Setup guide
