---
title: Modal
type: entity
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/hosting-modal-ltx-video.md
  - raw/inference-providers-overview.md
  - raw/cloud-deployment-platforms.md
tags:
  - hosting
  - cloud
  - self-deploy
  - modal
  - serverless
---

# Modal

Modal provides serverless GPU infrastructure for self-deploying LTX Video inference. Unlike managed API providers such as [[fal-ai]] or [[replicate]], Modal gives developers full control over deployment while handling GPU provisioning, scaling, and container management automatically.

**Website:** https://modal.com
**LTX T2V Example:** https://modal.com/docs/examples/ltx
**LTX I2V Example:** https://modal.com/docs/examples/image_to_video
**Docs:** https://modal.com/docs

See also: [[inference-providers-overview]]

## Key Features

- Serverless GPU infrastructure (no GPU management required)
- Model weights stored on Modal Volumes (avoids re-downloading from HF Hub on every boot)
- Generated outputs saved both locally and on Modal Volumes
- Support for text-to-video and image-to-video
- FastAPI web endpoints for inference
- Warm container reuse for lower latency on subsequent requests
- Any LTX model variant can be deployed (user controls the model)

## Architecture

Modal deployment uses the `@cls` decorator to:
- Specify infrastructure requirements (GPU type, memory)
- Control lifecycle of cloud containers
- Load models into GPU memory from Volume (or Hub if not cached) via the `enter` method
- Mark containers ready only after model loading completes

## Usage

### Text-to-Video

```bash
modal run ltx_text_to_video.py
```

This spins up a new replica to generate a video. By default, it generates a second video to demonstrate the lower latency of warm containers.

### Image-to-Video (Web API)

The inference logic is wrapped in a Modal `Cls` that:
- Ensures models are loaded and moved to GPU once (on instance start)
- Includes a `fastapi_endpoint` for HTTP API access
- Supports standard REST API patterns

### Workflow

1. Define the Modal app (GPU, dependencies, model loading)
2. Deploy or run via CLI
3. Call the endpoint via REST API or `modal run`
4. Results returned as video files
5. Warm containers serve subsequent requests with lower latency

## Data Management

- All outputs saved locally AND on a Modal Volume
- Volumes can be explored from the Modal Dashboard
- Command line access via `modal volume` commands

## When to Choose Modal

Modal is best suited for:
- Developers who need full control over the inference stack
- Custom model configurations not available on managed providers
- Production deployments where you want serverless scaling without managing infrastructure
- Workloads that benefit from warm container reuse (repeated inference)

For simpler API-only access without deployment overhead, consider [[fal-ai]], [[replicate]], or [[segmind]].
