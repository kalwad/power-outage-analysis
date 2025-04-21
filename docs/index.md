# ⚡ Predicting the Cause of Major U.S. Power Outages (2000–2016)

### 🔍 Overview

This project analyzes a dataset of **major power outages in the continental U.S.** between January 2000 and July 2016, compiled by Purdue University's LASCI Lab. These outages affected at least **50,000 customers** or caused **≥300 MW** of unplanned demand loss.

We aim to **predict the cause** of an outage—such as _severe weather_, _equipment failure_, or _intentional attack_—using real-time features available at the start of the outage. Reliable predictions can help utility providers deploy better response strategies and improve outage preparedness.

---

### 💾 Dataset Summary

- **Rows:** 1532
- **Columns:** 56
- **Time Range:** Jan 2000 – Jul 2016
- **Target Variable:** `CAUSE.CATEGORY` (multiclass)
- **Features Used:**
  - `OUTAGE_DURATION_HOURS`: Outage length (in hours)
  - `CUSTOMERS.AFFECTED`: People impacted
  - `DEMAND.LOSS.MW`: Total energy demand lost
  - `HURRICANE.NAMES`: Name(s) of storms (if any)

**Engineered Features:**
- `DEMAND_LOSS_PER_CUSTOMER`: Demand loss divided by customers affected
- `IS_HURRICANE`: Binary feature (1 if hurricane-related)

---

### 🧹 Data Cleaning

- Merged start and restoration times into unified timestamps
- Computed outage duration in hours
- Dropped rows missing target or essential timestamp data
- Created additional features based on domain knowledge

---

### 📊 Visualizations

#### 🔸 Outage Duration Distribution
<iframe src="assets/duration_histogram.html" width="800" height="600" frameborder="0"></iframe>

#### 🔸 Outage Cause Frequency
<iframe src="assets/cause_histogram.html" width="800" height="600" frameborder="0"></iframe>

#### 🔸 Outage Duration by Year
<iframe src="assets/duration_by_year.html" width="800" height="600" frameborder="0"></iframe>

#### 🔸 Duration vs. Customers Affected
<iframe src="assets/duration_vs_customers.html" width="800" height="600" frameborder="0"></iframe>

#### 🔸 Average Duration by Cause
<iframe src="assets/agg_table.html" width="800" height="400" frameborder="0"></iframe>

> Notably, **fuel supply emergencies** had the longest average duration (~225 hrs), while **islanding** and **intentional attacks** were shorter on average.

---

### 🧠 Modeling: Predicting the Cause of an Outage

This is a **multiclass classification** problem. We use both a baseline and a tuned model.

#### 🧪 Baseline Model: Logistic Regression

- **Features:** `OUTAGE_DURATION_HOURS`, `CUSTOMERS.AFFECTED`
- **Accuracy:** 82%
- **Strengths:** Performs well on *severe weather* and *intentional attack*
- **Weaknesses:** Minority classes like *equipment failure* and *public appeal* poorly predicted

> Logistic regression showed solid performance but was limited by class imbalance and lack of feature expressiveness.

---

### 🚀 Final Model: Random Forest with Engineered Features

We improved model performance using:
- `DEMAND_LOSS_PER_CUSTOMER`
- `IS_HURRICANE`

Hyperparameters tuned via **GridSearchCV**:
- Best parameters: `n_estimators=200`, `max_depth=None`
- **Cross-validation Accuracy:** ~83%
- **Test Accuracy:** ~85%

#### F1-Scores by Class:
- **Severe Weather:** 0.93
- **Intentional Attack:** 0.94
- **Public Appeal:** 0.50
- **Islanding:** 0.50
- **System Operability Disruption:** 0.34
- **Equipment Failure:** 0.00 (only 4 examples)
- **Fuel Supply Emergency:** N/A (no test examples)

---

### ✅ Conclusion

This project demonstrates that **real-time features** can be used to **reasonably predict the cause of a major power outage**. While common causes like *severe weather* are predicted with high accuracy, rare classes remain challenging.

#### Key Takeaways:
- **Random Forests** outperform Logistic Regression with richer feature sets.
- **Class imbalance** remains a major limitation.
- Future improvements could include:
  - Incorporating **geographic and infrastructure data**
  - **SMOTE or ensemble methods** to improve rare class recall
  - Leveraging external **climate or disaster records** for context

---

### 📌 References

- **Dataset:** [Purdue LASCI Power Outage Dataset](https://engineering.purdue.edu/LASCI/research-data/outages)
- **GitHub Repo:** [View Code Here](https://github.com/kalwad/power-outage-analysis)
