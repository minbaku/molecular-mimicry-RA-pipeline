# MimicryDB-Neuro: Key Results

*Neurodegenerative Diseases*

## Dataset Summary

| Metric | Value |
|--------|-------|
| Total pathogen-host pairs | 247 |
| Organisms | 16 |
| Disease contexts | AD, PD, HD, MS, and other CNS disorders |
| Confirmed mimics (Y, RMSD < 2.0 Å) | 170 (68.8%) |
| Non-mimics (N, RMSD ≥ 2.0 Å) | 77 |
| Strong mimics (RMSD < 1.0 Å) | 99 |
| Weak mimics (1.0–2.0 Å) | 71 |

## Primary Finding: Sequence-Structure Correlation

*Within-Y-class correlation only (genuine BLAST-matched mimics, BLOSUM80 score > 0).*
*The naive full-dataset correlation is not used as it includes zero-coded N-class entries,*
*which creates a spurious two-cluster artefact rather than a genuine relationship.*

| Threshold | n | r | p-value | R² |
|-----------|---|----|---------|-----|
| 2.0 Å (all confirmed mimics) | 170 | -0.127 | 0.0531 | 0.016 |
| 1.0 Å (strong mimics only) | 99 | -0.171 | 0.0907 | 0.029 |

## Random Forest Classifier Performance

### Y vs N (2.0 Å threshold)
- Test AUC: 1.000
- 5-fold CV AUC: 0.946 ± 0.042

### Strong vs Weak Mimics (1.0 Å threshold)
- Test AUC: 0.825
- 5-fold CV AUC: 0.788 ± 0.042
- AUC drop (categorical inflation): 0.175

## Mann-Whitney U Tests (Y vs N, Layer B — includes cross-paired zeros)

| Feature | p-value | Interpretation |
|---------|---------|----------------|
| BLOSUM80 raw score | 0.0000 | Reflects class construction |
| Identity percentage | 0.0000 | Reflects class construction |
| Alignment coverage | 0.0000 | Reflects class construction |

## Cross-Pair Validation

| RMSD Threshold | Cross-Pair Background Rate |
|----------------|------------------------------|
| 1.0 Å | 55.9% (76/136) — derived from Auto reference |
| 1.5 Å | 85.3% (116/136) — derived from Auto reference |
| 2.0 Å | 91.9% (125/136) — derived from Auto reference |

## Key Interpretations

1. **Sequence identity explains at most 1.6% of variance** in structural RMSD (R² = 0.016)
2. **Structural mimicry is the modal outcome** (91.9% (125/136) of random cross-pairs achieve RMSD < 2.0 Å)
3. **A substantial majority of structurally equivalent cross-pairs** have zero detectable sequence similarity
4. **Multivariate sequence signal exists** (AUC = 0.825) but is insufficient without structural validation

## Repository Contents

- `MimicryDB-Neuro.csv`: Full dataset (247 pairs)
- `data_description.md`: Column definitions and methodology
- Notebooks 01–04: Complete reproducible analysis
- `KEY_RESULTS_neuro.md`: This file
