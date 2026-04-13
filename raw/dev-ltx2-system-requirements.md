# LTX-2.3 System Requirements

**Source:** https://docs.ltx.video/open-source-model/getting-started/system-requirements
**Fetched:** 2026-04-13

---

## GPU Requirements

| Tier | Specification |
|------|---------------|
| **Minimum** | NVIDIA GPU with 32GB+ VRAM |
| **Recommended** | NVIDIA A100 (80GB) or H100 |

---

## Memory & Storage

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| System RAM | 32GB | 64GB+ |
| Storage | 100GB free space | 200GB+ SSD |

---

## Software Requirements

| Software | Minimum Version |
|----------|----------------|
| CUDA | 11.8 or higher |
| CUDA (Recommended) | 12.1 or higher |
| Python | 3.10 or higher |

---

## LTX Desktop Requirements

For the LTX Desktop standalone application:
- NVIDIA GPU with 16GB+ VRAM (Windows/Linux)
- 16GB system RAM minimum (32GB recommended)
- 160GB free disk space
- macOS users: API key required for cloud-based operation (no local GPU generation)

---

## Comparison: LTX-Video (0.9.8) vs LTX-2.3

| Specification | LTX-Video 0.9.8 | LTX-2.3 |
|---------------|-----------------|---------|
| Model Size | 2B - 13B | ~20B |
| Min VRAM (Full) | 16GB (13B) | 32GB |
| Min VRAM (Quantized) | 6GB (2B FP8) | TBD |
| CUDA Minimum | 12.2 | 11.8 |
| Python | 3.10.5+ | 3.10+ |
| PyTorch | 2.1.2+ | ~2.7 |

---

## ComfyUI-Specific Requirements

- ComfyUI installed and working
- CUDA GPU with 32GB+ VRAM
- 100GB+ free disk space
- Python 3.10+

---

## Related Links

- **System Requirements:** https://docs.ltx.video/open-source-model/getting-started/system-requirements
- **Quick Start:** https://docs.ltx.video/open-source-model/getting-started/quick-start
- **Overview:** https://docs.ltx.video/open-source-model/getting-started/overview
