# Customer Churn Analysis & Prediction

An end-to-end data science and data engineering project that identifies at-risk customers using machine learning and visualises churn patterns through an interactive Power BI dashboard — built around a real telecom business problem.

---

## Problem Statement

Telecom companies lose significant revenue when subscribers leave for competitors. Without a proactive early warning system, retention efforts are reactive and expensive. This project builds a complete pipeline that turns raw subscriber data into actionable churn predictions.

**Revenue at risk formula:**
> Churn Rate × ARPU × Subscriber Base = Revenue at Risk

---

## Project Structure

```
customer-churn-analysis/
│
├── data/
│   ├── raw/                    # IBM Telco Churn dataset (source)
│   ├── processed/              # Cleaned production data
│   └── predictions/            # Model output — scored churners CSV
│
├── sql/
│   ├── create_staging.sql      # Staging table creation
│   ├── clean_to_prod.sql       # Null handling, prod table insert
│   └── create_views.sql        # vw_churn_data, vw_join_data
│
├── notebooks/
│   └── churn_prediction.ipynb  # Full ML pipeline (EDA → model → export)
│
├── dashboard/
│   └── churn_analysis.pbix     # Power BI dashboard file
│
├── report/
│   └── Churn_Analysis_Report.docx
│
└── README.md
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Database & ETL | SQL Server, SQL Server Management Studio |
| Data Processing | Python, pandas, NumPy |
| Machine Learning | scikit-learn (Random Forest Classifier) |
| Visualisation | Power BI, DAX |
| Environment | Jupyter Notebook |

---

## Pipeline Overview

### 1. Data Ingestion & Cleaning (SQL Server)
- Loaded raw dataset into a staging table (`stg_churn`)
- Handled null values across all columns with contextual defaults
- Inserted cleaned data into a production table (`prod_churn`)
- Created two SQL Views:
  - `vw_churn_data` — historical churned/stayed customers for model training
  - `vw_join_data` — new joiners for batch prediction

### 2. Feature Engineering (Python)
- Age buckets: `< 20`, `20–35`, `35–50`, `> 50`
- Tenure groups: `< 6 months` through `>= 24 months`
- Monthly charge ranges: `< 20`, `20–50`, `50–100`, `> 100`
- Binary churn status column (`churn = 1`, `stayed = 0`)
- Label encoding across 15 categorical features

### 3. Machine Learning Model
- **Algorithm:** Random Forest Classifier (100 estimators)
- **Split:** 80% train / 20% test, stratified with fixed random state (42)
- **Accuracy:** 84% on held-out test data
- **Evaluation:** Confusion matrix, precision, recall, F1-score
- **Output:** Batch prediction CSV of flagged future churners

### 4. Power BI Dashboard
Two-page interactive dashboard:

**Page 1 — Executive Summary**
- KPI cards: Total Customers, New Joiners, Total Churned, Churn Rate
- Churn by gender, age group, tenure, contract type, payment method
- Top 5 regions by churn rate
- Churn by category and reason with drill-through tooltips
- Services matrix showing churn rate by subscription status
- Slicers: Monthly Charge Range, Marital Status

**Page 2 — Churn Prediction Profile**
- Count of predicted future churners
- Demographic breakdown: gender, age group, tenure, contract, payment method
- Geographic distribution
- Revenue grid: Total Revenue, Refunds, Charges per customer

---

## Key Findings

- Month-to-month contracts carry a significantly higher churn rate than annual or two-year contracts
- Fibre Optic subscribers churn at the highest rate of any internet type
- Competitor offers are the number one churn reason
- Customers without add-on services (security, device protection, tech support) churn at 70%+
- Churn is most concentrated in the first 6 months of tenure — the critical onboarding window

---

## Dataset

**IBM Telco Customer Churn Dataset**
- 7,043 customers, 33 features
- Includes demographics, service subscriptions, billing details, churn label, churn reason, CLTV, and churn score
- Available on [Kaggle](https://www.kaggle.com/datasets/yeanzc/telco-customer-churn-ibm-dataset)

---

## How to Run

**SQL Pipeline**
1. Open SQL Server Management Studio
2. Run `sql/create_staging.sql` to create the staging table and import the data
3. Run `sql/clean_to_prod.sql` to clean and load the production table
4. Run `sql/create_views.sql` to create the two views

**Python / ML**
```bash
pip install pandas numpy scikit-learn openpyxl matplotlib
jupyter notebook notebooks/churn_prediction.ipynb
```

**Power BI**
1. Open `dashboard/churn_analysis.pbix`
2. Update the SQL Server connection string to your local server
3. Refresh data

---

## Skills Demonstrated

- ETL pipeline design and database architecture
- Data cleaning, null handling, and feature engineering
- Machine learning model training and evaluation
- Batch prediction and model deployment to BI tools
- Business intelligence dashboard design with DAX measures
- Business impact quantification and stakeholder-ready reporting
