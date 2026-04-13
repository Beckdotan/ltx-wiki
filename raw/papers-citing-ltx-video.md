# Papers Citing or Building on LTX-Video

## Source
- Identified from references in LTX-2 and AVControl papers
- Cross-referenced with HuggingFace model pages, arXiv, Semantic Scholar
- Date: April 2026

---

## Papers by Lightricks Building on LTX-Video

### 1. LTX-2: Efficient Joint Audio-Visual Foundation Model
- **arXiv:** 2601.03233 (January 2026)
- **Authors:** HaCohen, Brazowski, Chiprut, et al. (Lightricks)
- **Relationship:** Direct successor. Extends LTX-Video's spatiotemporal latent space and architecture principles to joint audio-visual generation. Builds a 19B dual-stream model on top of LTX-Video's Video-VAE design.

### 2. AVControl: Efficient Framework for Training Audio-Visual Controls
- **arXiv:** 2603.24793 (March 2026)
- **Authors:** Halperin, Korem, Salama, et al. (Lightricks)
- **Relationship:** Uses LTX-2 (built on LTX-Video) as frozen backbone for training control LoRAs.

### 3. CAFA: A Controllable Automatic Foley Artist
- **arXiv:** 2504.06778 (April 2025)
- **Authors:** Benita, Finkelson, Halperin, Sterkin (Lightricks), Adi (HUJI)
- **Relationship:** Lightricks research in V2A that preceded the joint approach in LTX-2.

### 4. JUST-DUB-IT: Video Dubbing via Joint Audio-Visual Diffusion
- **arXiv:** 2601.22143 (January 2026)
- **Authors:** Chen, Korem, Halperin, Ben Yosef, Jelercic, Bibi (Lightricks); Patashnik, Cohen-Or (TAU)
- **Relationship:** Uses AVControl framework on LTX-2 for video dubbing. Adapts LTX-2 via lightweight LoRA for video-to-video dubbing with translated audio and synchronized facial motion.

### 5. ID-LoRA: Identity-Driven Audio-Video Personalization with In-Context LoRA
- **arXiv:** 2603.10256 (March 2026)
- **Authors:** Dahan, Yanuka, Kraicer, Wolf, Giryes (TAU/Lightricks collaboration)
- **Relationship:** Extends AVControl framework for identity-driven personalization.

### 6. In-Context Sync-LoRA for Portrait Video Editing
- **arXiv:** 2512.03013 (December 2025)
- **Authors:** Polaczek, Patashnik, Mahdavi-Amiri, Cohen-Or
- **Relationship:** Applies the IC-LoRA paradigm from LTX-Video ecosystem for portrait editing.

---

## External Papers Citing LTX-Video

### 7. Vid2World: Crafting Video Diffusion Models to Interactive World Models
- **arXiv:** 2505.14357 (May 2025)
- **Authors:** Huang, Wu, et al.
- **URL:** https://arxiv.org/abs/2505.14357
- **Relationship:** References LTX-Video as a key video diffusion model demonstrating how large-scale latent diffusion transformers translate textual descriptions into temporally coherent video. Builds on this foundation for interactive world models via video diffusion causalization.

### 8. Taming Diffusion Transformer for Efficient Mobile Video Generation in Seconds
- **arXiv:** 2507.13343 (July 2025)
- **Authors:** Yushu Wu, Yanyu Li, et al. (Snap Research)
- **URL:** https://arxiv.org/abs/2507.13343
- **Relationship:** Notes "LTX-Video applies a high-compression VAE and runs in real time on GPUs, yet its 1.9B parameters remain prohibitive for mobile devices." Uses LTX-Video as baseline; mobile-optimized 0.9B model achieves higher VBench total score.

### 9. AI-Generated Video Detection (One-Stop Solution)
- **arXiv:** 2601.11035 (January 2026)
- **URL:** https://arxiv.org/abs/2601.11035
- **Relationship:** Includes LTX-Video among video generation models evaluated for AI-generated content detection.

### 10. A Survey on Long-Video Storytelling Generation (ICCV 2025 Workshop)
- **Venue:** ICCV 2025 Workshop on Long-Video Foundation Models (LongVid-Foundation)
- **Authors:** Elmoghany et al.
- **URL:** https://openaccess.thecvf.com/content/ICCV2025W/LongVid-Foundation/
- **Relationship:** References LTX-Video as a key model in the landscape of long-video storytelling systems.

### 11. LLMPopcorn: Exploring LLMs as Assistants for Popular Micro-video Generation
- **arXiv:** 2502.12945 (February 2025)
- **URL:** https://arxiv.org/abs/2502.12945
- **Relationship:** Highlights LTX-Video alongside HunyuanVideo for video generation in micro-video pipeline.

### 12. T2VPhysBench: Physical Consistency in Text-to-Video Generation
- **arXiv:** 2505.00337 (May 2025)
- **URL:** https://arxiv.org/abs/2505.00337
- **Relationship:** Evaluates LTX-Video for physical consistency (gravity, collision, fluid dynamics).

### 13. T2VWorldBench: World Knowledge in Text-to-Video Generation
- **arXiv:** (July 2025)
- **URL:** https://arxiv.org/html/2507.18107v1
- **Relationship:** Evaluates LTX-Video's world knowledge, reporting average score of ~0.68.

### 14. IP-Bench: Image Protection Methods in Image-to-Video Generation
- **URL:** https://arxiv.org/html/2603.26154
- **Relationship:** Notes nearly all protection methods yield lowest VBench degradation rates on LTX and Skyreel.

### Papers Referenced in LTX-2 That Cite/Compare to LTX-Video

### 15. WAN 2.1 / Wan Video Generation
- **Authors:** Wang et al.
- **Relationship:** Compared against LTX-2 in benchmarks. LTX-2 is ~18x faster on H100.

### 16. HunyuanVideo
- **arXiv:** 2412.03603
- **Authors:** Kong, Tian, Zhang, et al.
- **Relationship:** Contemporary work cited in LTX-Video paper. Uses 8x8x4 VAE compression (vs LTX-Video's 32x32x8).

### 17. Ovi (Open-source Audio-Visual)
- **Relationship:** Concurrent open-source T2AV model compared against LTX-2. Uses two 5B streams fine-tuned from Wan 2.2.

### 18. BridgeDiT
- **Relationship:** Concurrent T2AV work combining existing T2V and T2A backbones. Cited for high computational overhead vs LTX-2.

---

## Citation Themes

LTX-Video citations cluster around several themes:
1. **Benchmarking:** Used as baseline in video generation benchmarks (VBench, T2VPhysBench, T2VWorldBench, IP-Bench)
2. **Efficiency research:** Referenced for high-compression VAE and real-time generation, targeted for further optimization (mobile deployment)
3. **World models:** Cited as foundation for interactive world models from video diffusion priors
4. **Detection:** Included in AI-generated content detection evaluations
5. **Surveys:** Referenced in comprehensive surveys of the video generation landscape
6. **Downstream tasks:** Foundation for dubbing, personalization, portrait editing, foley generation

## Community Adoption Metrics
- 567K+ monthly downloads (HuggingFace)
- 24 LoRA adapters published
- 25 community fine-tunes available
- 16 quantization variants created
- 100+ HuggingFace Spaces using the model

## Notes

Full citation tracking was limited by access restrictions during research. The papers above were identified through direct references in the LTX paper family, HuggingFace model pages, and arXiv searches. A comprehensive citation analysis via Semantic Scholar or Google Scholar would likely reveal hundreds of additional citing papers, as LTX-Video is one of the most prominent open-source video generation models.
