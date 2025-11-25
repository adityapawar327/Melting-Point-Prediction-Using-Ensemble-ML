# 🧪 Melting Point Prediction

[![Kaggle](https://img.shields.io/badge/Kaggle-Competition-blue)](https://www.kaggle.com/competitions/melting-point)
[![Python](https://img.shields.io/badge/Python-3.8+-green)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

Ensemble Machine Learning solution for predicting melting points of organic compounds using SMILES molecular descriptors. This repository contains my solution for the [Kaggle Thermophysical Property: Melting Point](https://www.kaggle.com/competitions/melting-point) competition.

## 📋 Competition Overview

Predicting the **melting point** of organic molecules is a long-standing challenge in chemistry and chemical engineering. Melting point is critical for drug design, material selection, and process safety, yet experimental measurements are often costly, time-consuming, or unavailable.

### Goal
Build ML models that predict melting point (in Kelvin) for organic compounds given molecular descriptors.

### Evaluation Metric
**Mean Absolute Error (MAE)** - Lower is better.

## 📊 Dataset Description

| Split | Samples | Percentage |
|-------|---------|------------|
| Train | 2,662 | 80% |
| Test | 666 | 20% |
| **Total** | **3,328** | 100% |

### Files
- `train.csv`: Features (SMILES) + Target (Tm)
- `test.csv`: Features only, no target
- `sample_submission.csv`: Template with columns [id, Tm]

### Columns
- `id`: Unique identifier
- `SMILES`: Molecular string representation
- `Group 1..N`: Descriptor features
- `Tm`: Melting point in Kelvin (train only)

## 🛠️ Solution Approach

### Feature Engineering
Extensive molecular feature extraction using RDKit:

1. **Basic Descriptors**: MolWt, LogP, TPSA, HBond donors/acceptors, Rotatable bonds
2. **Ring Features**: Aromatic/Aliphatic/Saturated ring counts, Ring density
3. **Charge Features**: Gasteiger charges (mean, std, max, min)
4. **Fragment Counts**: Benzene, phenol, ester, ether, aldehyde, ketone, etc.
5. **Morgan Fingerprints**: 1024-bit with radius 3
6. **MACCS Keys**: 167-bit structural keys
7. **Interaction Features**: HBond capacity, flexibility, polarity index, etc.

**Total Features**: ~1,233 features after processing

### Model Architecture
Ensemble of three gradient boosting models with optimized hyperparameters:

| Model | n_estimators | learning_rate | max_depth |
|-------|--------------|---------------|----------|
| LightGBM | 2000 | 0.02 | 10 |
| XGBoost | 1500 | 0.03 | 8 |
| CatBoost | 1500 | 0.03 | 8 |

### Ensemble Strategy
- 5-Fold Cross Validation
- Optimal weight optimization using scipy.optimize
- Final weights: ~35% LightGBM, ~32% XGBoost, ~33% CatBoost

## 📈 Results

### Cross-Validation Scores (MAE)
| Model | CV MAE |
|-------|--------|
| LightGBM | 28.70 |
| XGBoost | 28.57 |
| CatBoost | 28.75 |
| **Ensemble** | **28.16** |

## 🚀 Quick Start

### Prerequisites
```bash
pip install rdkit pandas numpy scikit-learn lightgbm xgboost catboost optuna scipy joblib tqdm matplotlib seaborn
```

### Run the Solution
```python
# Clone the repository
git clone https://github.com/adityapawar327/melting-point-prediction.git
cd melting-point-prediction

# Run the notebook
jupyter notebook ensemble-ml-for-melting-point-prediction.ipynb
```

## 📁 Project Structure
```
melting-point-prediction/
├── README.md
├── LICENSE
├── .gitignore
├── ensemble-ml-for-melting-point-prediction.ipynb  # Main solution notebook
└── submission.csv  # Final predictions
```

## 🔧 Key Dependencies

- `rdkit` - Molecular descriptor calculation
- `pandas`, `numpy` - Data manipulation
- `scikit-learn` - ML utilities
- `lightgbm` - LightGBM model
- `xgboost` - XGBoost model
- `catboost` - CatBoost model
- `optuna` - Hyperparameter optimization
- `scipy` - Weight optimization
- `matplotlib`, `seaborn` - Visualization

## 📝 Key Techniques

1. **SMILES Canonicalization**: Standardizing molecular representations
2. **Comprehensive Feature Engineering**: 1200+ molecular features
3. **Ensemble Learning**: Combining multiple gradient boosting models
4. **Optimal Weight Finding**: Using scipy optimization for ensemble weights
5. **5-Fold Cross Validation**: Robust model evaluation

## 🏆 Competition Link

[Kaggle: Thermophysical Property - Melting Point](https://www.kaggle.com/competitions/melting-point)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Aditya Pawar**
- GitHub: [@adityapawar327](https://github.com/adityapawar327)
- Kaggle: [Profile](https://www.kaggle.com/)

## 🙏 Acknowledgments

- Kaggle for hosting the competition
- RDKit developers for the excellent cheminformatics library
- The machine learning community for open-source implementations

---
⭐ If you found this helpful, please star the repository!
