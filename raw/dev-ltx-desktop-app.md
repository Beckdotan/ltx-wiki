# LTX Desktop: Open-Source Desktop Application

## Sources
- https://github.com/Lightricks/LTX-Desktop
- https://github.com/Lightricks/ltx-desktop/releases
- https://ltx.io/ltx-desktop
- https://github.com/Lightricks/LTX-Desktop/blob/main/docs/INSTALLER.md

---

## Overview

LTX Desktop is an open-source desktop application for generating videos with LTX models. It runs locally on supported Windows/Linux NVIDIA GPUs, with an API mode for unsupported hardware and macOS.

- **Repository:** https://github.com/Lightricks/LTX-Desktop
- **License:** Apache-2.0
- **Status:** Beta (expect breaking changes)
- **Model:** LTX-2.3 (20.9B parameters)

---

## Architecture

Three-layer architecture:

### Frontend
- TypeScript + React UI
- Calls local backend over HTTP at `http://localhost:8000`
- Communicates with Electron via preload bridge (`window.electronAPI`)

### Electron
- TypeScript main process + preload
- Owns app lifecycle and OS integration
- File dialogs, native export via ffmpeg
- Starts/manages the Python backend

### Backend
- Python FastAPI server
- Handles ML model orchestration
- Runs inference using LTX-2.3

---

## Features

- Local video generation on NVIDIA GPUs
- API mode for unsupported hardware and macOS
- Video generation powered by LTX-2.3
- Open-source and modifiable

**Note:** Video generation API usage (when using API mode) is paid.

---

## Supported Platforms

| Platform | Local Inference | API Mode |
|----------|----------------|----------|
| Windows (NVIDIA GPU) | Yes | Yes |
| Linux (NVIDIA GPU) | Yes | Yes |
| macOS | No | Yes |

---

## Installation

Releases available at: https://github.com/Lightricks/ltx-desktop/releases

For installer documentation, see: https://github.com/Lightricks/LTX-Desktop/blob/main/docs/INSTALLER.md

---

## Related Links

- **Website:** https://ltx.io/ltx-desktop
- **GitHub Releases:** https://github.com/Lightricks/ltx-desktop/releases
- **Repository:** https://github.com/Lightricks/LTX-Desktop
