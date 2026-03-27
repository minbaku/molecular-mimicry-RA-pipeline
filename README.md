# MimicryDB-Auto: Structural Validation Reveals the Inadequacy of Sequence-Based Molecular Mimicry Screening in Autoimmunity

## A Curated Multi-Pathogen Dataset and Reproducible Analysis Pipeline

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/)
[![DOI:10.5281/zenodo.19262752](https://zenodo.org/badge/1182709738.svg)](https://zenodo.org/doi/10.5281/zenodo.19262752)

---

## Abstract

Molecular mimicry—structural or sequence similarity between pathogen-derived and host self-peptides sufficient to trigger cross-reactive immune responses—has been proposed as a mechanism of autoimmune triggering across rheumatoid arthritis, systemic lupus erythematosus, ankylosing spondylitis, systemic sclerosis, antiphospholipid syndrome, dermatomyositis, and Guillain-Barré syndrome. Computational identification of mimicry candidates has historically relied on sequence-based metrics, resting on the untested assumption that sequence similarity predicts structural similarity at the MHC-presented peptide level.

We present **MimicryDB-Auto**, the first curated, labelled multi-pathogen dataset integrating MHC epitope prediction, sequence alignment, and atomic structural validation at the individual epitope level across both MHC class I and II presentations, comprising **399 pathogen-host peptide pairs** spanning **32 organisms** constructed through a reproducible seven-step pipeline.

**Key findings:**
- Sequence identity explains at most **1.6% of variance** in structural RMSD (r = −0.127, R² = 0.016)
- **91.9%** of randomly cross-paired sequence-similar 9-aa pairs achieve structural equivalence (RMSD < 2.0 Å)
- **99.2%** of structurally equivalent cross-pairs have zero detectable sequence similarity
- A multivariate sequence signal exists (AUC = 0.841) but cannot substitute for structural validation

MimicryDB-Auto and the complete reproducible pipeline are publicly available at this repository.

---

## Key Findings

| Finding | Statistical Evidence | Interpretation |
|---------|---------------------|----------------|
| Sequence identity vs RMSD | r = −0.127, p = 0.036, R² = 0.016 | Significant but negligible relationship |
| BLOSUM80 discriminability | p < 0.0001 (Mann-Whitney U) | Reflects class construction, not independent sequence discrimination |
| RF multivariate signal (Y vs N) | AUC = 0.958 (95% CI: 0.886–0.999) | Upper bound; inflated by categorical construction |
| RF multivariate signal (Strong vs Weak) | AUC = 0.841 | Genuine signal but insufficient alone |
| Cross-pair structural equivalence | 91.9% (125/136) | Structural equivalence is modal outcome |
| Sequence-dissimilar structural mimics | 99.2% of equivalent cross-pairs | Mimicry invisible to conventional screening |

---

## Pipeline Overview

```
┌─────────────────────────────────────────────────────────────┐
│                  STEP 1: Pathogen Selection                 │
│         Systematic literature review (32 organisms)         │           
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              STEP 2: MHC Epitope Prediction                 │
│        NetMHCpan 4.1 (Class I) / NetMHCIIpan 4.0 (Class II) │
│               %Rank EL ≤ 0.5, IC50 ≤ 50 nM                  │        
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│               STEP 3: Sequence Alignment                    │
│          BLASTp vs Human Proteome (BLOSUM80)                │
│        ≥50% identity, ≥90% coverage threshold               │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              STEP 4: Structural Extraction                  │
│    Peptide coordinates from PDB/Swiss-Prot/AlphaFold        │
│         PyMOL extraction of relevant residues               │
└─────────────────────┬───────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              STEP 5: Structural Validation                  │
│            TM-align structural superposition                │
│            RMSD < 2.0 Å = Confirmed Mimic (Y)               │
└──────────────────────┬───────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              STEP 6: Feature Extraction                     │
│      Sequence, structural, and immunological features       │               

└──────────────────────┬───────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                STEP 7: Analysis & ML                        │
│    Pearson correlation, Mann-Whitney U, Random Forest       │                     
└─────────────────────────────────────────────────────────────┘
```

---

## Repository Structure
```
molecular-mimicry-RA-pipeline/
│
├── Results/
│   └── KEY_RESULTS.txt
|
├── data/
│   ├── ML_targets_final.csv        # complete labelled dataset
│   └── data_description.md         # column definitions and methodology
│
├── figures/
│   ├── figure1_rmsd_vs_identity.png
│   ├── figure2_rmsd_by_disease.png
│   ├── figure3_rmsd_distribution.png
│   ├── figure4_boxplots.png
│   ├── figure5_correlation_heatmap.png
│   └── figure6_importance_roc.png
|   └── figure7_confusion_matrix.png
|
├── notebooks/
│   ├── 01_data_loading_cleaning.ipynb
│   ├── 02_statistical_analysis.ipynb
│   ├── 03_ml_model.ipynb
│   └── 04_figures.ipynb
│
├── README.md
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
| Confirmed mimics (Y, RMSD < 2.0 Å) | 262 |
| Non-mimics (N, RMSD ≥ 2.0 Å) | 137 |
| Strong mimics (RMSD < 1.0 Å) | 159 |
| Weak mimics (1.0–2.0 Å) | 103 |
| Organisms represented | 32 |
| MHC coverage | Class I and II |
| Autoimmune conditions | RA, SLE, GBS, AS, SSc, APS, dermatomyositis |
| Structural validation method | TM-align |
| RMSD confirmation threshold | 2.0 Å |
---

## Results Summary

### Classification Results
#### Y vs N Classifier (2.0 Å threshold)
Note: This classifier distinguishes Y-class (sequence-similar) from N-class (zero sequence similarity by construction) and represents an upper bound on classifier performance.

|Metric |	Value	| 95% CI |
|-------|-------|--------|
|AUC-ROC | 0.958 |	0.886–0.999 |
|Sensitivity |	1.000 |	1.000–1.000 |
|Specificity |	0.852 |	0.708–0.967 |
|Accuracy |	0.950 |	0.900–0.988 |
|5-fold CV AUC |	0.979 ± 0.018 |	— |
---

#### Strong vs Weak Mimic Classifier (1.0 Å threshold)
Biologically relevant test within the sequence-similar pool.

|Metric |	Value |
|-------|-------|
|Test AUC-ROC |	0.841 |
|5-fold CV AUC |	0.823 ± 0.053 |

---

## Preprint

> **Available on Zenodo**
> https://doi.org/10.5281/zenodo.19262132
> 
> *Ilahi, M. (2026). MimicryDB-Auto: Structural Validation Reveals the Inadequacy of Sequence-Based Molecular Mimicry Screening in Autoimmunity. Zenodo. https://doi.org/10.5281/zenodo.19262132*

---

## Publication

> **Manuscript in preparation**
> Target submission: March 2026

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
Minza-Ilahi. (2026). minbaku/molecular-mimicry-RA-pipeline: MimicryDB_Auto_v1 (MimicryDB_Auto_v.1). Zenodo. https://doi.org/10.5281/zenodo.19262752
```

---

## Acknowledgements

Supervisor: Prof. Sayan Chatterjee, GGSIPU Delhi
Structural validation: TM-align (Zhang & Skolnick, 2005)
Epitope prediction: NetMHCpan 4.1 (Reynisson et al., 2020)

---

*This project is part of an M.Tech final year dissertation at 
Guru Gobind Singh Indraprastha University, Delhi, India.*
