# MimicryDB-Auto: Key Results

## Dataset Summary

| Metric | Value |
|--------|-------|
| Total pathogen-host pairs | 399 |
| Confirmed mimics (Y, RMSD < 2.0 Å) | 262 |
| Non-mimics (N, RMSD ≥ 2.0 Å) | 137 |
| Strong mimics (RMSD < 1.0 Å) | 159 |
| Weak mimics (1.0–2.0 Å) | 103 |

## Primary Finding: Sequence-Structure Correlation

| Threshold | n | r | p-value | R² |
|-----------|---|----|---------|-----|
| 2.0 Å | 272 | -0.761 | 0.0000 | 0.579 |
| 1.0 Å | 159 | -0.046 | 0.5624 | 0.002 |

## Random Forest Classifier Performance

### Y vs N (2.0 Å threshold)
- Test AUC: 0.958
- 5-fold CV AUC: 0.979 ± 0.018

### Strong vs Weak Mimics (1.0 Å threshold)
- Test AUC: 0.841
- 5-fold CV AUC: 0.823 ± 0.053
- AUC drop (categorical inflation): 0.117

## Mann-Whitney U Tests (Y vs N)

| Feature | p-value | Interpretation |
|---------|---------|----------------|
| BLOSUM80 raw score | 0.0000 | Reflects class construction |
| Identity percentage | 0.0000 | Reflects class construction |
| Alignment coverage | 0.0000 | Reflects class construction |

## Key Interpretations

1. **Sequence identity explains at most 1.6% of variance** in structural RMSD (R² = 0.016)
2. **Structural mimicry is the modal outcome** (91.9% of random cross-pairs achieve RMSD < 2.0 Å)
3. **99.2% of structurally equivalent cross-pairs** have zero detectable sequence similarity
4. **Multivariate sequence signal exists** but is insufficient without structural validation

## Repository Contents

- `MimicryDB-Auto.csv`: Full dataset (399 pairs)
- `analysis_notebook.ipynb`: Complete reproducible analysis
- `README.md`: Repository overview and instructions
- `KEY_RESULTS.md`: This file
