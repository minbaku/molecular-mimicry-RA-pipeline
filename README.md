# MimicryDB: A Structural Immunoinformatics Resource for Molecular Mimicry in Autoimmune and Neurodegenerative Disease

**Two curated, labelled datasets. One reproducible pipeline. The first systematic atomic-level evidence that sequence-based mimicry screening is structurally insufficient.**

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19262752.svg)](https://doi.org/10.5281/zenodo.19262752)
[![Preprint](https://img.shields.io/badge/Preprint-Zenodo%20%2B%20Preprints.org-green)](https://doi.org/10.5281/zenodo.19262132)

---

## What Is This?

Molecular mimicry is the leading mechanistic hypothesis for how infections trigger autoimmune and neurodegenerative diseases — pathogen-derived peptides that structurally resemble human self-antigens can cause the immune system to attack its own tissues. The standard computational approach for identifying these mimicry candidates compares protein sequences, assuming that sequence similarity predicts structural similarity at the MHC-presented peptide level.

This repository tests that assumption systematically and finds it does not hold.

We present **MimicryDB-Auto** and **MimicryDB-Neuro** — the first curated, labelled multi-pathogen structural mimicry datasets integrating MHC epitope prediction, sequence homology filtering, and atomic structural validation at the individual epitope level, applied identically across two independent disease contexts. The central finding is consistent across both datasets: sequence identity explains at most 2.2% of variance in structural RMSD among confirmed mimics. The tool the field relies on is missing the majority of genuine structural mimics.

---

## Datasets at a Glance

| | MimicryDB-Auto | MimicryDB-Neuro |
|---|---|---|
| **Disease context** | Autoimmune rheumatic diseases + GBS | Neurodegenerative diseases |
| **Conditions** | RA, SLE, AS, SSc, APS, dermatomyositis, GBS | AD, PD, HD, and other CNS disorders |
| **Organisms** | 32 | 16 |
| **Total pairs** | 399 | 247 |
| **Confirmed mimics (RMSD < 2.0 Å)** | 262 (65.7%) | 170 (68.8%) |
| **Strong mimics (RMSD < 1.0 Å)** | 159 (39.8%) | 99 (39.9%) |
| **Non-mimics (RMSD ≥ 2.0 Å)** | 137 (34.3%) | 78 (31.5%) |
| **MHC coverage** | Class I and II | Class I and II |
| **Structural validation** | TM-align (RMSD < 2.0 Å) | TM-align (RMSD < 2.0 Å) |
| **Folder** | `MimicryDB-Auto/` | `MimicryDB-Neuro/` |

The near-identical mimic rates (65.7% vs 68.8%, χ² p > 0.05) and strong mimic rates (39.8% vs 39.9%) across two independently constructed datasets suggest that the prevalence of structural mimicry — and the inadequacy of sequence-based screening — is a general property of short MHC-presented peptides, not a disease-specific artefact.

---

## Key Findings

### 1. Sequence identity does not predict structural similarity

Within confirmed mimics — pairs that all passed stringent sequence thresholds (≥50% identity, ≥90% coverage) — sequence identity explains at most 2.2% of variance in structural RMSD:

| Dataset | Pearson r | R² | p-value |
|---|---|---|---|
| MimicryDB-Auto (n=262) | −0.082 | 0.007 | 0.187 |
| MimicryDB-Neuro (n=170) | −0.149 | 0.022 | 0.053 |
| Cross-dataset equivalence | Fisher's z = 0.686 | — | 0.493 |

Higher sequence identity does not predict better structural correspondence. This holds at both the 2.0 Å and 1.0 Å thresholds, and is confirmed non-parametrically by Spearman rank correlation.

### 2. Structural equivalence is the modal outcome among sequence-similar peptides

Cross-pairing validation — randomly reassigning sequence-similar peptides to new partners — revealed:
- **91.9%** of randomly cross-paired sequence-similar 9-mer pairs achieve RMSD < 2.0 Å
- **99.2%** of those structurally equivalent cross-pairs have zero detectable BLAST similarity

This directly quantifies the scope of mimicry invisible to conventional screening: the vast majority of genuine structural mimics would never be identified by sequence comparison alone.

### 3. A multivariate sequence signal exists but cannot substitute for structural validation

| Classifier | Dataset | AUC |
|---|---|---|
| Y vs N (sequence-similar vs zero-sequence) | Auto | 0.958 |
| Y vs N | Neuro | 0.946 (CV) |
| Strong vs Weak mimics (biologically valid task) | Auto | 0.841 |
| Strong vs Weak mimics | Neuro | 0.825 |

The AUC drop from ~0.95 to ~0.83 when moving to the biologically valid strong-vs-weak task quantifies how much classifier performance is inflated by categorical class construction. Among pairs sharing the same sequence threshold, structural outcomes remain poorly predicted.

### 4. MHC groove enforces structural convergence regardless of sequence

HADDOCK 2.4 peptide-MHC docking of convergent candidate **POLE** (DNA polymerase epsilon — identified independently in both datasets with different pathogens and HLA alleles):

| Complex | Bound RMSD | Sequence identity |
|---|---|---|
| E. coli Cdt / HLA-C*07:01 vs host POLE | 0.54 Å | 16.7% |
| C. jejuni NeuA / HLA-DRB1*07:01 vs host POLE | 1.47 Å | **0%** |

Structural equivalence is maintained in the MHC-bound state at zero sequence identity. A 100 ns GROMACS MD simulation confirmed thermodynamic stability of the host pMHC complex throughout the production run.

---

## Pipeline

The identical three-tier pipeline was applied to both datasets:

### STEP 1 — Pathogen & target selection
Systematic literature review
32 organisms (Auto) + 16 organisms (Neuro)

### STEP 2 — MHC epitope prediction
NetMHCpan 4.1 (Class I, 9-mer primary)
NetMHCIIpan 4.0 (Class II, 15-mer)
Threshold: %Rank EL ≤ 0.5, IC₅₀ ≤ 50 nM

### STEP 3 — Sequence homology filtering
BLASTp vs human proteome (BLOSUM80)
Retention: ≥50% identity, ≥90% query coverage

### STEP 4 — Structure extraction
PyMOL coordinate extraction from PDB / Swiss-Prot / AlphaFold
AlphaFold models quality-filtered by mean pLDDT ≥ 70

### STEP 5 — Structural validation
TM-align atomic superposition (Cα backbone)
RMSD < 2.0 Å → Confirmed mimic (Y)
RMSD ≥ 2.0 Å → Non-mimic (N)

### STEP 6 — Feature extraction
Sequence: BLOSUM80 score, identity %, alignment length,
identical residue count, coverage
Immunological: %Rank EL, binding affinity (nM)
Structural: RMSD, TM-score, alignment coverage %

### STEP 7 — Statistical analysis & ML
Pearson / Spearman correlations within mimic subsets
Three-layer Mann-Whitney U with Bonferroni correction
Fisher's z-test for cross-dataset generalisation
Random Forest classifier (Y vs N; Strong vs Weak)
Cross-pair validation, threshold sensitivity analysis

---

## Repository Structure:

molecular-mimicry-RA-pipeline/

│

├── MimicryDB-Auto/                         # Rheumatic diseases + GBS dataset

│   ├── data/

│   │   ├── ML_targets_final.csv            # labelled dataset (399 pairs)

│   │   └── data_description.md             # column definitions

│   ├── notebooks/

│   │   ├── 01_data_loading_cleaning.ipynb

│   │   ├── 02_statistical_analysis.ipynb

│   │   ├── 03_ml_model.ipynb

│   │   └── 04_figures.ipynb

│   ├── figures/                            # publication-quality figures

│   └── Results/

│       └── KEY_RESULTS.txt

│

├── MimicryDB-Neuro/                        # Neurodegenerative diseases dataset

│   ├── data/

│   │   ├── ML_targets_neuro.csv            # labelled dataset (247 pairs)

│   │   └── data_description.md

│   ├── notebooks/

│   │   ├── 01_data_loading_cleaning.ipynb

│   │   ├── 02_statistical_analysis.ipynb

│   │   ├── 03_ml_model.ipynb

│   │   └── 04_figures.ipynb

│   ├── figures/

│   └── Results/

│       └── KEY_RESULTS.txt

│

├── README.md

└── requirements.txt

---

## Quickstart

```bash
# Clone the repository
git clone https://github.com/minbaku/molecular-mimicry-RA-pipeline.git
cd molecular-mimicry-RA-pipeline

# Install dependencies
pip install -r requirements.txt

# Run either dataset — notebooks are identical in structure
# Start with MimicryDB-Auto:
jupyter notebook MimicryDB-Auto/notebooks/01_data_loading_cleaning.ipynb

# Or MimicryDB-Neuro:
jupyter notebook MimicryDB-Neuro/notebooks/01_data_loading_cleaning.ipynb

# Run in order: 01 → 02 → 03 → 04
```

All notebooks can be opened directly in Google Colab via the badge at the top of each file.

---

## Publications & Preprints

**Research Article (Lead Author)**
Ilahi, M. (2026). MimicryDB-Auto: Structural Validation Reveals the Inadequacy of Sequence-Based Molecular Mimicry Screening in Autoimmunity.
- Zenodo: https://doi.org/10.5281/zenodo.19262132
- Preprints.org: https://doi.org/10.20944/preprints202603.2306.v1

**Review (Lead Author)**
Ilahi, M. (2026). Immunogenicity of Therapeutic Antibodies: Mechanisms, Prediction, and Mitigation Strategies in the Era of Personalized Biologics. *Revised post peer-review; submission to F1000Research pending.*
- Preprints.org: https://doi.org/10.20944/preprints202603.2174.v1

**Conference**
Poster — International Conference of Medical Biotechnology (ICoMB), Amity University, Noida, 2026.

---

## Citation

```bibtex
@software{ilahi2026mimicrydb,
  author    = {Minza Ilahi},
  title     = {MimicryDB-Auto: Structural Validation Reveals the Inadequacy of
               Sequence-Based Molecular Mimicry Screening in Autoimmunity},
  year      = {2026},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.19262752},
  url       = {https://doi.org/10.5281/zenodo.19262752}
}
```

---

## Author

**Minza Ilahi**
M.Tech Biotechnology, Guru Gobind Singh Indraprastha University, Delhi
Supervisor: Prof. Sayan Chatterjee, Bioinformatics Lab, GGSIPU

[![ORCID](https://img.shields.io/badge/ORCID-0009--0001--8883--5226-green)](https://orcid.org/0009-0001-8883-5226)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-minza--ilahi-blue)](https://www.linkedin.com/in/minza-ilahi-9b6aba222)

📧 ilahiminza@gmail.com

---

## Acknowledgements

- Supervisor: Prof. Sayan Chatterjee, University School of Biotechnology, GGSIPU Delhi
- Structural validation: TM-align (Zhang & Skolnick, 2005)
- Epitope prediction: NetMHCpan 4.1 / NetMHCIIpan 4.0 (Reynisson et al., 2020)
- Docking: HADDOCK 2.4 (Honorato et al., 2024)
- MD simulations: GROMACS 2023 (Abraham et al., 2015)

*M.Tech final year dissertation, Guru Gobind Singh Indraprastha University, Delhi, India (2024–2026).*
