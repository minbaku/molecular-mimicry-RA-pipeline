# Dataset Description

## Overview
247 pathogen-host peptide pairs spanning 16 organisms implicated in neurodegenerative diseases (Alzheimer's disease, Parkinson's disease, Huntington's disease, and other CNS disorders). Constructed using the identical three-tier pipeline applied to MimicryDB-Auto, enabling direct cross-dataset comparison. Labels assigned based on TM-align structural validation (RMSD threshold 2.0 Å).

## Columns

| Column | Type | Description |
|--------|------|-------------|
| Organism | text | Source pathogen organism |
| Protein | text | Pathogen protein name |
| Position | text | Epitope position in protein |
| HLA Haplotype | text | MHC allele for presentation |
| Pathogen Peptide | text | Pathogen-derived peptide sequence |
| pathogen length | integer | Length of pathogen peptide in amino acids |
| %Rank_EL(X) | float | NetMHCpan eluted ligand rank score — lower is stronger binder |
| Aff(nM)(X) | float | Predicted MHC binding affinity in nanomolar |
| Immunogenicity | text | Prediction tool used (NetMHCpan) |
| Type of MHC | text | T-cell presentation context (Cytotoxic T-cells = Class I; Helper T-cells = Class II) |
| Human_match | text | Matched human protein |
| BLOSUM80 score | float | Raw BLAST alignment score using BLOSUM80 matrix |
| Identity percentage | float | Percentage sequence identity between peptide pairs |
| Alignment length (Sequence) | integer | Number of aligned residue positions |
| Identical aa | integer | Number of identical amino acids in alignment |
| Positions | text | Alignment position range |
| Human Peptide | text | Matched human peptide sequence |
| Human length | integer | Length of human peptide in amino acids |
| Alignment Length (Structure) | integer | Number of structurally aligned residues |
| Structural RMSD | float | Root mean square deviation of aligned structures in Angstroms |
| TM-align score (Human chain 2) | float | TM-score normalised by human peptide length |
| Structural alignment coverage % | float | Percentage of residues structurally aligned |
| RMSD_Mimic_Target (Y) | text | Ground truth label — Y confirmed mimic RMSD < 2.0, N non-mimic RMSD >= 2.0 |
| BLOSUM80_per_residue | float | BLOSUM80 score divided by alignment length — derived feature (N/A for N-class) |
| Alignment_coverage_sequence | float | Sequence alignment coverage capped at 100% — derived feature |

## Label Assignment
- **Y (Confirmed Mimic)**: Structural RMSD < 2.0 Å
- **N (Non-Mimic)**: Structural RMSD ≥ 2.0 Å

## Class Distribution
- Confirmed Mimics (Y): 170
- Non-Mimics (N): 77
- Total: 247

## Strong vs Weak Mimic Subclassification
- **Strong Mimics**: RMSD < 1.0 Å (n = 99)
- **Weak Mimics**: 1.0 Å ≤ RMSD < 2.0 Å (n = 71)
- **Non-Mimics**: RMSD ≥ 2.0 Å (n = 77)

## Missing Values
- BLOSUM-derived and sequence alignment columns are zero- or N/A-coded for N-class entries (negatives constructed by cross-pairing have no biologically meaningful BLAST signal)

## Key Findings (from analysis)
- Sequence identity explains at most 2.2% of variance in structural RMSD (r = −0.149, p = 0.053, R² = 0.022) within confirmed mimics
- Strong-mimics-only Spearman correlation confirms the finding non-parametrically (rs = −0.163, p = 0.108)
- Mimic rate (68.8%) and strong mimic rate (39.9%) are statistically indistinguishable from MimicryDB-Auto (χ² p > 0.05), indicating the prevalence of structural mimicry is comparable across disease contexts
- Random Forest classifier (Strong vs Weak mimics) achieved AUC = 0.825 (5-fold CV AUC = 0.788 ± 0.042), closely mirroring the rheumatic dataset (AUC = 0.841)
- Cross-dataset comparison of Pearson correlations confirmed via Fisher's z-test (z = 0.686, p = 0.493) that the near-zero sequence-structure relationship generalises across disease contexts
- BLOSUM80 per residue was again the top-ranked predictive feature, consistent with MimicryDB-Auto

## Notes
Negative examples (N class) were constructed using the identical cross-pairing protocol applied to MimicryDB-Auto — pathogen peptides paired with human peptide matches drawn from the same pre-filtered pool of BLAST-confirmed sequence-similar proteins, reassigned to a different pathogen peptide than their original match. This dataset was constructed specifically to test whether the sequence-structure decoupling observed in autoimmune rheumatic disease generalises to CNS-implicated targets, motivated by the hypothesis of a microbial bridge linking peripheral autoimmunity to neurodegeneration.

See manuscript Methods Section 3.4–3.5 and Discussion Section 5.4 for full details, comparative analysis, and associated limitations.

## Citation
If using this dataset, please cite:

Ilahi M. MimicryDB-Auto: Structural Validation Reveals the Inadequacy of Sequence-Based Molecular Mimicry Screening in Autoimmunity. 2026. Available at: https://github.com/minbaku/molecular-mimicry-RA-pipeline
