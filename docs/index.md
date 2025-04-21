# ⚡ Predicting the Cause of Major U.S. Power Outages (2000–2016)

### 🔍 Overview

**Introduction:**
This project takes a look at the dataset "Major Power Outage Events in the Continental U.S. (2000–2016)." The dataset has detailed records of large power outages which includes information on their time of occurence,  affected states, outage duration, customers affected, and the cause of the outage. The causes section include severe weather, intentional attack, equipment failure, etc.

**Research Question:**
*What factors can predict the cause of a major power outage?*

Predicting the causes of power outages is important because it can help utility companies and emergency planners anticipate and respond to outages better and more efficiently. The analysis and predictive modeling done in this project focus on determining which features, (such as the duration of the outage, number of customers affected, etc.), are most related to the underlying cause of the outage.

---

### 💾 Dataset Summary

**Dataset Overview:**
   - **Relevant Columns for our analysis include:**
   - `OUTAGE_DURATION_HOURS`: The duration between the start of outage and restoration times.
   - `CUSTOMERS.AFFECTED`: The amount of customers that were impacted by the outage.
   - `DEMAND.LOSS.MW`: The recorded loss of demand in megawatts.
   - `HURRICANE.NAMES`: This indicates whether a hurricane is involved or not.
   - `CAUSE.CATEGORY`: The target variable (such as, 'severe weather', 'intentional attack', etc.)

**Engineered Features:**
- `DEMAND_LOSS_PER_CUSTOMER`: Demand loss divided by customers affected
- `IS_HURRICANE`: Binary feature

---

### 🧹 Data Cleaning and EDA

After loading in the excel file, dropping unnamed extra columns, and reassiging them to easy-to-read names, I can begin the actual data cleaning.

1. First, I merged date and time into single datetime columns.

2. Then, the outage duration in hours was computed.

3. Missing values were then explored.

4. Finally, univariate and bivariate plots were performed.

The univariate plot shows the distribution of outage durations. The first histogram shows most outages are short with a long tail of multi-day events succeeding them. The second histogram shows the frequency of power outage cuases. Severe weather and intentional attacks dominate the dataset.

 **Exploratory Data Analysis:**
 - I plot the distribution of `OUTAGE_DURATION_HOURS` (univariate analysis).
 - I visualize the distribution of `CAUSE.CATEGORY`.
 - I produce a box plot of outage duration by year and a scatter plot of outage duration vs. customers affected.
 - I also compute the mean outage duration per cause category as an aggregate table.

The first bivariate plot shows the outage duration by year. It is a box plot that reveals year-to-year variability in outage lengths.

The second bivariate plot is a scatterplot that shows duration vs. customers affected. There was a modest positive trend where longer outages tend to affect more customers than shorter outages. This is to be expected.

The aggregate table shows the mean duration by cause category. It essentially shows average outage lengths per cause.

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

> Note that **fuel supply emergencies** had the longest average duration (~225 hrs), while **islanding** and **intentional attacks** were shorter on average.

---

### 🧠 Modeling: Predicting the Cause of an Outage

This is a **multiclass classification** problem. I use both a baseline and a tuned model.

#### 🧪 Baseline Model: Logistic Regression

Next, I will create a baseline model using logisitic regression. The pipeline consists of StandardScaler + multinomial LogisticRegression.

As you can see below, an accuracy of ~82% was acheived, however there was poor recall on minority classes.

 **Prediction Problem:**
 I am trying to predict the **CAUSE.CATEGORY** of a major power outage.

 **Features Used:**
 - `OUTAGE_DURATION_HOURS`: Duration of the outage.
 - `CUSTOMERS.AFFECTED`: Number of customers impacted.

 **Type of Problem:**
 This is a **multiclass classification** problem because the target, `CAUSE.CATEGORY`, has multiple different categorical classes.

 **Evaluation Metrics:**
 I can use accuracy as well as precision, recall, and f1-score, macro and weighted averages, to assess the model’s performance.

> Logistic regression showed pretty good performance but was limited by class imbalance and lack of feature expressiveness.

---

### 🚀 Final Model: Random Forest with Engineered Features

 To improve upon the baseline, I engineered two additional features:

 1. **DEMAND_LOSS_PER_CUSTOMER:**
    This is calculated as ratio of `DEMAND.LOSS.MW` to `CUSTOMERS.AFFECTED`. It gives a measure of the outage impact per affected customer.

 2. **IS_HURRICANE:**
    This is a binary indicator that is set to 1 if the `HURRICANE.NAMES` field shows that a hurricane is involved. If not it gives a 0.

 The final model uses these engineered features as well as the original two features in a Random Forest classifier. I can perform hyperparameter tuning using GridSearchCV, which searches over the number of estimators and maximum depth, and uses balanced class weights.

 Finally, I evaluated the final model on the test set and compared its performance to baseline.

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

This project demonstrates that **real-time features** can be used to **reasonably predict cause of a major power outage**. While common causes like *severe weather* are predicted with high accuracy, rare classes are still challenging.

#### Key Takeaways:
- **Random Forests** outperform Logistic Regression because of richer feature sets.
- **Class imbalance** remains major limitation.
- Future improvements could include:
  - Incorporating **geographic and infrastructure data**
  - **SMOTE or ensemble methods** to improve rare class recall
  - Using external **climate or disaster records** for context

---

### 📌 References

- **Dataset:** [Purdue LASCI Power Outage Dataset](https://engineering.purdue.edu/LASCI/research-data/outages)
- **GitHub Repo:** [View Code Here](https://github.com/kalwad/power-outage-analysis)
