# Related Lightricks Research Papers and Publications

## Overview

This document catalogs research papers from Lightricks team members, including papers directly related to the LTX model family and broader computer vision/AI research from the team.

---

## Core LTX Model Papers

### 1. LTX-Video: Realtime Video Latent Diffusion (2024)
- **arXiv:** 2501.00103
- **Authors:** Yoav HaCohen, Nisan Chiprut, Benny Brazowski, et al. (16 authors)
- **Summary:** Foundational paper introducing the LTX-Video architecture with 1:192 VAE compression and faster-than-real-time generation.
- **See:** `paper-ltx-video-arxiv.md` for full details.

### 2. LTX-2: Efficient Joint Audio-Visual Foundation Model (2026)
- **arXiv:** 2601.03233
- **Authors:** Yoav HaCohen, Benny Brazowski, Nisan Chiprut, et al. (29 authors)
- **Summary:** First open-source joint audio-visual foundation model with 19B-parameter asymmetric dual-stream architecture.
- **See:** `paper-ltx2-arxiv.md` for full details.

### 3. JUST-DUB-IT: Video Dubbing via Joint Audio-Visual Diffusion (2026)
- **arXiv:** 2601.22143
- **Authors:** Anthony Chen, Naomi Ken Korem, Tavi Halperin, Matan Ben Yosef, Urska Jelercic, Ofir Bibi, Or Patashnik, Daniel Cohen-Or
- **Affiliations:** Tel Aviv University, Lightricks
- **Summary:** Adapts LTX-2 for video dubbing via lightweight LoRA, enabling translated audio and synchronized facial motion in a single model.
- **See:** `paper-just-dub-it.md` for full details.

---

## Earlier Lightricks Research (Pre-LTX)

### 4. Endless Loops: Detecting and Animating Periodic Patterns in Still Images (2021)
- **Venue:** ACM Transactions on Graphics (SIGGRAPH)
- **Authors:** Ofir Bibi et al. (Lightricks)
- **Summary:** Detects periodic visual patterns in still images and automatically generates seamless animated loops. An early example of Lightricks' research into motion generation from static imagery.

### 5. Temporally Stable Video Segmentation Without Video Annotations (2022)
- **Venue:** WACV 2022
- **Authors:** Ofir Bibi et al. (Lightricks)
- **Summary:** Achieves temporally consistent video segmentation without requiring video-level annotations, using only image-level training data. Demonstrates Lightricks' focus on temporal consistency — a theme that carries through to LTX-Video.

---

## Key Research Team Members

### Yoav HaCohen
- **Role:** Generative AI Research Team Lead at Lightricks
- **Education:** PhD from Hebrew University
- **Research Focus:** Multimodal Generative AI, Computational Photography, Computer Vision
- **Google Scholar:** https://scholar.google.com/citations?user=gKJ3MfIAAAAJ
- **Key Papers:** LTX-Video, LTX-2

### Ofir Bibi
- **Role:** VP of Research at Lightricks
- **Affiliation:** Lightricks and Hebrew University of Jerusalem
- **Research Interests:** Machine Learning, Deep Learning, Artificial Intelligence, Statistical Signal Processing
- **Google Scholar:** https://scholar.google.com/citations?user=UXYmnbgAAAAJ
- **Citations:** 443+ researchers have cited his work
- **Career:** Joined Lightricks in 2015 as Senior Researcher; built and scaled the research team; manages 40+ researchers
- **Key Papers:** LTX-Video, LTX-2, JUST-DUB-IT, Endless Loops, Temporally Stable Video Segmentation

---

## Company Research Context

### Lightricks Overview
- **Founded:** January 2013, Jerusalem, Israel
- **Employees:** ~600
- **Known Products:** Facetune (Apple's most downloaded paid app 2017), Videoleap (Apple App of the Year 2017), LTX Studio
- **Research Focus:** Computer vision, image/video processing, generative AI

### Research Timeline
| Year | Milestone |
|------|-----------|
| 2013 | Lightricks founded; Facetune launched |
| 2015 | Ofir Bibi joins as Senior Researcher |
| 2017 | Videoleap launched |
| 2021 | Endless Loops paper (SIGGRAPH) |
| 2022 | Temporally Stable Video Segmentation (WACV) |
| Nov 2024 | LTX-Video 2B open-source release |
| Dec 2024 | LTX-Video paper on arXiv |
| Mar 2025 | LTX-Video v0.9.5 |
| May 2025 | LTXV-13B with multiscale rendering |
| Jul 2025 | 60-second video generation barrier broken |
| Jan 2026 | LTX-2 (19B) open-sourced |
| Jan 2026 | JUST-DUB-IT paper |
| Mar 2026 | LTX-2.3 (22B) released |

### Open-Source Philosophy
Lightricks made LTXV open-source to encourage further innovation in the AI industry. All LTX model weights, code, and training infrastructure are publicly available under open-source licensing (LTX-2 under CC BY 4.0).

## Sources

- https://scholar.google.com/citations?user=UXYmnbgAAAAJ (Ofir Bibi)
- https://scholar.google.com/citations?user=gKJ3MfIAAAAJ (Yoav HaCohen)
- https://dblp.org/pid/99/9924.html (Yoav HaCohen DBLP)
- https://dblp.org/pid/129/8664.html (Ofir Bibi DBLP)
- https://en.wikipedia.org/wiki/Lightricks
- https://arxiv.org/abs/2501.00103
- https://arxiv.org/abs/2601.03233
- https://arxiv.org/abs/2601.22143
