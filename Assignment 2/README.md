# Assignment 2 — QSAR Data Curation

> Curating a clean bioactivity dataset for QSAR modeling from ChEMBL

---

## Target Information

| Property | Value |
|----------|-------|
| **Target** | Acetylcholinesterase (AChE) |
| **ChEMBL ID** | CHEMBL220 |
| **Disease** | Alzheimer's Disease |
| **Bioactivity** | IC50 (nM) |

---

## Workflow

1. Search target in ChEMBL
2. Retrieve IC50 data
3. Remove missing values
4. Standardize units (nM)
5. Classify bioactivity (Active / Intermediate / Inactive)
6. Remove invalid SMILES
7. Remove duplicates
8. Save final CSV

---

## Bioactivity Classification

| Class | Criterion |
|-------|-----------|
| **Active** | IC50 ≤ 1,000 nM |
| **Intermediate** | 1,000–10,000 nM |
| **Inactive** | IC50 ≥ 10,000 nM |

---

## Files

```
Assignment 2/
├── data/
│   ├── bioactivity_raw_data.csv            # Raw ChEMBL download
│   └── bioactivity_preprocessed_data.csv   # Cleaned & classified
└── notebooks/
    └── Assignment_2_QSAR_data_curation.ipynb
```

---

## How to Run

```bash
cd "Assignment 2"
jupyter notebook notebooks/Assignment_2_QSAR_data_curation.ipynb
```
