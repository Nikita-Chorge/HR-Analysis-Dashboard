# HR Analysis Dashboard

This repository contains an end-to-end HR analytics project combining Power BI dashboarding with machine learning-based attrition prediction.

## Contents

- `HR Analysis dashboard.pbix` — Power BI dashboard analyzing workforce metrics across departments, salary bands, and attrition trends
- `HR_Analytics.csv` — Source dataset (1,480 employees, 38 features)
- `ml_model/` — Machine learning extension: attrition prediction model, threshold analysis, and employee risk report

## ML Model Highlights

- Logistic Regression selected over Random Forest based on recall performance on minority class
- Threshold analysis to support HR intervention capacity planning
- Employee-level risk scores exported for actionable HR decisions

See `ml_model/README.md` for full details.

## Tools Used
Power BI, Python, Pandas, Scikit-learn, Matplotlib, Seaborn