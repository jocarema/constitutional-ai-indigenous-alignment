# Constitutional AI for Indigenous Alignment

**Aligning Language Models with Indigenous Community Cosmovision through Constitutional AI**

[![License: Code GPLv3 · Content CC BY-NC-SA](https://img.shields.io/badge/License-GPLv3%20%7C%20CC%20BY--NC--SA-blue.svg)](#license)
[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
<!-- [![arXiv](https://img.shields.io/badge/arXiv-2610.25298-b31b1b.svg)](https://arxiv.org/abs/2610.25298) -->

> Leer en español: [README.es.md](README.es.md)

---

## Overview

Large language models are predominantly trained on corpora that reflect Western, anglophone worldviews. This asymmetry is not neutral: when a model answers questions about territory, community authority, reciprocity, or the relationship with nature, it does so from philosophical assumptions that may be alien — or directly opposed — to those of the communities using the system.

This repository accompanies the following work:

> Fernández Sabido, S., Reyes Magaña, J., Chay Tun, J. A., Buenfil, J. C., González Nahuat, V., & Morales, F. (2026). *Alineando modelos de lenguaje con la cosmovisión indígena comunitaria: Constitutional AI y justicia epistémica desde el Sur Global* [Aligning Language Models with Indigenous Community Cosmovision through Constitutional AI]. **Texto Livre**, dossier *Linguística na IA e IA na Linguística* [AI in Linguistics / Linguistics in AI]. Being prepared for submission.

An English preprint is available on arXiv:

> Fernández Sabido, S., Reyes Magaña, J., Chay Tun, J. A., Buenfil, J. C., González Nahuat, V., & Morales, F. (2026). *Aligning Language Models with Indigenous Community Cosmovision through Constitutional AI*. arXiv:2610.25298 [cs.AI]. https://arxiv.org/abs/2610.25298

It contains the complete system developed to align a language model with the ethical and philosophical principles of five indigenous communities in Latin America, through a **Cultural Constitution** co-created with community leaders and a **Constitutional AI (CAI)** pipeline combining supervised fine-tuning (SFT) and direct preference optimization (DPO), deployed with retrieval-augmented generation (RAG).

## Repository structure

| Folder | Contents |
|---|---|
| [`constitution/`](constitution/) | The Cultural Constitution: 12 operational principles with traceability to community testimonies |
| [`training/`](training/) | Notebooks for SFT, DPO, and evaluation, and the SFT and preference datasets |
| [`results/`](results/) | Comparative results across three experimental configurations (A/B/C) |
| [`rag/`](rag/) | Description of the RAG architecture |
| [`data/`](data/) | Description of the data, governance, and knowledge-sharing agreement. **No raw audio or transcriptions are distributed** |
| [`docs/`](docs/) | Language policy, data governance (CARE+S), interview guide, knowledge-sharing agreement, qualitative codebook, analysis skill, and licensing framework |

## The Cultural Constitution

The Cultural Constitution is a structured JSON document (`codigo_moral_v2.1.json`) that operationalizes ethical and community principles into a format processable by AI systems. It was built through semi-structured interviews with five indigenous leaders, writers, and defenders — members of URI (United Religions Initiative) — between April and May 2026. They participated not as anonymous informants but as **co-creators**, contributing from their recognized community authority.

The process produced **12 operational principles**:

1. Territorial respect
2. Good living with sufficiency
3. Reciprocity and solidarity
4. Collective decision-making
5. Situated authority
6. Reparation before punishment
7. Hospitality with safeguard
8. Intergenerational transmission
9. Technology with contextual evaluation
10. Consultation and consent
11. Linguistic and translation caution
12. Realism without stigma

Each principle is structured with: name, operational rules, supporting interview fragment, concrete situations with aligned and misaligned responses, critique instructions, revision instructions, and a preference question for DPO. Interviewee names have been replaced with codes (`Entrevistado 1`, `Entrevistado 2`, ...) for public release; the mapping between codes and names is available in the paper.

## The CAI pipeline

The training follows the Constitutional AI pipeline in two phases:

- **SFT (Supervised Fine-Tuning)** — The base model generates an initial response; a revision prompt including the original response and the corresponding constitutional principle guides the model to produce a revised response. The revised response, together with aligned examples from the constitution, forms the supervised training dataset.
- **DPO (Direct Preference Optimization)** — 24 pairs of aligned and misaligned responses optimize the model's preferences directly, without a separate reward model.
- **Base model** — Qwen2.5-7B-Instruct and Qwen2.5-1.5B with LoRA adaptation (r=16, α=16, dropout=0.05).
- **Deployment** — A conversational interface with retrieval-augmented generation (RAG).

Three experimental configurations were run to estimate robustness:
- (A) 7B with packing
- (B) 1.5B with packing
- (C) 7B without packing

Configurations A and C share the same base model and datasets; they differ only in the use of sequence packing during DPO training.

## Key findings

- The most consistent improvements were observed in **intergenerational transmission**, **linguistic caution**, and **consultation and consent** — principles with clear operational rules and well-defined concrete examples.
- The **1.5B configuration** achieved the highest average RL>SL score (0.523), outperforming both 7B configurations. With only 24 preference pairs and a single run per configuration, this should be read as an observation of this setting rather than as evidence that model size is irrelevant.
- **Sequence packing negatively affected cultural alignment** in the 7B model (0.324 with packing vs. 0.474 without), suggesting that mixing examples from different categories in a single sequence dilutes the learning signal for small datasets.
- **No configuration consistently activated epistemic self-limitation** on generalizing questions, indicating that complex philosophical principles requiring self-restraint need more contrasting negative examples and possibly strategies beyond standard DPO.
- The training did **not** cause catastrophic forgetting: general knowledge questions were answered correctly after SFT and DPO.

## Community co-creators

This work would not have been possible without the generosity and disposition of the five indigenous community members who shared values and practices that are part of their cosmovision:

| Name | People | Country | Role |
|---|---|---|---|
| Felipe Chuc Yah | Maya | Mexico | Farmer and producer, mainly of vegetables; also raises animals (chickens, pigs) at home; knowledgeable in community norms and land work. |
| Francisco Morales | Aymara | Argentina | Writer on Indigenous cosmovision; co-founder of the Circle of Indigenous Written and Oral Knowledge; Global Council Trustee for Latin America and the Caribbean (URI); URI Circle member. |
| Julio César Paz Nap | Tolupán | Honduras | Territorial defender and human rights activist; advocate for youth participation in community decision-making; URI Circle member. |
| Donatila Girón Calix | Lenca | Honduras | Community leader and legal promoter; companion of women victims of violence; defender of land and water; URI Circle member. |
| Marvin Ramón Castellón | Chorotega | Honduras | Indigenous Chorotega representative of the southern Choluteca zone; traditional midwife and practitioner of traditional medicine; water and land rights activist; URI Circle member and Sub-Coordinator of his Cooperation Circle. |

The five co-creators contributed from their respective communities and organizational affiliations. URI (United Religions Initiative, urialc.org) accompanied four of them through its network.

## Author contributions

- **Silvia Fernández Sabido** — Study design, funding acquisition, project coordination, interviews, experiments, manuscript writing.
- **Jorge Reyes Magaña** — Experiment co-design, technical supervision, methodological guidance, manuscript contributions.
- **Jesús Adrián Chay Tun** — Interview guide, community interviews, Cultural Constitution, technical report.
- **Jean Carlos Buenfil** — Training pipeline (SFT and DPO), initial alignment experiments, technical report.
- **Victor González Nahuat** — RAG architecture and conversational interface, technical report.
- **Francisco Morales** — Writer on Indigenous cosmovision; community co-creation, Cultural Constitution contributions, manuscript review.

## Language policy

This repository is **bilingual by design**. Code, folder names, and metadata are in English for technical interoperability. Scientific artifacts — the Cultural Constitution, the qualitative codebook, the analysis skill, and the derived datasets — remain in Spanish, the language in which they were co-created with community leaders. Translating them would reproduce the epistemic asymmetry this work seeks to address.

See [`docs/language_policy.md`](docs/language_policy.md) for details.

## Data governance

This project follows the **CARE Principles for Indigenous Data Governance** (Collective Benefit, Authority to Control, Responsibility, Ethics) — extended in this work to **CARE+S**, adding *Digital Sovereignty and Active Participation* — alongside the FAIR principles. The governance framework is articulated in the **Indigenous Governance Protocol for Technology Development and Data Collection** (SECIHTI CBF-2025-G-368), adapted to the context of this work.

Key commitments:

- **Community co-authorship** — The five community leaders who contributed to the Cultural Constitution are recognized as co-creators, not anonymous informants. Their names and communities appear in the paper and in this repository.
- **Assembly as decision-making authority** — Any initial sharing agreement, veto, or request for total revocation and destruction of data falls under the authority of the corresponding Community Assembly, operating under its own normative systems.
- **Community oversight committee** — An operational body designated by the Assembly supervises that project files are used only for the approved purposes and reports periodically on the state of the data.
- **Right to refusal and revocation** — Any community or individual may refuse documentation of their language, or revoke their authorization at any stage. In case of revocation, the team is obligated to securely delete the corresponding files from all project servers and databases.
- **Non-commercial use** — Data use for commercial products, trademark registration, paid licensing, or training AI models for private profit is strictly prohibited.
- **Restricted access** — Original audio recordings and transcriptions are classified under *Restricted Access* and are not distributed in this repository. Access for scientific validation requires a formal written request, an analysis of community benefit, and authorization from the corresponding Community Assembly. See [`data/README.md`](data/README.md).
- **Derived datasets** — The SFT and preference datasets are derived from the Cultural Constitution and do not contain raw interview data. They are distributed in [`training/`](training/) under the same license as the scientific artifacts (CC BY-NC-SA 4.0).

The project does not yet count on a formal indigenous council with binding veto power across all stages; establishing one — beginning with a consultative council of the current five co-creators and institutional participation from URI — is an explicit goal for future iterations.

See [`docs/data_governance.md`](docs/data_governance.md) for the full framework and [`docs/protocol_reference.md`](docs/protocol_reference.md) for the source protocol.

## Funding

This work was supported by a seed fund from URI — United Religions Initiative (urialc.org). The funders had no role in the design of the study, the construction of the Cultural Constitution, the training of the models, or the interpretation of results.

## Citation

This work is being prepared for submission to Texto Livre. If you use it, please cite the preprint (or the paper once published):

```bibtex
@misc{fernandezsabido2026constitutional,
  title         = {Aligning Language Models with Indigenous Community Cosmovision
                   through Constitutional AI},
  author        = {Fernández Sabido, Silvia and Reyes Magaña, Jorge and
                   Chay Tun, Jesús Adrián and Buenfil, Jean Carlos and
                   González Nahuat, Victor and Morales, Francisco},
  year          = {2026},
  eprint        = {XXXX.XXXXX},
  archivePrefix = {arXiv},
  primaryClass  = {cs.AI},
  url           = {https://arxiv.org/abs/2610.25298}
}
```

A `CITATION.cff` file is included for GitHub's "Cite this repository" button.

## License

This repository combines two licensing regimes, consistent with the *Indigenous Governance Protocol for Technology Development and Data Collection* (SECIHTI CBF-2025-G-368):

- **Code** (notebooks, scripts) — GNU General Public License v3.0 (GPLv3). Copyleft: derivative works must remain open.
- **Scientific artifacts** (Cultural Constitution, documentation) — Creative Commons Attribution-NonCommercial-ShareAlike 4.0 (CC BY-NC-SA 4.0) and Traditional Knowledge (TK) Labels.
- **Datasets** — The SFT and preference datasets are distributed in [`training/`](training/) under CC BY-NC-SA 4.0. Raw audio recordings and transcriptions are not distributed; access is restricted and requires authorization from the corresponding Community Assembly. See [`data/README.md`](data/README.md) for details.

See [`LICENSE.md`](LICENSE.md) and [`docs/data_governance.md`](docs/data_governance.md) for details.
