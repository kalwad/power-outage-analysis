# Predicting the Cause of Major U.S. Power Outages (2000–2016)

### 🔍 Overview
This project analyzes major U.S. power outages from 2000 to 2016, using a dataset compiled by Purdue LASCI. These outages either affected 50,000+ customers or caused ≥300 MW of energy loss.
We investigate **what features can be used to predict the cause** of an outage (e.g., *severe weather*, *intentional attack*).

---

### 💾 Dataset Summary
- **Rows:** ~1500
- **Time Range:** Jan 2000 – Jul 2016
- **Columns Used:**
  - `OUTAGE_DURATION_HOURS` – duration of each outage
  - `CUSTOMERS.AFFECTED` – people impacted
  - `CAUSE.CATEGORY` – event type (target variable)
  - Engineered:
    - `DEMAND_LOSS_PER_CUSTOMER`
    - `IS_HURRICANE`

---

### 🧹 Data Cleaning and Feature Engineering

- Combined date + time into proper timestamps
- Computed duration in hours
- Engineered features:
  - **Demand Loss per Customer**
  - **Hurricane Indicator**

---

### 📊 Visualizations

**Outage Duration Distribution:**
<iframe src="assets/duration_histogram.html" width="800" height="600" frameborder="0"></iframe>

**Outage Cause Frequency:**
<iframe src="assets/cause_histogram.html" width="800" height="600" frameborder="0"></iframe>

**Outage Duration by Year:**
<iframe src="assets/duration_by_year.html" width="800" height="600" frameborder="0"></iframe>

**Duration vs Customers Affected:**
<iframe src="assets/duration_vs_customers.html" width="800" height="600" frameborder="0"></iframe>

**Average Duration by Cause:**
<iframe src="assets/agg_table.html" width="800" height="400" frameborder="0"></iframe>

---

### 🧠 Modeling

#### 🧪 Baseline: Logistic Regression
- Features: `OUTAGE_DURATION_HOURS`, `CUSTOMERS.AFFECTED`
- Accuracy: **~82%**
- Strength: good performance on dominant classes
- Weakness: fails on rare causes

#### 🧪 Final Model: Random Forest (with engineered features)
- Added:
  - `DEMAND_LOSS_PER_CUSTOMER`
  - `IS_HURRICANE`
- Tuned `n_estimators` and `max_depth` via GridSearchCV
- Accuracy: **~85%**
- F1 for:
  - *Severe Weather*: **0.93**
  - *Intentional Attack*: **0.94**
- Weakness: rare categories still struggle due to class imbalance

---

### ✅ Conclusion

This model shows promising performance in **predicting the cause of major outages** using real-time features available at the onset of an event.
Better handling of rare classes and incorporating geographic or infrastructure data could push accuracy even further.

---

### 📌 Notes

- Dataset from Purdue LASCI: [LASCI Data Portal](https://engineering.purdue.edu/LASCI/research-data/outages)
- Code available on GitHub (see this repo).
