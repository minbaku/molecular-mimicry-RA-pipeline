# Dataset Description

## File: ML_targets_final.csv

## Overview
399 peptide pairs spanning 32 RA-associated organisms.
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
- Y (Confirmed Mimic): Structural RMSD < 2.0 Å
- N (Non-Mimic): Structural RMSD >= 2.0 Å

## Class Distribution
- Confirmed Mimics (Y): 262
- Non-Mimics (N): 137
- Total: 399

## Missing Values
- BLOSUM-derived features are NaN for negative examples
  (negatives were not processed through BLAST alignment)
- 1 missing TM-score value

## Notes
Negative examples were constructed by cross-pairing pathogen and human 
peptides from the same pre-filtered pool. See manuscript Methods section 
for full details and associated limitations.
