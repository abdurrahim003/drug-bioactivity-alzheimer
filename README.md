# Drug Bioactivity Prediction for Alzheimer's Disease Target

> **Machine learning classification of drug bioactivity against Acetylcholinesterase (AChE) — a key therapeutic target for Alzheimer's disease — using molecular descriptors computed from SMILES.**

[![Kaggle Notebook](https://img.shields.io/badge/Kaggle-Notebook-blue?logo=kaggle)](https://www.kaggle.com/code/abdurrahim003/drug-bioactivity-prediction-for-alzheimer-s-target)
[![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## Table of Contents

- [Overview](#overview)
- [Why This Matters (Chemistry Context)](#why-this-matters-chemistry-context)
- [Dataset](#dataset)
- [Methods](#methods)
- [Results](#results)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Limitations and Future Work](#limitations-and-future-work)
- [Author](#author)

---

## Overview

Alzheimer's disease is a progressive neurodegenerative disorder affecting millions of people worldwide. One of the main treatment strategies targets the enzyme **Acetylcholinesterase (AChE)**, which breaks down the neurotransmitter acetylcholine in the brain. By inhibiting AChE, we can increase acetylcholine levels and improve cognitive function.

This project uses **machine learning** to predict whether a molecule is **active** or **inactive** against AChE, based on its chemical structure. This kind of prediction can help researchers screen thousands of candidate molecules quickly before spending time and money on lab experiments.

### What This Project Does

1. Downloads bioactivity data for AChE from the **ChEMBL database** (a large public database of drug-like molecules)
2. Converts IC50 activity values to **pIC50** (a standardized measure of potency)
3. Classifies molecules as **Active** (pIC50 ≥ 6.3) or **Inactive** (pIC50 < 5.0)
4. Computes **molecular descriptors** from SMILES using RDKit
5. Trains **Random Forest** and **Gradient Boosting** classifiers
6. Evaluates models with accuracy, precision, recall, F1-score, and ROC-AUC
7. Analyzes results with **Lipinski's Rule of Five** for drug-likeness

---

## Why This Matters (Chemistry Context)

| Concept | Explanation |
|---------|-------------|
| **Acetylcholinesterase (AChE)** | An enzyme that breaks down the neurotransmitter acetylcholine. In Alzheimer's, inhibiting this enzyme is a primary treatment strategy (e.g., Donepezil, Rivastigmine). |
| **IC50** | The concentration of a drug needed to inhibit 50% of the enzyme activity. Lower IC50 = more potent drug. |
| **pIC50** | Negative log of IC50 in molar units: pIC50 = -log10(IC50). Higher pIC50 = more potent. Converts wide-range IC50 values into a manageable scale. |
| **SMILES** | A text notation that encodes molecular structure (e.g., CCO = ethanol, c1ccccc1 = benzene). |
| **Lipinski's Rule of Five** | A guideline for drug-likeness: MW ≤ 500, LogP ≤ 5, H-bond donors ≤ 5, H-bond acceptors ≤ 10. Molecules obeying these rules are more likely to be orally absorbed. |

---

## Dataset

- **Source:** [ChEMBL Database](https://www.ebi.ac.uk/chembl/) — Target: Human Acetylcholinesterase (CHEMBL220)
- **Original size:** 5,638 unique molecules with IC50 data
- **After cleaning:** 3,648 molecules (removed duplicates, invalid SMILES, and intermediate-activity compounds)
- **Classes:**
  - **Active** (pIC50 ≥ 6.3): **1,975 molecules** (54.1%)
  - **Inactive** (pIC50 < 5.0): **1,673 molecules** (45.9%)
- **License:** ChEMBL data is available under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/)

### Data Cleaning Steps

1. Removed duplicate SMILES (kept median IC50 for duplicates)
2. Converted IC50 from nM to Molar, then to pIC50
3. Removed molecules with intermediate activity (5.0 ≤ pIC50 < 6.3) to create clear active/inactive classes
4. Removed molecules with invalid SMILES (could not be parsed by RDKit)

---

## Methods

### Molecular Descriptors (Features)

Ten physicochemical descriptors were computed from each molecule's SMILES using **RDKit**:

| Descriptor | What It Measures |
|-----------|-----------------|
| **MolWt** | Molecular weight (Da) — size of the molecule |
| **MolLogP** | Partition coefficient — how lipophilic (fat-soluble) the molecule is |
| **NumHDonors** | Number of hydrogen bond donors |
| **NumHAcceptors** | Number of hydrogen bond acceptors |
| **TPSA** | Topological Polar Surface Area — relates to membrane permeability |
| **NumRotatableBonds** | Rotational flexibility of the molecule |
| **NumAromaticRings** | Number of aromatic (benzene-like) rings |
| **HeavyAtomCount** | Number of non-hydrogen atoms |
| **RingCount** | Total number of rings in the molecule |
| **FractionCSP3** | Fraction of sp3 carbons — indicates 3D complexity |

### Machine Learning Pipeline

- **Train set:** 2,918 molecules (80%)
- **Test set:** 730 molecules (20%)
- **Random seed:** 42 (for reproducibility)
- **Models used:**
  - Random Forest Classifier (100 trees)
  - Gradient Boosting Classifier

---

## Results

### Model Performance on Test Set

| Metric | Random Forest | Gradient Boosting |
|--------|:------------:|:-----------------:|
| **Accuracy** | **0.8712** | 0.7932 |
| **Precision** | **0.8772** | 0.8020 |
| **Recall** | **0.8861** | 0.8300 |
| **F1-Score** | **0.8816** | 0.8158 |
| **ROC-AUC** | **0.9462** | 0.8797 |

### Cross-Validation (5-Fold)

| Metric | Mean ± Std |
|--------|-----------|
| Accuracy | 0.7138 ± 0.0182 |
| ROC-AUC | 0.7779 ± 0.0210 |

> **Note:** The gap between test set performance and cross-validation suggests some overfitting. The model generalizes reasonably but has room for improvement.

### Feature Importance (Random Forest)

The model identified these molecular properties as most important for predicting AChE activity:

| Rank | Feature | Importance |
|------|---------|-----------|
| 1 | MolLogP | High |
| 2 | TPSA | High |
| 3 | MolWt | Medium |
| 4 | HeavyAtomCount | Medium |
| 5 | NumAromaticRings | Medium |

### Lipinski Drug-Likeness Analysis

| Class | Drug-Like (%) | Interpretation |
|-------|:------------:|---------------|
| Active | 73.5% | Most active molecules follow drug-like rules |
| Inactive | 89.7% | Inactive molecules also tend to be drug-like — activity depends on more than just drug-likeness |

---

## Key Findings

1. **Random Forest outperformed Gradient Boosting** on all metrics, with ROC-AUC of 0.9462 (excellent discrimination between active and inactive molecules).

2. **Lipophilicity (MolLogP) and polar surface area (TPSA) are the most important features** — this makes chemical sense because AChE's active site has both hydrophobic and polar regions.

3. **Drug-likeness alone does not determine activity** — 89.7% of inactive molecules are also drug-like. The specific molecular interactions with AChE matter more than general drug-like properties.

4. **Cross-validation shows the model is honest but not perfect** — the CV accuracy (71.4%) is lower than test accuracy (87.1%), suggesting moderate overfitting. More data and advanced features (e.g., fingerprints) could help.

5. **pIC50 conversion standardizes potency comparison** — instead of dealing with IC50 values spanning 0.001 nM to 100,000 nM, pIC50 puts everything on a 0-12 scale.

---

## Project Structure

> **Note:** The full interactive notebook with all outputs is available on [Kaggle](https://www.kaggle.com/code/abdurrahim003/drug-bioactivity-prediction-for-alzheimer-s-target).

---

## How to Run

### Option 1: Run on Kaggle (Recommended)

1. Open the notebook: [Drug Bioactivity Prediction on Kaggle](https://www.kaggle.com/code/abdurrahim003/drug-bioactivity-prediction-for-alzheimer-s-target)
2. Click **"Copy & Edit"** to create your own copy
3. Make sure **Internet is ON** in notebook settings (needed for ChEMBL data download)
4. Click **"Run All"** to execute all cells

### Option 2: Run Locally

```bash
# Clone this repository
git clone https://github.com/abdurrahim003/drug-bioactivity-alzheimer.git
cd drug-bioactivity-alzheimer

# Install dependencies
pip install -r requirements.txt

# Open the notebook
jupyter notebook drug-bioactivity-prediction-for-alzheimer-s-target.ipynb
```

### Requirements

- Python 3.8+
- pandas, numpy, matplotlib, seaborn
- scikit-learn
- rdkit
- chembl_webresource_client (for downloading data from ChEMBL)

---

## Limitations and Future Work

### Current Limitations

- **Only 10 2D descriptors used** — molecular fingerprints (e.g., Morgan/ECFP) could capture more structural information
- **No 3D features** — the shape and 3D orientation of molecules affect how they bind to AChE
- **Binary classification only** — a regression model predicting exact pIC50 values would be more informative
- **Moderate overfitting** — the gap between test and CV performance suggests the model may not generalize well to completely new chemical series

### Possible Improvements

- Add **Morgan fingerprints** (2048-bit) for richer structural representation
- Try **XGBoost** or **neural network** models
- Use **SMOTE** or other techniques if class imbalance worsens with different thresholds
- Include **3D descriptors** (if 3D conformers are generated)
- Build a **regression model** for continuous pIC50 prediction
- Expand to **multi-target prediction** (e.g., AChE + BuChE selectivity)

---

## Author

**Abdur Rahim**
- B.Sc. in Chemistry, Gopalganj Science and Technology University (GSTU), Bangladesh
- CGPA: 3.53 / 4.00
- Email: abdurrahimche003@gmail.com
- Kaggle: [abdurrahim003](https://www.kaggle.com/abdurrahim003)
- GitHub: [abdurrahim003](https://github.com/abdurrahim003)

### Other ML Projects

| # | Project | Type | Link |
|---|---------|------|------|
| 1 | Molecular Solubility Prediction | Regression | [Kaggle](https://www.kaggle.com/code/abdurrahim003/molecular-solubility-prediction-from-molecular-des) |
| 2 | Water Quality Prediction | Classification | [Kaggle](https://www.kaggle.com/code/abdurrahim003/water-quality-prediction-safe-or-unsafe) |
| 3 | Melting Point Prediction | Regression | [Kaggle](https://www.kaggle.com/code/abdurrahim003/predicting-melting-point-using-machine-learning) |
| 4 | Drug Bioactivity for Alzheimer's | Classification | [Kaggle](https://www.kaggle.com/code/abdurrahim003/drug-bioactivity-prediction-for-alzheimer-s-target) |

---

## References

- Gaulton, A., et al. (2017). The ChEMBL database in 2017. *Nucleic Acids Research*, 45(D1), D945-D954.
- Lipinski, C.A., et al. (1997). Experimental and computational approaches to estimate solubility and permeability in drug discovery and development settings. *Advanced Drug Delivery Reviews*, 23(1-3), 3-25.
- RDKit: Open-source cheminformatics. [https://www.rdkit.org/](https://www.rdkit.org/)

---

*This project was developed as part of a machine learning portfolio for chemistry research applications.*
