## Problem Statement

Predict which employees are at risk of attrition so HR can intervene proactively. The dataset contains 1,480 employees with 38 features including compensation, tenure, satisfaction scores, and work conditions.

## Key Steps

- Data cleaning: dropped zero-variance and derived columns, imputed 57 nulls in `YearsWithCurrManager`
- Encoding: binary encoding for `OverTime`, `Gender`; one-hot encoding for categorical features
- Class imbalance handled via `class_weight='balanced'` (16% attrition vs 84% retention)
- Models trained: Logistic Regression and Random Forest
- Evaluation focused on recall and ROC-AUC for the minority class, not accuracy

## Results

| Model | Recall (Left) | Precision (Left) | ROC-AUC |
|---|---|---|---|
| Logistic Regression | 0.73 | 0.35 | 0.814 |
| Random Forest | 0.17 | 0.80 | 0.817 |

**Logistic Regression was selected** despite lower accuracy — Random Forest achieved 86% accuracy by predicting "Stayed" almost always, catching only 17% of actual attrition cases. In an HR context, missing at-risk employees is far more costly than false alarms.

## Threshold Analysis

Rather than defaulting to a 0.5 classification threshold, a business-driven threshold analysis was performed:

| Threshold | Flagged | Precision | Recall | Caught | Missed |
|---|---|---|---|---|---|
| 0.30 | 171 | 0.24 | 0.85 | 41 | 7 |
| 0.40 | 127 | 0.30 | 0.79 | 38 | 10 |
| 0.50 | 99 | 0.35 | 0.73 | 35 | 13 |
| 0.55 | 86 | 0.40 | 0.71 | 34 | 14 |
| 0.60 | 66 | 0.47 | 0.65 | 31 | 17 |

**Recommended threshold: 0.55** — flags 86 employees, catches 71% of actual attrition cases. For HR teams with limited intervention capacity, threshold 0.60 offers a tighter list of 66 employees with nearly 1 in 2 being genuine attrition risks.

## Top Attrition Drivers

1. **MonthlyIncome** — highest feature importance; low earners leave at 5x the rate of high earners
2. **OverTime** — employees working overtime show 3x higher attrition rate (30% vs 10%)
3. **Age & Tenure** — younger, less tenured employees are at significantly higher risk

## Employee Risk Report

A full employee-level risk report is exported to `outputs/employee_risk_report.csv` containing:
- `EmpID` — employee identifier
- `Attrition_Probability` — model confidence score (0 to 1)
- `At_Risk` — flagged as at risk at threshold 0.55 (1 = yes, 0 = no)
- `Actual_Attrition` — ground truth label

At threshold 0.55, **434 employees are flagged as at risk**. The report is sorted by probability descending so HR can prioritize the highest-risk employees first. Employee RM1087 carries the highest attrition probability at 0.969.

## Future Improvements

- SMOTE oversampling for handling class imbalance
- Target encoding for high-cardinality categorical features
- Hyperparameter tuning via GridSearchCV
- SHAP values for individual-level explainability

## Tools Used

Python, Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn, Jupyter Notebook, Power BI