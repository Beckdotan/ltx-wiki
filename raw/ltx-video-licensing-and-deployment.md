# LTX-Video Licensing, Deployment Options, and Ecosystem

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://arxiv.org/abs/2501.00103

---

## Licensing

### LTX-Video Open-Weights License
- Custom license used from v0.9.6 onwards
- Available in individual model repositories on Hugging Face
- Not a standard open-source license (e.g., not MIT, Apache, or GPL)
- Specific terms should be reviewed in the license files

### Earlier Versions
- v0.9, v0.9.1, v0.9.5 may have different license terms
- Check individual repositories for details

## Deployment Options

### 1. Local Inference (GitHub)
- **Repository:** https://github.com/Lightricks/LTX-Video
- **Method:** Clone repo, install dependencies, run inference.py
- **Best for:** Custom pipelines, research, batch processing

### 2. HuggingFace Diffusers
- **Method:** Install diffusers, load model from Hub
- **API:** `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`
- **Best for:** Python integration, custom applications

### 3. ComfyUI
- **Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo/
- **Method:** Install as ComfyUI custom node
- **Workflow files:** Provided for various configurations
- **Best for:** Visual workflow design, creative use

### 4. LTX-Studio (Official Web UI)
- **URL:** https://app.ltx.studio/ltx-video
- **URLs:**
  - 13B-mix: https://app.ltx.studio/motion-workspace?videoModel=ltxv-13b
  - 13B distilled: https://app.ltx.studio/motion-workspace?videoModel=ltxv
- **Best for:** Quick testing, non-technical users

### 5. Fal.ai (Cloud API)
- **Text-to-Video:** https://fal.ai/models/fal-ai/ltx-video
- **13B full:** https://fal.ai/models/fal-ai/ltx-video-13b-dev/image-to-video
- **13B distilled:** https://fal.ai/models/fal-ai/ltx-video-13b-distilled/image-to-video
- **Best for:** API integration, scalable cloud inference

### 6. Replicate
- **URL:** https://replicate.com/lightricks/ltx-video
- **Best for:** Simple API access, Docker-based deployment

## Limitations and Responsible Use

### Technical Limitations
- Not designed for factual information generation
- Prompt following varies with prompt quality/clarity
- Limited to ~10 seconds of video
- English prompts only
- Resolution and frame count constraints

### Ethical Considerations
- May amplify existing societal biases present in training data
- Potential for misuse in generating misleading content
- Clear guidelines and disclaimers included in documentation

### Social Impact (from Paper)
- **Accessibility:** Designed to run on consumer-grade GPUs
- **Open-source:** Fosters innovation and collaboration
- **Environmental:** Small model size reduces energy consumption
- **Democratization:** Makes advanced video generation accessible to researchers and small enterprises
