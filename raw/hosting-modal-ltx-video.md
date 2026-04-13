# Modal: LTX Video Serverless GPU Deployment

## Sources
- https://modal.com/docs/examples/ltx
- https://modal.com/docs/examples/image_to_video

---

## Overview

Modal provides serverless GPU infrastructure for deploying LTX-Video inference. The platform handles GPU provisioning, scaling, and container management, allowing developers to run LTX-Video without managing infrastructure. Modal stores both model weights and generated outputs on Modal Volumes.

---

## Key Features

- Serverless GPU infrastructure (no GPU management)
- Model weights stored on Modal Volumes (no Hugging Face Hub download on every boot)
- Generated outputs saved both locally and on Modal Volumes
- Support for text-to-video and image-to-video
- FastAPI web endpoints for inference
- Warm container reuse for lower latency on subsequent requests

---

## Architecture

The Modal deployment uses the `@cls` decorator to:
- Specify infrastructure requirements (GPU type, memory)
- Control lifecycle of cloud containers
- The `enter` method loads the model into GPU memory from the Volume (or Hub if not cached)
- Container is marked ready only after model loading completes

---

## Text-to-Video Deployment

### Running Inference

```bash
modal run ltx_text_to_video.py
```

This spins up a new replica to generate a video. By default, generates a second video to demonstrate lower latency on warm containers.

### Data Management

- All outputs saved locally AND on a Modal Volume
- Volumes can be explored from the Modal Dashboard
- Command line access: `modal volume` commands

---

## Image-to-Video Deployment

### Web API Endpoint

The inference logic is wrapped in a Modal `Cls` that:
- Ensures models are loaded and moved to GPU once (on instance start)
- Includes a `fastapi_endpoint` for HTTP API access
- Supports standard REST API patterns

---

## Usage Pattern

1. Deploy the Modal app (defines GPU, dependencies, model loading)
2. Call the endpoint via REST API or `modal run`
3. Results are returned as video files
4. Warm containers serve subsequent requests with lower latency

---

## Related Links

- **Text-to-Video Example:** https://modal.com/docs/examples/ltx
- **Image-to-Video Example:** https://modal.com/docs/examples/image_to_video
- **Modal Documentation:** https://modal.com/docs
