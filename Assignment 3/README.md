# Assignment 3 — Computational Drug Discovery

> Exploratory Data Analysis, Molecular Descriptors, and Fingerprint Generation on **6,843 ChEMBL Bioactivity Compounds**

---

## Files

```
Assignment 3/
├── data/
│   ├── bioactivity_dataset.csv        # Raw ChEMBL dataset + pIC50 (6,843 compounds)
│   ├── descriptors_2D.csv             # Lipinski + 9 extended RDKit 2D descriptors
│   └── maccs_fingerprints.csv         # 166-bit MACCS key fingerprint matrix
├── notebooks/
│   ├── Task1_EDA.ipynb                # Exploratory Data Analysis
│   ├── Task2_Descriptors.ipynb        # 2D Molecular Descriptor Calculation
│   └── Task3_Fingerprints.ipynb       # MACCS Key Fingerprint Generation
├── figures/
│   ├── task1_pIC50_distribution.png
│   ├── task1_bioactivity_counts.png
│   ├── task1_boxplots.png
│   ├── task2_descriptor_distributions.png
│   ├── task2_lipinski_compliance.png
│   ├── task2_correlation_heatmap.png
│   ├── task3_maccs_analysis.png
│   └── task3_maccs_heatmap.png
├── reports/
│   └── Task4_Summary_Report.md        # Written summary of all tasks
└── README.md
```

---

## Dataset

- **Source:** ChEMBL (curated in Assignment 2)
- **Compounds:** 6,843
- **Columns:** `chembl_id`, `smiles`, `standard_value` (IC50 in nM), `pIC50`, `bioactivity_class`
- **Classes:** Active (pIC50 ≥ 6.0) = 2,816 | Inactive (≤ 5.0) = 2,264 | Intermediate = 1,763

---

## Tasks at a Glance

| Task | Topic | Key Output |
|------|-------|-----------|
| **1** | EDA — pIC50 statistics, histogram, bar plot | 3 figures |
| **2** | 2D Descriptors — Lipinski Ro5, RDKit, correlation | `descriptors_2D.csv` + 3 figures |
| **3** | Fingerprints — MACCS Keys 166-bit | `maccs_fingerprints.csv` + 2 figures |
| **4** | Summary report | `Task4_Summary_Report.md` |

---

## Setup

### Option 1: conda (recommended — includes RDKit)
```bash
conda create -n drug_discovery python=3.10
conda activate drug_discovery
conda install -c conda-forge rdkit pandas matplotlib seaborn scipy tqdm jupyter
```

### Option 2: pip
```bash
pip install rdkit pandas matplotlib seaborn scipy tqdm notebook
```

---

## How to Run

```bash
cd "Assignment 3"
jupyter notebook
```

Run notebooks in order:
1. `notebooks/Task1_EDA.ipynb`
2. `notebooks/Task2_Descriptors.ipynb`
3. `notebooks/Task3_Fingerprints.ipynb`

> **Note:** Each notebook auto-detects RDKit. If not installed, it loads pre-computed CSVs from `data/` automatically — no manual steps needed.

---

## Tools & Libraries

| Tool | Version | Purpose |
|------|---------|---------|
| [RDKit](https://www.rdkit.org/) | ≥ 2022.09 | Descriptors & fingerprints |
| pandas | ≥ 1.5 | Data handling |
| matplotlib | ≥ 3.6 | Plotting |
| seaborn | ≥ 0.12 | Statistical visualization |
| numpy | ≥ 1.23 | Numerical computation |
