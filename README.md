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
df['loan_status_encoded'] = df['loan_status'].apply(lambda x: 1 if x in ['Default', 'Charged Off'] else 0)

# Filter valid records & drop empty critical keys
df_clean = df.dropna(subset=['Loan_ID', 'loan_amnt', 'purpose'])

# Export cleaned data frame into SQLite database
conn = sqlite3.connect("CCUA_Analytics.db")
df_clean.to_sql("Dim_Loan_Account", conn, if_exists="replace", index=False)
conn.close()

print("Data transformation complete. Database populated.")


---

## 🎨 Sprint 2 Design & Wireframe

The Sprint 2 user experience was designed to shift focus from overall macro portfolio performance to **Borrower Financial Stability & Risk Profiling**, isolating how job tenure (`emp_length`) and housing obligations (`home_ownership`) impact loan default frequency.

### Wireframe: Borrower Stability & Risk Profiling
> **Objective:** Evaluate borrower risk indicators across housing obligations and employment tenure to identify early-warning default indicators.

![Sprint 2 Wireframe](Wireframe_Sprint_2.png)

* **Key Layout Elements:**
  * **Top KPI Row:** Metric cards for Highest Risk Employment Segment (`< 1 year`), Unverified Employment Default Rate (`46.61%`), and Total Evaluated Applicants (`2.260668M`).
  * **Housing Portfolio Distribution:** Donut chart breaking down capital and applicant volume by homeownership status (`MORTGAGE`, `RENT`, `OWN`).
  * **Tenure Risk Profiling:** Horizontal bar chart ordering default frequency across employment tenure bands.
  * **Cross-Tabulation Risk Matrix:** Granular matrix table displaying default percentage interactions between housing obligation and tenure.

---

## 📊 Sprint 2 Executive Summary & Analytical Results

### Key Analytical Findings & Empirical Results

#### 1. Macro Borrower Base & Housing Obligations
* **Total Portfolio Scope:** Evaluated **2,260,668 (2.26M)** total loan applications.
* **Dominant Housing Commitments:** 
  * **MORTGAGE:** Comprises **49.16% (1.11M)** of the applicant pool.
  * **RENT:** Represents **39.59% (0.89M)** of applications.
  * **OWN:** Represents **11.19% (0.25M)** of applications.
* **Operational Risk Insight:** Over **88.75%** of applicants maintain recurring housing obligations (Mortgage or Rent), leaving the portfolio highly sensitive to macroeconomic shifts in housing costs and interest rates.

#### 2. Employment Stability & Default Frequency
* **Unverified Employment Exposure:** Applicants with an `Unknown` employment status exhibit the single highest default rate at **46.61%**.
* **Short Job Tenure:** Borrowers with **`< 1 year`** of employment history represent the second highest default tier (~43.1%).
* **Tenured Borrower Baseline:** Even borrowers with **`10+ years`** of employment history maintain a baseline default rate of **~41.5%**, proving that employment stability alone does not insulate borrowers from financial distress without strong debt service management.

#### 3. Combined Risk Matrix Findings (`home_ownership` x `emp_length`)
* **Extreme Default Bands:** Non-standard housing tiers combined with unverified or short tenure exhibit severe risk spikes (e.g., `ANY` housing status with `< 1 year` tenure reaches a **77.86%** default rate; `ANY` with `Unknown` tenure reaches **83.55%**).
* **Mortgage Stability:** Mortgage holders consistently maintain lower default rates across every employment band (ranging between **35.36%** and **45.97%**) compared to renters and other categories.

---

## 💡 Sprint 2 Operational Recommendations
1. **Mandatory Proof of Employment:** Require mandatory documentation for all applications categorized as `Unknown` employment status to reduce exposure to the **46.61%** default band.
2. **Short-Tenure Underwriting Controls:** Enforce stricter debt service caps or lower maximum principal limits for borrowers with **`< 1 year`** of employment tenure.
