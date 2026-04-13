# RunPod and RunComfy: LTX Video Cloud GPU Hosting

## Sources
- https://www.runpod.io/blog/ltxvideo-comfyui-runpod-setup
- https://civitai.com/articles/24620/quick-and-dirty-guide-to-testing-ltx-2-on-runpod
- https://ltx-23.app/blog/ltx-video-23-runpod
- https://civitai.com/articles/14435/runpod-template-ltx-13b-t2vi2v-comfyui-workflows-included
- https://www.runcomfy.com/comfyui-workflows/ltx-2-comfyui-workflow-real-time-video-generation-speed
- https://www.runcomfy.com/comfyui-workflows/ltx-2-3-image-to-video-in-comfyui-realistic-motion-workflow
- https://civitai.com/articles/27761/yet-another-workflow-for-ltx-23-step-by-step-with-runpod-template-v039

---

## RunPod

### Overview

RunPod is a cloud GPU rental service for running LTX Video inference without local hardware. It offers on-demand GPU instances and a serverless API.

### Setup Options

1. **Manual Setup**: Spin up a GPU instance via JupyterLab and terminal, install ComfyUI and LTX-Video manually
2. **Community Templates**: Pre-configured templates available on Civitai and RunPod marketplace
3. **Serverless Endpoints**: Deploy ComfyUI workflows as serverless APIs

### Available Templates

- **LTX 13B T2V/I2V - ComfyUI**: Pre-configured template with workflows included
- **LTX-2 on RunPod**: Quick setup guide available on Civitai
- **LTX-2.3 Templates**: Step-by-step guides with RunPod template (v0.39+)

### Requirements

- GPU with sufficient VRAM (24GB+ recommended for full models)
- Storage for model weights (100GB+)

### Serverless API

Once deployed, ComfyUI workflows run as a serverless API on RunPod, allowing video generation without manually managing GPU instances.

---

## RunComfy

### Overview

RunComfy provides hosted ComfyUI instances with a more user-friendly interface than raw GPU rental. It offers both a traditional app interface (Playground) and ComfyUI workspace.

### LTX Video Workflows Available

- **LTX-2 ComfyUI Workflow**: Real-time video generation speed
- **LTX-2.3 Image-to-Video**: Realistic motion workflow
- **LTX-2 Video Retake**: Modify video with AI for precise retakes

### Pricing

- Pay-as-you-go plan for getting started
- Pro plan with advanced features (persistent storage, CPU machines)

### Features

- Pre-loaded LTX Video workflows
- Playground interface for easy model access
- No local hardware required
- Persistent storage options

---

## Comparison

| Feature | RunPod | RunComfy |
|---------|--------|----------|
| **Type** | Raw GPU rental | Managed ComfyUI hosting |
| **Setup** | Manual or template-based | Pre-configured |
| **Interface** | JupyterLab / Terminal / API | ComfyUI Playground |
| **Flexibility** | High (any framework) | ComfyUI-focused |
| **Serverless** | Yes (endpoints) | Managed |
| **Best For** | Power users, custom deployments | Quick experiments, non-technical users |

---

## Related Links

- **RunPod:** https://www.runpod.io
- **RunComfy:** https://www.runcomfy.com
- **RunPod LTX Setup Guide:** https://www.runpod.io/blog/ltxvideo-comfyui-runpod-setup
