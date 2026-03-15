# Computational Identification of Molecular Mimicry in RA-Associated Pathogens
## A Structural Validation and Machine Learning Approach

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/)
[![Status: Complete](https://img.shields.io/badge/status-complete-green.svg)]()

---

## Abstract

Molecular mimicry between pathogen and host peptides is a proposed mechanism 
for autoimmune triggering in Rheumatoid Arthritis, yet most computational 
studies rely exclusively on sequence similarity metrics for mimic identification. 
This study presents an integrative three-tier pipeline combining MHC epitope 
prediction, BLAST-based sequence alignment, and atomic structural validation 
via TM-align across 32 RA-associated pathogens. Sequence identity showed 
near-zero correlation with structural similarity (r = -0.127), demonstrating 
that structural validation is indispensable for reliable mimicry identification 
and cannot be substituted by sequence-based screening alone.

---

## Key Findings

- **399 peptide pairs** curated across **32 organisms**, covering both 
  MHC class I and class II epitopes
- **96.7% structural confirmation rate** — peptide pairs predicted 
  computationally and validated at atomic resolution (RMSD < 2.0 Å)
- **r = -0.127** between sequence identity and structural RMSD — 
  near-zero correlation confirming sequence similarity does not predict 
  structural mimicry
- **Random forest classifier AUC = 0.954** with **100% sensitivity** 
  for confirmed mimics (zero false negatives)
- **BLOSUM80 per residue** ranked as top predictive feature 
  (importance = 0.257), outperforming raw sequence metrics

---

## Pipeline Overview
```
┌─────────────────────────────────────────────────────────────┐
│                    INPUT: Pathogen Peptides                  │
│              (literature-curated RA-associated)              │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              TIER 1: Epitope Prediction                      │
│         NetMHCpan 4.1 — MHC binding affinity                │
│         %Rank_EL threshold, immunogenicity scoring          │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              TIER 2: Sequence Alignment                      │
│         BLAST with BLOSUM80 substitution matrix             │
│         Identity %, alignment length, coverage              │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              TIER 3: Structural Validation                   │
│         TM-align — atomic structural comparison             │
│         RMSD threshold 2.0 Å for mimic confirmation         │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              TIER 4: ML Classification                       │
│         Random Forest — leakage-free feature set            │
│         AUC 0.954, 100% sensitivity, 95% accuracy           │
└─────────────────────────────────────────────────────────────┘
```

---

## Repository Structure
```
molecular-mimicry-RA-pipeline/
│
├── README.md
│
├── data/
│   ├── ML_targets_final.csv        # complete labelled dataset
│   └── data_description.md         # column definitions and methodology
│
├── notebooks/
│   ├── 01_data_loading_cleaning.ipynb
│   ├── 02_statistical_analysis.ipynb
│   ├── 03_figures.ipynb
│   └── 04_ml_model.ipynb
│
├── figures/
│   ├── figure1_rmsd_vs_identity.png
│   ├── figure2_rmsd_distribution.png
│   ├── figure3_boxplots.png
│   ├── figure4_correlation_heatmap.png
│   ├── figure5_importance_roc.png
│   └── figure6_confusion_matrix.png
│
├── results/
│   └── classification_report.txt
│
└── requirements.txt
```

---

## How to Run
```bash
# 1. Clone the repository
git clone https://github.com/minbaku/molecular-mimicry-RA-pipeline.git
cd molecular-mimicry-RA-pipeline

# 2. Install required libraries
pip install -r requirements.txt

# 3. Open notebooks in order
jupyter notebook notebooks/01_data_loading_cleaning.ipynb

# 4. Run each notebook sequentially
# 01 → 02 → 03 → 04
```

Alternatively open directly in Google Colab by clicking the badge at 
the top of each notebook.

---

## Dataset

| Property | Value |
|----------|-------|
| Total peptide pairs | 399 |
| Confirmed mimics (Y) | 262 |
| Non-mimics (N) | 137 |
| Organisms | 32 |
| MHC coverage | Class I and II |
| Missing values | 0 |
| Structural validation method | TM-align |
| RMSD threshold | 2.0 Å |

---

## Results Summary

| Metric | Value |
|--------|-------|
| Structural confirmation rate | 96.7% |
| Identity % vs RMSD correlation | -0.127 |
| BLOSUM80 vs TM-score correlation | -0.039 |
| Classifier AUC-ROC | 0.954 |
| Sensitivity (mimic recall) | 100% |
| Specificity | 85.2% |
| Overall accuracy | 95% |
| Top feature | BLOSUM80 per residue (0.257) |

---

## Preprint

> **Coming soon on bioRxiv**
> Link will be added upon publication.
> 
> *Ilahi M. (2026). Computational Identification of Molecular Mimicry 
> in RA-Associated Pathogens: A Structural Validation and Machine 
> Learning Approach.*

---

## Publication

> **Manuscript in preparation**
> Target submission: Exploration of Immunology, March 2026

---

## Author

**Minza Ilahi**
M.Tech Biotechnology, Guru Gobind Singh Indraprastha University, Delhi

- Email: ilahiminza@gmail.com
- LinkedIn: [minza-ilahi](https://www.linkedin.com/in/minza-ilahi-9b6aba222)
- ORCID: [0009-0001-8883-5226](https://orcid.org/0009-0001-8883-5226)
- GitHub: [minbaku](https://github.com/minbaku)

---

## Citation

If you use this dataset or pipeline, please cite:
```
Ilahi, M. (2026). Computational Identification of Molecular Mimicry 
in RA-Associated Pathogens: A Structural Validation and Machine 
Learning Approach. Manuscript in preparation.
```

---

## Acknowledgements

Supervisor: Prof. Sayan Chatterjee, GGSIPU Delhi
Structural validation: TM-align (Zhang & Skolnick, 2005)
Epitope prediction: NetMHCpan 4.1 (Reynisson et al., 2020)

---

*This project is part of an M.Tech final year dissertation at 
Guru Gobind Singh Indraprastha University, Delhi, India.*
