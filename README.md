# Capstone_Group_4
# CCUA Credit Risk & Default Portfolio Analytics

![CCUA Logo](CCUA-logo.png)

## 📌 Project Overview
This repository contains the end-to-end data pipeline, analytical models, and dashboard implementations for the **Canadian Credit Union Association (CCUA)** Credit Risk Analysis Project. 

**Sprint 1 Focus:** Establishing baseline portfolio health, quantifying macro default rates, and isolating high-risk lending categories across principal deployment tiers.

---

## 🎨 Sprint 1 Design & Wireframes

The Sprint 1 user experience and visual layout were designed using a multi-wireframe approach to map executive user flows from macro overview down to category deep-dives.

### Wireframe 1: Executive High-Level Overview
> **Objective:** Present top-level portfolio KPIs and immediate default frequency by loan purpose.

![Wireframe 1 - Executive Overview](Sprint_1_01.png)

* **Key Elements:** Top KPI summary row (Funded Capital, Default Rate, Total Applications), horizontal default rate distribution, and high-risk category callouts.

---

### Wireframe 2: Risk Concentration Matrix (Drill-Down View)
> **Objective:** Evaluate multi-dimensional risk cross-tabulating loan purpose against principal funding tiers.

![Wireframe 2 - Risk Matrix](Sprint_1_02.png)

* **Key Elements:** Interactive heatmap grid, cross-category filter slicers, and concentration severity highlighting.

---

### Wireframe 3: Category Detail Drawer (Interactive Flow)
> **Objective:** Provide underwriting teams with granular category-level metrics and action items.

![Wireframe 3 - Detail Drawer](Sprint_1_03.png)

* **Key Elements:** Category-specific trends, collateral requirements checklist, and risk mitigation action panels.

---

## 🐍 Data Cleaning & Transformation Pipeline (Python)

Before importing data into the SQLite database (`CCUA_Analytics.db`) and Power BI, initial data cleaning, binary status encoding, and schema transformations were executed in Python.

### Transformation Script

```python
import pandas as pd
import sqlite3

# Load raw dataset
df = pd.read_csv("raw_loan_data.csv")

# Clean & encode binary loan status (1 = Default, 0 = Paid)
df['loan_status_encoded'] = df['loan_status'].apply(
    lambda x: 1 if x in ['Default', 'Charged Off'] else 0
)

# Filter valid records & drop empty critical keys
df_clean = df.dropna(subset=['Loan_ID', 'loan_amnt', 'purpose'])

# Export cleaned data frame into SQLite database
conn = sqlite3.connect("CCUA_Analytics.db")
df_clean.to_sql("Dim_Loan_Account", conn, if_exists="replace", index=False)
conn.close()

print("Data transformation complete. Database populated.")
```

---

# Sprint 2: Borrower Financial Stability & Risk Profiling

## 🎨 Sprint 2 Design & Wireframe

Sprint 2 shifts the dashboard focus from portfolio-level credit risk to **Borrower Financial Stability & Risk Profiling**.

The wireframe was designed to explore how borrower employment tenure (`emp_length`) and homeownership status (`home_ownership`) can be used to identify patterns associated with loan default risk.

### Wireframe: Borrower Financial Stability & Risk Profiling

> **Objective:** Provide an executive-level view of borrower stability by examining employment tenure, homeownership distribution, and the interaction between these factors and loan default rates.

![Sprint 2 - Borrower Financial Stability & Risk Profiling Wireframe](Wireframe_Sprint_2.png)

- **Key Elements:**
  - **Top KPI Summary Row:** Highlights the Highest Risk Employment Segment (`< 1 year`), Unverified Employment Default Rate (`46.61%`), and Total Evaluated Applicants (`2.260668M`).
  - **Homeownership Distribution:** Donut chart displaying the distribution of applicants across `MORTGAGE`, `RENT`, `OWN`, and minority homeownership categories.
  - **Employment Tenure Risk Profile:** Horizontal bar chart comparing default rates across employment-length categories, from `Unknown` and `< 1 year` through `10+ years`.
  - **Borrower Risk Matrix:** Heatmap matrix cross-tabulating `home_ownership` against `emp_length`, with conditional shading used to identify combinations associated with higher default rates.

---
