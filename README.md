# M115 Lab 1 — Phishing URL Detection

**EDA · Parameterized Logistic Regression · SHAP Explainability**

ENSIASD Taroudant — Ibn Zohr University
Master in Intelligent Systems and Cyber Defense — Module M115 (AI & Intelligent Systems)
Instructor: Prof. Sara EL-ATEIF · Student: **GHADOUF ALEM** · 2026–2027

---

## Overview

A SOC-oriented phishing URL classifier built on the [PhiUSIIL Phishing URL Dataset](https://www.kaggle.com/datasets/kaggleprollc/phishing-url-websites-dataset-phiusiil/data) (235,795 URLs, 56 lexical/network/HTML features). The lab covers the full pipeline: data exploration, leakage-free preprocessing, interpretable Logistic Regression, SHAP explanations, and operational threshold tuning.

## Notebook Structure

| Part | Content |
|------|---------|
| **1. EDA** | Data quality checks, label flip (`Phishing = 1`), class balance, binary indicators, feature–target correlations |
| **2. Preprocessing** | Stratified 75/25 split, `StandardScaler` fit on training data only |
| **3. Logistic Regression** | 5 configurations (unregularized, L2, L1, class-weighted), L1 regularization path, odds ratios |
| **4. Explainability** | Global SHAP (bar + beeswarm), local waterfall plots (TP / TN / FP), threshold tuning (τ = 0.20 / 0.50 / 0.85) |
| **5. Synthesis** | SOC engineering recommendations |

## Key Results

- **Dataset:** 42.81% phishing / 57.19% legitimate; no missing values or duplicates.
- **Best trade-off — M4 (L1, C=0.01):** Precision 0.9993, Recall 0.9971, PR-AUC 1.0, while pruning **24 of 49 features**.
- **Top phishing indicators:** `LetterRatioInURL` (OR ≈ 112), `DegitRatioInURL`, `SpacialCharRatioInURL`.
- **Top legitimate indicators:** `IsHTTPS` (OR ≈ 0.001), `NoOfJS`, `NoOfSelfRef`, `HasSocialNet`.
- **Insight:** ~49% of phishing sites use HTTPS — "HTTPS = safe" is not a valid rule.

### Recommended SOC Threshold Policy

| P(Phishing) | Action | Result |
|-------------|--------|--------|
| ≥ 0.85 | Automatic block | 0 false positives |
| 0.20 – 0.85 | Analyst review queue | Small manual workload |
| < 0.20 | Allow and log | 15 missed detections |

## How to Run

```bash
pip install numpy pandas matplotlib seaborn scikit-learn shap
```

1. Download `PhiUSIIL_Phishing_URL_Dataset.csv` from Kaggle and place it next to the notebook.
2. Open `M115_Lab1_Template.ipynb` in Jupyter / Colab and run all cells (`SEED = 42`).

## Tech Stack

Python · pandas · NumPy · scikit-learn · SHAP · Matplotlib · Seaborn
