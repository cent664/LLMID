# Invariant Features in Language Models: Geometric Characterization and Model Attribution
---

**Agnibh Dasgupta\***, **Abdullah All Tanvir\***, **Xin Zhong†**  
Department of Computer Science, University of Nebraska Omaha

\* Equal contribution. † Corresponding author.

[🌐 Project page](https://cent664.github.io/LLMID/) · [📄 Paper (arXiv)](https://arxiv.org/abs/2605.06458) · [💻 Code](https://github.com/XinZhong-AI/InvariantFeaturesLanguageModels)

**🔍 Project Overview**
---
We give a local geometric characterization of semantic invariance in language models: paraphrases tend to move along nuisance directions, while meaning lives in a more stable subspace. Contrasting meaning-preserving and meaning-changing edits recovers that subspace. Interventions test it, and its model-specific geometry supports zero-shot model attribution.

**Overview of the proposed scheme:**
---
![overview](docs/figures/1_PIA.png)

**🧪 Synthetic Data Set**
---
To isolate meaning-preserving (SP) vs meaning-changing (SC) variation, we construct a synthetic English dataset of short prompts organized into semantic groups. Each group starts from an anchor sentence; SP variants paraphrase the anchor, soft SC variants keep the same topic family but change the intended meaning, and hard SC variants move to a different topic or domain. Groups span multiple domains, with disjoint discovery and validation splits (1,400 groups · 13 domains · 57,400 sentences overall). The table below shows the per-group paraphrase budget around each anchor for ten example groups from the explorer.

| Group | SP | Soft SC | Hard SC |
| --- | ---: | ---: | ---: |
| 🐈 Cats | 20 | 10 | 10 |
| 🇫🇷 France | 20 | 10 | 10 |
| 🐍 Python | 20 | 10 | 10 |
| 🌱 Photosynthesis | 20 | 10 | 10 |
| 🗳️ Democracy | 20 | 10 | 10 |
| 🧠 Einstein | 20 | 10 | 10 |
| 🌡️ Climate Change | 20 | 10 | 10 |
| ∫ Calculus | 20 | 10 | 10 |
| 🧘 Meditation | 20 | 10 | 10 |
| 🍞 Bread | 20 | 10 | 10 |

**📊 Results Snapshot**
---
Invariant-zone signatures identify the source model under base, fine-tuned, and distilled variants. The plot below shows Group 1 attribution accuracy (%) from the project page (Figure 7), with strong performance across model families.
![attribution](docs/figures/attribution_g1.png)

**📚 Citation**
---
```bibtex
@article{dasgupta2026invariantArxiv,
  title   = {Invariant Features in Language Models: Geometric Characterization and Model Attribution},
  author  = {Dasgupta, Agnibh and Tanvir, Abdullah All and Zhong, Xin},
  journal = {arXiv preprint arXiv:2605.06458},
  year    = {2026}
}
```
📌 A. Dasgupta, A. A. Tanvir, and X. Zhong, "Invariant Features in Language Models: Geometric Characterization and Model Attribution," arXiv preprint arXiv:2605.06458, 2026.

*Citations will be updated once the final venue details become available.*
