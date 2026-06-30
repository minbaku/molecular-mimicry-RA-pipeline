# Dataset Description

## File: MimicryDB-Auto.csv

## Overview
399 pathogen-host peptide pairs spanning 32 organisms implicated in autoimmune rheumatic diseases and Guillain-Barré syndrome.
Labels assigned based on TM-align structural validation (RMSD threshold 2.0 Å).

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
| Immunogenicity | float | Predicted T cell immunogenicity score |
| Type of MHC | text | MHC class I or class II |
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
| BLOSUM80_per_residue | float | BLOSUM80 score divided by alignment length — derived feature |
| Alignment_coverage_sequence | float | Sequence alignment coverage capped at 100% — derived feature |

## Label Assignment
- **Y (Confirmed Mimic)**: Structural RMSD < 2.0 Å
- **N (Non-Mimic)**: Structural RMSD ≥ 2.0 Å

## Class Distribution
- Confirmed Mimics (Y): 262
- Non-Mimics (N): 137
- Total: 399

## Strong vs Weak Mimic Subclassification
- **Strong Mimics**: RMSD < 1.0 Å (n = 159)
- **Weak Mimics**: 1.0 Å ≤ RMSD < 2.0 Å (n = 103)

## Missing Values
- BLOSUM-derived features and sequence alignment columns are zero-coded for N-class entries
  (negatives constructed by cross-pairing have no biologically meaningful BLAST signal)

## Key Findings (from analysis)
- Sequence identity explains at most 1.6% of variance in structural RMSD (r = -0.127, R² = 0.016)
- No individual sequence feature significantly discriminates mimics from non-mimics
- 91.9% of randomly cross-paired sequence-similar 9-aa pairs achieve RMSD < 2.0 Å
- 99.2% of structurally equivalent cross-pairs have zero detectable sequence similarity

## Notes
Negative examples (N class) were constructed by cross-pairing pathogen peptides with human
peptide matches drawn from the same pre-filtered pool of BLAST-confirmed sequence-similar
proteins, paired with a different pathogen peptide than their original match.
See manuscript Methods Section 2.5.5 for full details and associated limitations.

## Citation
If using this dataset, please cite:
Ilahi M. MimicryDB-Auto: Structural Validation Reveals the Inadequacy of
Sequence-Based Molecular Mimicry Screening in Autoimmunity. 2026.
Available at: https://github.com/minbaku/molecular-mimicry-RA-pipeline
