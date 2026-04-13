# LTX Video GitHub Issues and Discussions

> **Sources:**
> - <https://github.com/Lightricks/LTX-Video/issues>
> - <https://github.com/Lightricks/ComfyUI-LTXVideo/issues>
> - <https://github.com/Lightricks/LTX-Video-Q8-Kernels/issues>
> - <https://github.com/Lightricks/LTX-Video-Trainer/blob/main/docs/troubleshooting.md>

---

## Issue Statistics (Lightricks/LTX-Video)

- **Open Issues:** 78
- **Total Issues (estimated):** 276+ (based on highest issue number)
- Activity spans from initial release (late 2024) through March 2026

---

## Recent Issues (2026)

| # | Title | Date | Category |
|---|-------|------|----------|
| 276 | NaN outputs when saving video on 8x Tesla P40 | Mar 10, 2026 | Bug |
| 275 | LoRA silently ignored with TI2VidTwoStagesPipeline (missing sd_ops renaming map) | Mar 7, 2026 | Bug |
| 274 | LTX 2.3 First frame last frame | Mar 6, 2026 | Question |
| 268 | Looking for help / freelancer to finalize LTX-Video setup | Feb 2, 2026 | Support |
| 267 | LTX2 Efficient node / GGUF problem | Feb 2, 2026 | Bug |
| 264 | Increase to 48fps breaks video coherency and quality | Jan 14, 2026 | Bug/Limitation |
| 263 | Audio breaks export when using 4K | Jan 14, 2026 | Bug |
| 262 | LTX2 GGUF got artifacts noise result without errors | Jan 14, 2026 | Bug |
| 260 | TypeError when running image to video generation | Jan 4, 2026 | Bug |
| 259 | IC-LoRA inference in diffusers pipeline not working | Jan 3, 2026 | Bug |
| 257 | Full model weights and tooling release delayed (promised late Nov 2025) | Nov 30, 2025 | Meta |

---

## Notable Issue Categories

### Technical Bugs
- **NaN outputs** on multi-GPU setups (Tesla P40) -- issue #276
- **FP8 compatibility:** `FP8Linear.forward()` argument mismatch -- issue #231
- **GGUF artifacts:** Noise results without error messages -- issue #262
- **Video output fails to play** from image-to-video inference -- issue #55
- **Generation stuck at 0%** -- issue #66
- **UnboundLocalError** in self_attn_func -- issue #233

### Quality Concerns
- **48fps breaks coherency** and quality -- issue #264
- **Plastic skin rendering** in 0.9.8 distilled -- issue #230
- **How to generate good videos?** -- issue #15 (early guidance request)

### Feature Requests
- **Multi-GPU support** for inference -- issue #247
- **Chinese language support** for LTX2 -- issue #253
- **Sketch to video** capability -- issue #245

### Integration Issues
- **LoRA silently ignored** in programmatic pipeline use -- issue #275
- **IC-LoRA not working** with diffusers pipeline -- issue #259
- **ComfyUI missing nodes** even with ComfyUI-LTXVideo installed -- issue #240
- **Diffusers config mapping** issues -- issue #241
- **No CLIP/text encoder weights** in checkpoint -- issue #219

### Setup / Support
- Freelancer requests for setup assistance -- issue #268
- Various "how to use FP8" questions -- issue #227

---

## Known Limitations (Documented)

1. **Resolution constraints:** Model works best on resolutions under 720x1280 and frame counts below 257
2. **Diffusers suboptimality:** The default diffusers inference pipeline does not include STG or other inference-time tricks; the official LTX-Video repo inference script is recommended
3. **Frame count requirements:** Input videos must contain frames in multiples of 8 plus 1 (9, 17, 25, etc.)
4. **Audio at 4K:** Audio breaks export when using 4K resolution -- issue #263
5. **High FPS quality degradation:** Increasing to 48fps can break video coherency -- issue #264
6. **VRAM requirements:** 16GB minimum for LTX-Video, 32GB+ recommended for ComfyUI workflows
7. **Quantized model artifacts:** GGUF quantized versions can produce noise artifacts
8. **LoRA compatibility:** Some LoRA configurations silently fail in certain pipeline modes

---

## Community Troubleshooting Resources

The LTX-Video-Trainer repository maintains a dedicated troubleshooting guide at:
<https://github.com/Lightricks/LTX-Video-Trainer/blob/main/docs/troubleshooting.md>

The Discord community (<https://discord.gg/ltxplatform>) is the primary real-time support channel.

---

## ComfyUI-LTXVideo Issues

Separate issue tracker at <https://github.com/Lightricks/ComfyUI-LTXVideo/issues>. Common themes include:
- Node compatibility issues
- Model download/loading problems
- Workflow configuration questions
- VRAM optimization requests

---

## Pull Requests

Active PRs at <https://github.com/Lightricks/LTX-Video/pulls>. The project accepts community contributions but primary development is driven by Lightricks team.
