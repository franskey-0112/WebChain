<div align="center">

# WebChain

### A Large-Scale Human-Annotated Dataset of Real-World Web Interaction Traces

[![Paper](https://img.shields.io/badge/Paper-arXiv%202603.05295-red?style=flat-square&logo=arxiv)](https://arxiv.org/abs/2603.05295)
[![Dataset](https://img.shields.io/badge/Dataset-HuggingFace-yellow?style=flat-square&logo=huggingface)](https://huggingface.co/datasets/computer-use-agent-Lab/WebChain)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue?style=flat-square)](LICENSE)

</div>

---

## Overview

**WebChain** is the largest open-source dataset of human-annotated trajectories collected from real-world websites, designed to accelerate reproducible research in web agents.

Unlike synthetic datasets that are blocked by anti-bot detection, CAPTCHAs, or authentication walls, WebChain is built entirely by human annotators operating on live, diverse websites — capturing the complex, authenticated workflows that matter most.

### Key Statistics

| Metric | Value |
|--------|-------|
| Total Trajectories | **31,725** |
| Total Interaction Steps | **317,993** |
| Unique Domains | **428** |
| Avg. Trajectory Length | **10.02 steps** |
| Avg. Task Duration | **1.07 min** |

### Why WebChain?

Compared to existing web interaction datasets (Mind2Web, WebLINX, GUIAct):

- **10× more trajectories** than the next largest human-annotated dataset
- **Covers authenticated workflows** (e-commerce checkout, banking, travel booking) that synthetic methods cannot capture
- **Triple Alignment**: every step synchronizes visual, structural, and action data
- **Fully open-source** — data, collection tools, and benchmarks are all public

---

## Dataset

The dataset is hosted on HuggingFace: **[computer-use-agent-Lab/WebChain](https://huggingface.co/datasets/computer-use-agent-Lab/WebChain)**

```python
from datasets import load_dataset

dataset = load_dataset("computer-use-agent-Lab/WebChain")
```

### Data Schema

Each step in a trajectory contains **Triple Alignment** across three modalities:

| Modality | Fields |
|----------|--------|
| **Visual Context** | Full-page screenshot, viewport-specific crop |
| **Structural Context** | HTML snapshot, Accessibility (AX) tree |
| **Action Grounding** | Action type, pixel coordinates, bounding box, CSS selector, XPath |
| **Reasoning** | Chain-of-Thought (CoT) rationale for the action |

### Domain Coverage

WebChain spans **428 unique domains** across diverse categories:

- Shopping & E-commerce
- Flight & Travel Booking
- Accommodation & Hotels
- Real Estate & Property
- Fintech & Banking
- And many more...

---

## Construction Pipeline

WebChain is built with a three-stage pipeline ensuring both scale and quality:

**Stage 1 — Constraint-Based Task Synthesis**
An LLM conditioned on a site's extracted functional schema generates grounded, executable tasks stratified by complexity (simple retrieval → multi-constraint navigation → conditional dependency tasks).

**Stage 2 — Human-in-the-Loop Trajectory Collection**
Human annotators complete tasks via *WebChain Builder*, a passive logging tool that records DOM snapshots, precise action coordinates, element metadata, and timestamps for every step.

**Stage 3 — Post-processing Contextual Enrichment**
- **Visual Grounding Densification**: All interactive elements on each viewport are parsed and annotated (not just the clicked element), enabling dense layout-aware supervision.
- **Synthetic CoT Generation**: A VLM generates chain-of-thought rationales for each action, teaching agents interpretable multi-step planning.

---

## Experiments & Results

### Dual Mid-Training Recipe

We propose **Dual Mid-Training**, a training paradigm that decouples spatial grounding from long-horizon planning:

1. **LCRL** — Long-Chain-oriented RLVR post-training on planning trajectories
2. **CoT-SFT** — Supervised fine-tuning on CoT-augmented data for planning initialization
3. **SGRL** — Spatial Grounding RLVR training with Visual Grounding Densification

### State-of-the-Art Results

Our models achieve SOTA on **GUI-Act** and **OmniAct** benchmarks:

| Model | GUI-Act-Web SR | OmniAct-Web SR | Overall |
|-------|---------------|----------------|---------|
| Qwen2.5-VL-7B (Zero-Shot) | 83.9 | 70.3 | 70.9 |
| GUI-R1-7B | 80.3 | 77.3 | 74.2 |
| **WebChain-7B (Dual Mid-Training)** | **87.6** | **78.9** | **81.4** |

### Scaling Laws

We demonstrate a clear positive correlation between data volume and long-horizon planning performance — confirming that WebChain's scale is essential for unlocking robust web agent capabilities.

---

## Citation

If you find WebChain useful in your research, please cite our paper:

```bibtex
@article{fan2026webchain,
  title     = {WebChain: A Large-Scale Human-Annotated Dataset of Real-World Web Interaction Traces},
  author    = {Sicheng Fan and Rui Wan and Yifei Leng and Gaoning Liang and Li Ling and Yanyi Shang and Dehan Kong},
  journal   = {arXiv preprint arXiv:2603.05295},
  year      = {2026}
}
```

---

## License

This project is released under the [Apache 2.0 License](LICENSE).

---

## Contact

For questions or collaboration, please open an issue or reach out via the paper contact information.

<div align="center">
<b>Fudan University &nbsp;·&nbsp; IMeanAI &nbsp;·&nbsp; Shanghai Innovation Institute</b>
</div>
