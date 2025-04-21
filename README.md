# ⚡ Power Outage Cause Prediction (2000–2016)

This project analyzes major power outages in the U.S. from 2000 to 2016 using a dataset from [Purdue LASCI](https://engineering.purdue.edu/LASCI/research-data/outages). It uses machine learning to predict **cause** of each outage using features that were available to me at the **start** of the outage.

---

## 📌 Project Overview

- **Goal:** Predict cause of major power outages using data in real time
- **Type:** Multiclass classification using Logistic Regression and Random Forest classifier
- **Tools:** Python, pandas, scikit-learn, Plotly, GitHub Pages

---

## 🧠 Key Findings

| Model                 | Accuracy | Notes |
|----------------------|----------|-------|
| Logistic Regression  | ~82%     | High accuracy, poor recall on minority classes |
| Random Forest (tuned)| ~85%     | Strong F1 for dominant causes: *severe weather* and *intentional attack* |

**Engineered Features:**
- `DEMAND_LOSS_PER_CUSTOMER`
- `IS_HURRICANE`

---

## 🌐 Live Website

👉 [Project Website](https://kalwad.github.io/power-outage-analysis/)

Contains:
- 📊 Interactive Plotly charts
- 🔎 EDA and visual summaries
- 🧠 ML pipeline and final model
- ✅ Performance metrics and conclusion

---

## 📁 Project Structure

```bash
.
├── docs/                    # GitHub Pages site source
│   ├── assets/              # HTML visualizations and tables
│   ├── index.md             # Website content (Markdown)
│   └── _config.yml          # Jekyll theme config
├── template.ipynb           # Full Jupyter Notebook analysis
├── outage.xlsx              # Raw dataset 
├── lec_utils.py             # Utility functions for styling
└── README.md                # This file
