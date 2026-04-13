# LTX Video Community Feedback and Discussions

**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions, https://huggingface.co/Lightricks/LTX-2/discussions, https://huggingface.co/Lightricks/LTX-2.3/discussions

---

## Common Praise

### Speed
- "Insanely fast" render times consistently highlighted
- LTX-2.3 distilled: 8 steps in under 1 minute on RTX 5090
- LTX-Video 2B: "faster-than-real-time generation" confirmed by users
- User DflippinK: "It's pretty insane" after properly configuring LTX-2.3
- User RuneXX: Praised the speed-to-quality ratio with 8-step distilled model

### Quality
- User DflippinK: "Pretty stellar results" when using proper workflows
- User DflippinK: "Waaaaaayyy better than I was getting in LTX-2" (comparing 2.3 to 2.0)
- User hashtagg1: Rated GTX 1650 experience "9.5/10"
- User TheBigBlockPC: "High quality and low demands" (Discussion #49)

### Accessibility
- Runs on consumer hardware (GTX 1650 with 4GB VRAM)
- Multiple quantization options make it accessible
- Free community Spaces on HuggingFace Zero GPU

### Audio-Video Integration
- LTX-2/2.3's joint audio-video generation widely praised
- Lip-sync capabilities seen as major differentiator
- Pinned discussion for sharing examples shows active community engagement

## Common Complaints

### Quality Issues
- Watermark artifacts in 0.9.8 distilled fp8 output (Discussion #117)
- Screen distortion and unwanted spots in distilled version (Discussion #94)
- Teeth anomalies in face generation
- Blotchy output at certain resolutions
- Temporal upscaler reported broken for LTX-2.3 (Discussion #35)
- Character LoRA severely reduces motion and ignores action prompts (Discussion #36)

### Configuration Complexity
- Sigma values differ between versions, causing confusion
- Users must use official workflows, not ComfyUI built-in templates
- Multiple reports of incorrect setup leading to poor output quality

### Performance on Specific Hardware
- RTX 4090 reported as "INCREDIBLY slow" with SamplerCustomAdvanced (Discussion #28)
- "Very very slowly RTX4090" (Discussion #24)
- Apple Silicon Torch 2.5+ produces noise output; requires Torch 2.4.1 downgrade

### Loading/Installation Issues
- Multiple reports of loading errors (Discussions #99, #82, #96, #111)
- T5 tokenizer fatal errors with 0.9.7 versions
- ComfyUI Manager installation errors
- Permission/download errors for LTX-2 (Discussion #48)

### Missing Features
- No official Diffusers support for LTX-2.3 (most requested feature)
- Missing `model_index.json` for Diffusers compatibility
- No MLX/Swift support for Apple Silicon
- Detailer for LTX-2 doesn't work well on LTX-2.3

## Feature Requests (Most Requested)

1. **Diffusers integration for LTX-2.3** - Multiple discussions, pending PR
2. **ControlNet support** - "Give us Controlnet, Pretty Please" (Discussion #81)
3. **Longer video generation** - Multiple requests (Discussion #25)
4. **More quantization variants** - INT8, NVFP4 for distilled models
5. **Better Mac/Apple Silicon support** - M3/M4 compatibility (Discussion #26)
6. **TeaCache optimization** - Performance optimization request (Discussion #87)
7. **End frame control** - Ability to specify last frame (Discussion #37)
8. **Audio effects** - How to add sound effects like beep censoring (Discussion #38)

## Interesting Technical Discussions

### Architecture Questions
- "This is not a UNet model" (Discussion #3) - Clarification that LTX-Video uses DiT architecture, not UNet
- "Is this just finetuned SD models?" (Discussion #13) - Clarification that LTX-Video is an original architecture

### Hardware & Optimization
- Hardware recommendations discussion (Discussion #6): RTX 4090 does 121 frames in 11 seconds at 512x512
- H100/fal.ai: 512x768 with 121 frames in 4 seconds
- FP8 vs Full-Dev quality difference described as "minimal and negligible"
- NV4 quantization expected to provide 200-300% speed improvement with ~10% quality loss

### Legal
- Copyright for generated content discussion (Discussion #9) - Community seeking clarity on rights

### LoRA Fine-tuning
- Active discussion about fine-tuning capabilities on quantized models
- Community interest in training custom LoRAs
- Official Lightricks trainer supports motion, style, and likeness LoRAs in under an hour
