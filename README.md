
> **Note:** The full interactive notebook with all outputs is available on [Kaggle](https://www.kaggle.com/code/abdurrahim003/predicting-melting-point-using-machine-learning).

---

## How to Run

### Option 1: Run on Kaggle (Recommended)

1. Open the notebook: [Melting Point Prediction on Kaggle](https://www.kaggle.com/code/abdurrahim003/predicting-melting-point-using-machine-learning)
2. Click **"Copy & Edit"** to create your own copy
3. Make sure **Internet is ON** in notebook settings (needed to install RDKit)
4. Click **"Run All"** to execute all cells

### Option 2: Run Locally

```bash
# Clone this repository
git clone https://github.com/abdurrahim003/melting-point-prediction.git
cd melting-point-prediction

# Install dependencies
pip install -r requirements.txt

# Open the notebook
jupyter notebook predicting-melting-point-using-machine-learning.ipynb
```

### Requirements

- Python 3.8+
- pandas, numpy, matplotlib, seaborn
- scikit-learn
- rdkit

---

## Limitations and Future Work

### Current Limitations

- **Only 8 2D descriptors used** — molecular fingerprints (e.g., Morgan/ECFP) could capture more structural patterns
- **No 3D features** — melting point fundamentally depends on 3D crystal packing, which 2D descriptors cannot capture
- **Average error of 34°C** — useful for rough screening, but not precise enough for compound identification
- **Dataset limited to organic molecules** — results may not apply to organometallics, salts, or polymers

### Possible Improvements

- Add **Morgan fingerprints** (2048-bit) for richer structural information
- Include **3D descriptors** if 3D conformers are generated
- Try **XGBoost** or **neural network** models
- Use a **larger dataset** with more diverse molecules
- Add **crystal packing features** if available (e.g., from Cambridge Structural Database)
- Combine with **group contribution methods** for better accuracy

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
| 4 | Drug Bioactivity for Alzheimer's | Classification | [Kaggle](https://www.kaggle.com/code/abdurrahim003/drug-bioactivity-prediction-for-alzheimer-s-target) · [GitHub](https://github.com/abdurrahim003/drug-bioactivity-alzheimer) |

---

## References

- Bradley, J.-C., Lang, A., & Williams, A. (2014). Jean-Claude Bradley Open Melting Point Dataset (DoublePlusGood). *Figshare*. doi:10.6084/m9.figshare.1031637
- RDKit: Open-source cheminformatics. [https://www.rdkit.org/](https://www.rdkit.org/)

---

*This project was developed as part of a machine learning portfolio for chemistry research applications.*
