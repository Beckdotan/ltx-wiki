# Hugging Face Inference Providers for LTX-Video

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://huggingface.co/docs/inference-providers
**Date:** 2026-04-13

---

## Overview

Hugging Face acts as a unified gateway to multiple inference providers for LTX-Video. Through the Hugging Face Inference API and Inference Endpoints, users can access LTX-Video models hosted on various backend providers.

---

## Hugging Face Inference API

### Serverless Inference
Hugging Face provides a free serverless inference API for supported models. For video generation models like LTX-Video, this may be available through partner providers.

**API Endpoint:**
```
POST https://api-inference.huggingface.co/models/Lightricks/LTX-Video
```

**Authentication:**
```bash
Authorization: Bearer hf_YOUR_TOKEN
```

### Python Client (huggingface_hub)
```bash
pip install huggingface_hub
```

```python
from huggingface_hub import InferenceClient

client = InferenceClient(token="hf_YOUR_TOKEN")

# Text-to-video (if supported by the provider)
video = client.text_to_video(
    prompt="A sunset over the ocean",
    model="Lightricks/LTX-Video",
)
```

---

## Inference Endpoints (Dedicated)

For production use, Hugging Face Inference Endpoints provide dedicated GPU instances:

### Create Endpoint
```python
from huggingface_hub import create_inference_endpoint

endpoint = create_inference_endpoint(
    name="ltx-video-endpoint",
    repository="Lightricks/LTX-Video-0.9.8-dev",
    framework="pytorch",
    task="text-to-video",
    accelerator="gpu",
    instance_type="nvidia-a100",
    instance_size="x1",
    region="us-east-1",
)

endpoint.wait()  # Wait for deployment
```

### Use Endpoint
```python
from huggingface_hub import InferenceClient

client = InferenceClient(model="https://YOUR_ENDPOINT_URL.endpoints.huggingface.cloud")
result = client.text_to_video("A bird flying over mountains")
```

---

## Third-Party Inference Providers via Hugging Face

Hugging Face partners with multiple inference providers. For LTX-Video, the known providers are:

### fal.ai (via Hugging Face)
- Accessible through the HF Inference API
- Endpoint: `fal-ai/ltx-video`
- Supports image-to-video and text-to-video
- See dedicated fal.ai documentation for full API details

### Replicate (via Hugging Face)
- Model: `lightricks/ltx-video`
- Accessible via Replicate API or HF gateway
- See dedicated Replicate documentation for full API details

---

## Hugging Face Spaces (Community Demos)

Over 100 community Spaces host LTX-Video demos:

### Notable Spaces
- Various Gradio-based demo interfaces
- Community fine-tune showcases
- Comparison tools between model versions

### Creating Your Own Space
```python
# Example Gradio app for LTX-Video
import gradio as gr
import torch
from diffusers import LTXConditionPipeline

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
).to("cuda")

def generate_video(prompt, image):
    # ... generation logic
    return "output.mp4"

demo = gr.Interface(
    fn=generate_video,
    inputs=[gr.Textbox(label="Prompt"), gr.Image(label="Input Image")],
    outputs=gr.Video(label="Generated Video"),
)

demo.launch()
```

---

## Model Downloads & Programmatic Access

### Download Models
```python
from huggingface_hub import hf_hub_download, snapshot_download

# Download specific file
hf_hub_download(
    repo_id="Lightricks/LTX-Video",
    filename="ltxv-13b-0.9.8-dev/config.json"
)

# Download entire model
snapshot_download(
    repo_id="Lightricks/LTX-Video-0.9.8-dev",
    local_dir="./ltx-video-13b-dev"
)
```

### List Available Files
```python
from huggingface_hub import list_repo_files

files = list_repo_files("Lightricks/LTX-Video")
for f in files:
    print(f)
```

---

## Related Links

- **HF Inference API Docs:** https://huggingface.co/docs/api-inference
- **HF Inference Endpoints:** https://huggingface.co/docs/inference-endpoints
- **HF Inference Providers:** https://huggingface.co/docs/inference-providers
- **Model Hub:** https://huggingface.co/Lightricks/LTX-Video
- **Diffusers Docs:** https://huggingface.co/docs/diffusers/main/en/api/pipelines/ltx_video
