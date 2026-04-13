# AVControl: Efficient Framework for Training Audio-Visual Controls

## Metadata
- **Title:** AVControl: Efficient Framework for Training Audio-Visual Controls
- **Authors:** Tavi Halperin*, Naomi Ken Korem*, Mohammad Salama, Harel Cain, Asaf Joseph, Anthony Chen, Urska Jelercic, Ofir Bibi (* equal contribution)
- **Affiliation:** Lightricks
- **Date:** March 27, 2026 (arXiv: 2603.24793)
- **Source URL:** https://arxiv.org/abs/2603.24793
- **Project Page:** https://matanby.github.io/AVControl/
- **Code:** https://github.com/Lightricks/LTX-2/tree/main/packages/ltx-trainer
- **Video:** https://youtu.be/f8D2CwF6BIY

## Abstract

Controlling video and audio generation requires diverse modalities, from depth and pose to camera trajectories and audio transformations, yet existing approaches either train a single monolithic model for a fixed set of controls or introduce costly architectural changes for each new modality. We introduce AVControl, a lightweight, extendable framework built on LTX-2, a joint audio-visual foundation model, where each control modality is trained as a separate LoRA on a parallel canvas that provides the reference signal as additional tokens in the attention layers, requiring no architectural changes beyond the LoRA adapters themselves. We show that simply extending image-based in-context methods to video fails for structural control, and that our parallel canvas approach resolves this. On the VACE Benchmark, we outperform all evaluated baselines on depth- and pose-guided generation, inpainting, and outpainting, and show competitive results on camera control and audio-visual benchmarks. Our framework supports a diverse set of independently trained modalities: spatially-aligned controls such as depth, pose, and edges, camera trajectory with intrinsics, sparse motion control, video editing, and, to our knowledge, the first modular audio-visual controls for a joint generation model. Our method is both compute- and data-efficient: each modality requires only a small dataset and converges within a few hundred to a few thousand training steps, a fraction of the budget of monolithic alternatives. We publicly release our code and trained LoRA checkpoints.

## Key Contributions

1. **Parallel Canvas Conditioning:** A compute- and data-efficient framework for training per-modality control LoRAs on a parallel canvas, enabling faithful video structural control and fine-grained inference-time strength modulation.

2. **Diverse Control Modalities:** A diverse set of independently trained control modalities, from spatially-aligned controls and camera trajectory to audio-visual applications, demonstrating the framework's flexibility.

3. **Small-to-Large Control Grid:** A training strategy that reduces the reference canvas resolution for spatially sparse modalities, lowering latency without sacrificing control fidelity.

## Methodology

### Core Approach: Parallel Canvas
- Reference signal provided as **additional tokens in self-attention layers** (not concatenated spatially)
- LTX-2's per-token timestep mechanism naturally distinguishes reference (clean) tokens from generation (noisy) tokens
- No positional encoding changes required (unlike Flux Kontext which needs new RoPE dimension)
- Only trainable component: minimal LoRA adapter on frozen audio-visual backbone
- Reference influence continuously modulated at inference time (globally or locally)

### Why Not IC-LoRA (Spatial Concatenation)?
- IC-LoRA concatenates reference and target side-by-side
- Large spatial distance between semantically corresponding positions weakens attention interaction
- Fails for structural controls (depth, pose) in video
- Parallel canvas resolves this by placing reference tokens directly in attention computation

### Per-Modality LoRA Training
- Each control modality trained as separate lightweight LoRA
- LoRA rank 128 (default; ranks 32-128 differ by <1 VBench point)
- Training converges in ~1,000-3,000 steps for spatial controls
- Total budget across all 13 modalities: ~55K steps (vs VACE's 200K steps)

### Control Modalities Supported (13 total)
**Spatial Controls:**
- Depth-guided generation
- Pose-guided generation
- Canny edge control

**Camera Control:**
- Camera trajectory from image
- Camera trajectory from video (re-rendering)

**Motion Control:**
- Sparse point tracking

**Video Editing:**
- Cut-on-action
- Inpainting
- Outpainting

**Audio-Visual Controls:**
- Who-is-talking (speaker-aware generation)
- Audio intensity to waveform
- Audio-visual synchronization controls

## Results

### VACE Benchmark
- **Outperforms all evaluated baselines** on depth-guided, pose-guided generation, inpainting, and outpainting
- **Competitive results** on camera control
- Competitive on audio-visual benchmarks

### Efficiency
- Total training budget: ~55K steps across all modalities (vs VACE's 200K single training run)
- Each modality requires only small dataset
- Convergence in few hundred to few thousand steps
- Generalization from synthetic data demonstrated for several LoRAs

### Ablations
- Depth VBench scores: 81.1 at 1,000 steps, 81.6 at 3,000 steps
- LoRA ranks 32/64/128 yield VBench averages of 80.9/81.3/81.6

## Downstream Adoption
The framework has been adopted by concurrent work:
- **JUST-DUB-IT:** Video dubbing via joint audio-visual diffusion
- **ID-LoRA:** Identity-driven audio-video personalization
- **In-Context Sync-LoRA:** Portrait video editing

## Limitations
- Inherits base model (LTX-2) capabilities and limitations
- Mask representation: rare failures when video colors match designated inpainting color
- Complex character motion: temporal jitter with rapid intricate movements
- Camera control: stretching/ghosting in scenes with rapid non-rigid motion
- Reference image conditioning (identity preservation) is a capability gap vs VACE

## Relationship to LTX Ecosystem
- Built directly on **LTX-2** as the frozen backbone
- Uses LTX-2's per-token timestep mechanism as key enabler
- Released as part of the LTX-2 model family (IC-LoRA checkpoints)
- Available models: Union Control (Depth+Pose+Canny), Detailer, Motion Track, etc.
