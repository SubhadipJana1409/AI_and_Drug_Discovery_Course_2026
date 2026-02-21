# Assignment 3 — Summary Report
**Course:** Computational Drug Discovery  
**Dataset:** ChEMBL Bioactivity Dataset (`bioactivity_preprocessed_data.csv`)  
**Total Compounds:** 6,843  
**Target:** Acetylcholinesterase inhibitors (from ChEMBL)

---

## Task 1: Key EDA Findings

### pIC50 Descriptive Statistics

> pIC50 = −log₁₀(IC50 in Molar) = 9 − log₁₀(IC50 in nM)

| Statistic | Value |
|-----------|-------|
| **Count** | 6,843 |
| **Mean** | **5.797** |
| **Std Dev** | **1.543** |
| **Min** | **1.305** |
| 25th Percentile | 4.777 |
| **Median** | **5.602** |
| 75th Percentile | 6.770 |
| **Max** | **14.301** |

### Key EDA Observations

**1. Distribution Shape:**  
The pIC50 distribution is roughly bell-shaped but right-skewed, with a long tail toward high activity. The median (5.60) is slightly below the mean (5.80), confirming the right skew. The wide standard deviation (1.54) indicates high chemical diversity in the dataset.

**2. Bioactivity Class Breakdown:**

| Class | Count | Percentage |
|-------|-------|-----------|
| **Active** (pIC50 ≥ 6.0) | 2,816 | 41.2% |
| **Inactive** (pIC50 ≤ 5.0) | 2,264 | 33.1% |
| **Intermediate** (5 < pIC50 < 6) | 1,763 | 25.8% |

**3. Class Balance:**  
The dataset is moderately balanced with a slight excess of active compounds. This is typical of ChEMBL bioactivity datasets — it is more balanced than many real-world drug discovery screens.

**4. Density Plot Findings:**  
Clear separation exists between active and inactive class distributions. Active compounds peak near pIC50 = 7, inactive near pIC50 = 4. The intermediate class bridges both distributions as expected.

**5. Notable Outlier:**  
Max pIC50 of 14.30 corresponds to an IC50 of ~0.05 pM — an extraordinarily potent compound that warrants validation. Values below pIC50 2.0 (~10 mM IC50) may indicate data quality issues.

### Visualizations (Task 1)
| Figure | Description |
|--------|-------------|
| `task1_pIC50_distribution.png` | Histogram (50 bins) + KDE by class |
| `task1_bioactivity_counts.png` | Bar chart + pie chart of class counts |
| `task1_boxplots.png` | Box plot + violin plot by class |

---

## Task 2: Lipinski & 2D Descriptor Statistics

**Tool used: RDKit** (open-source, Python-native cheminformatics)

### 2D Descriptor Summary

| Descriptor | Mean | Std | Min | Max | Lipinski Limit |
|-----------|------|-----|-----|-----|----------------|
| **MW (Da)** | 422.4 | 122.9 | 62.6 | 800.0 | ≤ 500 |
| **LogP** | 2.48 | 2.10 | −5.0 | 10.0 | ≤ 5 |
| **HBD** | 1.11 | 0.79 | 0 | 7 | ≤ 5 |
| **HBA** | 5.28 | 2.19 | 0 | 15 | ≤ 10 |
| **TPSA (Å²)** | 74.8 | 36.2 | 0 | 200 | — |
| **RotBonds** | 3.20 | 1.87 | 0 | 15 | — |
| **QED** | 0.67 | 0.18 | 0.10 | 1.0 | — |

### Lipinski Ro5 Key Findings

- The dataset mean MW (422 Da) is comfortably below the 500 Da limit — compounds are drug-sized
- Mean LogP (2.48) is well within the ≤5 limit, indicating reasonable lipophilicity
- Low mean HBD (1.11) and moderate HBA (5.28) are typical for drug-like CNS-penetrant compounds (important for AChE inhibitors targeting Alzheimer's disease)
- ~95% of compounds pass Lipinski Ro5 (≤1 violation)
- Active compounds tend to have slightly **lower MW and TPSA** compared to inactive ones, consistent with better CNS penetration (BBB crossing) requirements
- Mean QED of 0.67 indicates the dataset contains drug-like, optimized compounds

### Descriptor Interpretation for AChE Inhibitors
TPSA < 90 Å² favors BBB permeability (critical for CNS targets like AChE). The mean TPSA of 74.8 Å² suggests the dataset is appropriately enriched with CNS-penetrant compounds.

### Visualizations (Task 2)
| Figure | Description |
|--------|-------------|
| `task2_descriptor_distributions.png` | 6-panel descriptor histograms by class |
| `task2_lipinski_compliance.png` | Pass/Fail bar + violation count distribution |
| `task2_correlation_heatmap.png` | Pearson correlation matrix of all descriptors |

---

## Task 3: Molecular Fingerprints

### Method: MACCS Keys (166-bit)

**Tool:** `RDKit` — `rdkit.Chem.MACCSkeys.GenMACCSKeys()`  
**Output:** Binary matrix of shape **6,843 × 166**

### Why MACCS Keys?

| Criterion | Reasoning |
|----------|-----------|
| **Interpretability** | Each of the 166 bits corresponds to a named structural feature (e.g., bit 160 = aromatic ring). Unlike hashed fingerprints (Morgan/ECFP), MACCS bits can be chemically explained. |
| **Standardization** | SMARTS-based keys — same bit always encodes the same feature across all tools and publications. |
| **Appropriate for dataset size** | 166 bits is compact. With 6,843 compounds, using 2048-bit ECFP would risk sparsity and dimensionality issues in downstream ML models. |
| **Open source** | Fully available in RDKit — no commercial license required. |
| **Validated** | Extensively used in QSAR, scaffold analysis, and virtual screening literature over 30+ years. |
| **Tanimoto similarity** | Tanimoto similarity on MACCS keys gives good scaffold-level compound matching, well-validated for hit-finding. |

### Fingerprint Analysis Results

- **Average bits set per compound:** ~30–35 of 166 (bit density ~20%)
- **Active compounds:** Highest average bits set — reflecting structural complexity of potent inhibitors
- **Inactive compounds:** Fewest bits set — simpler, less decorated scaffolds
- **Tanimoto similarity (same class):** Higher than cross-class, confirming MACCS keys capture meaningful structural differences between active and inactive compounds
- **Top frequent bits:** Bits in the 140–166 range (aromatic features, ring systems, common functional groups) are most frequent — consistent with drug-like aromatic scaffolds

### Visualizations (Task 3)
| Figure | Description |
|--------|-------------|
| `task3_maccs_analysis.png` | 4-panel: bit frequency, class frequency, Tanimoto, bits/compound |
| `task3_maccs_heatmap.png` | Heatmap of top 40 bits × first 100 compounds |

---

## Complete File Inventory

| File | Type | Description |
|------|------|-------------|
| `data/bioactivity_dataset.csv` | CSV | Raw dataset + pIC50 (6,843 × 5) |
| `data/descriptors_2D.csv` | CSV | Dataset + 9 RDKit 2D descriptors (6,843 × 16) |
| `data/maccs_fingerprints.csv` | CSV | MACCS 166-bit fingerprint matrix (6,843 × 169) |
| `notebooks/Task1_EDA.ipynb` | Jupyter | EDA notebook (reproducible) |
| `notebooks/Task2_Descriptors.ipynb` | Jupyter | Descriptor calculation notebook |
| `notebooks/Task3_Fingerprints.ipynb` | Jupyter | Fingerprint generation notebook |
| `figures/task1_pIC50_distribution.png` | PNG | Histogram + density |
| `figures/task1_bioactivity_counts.png` | PNG | Bar + pie chart |
| `figures/task1_boxplots.png` | PNG | Box + violin plots |
| `figures/task2_descriptor_distributions.png` | PNG | 6-panel descriptor histograms |
| `figures/task2_lipinski_compliance.png` | PNG | Lipinski Ro5 pass/fail |
| `figures/task2_correlation_heatmap.png` | PNG | Descriptor correlation matrix |
| `figures/task3_maccs_analysis.png` | PNG | 4-panel fingerprint analysis |
| `figures/task3_maccs_heatmap.png` | PNG | MACCS bit heatmap |
| `reports/Task4_Summary_Report.md` | Markdown | This report |
| `README.md` | Markdown | GitHub repository guide |

---

*Report generated for Assignment 3 — Computational Drug Discovery*
