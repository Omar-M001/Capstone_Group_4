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
---

## 🎨 Sprint 3 Design & Wireframes

**Sprint 3 Focus:** Tracking longitudinal loan status transitions and repayment trends over time, segmented by loan purpose, region, and vintage year, to surface where repayment performance is deteriorating.

### Wireframe 1: Loan Status Trend Overview

> **Objective:** Present portfolio-wide KPIs and loan status movement (Current, Paid Off, Late, Default) over time to establish a baseline repayment trend.

![Wireframe 1 - Loan Status Trend Overview](Sprint_3_01.png.png)

- **Key Elements:** Top KPI summary row (Total Loans, Overall Repayment Rate, Current Default Trend, Avg Days to Repay), multi-line loan status trend chart (2019–2022), filters by loan purpose/region/year, and an insight callout flagging declining repayment in late-stage cohorts.

---

### Wireframe 2: Segment Breakdown (Drill-Down View)

> **Objective:** Compare repayment trends across loan purposes and evaluate default concentration by purpose × tenor.

![Wireframe 2 - Segment Breakdown](Sprint_3_02.png.png)

- **Key Elements:** Side-by-side repayment trend charts for Debt Consolidation, Auto Loans, and Small Business segments, and a concentration matrix heatmap showing default rate by loan purpose crossed with tenor (12/24/36/48mo).

---

### Wireframe 3: Segment Detail (Interactive Flow)

> **Objective:** Provide underwriting teams with granular, segment-specific repayment timelines and actionable risk mitigation steps.

![Wireframe 3 - Segment Detail](Sprint_3_03.png.png)

- **Key Elements:** Segment-specific repayment timeline (e.g., small business, 36mo tenor) with anomaly callouts (e.g., "Month 18 – late spike"), ranked contributing risk factors, and recommended action item cards.
# Sprint 4: Debt-to-Income (DTI) & Credit Stress Analysis

## 🎨 Sprint 4 Design & Wireframes

Sprint 4 focuses on **Debt-to-Income (DTI) & Credit Stress Analysis** and addresses Diagnostic Questions 2 and 3. The wireframes were designed to examine how borrower DTI and revolving credit utilization relate to resolved loan default and repayment performance.

### Wireframe 1: DTI & Credit Stress Overview
> **Objective:** Summarize portfolio-level DTI and utilization stress through executive KPIs, banded trends, and review triggers.

![Sprint 4 Wireframe 1 - DTI and Credit Stress Overview](Sprint_4_01.png)

- **Key Elements:**
  - **Top KPI Summary Row:** Displays the resolved default rate, high-DTI risk lift, and high-utilization risk lift.
  - **DTI Risk Profile:** Horizontal bar chart comparing default rates across DTI bands from `<10%` through `≥40%`.
  - **High-Risk Credit Bands:** Highlights elevated-risk DTI, utilization, and combined-stress categories.
  - **Executive Insight:** Summarizes the relationship between increasing credit stress and default risk.

---

### Wireframe 2: Joint DTI × Utilization Matrix
> **Objective:** Cross-tabulate DTI and revolving-utilization bands to reveal where combined credit stress is concentrated.

![Sprint 4 Wireframe 2 - Joint DTI and Utilization Matrix](Sprint_4_02.png)

- **Key Elements:**
  - **DTI × Utilization Heatmap:** Cross-tabulates DTI bands against revolving-utilization bands.
  - **Conditional Shading:** Uses darker cells to identify combinations with higher resolved default rates.
  - **Risk Takeaways:** Explains how high utilization can compound borrower affordability pressure.
  - **Review Support:** Helps underwriting teams identify segments requiring further investigation.

---

### Wireframe 3: High-Stress Segment Detail
> **Objective:** Give underwriting staff a drill-down view of a selected high-stress borrower segment.

![Sprint 4 Wireframe 3 - High-Stress Segment Detail](Sprint_4_03.png)

- **Key Elements:**
  - **Quick Statistics:** Displays resolved-loan volume, segment default rate, and risk lift.
  - **Combined Risk Profile:** Shows the change in default rate across utilization bands for the selected DTI segment.
  - **Underwriting Checklist:** Provides review prompts for obligations, income, affordability, payment history, and compensating factors.
  - **Interactive Drawer:** Demonstrates how a user can move from the summary dashboard into a selected high-stress segment.

---

## 🐍 Sprint 4 Python Analysis: Debt-to-Income (DTI) & Credit Stress

This analysis addresses:

* **Diagnostic Question 2:** Why do applicants with high debt-to-income ratios show higher financial risk?
* **Diagnostic Question 3:** How does revolving credit utilization influence repayment performance?

The script keeps only resolved outcomes—paid or defaulted loans—when calculating historical default rates. Current, late, and grace-period loans are not incorrectly counted as paid loans.

```python
import numpy as np
import pandas as pd

DATA_FILE = "cleaned_loan_data.csv"

PAID_STATUSES = {
    "Fully Paid",
    "Does not meet the credit policy. Status:Fully Paid",
}
DEFAULT_STATUSES = {
    "Charged Off",
    "Default",
    "Does not meet the credit policy. Status:Charged Off",
}

# Load only the columns required for the Sprint 4 diagnostic questions.
loans = pd.read_csv(
    DATA_FILE,
    usecols=["loan_status", "dti", "revol_util"],
)

loans["repayment_outcome"] = np.select(
    [
        loans["loan_status"].isin(DEFAULT_STATUSES),
        loans["loan_status"].isin(PAID_STATUSES),
    ],
    ["Default", "Paid"],
    default="Other",
)

# Historical default analysis uses resolved outcomes only.
resolved = loans.loc[
    loans["repayment_outcome"].isin(["Default", "Paid"])
].copy()
resolved["default_flag"] = resolved["repayment_outcome"].eq("Default").astype(int)
resolved["dti"] = pd.to_numeric(resolved["dti"], errors="coerce")
resolved["revol_util"] = pd.to_numeric(resolved["revol_util"], errors="coerce")

resolved["dti_band"] = pd.cut(
    resolved["dti"],
    bins=[-np.inf, 10, 20, 30, 40, np.inf],
    labels=["<10%", "10–<20%", "20–<30%", "30–<40%", "40%+"],
    right=False,
)
resolved["utilization_band"] = pd.cut(
    resolved["revol_util"],
    bins=[-np.inf, 20, 40, 60, 80, 100, np.inf],
    labels=["<20%", "20–<40%", "40–<60%", "60–<80%", "80–<100%", "100%+"],
    right=False,
)

def summarize_risk(data, group_column):
    summary = (
        data.dropna(subset=[group_column])
        .groupby(group_column, observed=True)["default_flag"]
        .agg(loans="size", defaults="sum", default_rate="mean")
        .reset_index()
    )
    summary["default_rate_pct"] = summary["default_rate"] * 100
    return summary.drop(columns="default_rate")

dti_summary = summarize_risk(resolved, "dti_band")
utilization_summary = summarize_risk(resolved, "utilization_band")

# Combined DTI and utilization view used by the Sprint 4 risk matrix.
joint_summary = (
    resolved.dropna(subset=["dti_band", "utilization_band"])
    .groupby(["dti_band", "utilization_band"], observed=True)["default_flag"]
    .agg(loans="size", defaults="sum", default_rate="mean")
    .reset_index()
)
joint_summary["default_rate_pct"] = joint_summary["default_rate"] * 100
joint_summary = joint_summary.drop(columns="default_rate")

dti_summary.to_csv("sprint4_dti_summary.csv", index=False)
utilization_summary.to_csv("sprint4_utilization_summary.csv", index=False)
joint_summary.to_csv("sprint4_joint_risk_matrix.csv", index=False)

print(dti_summary)
print(utilization_summary)
print(joint_summary)
```

---

# Sprint 5: Multi-Factor Risk Segmentation & Credit Inquiries

## 🎨 Sprint 5 Design & Wireframes

Sprint 5 focuses on **Multi-Factor Risk Segmentation & Credit Inquiries** and addresses Diagnostic Questions 4 and 5. The wireframes were designed to compare transparent borrower-risk segments and examine how employment history and recent credit inquiries relate to resolved loan outcomes.

### Wireframe 1: Borrower Segment Risk Overview
> **Objective:** Compare default risk across transparent, multi-factor borrower segments.

![Sprint 5 Wireframe 1 - Borrower Segment Risk Overview](Sprint_5_01.png)

- **Key Elements:**
  - **Top KPI Summary Row:** Highlights the highest-risk segment, compound-stress default rate, and the default rate for borrowers with three or more inquiries.
  - **Borrower Segment Comparison:** Horizontal bar chart comparing resolved default rates across six explainable borrower segments.
  - **High-Risk Segment Panel:** Highlights Compound Stress, Credit-Seeking Stress, and Affordability Stress.
  - **Executive Insight:** Explains how multiple simultaneous stress signals identify higher-risk borrowers.

---

### Wireframe 2: Employment History × Credit Inquiries
> **Objective:** Cross-tabulate employment tenure and recent credit inquiries to reveal interaction patterns in repayment outcomes.

![Sprint 5 Wireframe 2 - Employment History and Credit Inquiries](Sprint_5_02.png)

- **Key Elements:**
  - **Employment × Inquiry Matrix:** Cross-tabulates employment-tenure bands against recent credit-inquiry bands.
  - **Conditional Shading:** Uses darker cells to identify combinations with higher resolved default rates.
  - **Portfolio Filters:** Allows analysis by loan grade, purpose, and income band.
  - **Risk Takeaways:** Summarizes how short employment tenure and repeated inquiries can interact.

---

### Wireframe 3: Segment Detail & Inquiry Drill-Down
> **Objective:** Provide an interactive drill-down for a selected borrower segment.

![Sprint 5 Wireframe 3 - Segment Detail and Inquiry Drill-Down](Sprint_5_03.png)

- **Key Elements:**
  - **Quick Statistics:** Displays the selected segment's loan count, resolved default rate, and inquiry level.
  - **Inquiry Risk Trend:** Shows how resolved default rates change as recent inquiries increase.
  - **Underwriting Checklist:** Prompts the reviewer to verify employment, inquiry purpose, revolving obligations, DTI, and utilization.
  - **Interactive Drawer:** Demonstrates how the user can drill into a selected borrower segment without leaving the main dashboard.

---

## 🐍 Sprint 5 Python Analysis: Multi-Factor Risk Segmentation & Credit Inquiries

This analysis addresses:

* **Diagnostic Question 4:** Why are certain borrower segments more likely to default than others?
* **Diagnostic Question 5:** What relationships exist between employment history, credit inquiries, and loan repayment outcomes?

The segment rules are mutually exclusive and listed in priority order so that every resolved loan receives one clear, explainable borrower-segment label.

```python
import numpy as np
import pandas as pd

DATA_FILE = "cleaned_loan_data.csv"

PAID_STATUSES = {
    "Fully Paid",
    "Does not meet the credit policy. Status:Fully Paid",
}
DEFAULT_STATUSES = {
    "Charged Off",
    "Default",
    "Does not meet the credit policy. Status:Charged Off",
}

# Load only the fields needed for segmentation and inquiry analysis.
loans = pd.read_csv(
    DATA_FILE,
    usecols=[
        "loan_status",
        "emp_length_years",
        "inq_last_6mths",
        "annual_inc",
        "home_ownership",
        "dti",
        "revol_util",
        "grade",
    ],
)

loans["repayment_outcome"] = np.select(
    [
        loans["loan_status"].isin(DEFAULT_STATUSES),
        loans["loan_status"].isin(PAID_STATUSES),
    ],
    ["Default", "Paid"],
    default="Other",
)

resolved = loans.loc[
    loans["repayment_outcome"].isin(["Default", "Paid"])
].copy()
resolved["default_flag"] = resolved["repayment_outcome"].eq("Default").astype(int)

numeric_columns = [
    "emp_length_years",
    "inq_last_6mths",
    "annual_inc",
    "dti",
    "revol_util",
]
resolved[numeric_columns] = resolved[numeric_columns].apply(
    pd.to_numeric,
    errors="coerce",
)

# Complete cases are used so missing values are not silently treated as low risk.
analysis = resolved.dropna(
    subset=["emp_length_years", "inq_last_6mths", "dti", "revol_util"]
).copy()

analysis["employment_band"] = pd.cut(
    analysis["emp_length_years"],
    bins=[-np.inf, 1, 3, 5, 10, np.inf],
    labels=["<1 year", "1–<3 years", "3–<5 years", "5–<10 years", "10+ years"],
    right=False,
)
analysis["inquiry_band"] = pd.cut(
    analysis["inq_last_6mths"],
    bins=[-np.inf, 1, 2, 3, np.inf],
    labels=["0", "1", "2", "3+"],
    right=False,
)

# Mutually exclusive, explainable borrower segments.
conditions = [
    (analysis["dti"] >= 30)
    & (analysis["revol_util"] >= 80)
    & (analysis["inq_last_6mths"] >= 3),
    (analysis["inq_last_6mths"] >= 3)
    & ((analysis["dti"] >= 30) | (analysis["revol_util"] >= 80)),
    (analysis["dti"] >= 30) | (analysis["revol_util"] >= 80),
    (analysis["emp_length_years"] < 3)
    & (analysis["inq_last_6mths"] >= 1),
    (analysis["emp_length_years"] >= 5)
    & (analysis["inq_last_6mths"] == 0)
    & (analysis["dti"] < 30)
    & (analysis["revol_util"] < 80),
]
segment_names = [
    "Compound Stress",
    "Credit-Seeking Stress",
    "Affordability Stress",
    "Employment Vulnerability",
    "Established Low-Inquiry",
]
analysis["borrower_segment"] = np.select(
    conditions,
    segment_names,
    default="Moderate Profile",
)

segment_summary = (
    analysis.groupby("borrower_segment")
    .agg(
        loans=("default_flag", "size"),
        defaults=("default_flag", "sum"),
        default_rate=("default_flag", "mean"),
        median_dti=("dti", "median"),
        median_utilization=("revol_util", "median"),
        median_inquiries=("inq_last_6mths", "median"),
        median_employment_years=("emp_length_years", "median"),
        median_annual_income=("annual_inc", "median"),
    )
    .reset_index()
)
segment_summary["default_rate_pct"] = segment_summary["default_rate"] * 100
segment_summary = segment_summary.drop(columns="default_rate")

# Employment tenure × recent inquiry matrix used by Wireframe 2.
employment_inquiry_summary = (
    analysis.groupby(["employment_band", "inquiry_band"], observed=True)[
        "default_flag"
    ]
    .agg(loans="size", defaults="sum", default_rate="mean")
    .reset_index()
)
employment_inquiry_summary["default_rate_pct"] = (
    employment_inquiry_summary["default_rate"] * 100
)
employment_inquiry_summary = employment_inquiry_summary.drop(columns="default_rate")

segment_summary.to_csv("sprint5_borrower_segment_summary.csv", index=False)
employment_inquiry_summary.to_csv(
    "sprint5_employment_inquiry_matrix.csv",
    index=False,
)

print(segment_summary.sort_values("default_rate_pct", ascending=False))
print(employment_inquiry_summary)
```

---

## 🎨 Sprint 8 Design & Wireframes

**Sprint 8 Focus:** Regional Risk Concentration — identifying which regional credit union branches carry the highest concentration of High and Critical Risk loans, and giving underwriting leadership an interactive way to drill from a portfolio-wide view down to a single branch.

**Guiding Question:** Which regional credit union branches carry the highest concentration of High and Critical Risk loans?

**Feature:** Interactive Regional Heatmap & Branch Drill-Through Filter.

### Wireframe 1: Executive Regional Overview

> **Objective:** Present top-level regional risk KPIs and immediate risk concentration by branch location, so leadership can spot problem regions at a glance.

![Wireframe 1 - Executive Regional Overview](Sprint_8_01.png)

- **Key Elements:** Top KPI summary row (Active Branches, Avg Risk Score, Critical Risk Loans, High-Risk Concentration %), a regional map with risk-tier-coloured branch markers, and a ranked "Top 5 Branches by Risk" sidebar.

---

### Wireframe 2: Regional Heatmap Matrix (Drill-Down View)

> **Objective:** Evaluate multi-dimensional risk cross-tabulating branch region against risk tier concentration.

![Wireframe 2 - Regional Heatmap Matrix](Sprint_8_02.png)

- **Key Elements:** Interactive heatmap grid (Region × Risk Tier), cross-category filter slicers, and concentration severity highlighting.

---

### Wireframe 3: Branch Detail Drawer (Interactive Flow)

> **Objective:** Provide regional managers and underwriting teams with granular branch-level risk metrics and recommended action items.

![Wireframe 3 - Branch Detail Drawer](Sprint_8_03.png)

- **Key Elements:** Branch-specific trends, top Critical-risk loans table, and risk mitigation action panels.

---

## 🐍 Sprint 8 Data Pipeline (Python)

Sprint 8 joins the scored loan population against branch/region reference data, aggregates risk tiers per branch, and computes concentration percentages that feed the regional heatmap in Power BI.

### Transformation Script

```python
import pandas as pd
import sqlite3

conn = sqlite3.connect("CCUA_Analytics.db")

# Pull scored loans joined against branch/region reference data
query = """
SELECT
    l.Loan_ID,
    l.branch_id,
    b.branch_name,
    b.region,
    b.province,
    r.risk_score,
    r.risk_tier
FROM Dim_Loan_Account l
JOIN Dim_Branch b ON l.branch_id = b.branch_id
JOIN Fact_Risk_Score r ON l.Loan_ID = r.Loan_ID
"""
df = pd.read_sql(query, conn)

# Aggregate risk-tier counts per branch/region
concentration = (
    df.groupby(["region", "branch_name", "risk_tier"])
      .agg(loan_count=("Loan_ID", "count"))
      .reset_index()
)

# Compute % concentration of High + Critical within each branch
branch_totals = df.groupby("branch_name")["Loan_ID"].count().rename("total_loans")
high_crit = (
    df[df["risk_tier"].isin(["High", "Critical"])]
      .groupby("branch_name")["Loan_ID"].count()
      .rename("high_critical_loans")
)
branch_summary = pd.concat([branch_totals, high_crit], axis=1).fillna(0)
branch_summary["high_critical_pct"] = (
    branch_summary["high_critical_loans"] / branch_summary["total_loans"]
).round(4)

# Stage results for the Power BI heatmap visual
concentration.to_sql("Fact_Regional_Risk_Concentration", conn, if_exists="replace", index=False)
branch_summary.reset_index().to_sql("Fact_Branch_Risk_Summary", conn, if_exists="replace", index=False)

conn.close()
print("Regional risk concentration tables staged for Power BI.")
```

---

## 🎨 Sprint 9 Design & Wireframes

**Sprint 9 Focus:** Borrower Demographic Analysis — understanding how applicant default risk correlates with loan purpose, employment length, and credit grade.

**Guiding Question:** How does applicant default risk correlate with loan purpose, employment length, and credit grade?

**Feature:** Multi-Attribute Scatter Plot & Demographic Slicer Panel.

### Wireframe 1: Executive Demographic Overview

> **Objective:** Present headline default-rate correlations across loan purpose, employment length, and credit grade.

![Wireframe 1 - Executive Demographic Overview](Sprint_9_01.png)

- **Key Elements:** Top KPI summary row (Avg Default Rate, Highest-Risk Purpose, Highest-Risk Grade, Median Employment Length), a DTI-vs-default-probability scatter plot coloured by credit grade, and a "Risk by Purpose" ranked sidebar.

---

### Wireframe 2: Multi-Attribute Scatter Matrix (Drill-Down View)

> **Objective:** Evaluate multi-dimensional risk cross-tabulating loan purpose, employment length, and credit grade against default probability.

![Wireframe 2 - Multi-Attribute Scatter Matrix](Sprint_9_02.png)

- **Key Elements:** Interactive scatter plot with a configurable axis selector, a demographic slicer panel, and a correlation-coefficient callout.

---

### Wireframe 3: Applicant Detail Drawer (Interactive Flow)

> **Objective:** Provide underwriting teams with granular applicant-level demographic and risk detail.

![Wireframe 3 - Applicant Detail Drawer](Sprint_9_03.png)

- **Key Elements:** Applicant profile summary, cohort-comparison chart, risk-tier badge, and underwriter routing recommendation.

---

## 🐍 Sprint 9 Data Pipeline (Python)

Sprint 9 joins the scored loan population against demographic attributes, buckets continuous fields for slicer use, and computes correlation statistics that feed the scatter matrix and callouts.

### Transformation Script

```python
import pandas as pd
import sqlite3
import numpy as np

conn = sqlite3.connect("CCUA_Analytics.db")

query = """
SELECT
    l.Loan_ID,
    l.purpose,
    l.emp_length,
    l.grade,
    l.dti,
    r.risk_score,
    r.risk_tier
FROM Dim_Loan_Account l
JOIN Fact_Risk_Score r ON l.Loan_ID = r.Loan_ID
"""
df = pd.read_sql(query, conn)

# Bucket employment length for the demographic slicer panel
bins = [-1, 1, 3, 5, 10, 100]
labels = ["<1 yr", "1-3 yrs", "3-5 yrs", "5-10 yrs", "10+ yrs"]
df["emp_length_bucket"] = pd.cut(df["emp_length"], bins=bins, labels=labels)

# Default rate by purpose
purpose_risk = (
    df.groupby("purpose")
      .agg(avg_default_prob=("risk_score", "mean"), loan_count=("Loan_ID", "count"))
      .sort_values("avg_default_prob", ascending=False)
      .reset_index()
)

# Default rate by employment length bucket x credit grade (for the scatter matrix)
demo_matrix = (
    df.groupby(["emp_length_bucket", "grade"], observed=True)
      .agg(avg_default_prob=("risk_score", "mean"), loan_count=("Loan_ID", "count"))
      .reset_index()
)

# Correlation coefficient: employment length vs. risk score
correlation = np.corrcoef(df["emp_length"].fillna(0), df["risk_score"])[0, 1]
print(f"Employment length vs. default probability correlation: r = {correlation:.2f}")

# Stage results for Power BI
df.to_sql("Fact_Borrower_Demographics", conn, if_exists="replace", index=False)
purpose_risk.to_sql("Fact_Risk_By_Purpose", conn, if_exists="replace", index=False)
demo_matrix.to_sql("Fact_Demographic_Risk_Matrix", conn, if_exists="replace", index=False)

conn.close()
print("Demographic correlation tables staged for Power BI.")
```
