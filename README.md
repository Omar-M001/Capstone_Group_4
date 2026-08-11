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

# Sprint 2: Borrower Financial Stability & Risk Profiling

## Overview

Sprint 2 shifts the analysis from overall portfolio-level credit risk to **borrower financial stability and risk profiling**.

The objective is to evaluate how two borrower characteristics influence loan default behavior:

- Employment tenure (`emp_length`)
- Homeownership status (`home_ownership`)

The analysis is designed to identify borrower segments that may represent early-warning indicators of elevated default risk.

---

## Dashboard Wireframe

### Borrower Stability & Risk Profiling

**Objective:** Evaluate borrower risk across housing obligations and employment tenure to identify high-risk borrower segments.

### Key Dashboard Components

#### KPI Summary

The top section of the dashboard highlights three key borrower-risk indicators:

- **Highest Risk Employment Segment:** `< 1 year`
- **Unverified Employment Default Rate:** `46.61%`
- **Total Evaluated Applicants:** `2.260668M`

#### Homeownership Distribution

A donut chart displays the distribution of applicants across major homeownership categories:

- `MORTGAGE`
- `RENT`
- `OWN`
- Other minority housing categories

This visual provides an overview of the housing obligations represented within the borrower population.

#### Employment Tenure Risk Profile

A horizontal bar chart compares **default rates across employment-length categories**, including:

- Unknown
- < 1 year
- 1–9 years
- 10+ years

The visual helps identify whether shorter or unverified employment histories are associated with higher default frequency.

#### Homeownership × Employment Risk Matrix

A matrix heatmap cross-tabulates:

- `home_ownership`
- `emp_length`

Each cell represents the **default rate for a specific borrower segment**.

Darker cells indicate higher default risk, allowing high-risk combinations of housing status and employment tenure to be identified quickly.

---

## Executive Summary

### 1. Borrower Base and Housing Obligations

The analysis evaluates **2,260,668 loan applications**.

The three dominant homeownership categories are:

| Homeownership Status | Share of Applicants | Approx. Applicants |
|---|---:|---:|
| MORTGAGE | 49.16% | 1.11M |
| RENT | 39.59% | 0.89M |
| OWN | 11.19% | 0.25M |

Approximately **88.75% of applicants are either mortgage holders or renters**, indicating that most borrowers maintain recurring housing obligations.

This suggests that changes in housing costs, interest rates, and broader household expenses may have a meaningful impact on borrower repayment capacity.

---

### 2. Employment Stability and Default Risk

Applicants with an **Unknown employment-length status** recorded the highest observed default rate at:

**46.61%**

Borrowers with shorter employment tenure also showed elevated default risk.

At the same time, borrowers with **10+ years of employment history** still maintained a default rate of approximately:

**41.5%**

This indicates that employment tenure may contribute to borrower-risk assessment, but **employment stability alone is not sufficient to determine creditworthiness**.

Other financial indicators should also be considered when evaluating repayment risk.

---

### 3. Combined Borrower Risk Segmentation

The homeownership × employment-length matrix provides a more granular view of borrower risk.

The analysis shows that certain combinations of:

- Short or unverified employment tenure
- Non-standard housing classifications
- Higher-risk homeownership categories

can produce substantially higher default rates than the portfolio baseline.

Mortgage holders generally demonstrate more stable default behavior across employment-tenure categories compared with several other housing segments.

The matrix therefore provides a useful method for identifying **specific borrower combinations that may require additional underwriting review**.

---

## Operational Recommendations

### 1. Strengthen Employment Verification

Applications with an `Unknown` employment-length classification should receive additional verification before approval.

Possible controls include:

- Proof of employment
- Recent pay documentation
- Employer verification
- Income consistency checks

This may help reduce exposure to the high-default segment associated with unverified employment information.

### 2. Apply Additional Controls to Short-Tenure Borrowers

Borrowers with limited employment tenure may require additional underwriting controls.

Potential actions include:

- Lower maximum loan amounts
- More conservative debt-service thresholds
- Additional income verification
- Enhanced affordability assessment

These controls should be combined with other borrower financial indicators rather than using employment tenure as a standalone approval criterion.

---

## Key Takeaway

Sprint 2 demonstrates that borrower stability is better understood by examining **multiple financial and demographic indicators together**.

Employment tenure provides useful risk information, but the strongest insights emerge when it is combined with homeownership status and other borrower characteristics.

The resulting segmentation helps identify borrower groups that may benefit from additional verification or more conservative underwriting controls.
