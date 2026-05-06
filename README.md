# TruSecure-Insurance-PBI-Dashboard
A simulated insightful dashboard for a insurance company named TruSecure.  

A 4-page interactive Power BI dashboard built on insurance policy data covering
10,000 policies across agents, regions, customers, and risk profiles.

---

## 📊 Dashboard Pages

### Page 1 — Business Overview
![Business Overview](images/page1.png)

The executive summary page. Designed for C-suite and management to get a
bird's-eye view of the entire portfolio at a glance.

**Key Visuals:**
- **KPI Cards** — Total Policies, Active Policies, Lapsed Policies, Avg Premium
  Per Policy, Total Premium Collected, Total Sum Assured
- **Line Chart** — Total Premium Collected by Purchase Year broken down by
  Policy Type (Endowment, Universal, Whole) to track revenue trends over time
- **Bar Chart** — Total Policies by Policy Status (Active, Surrendered, Lapsed,
  Claimed) showing portfolio health distribution
- **Donut Chart** — Total Premium Collected by Policy Type showing revenue share
  across Endowment, Universal, and Whole Life policies
- **Waterfall Chart** — Total Premium Collected by Purchase Quarter showing
  quarter-on-quarter growth and decline patterns
- **Bubble Map** — Total Sum Assured by State giving a geographic view of
  coverage concentration across India
- **Summary Table** — Business Summary matrix breaking down Total Customers,
  Total Sum Assured, and Total Premium by Policy Name and Policy Status
  (Active, Claimed, Lapsed, Surrendered)

**Slicers:** Policy Name · Policy Status · Policy Type · Region · Purchase Date

---

### Page 2 — Customer Analysis
![Customer Analysis](images/page2.png)

Deep-dive into who the customers are — demographics, risk profile, loan
behaviour, and maturity value by segment.

**Key Visuals:**
- **KPI Cards** — Total Loan Amount Allowed, Average Amount, Total Customers,
  Average Current Age
- **Donut Chart** — Total Policies by Gender (Male / Female / Other) showing
  customer gender distribution
- **Clustered Bar Chart** — Total Customers by Age Band and Gender showing
  which age groups dominate the portfolio and how gender splits within each band
- **Donut Chart** — Total Customers by Medical Exam Required (Yes/No) giving
  a quick read on underwriting complexity
- **Heatmap Matrix** — Avg Premium Per Policy by Age Band and Gender revealing
  which demographic segments pay the most premium on average
- **Horizontal Bar Chart** — Total Customers by Occupation identifying the
  top professions among policyholders
- **Bubble Map** — Total Customers by State showing geographic customer
  concentration across India
- **Column Chart** — Average Maturity Amount by Age Band and Gender helping
  identify which customer segments yield the highest maturity payout
- **Summary Table** — Customer Overview showing Avg Current Age, Total
  Customers, Total Loan Amount Allowed, and Smoker Customer % per Policy Name

**Slicers:** Policy Name · Payment Bucket · Policy Status · Policy Type ·
Region · Purchase Date

---

### Page 3 — Agent & Regional Performance
![Agent & Regional Performance](images/page3.png)

Sales performance page for Sales Heads and Regional Managers to evaluate
agent productivity and regional revenue contribution.

**Key Visuals:**
- **KPI Cards** — Total Active Agents, Policies Per Agent, Premium Per Agent,
  Net Premium After Expenses, Total Underwriting Expenses, Underwriting
  Expense Per Policy
- **Gauge Chart** — Total Premium Collected vs target showing attainment at
  a glance
- **Horizontal Bar Chart** — Total Premium Collected by Region (North, Central,
  South, East, West) ranking regions by revenue contribution
- **Stacked Column Chart** — Total Policies by Zonal Manager Name and Policy
  Type showing each zonal manager's portfolio mix across policy types
- **Bubble Map** — Total Premium Collected by State for granular geographic
  revenue visibility
- **Line + Column Combo Chart** — Policies Per Agent, Total Sum Assured, and
  Total Premium Collected by Region on a dual axis for multi-metric regional
  comparison
- **Agent Overview Table** — Sales Agent, State, Total Policies, Total Premium
  Collected, Total Sum Assured — the agent leaderboard sortable by any column

**Slicers:** Policy Name · Agent Name · Zonal Manager Name · Policy Type ·
Region · Purchase Date

---

### Page 4 — Policy & Risk Analytics
![Policy & Risk Analytics](images/page4.png)

Underwriting and risk page for actuarial and product teams to analyse policy
structure, payment behaviour, tenure concentration, and risk exposure.

**Key Visuals:**
- **KPI Cards** — Total Claims, Avg Tenure Years, Avg Sum Assured, High Risk
  Policies, Quarterly Policy Count, Peak Purchase Month
- **Horizontal Bar Chart** — Avg Sum Assured by Policy Type comparing coverage
  levels across Endowment, Whole, and Universal policies
- **Column Chart** — Average Maturity Amount by Tenure Band showing how
  maturity payout grows with longer tenure commitments
- **Line Chart** — Total Policies by Purchase Month revealing seasonality
  patterns in policy purchases across the year
- **Horizontal Bar Chart** — High Risk Policies by Policy Name identifying
  which products carry the most risk concentration
- **Donut Chart** — Total Policies by Payment Frequency (Monthly, Quarterly,
  Annually) showing cash flow pattern of the portfolio
- **Matrix Table** — Agent Overview breaking down Total Policies, Avg Premium
  Per Policy, and Total Sum Assured by Policy Type × Payment Frequency
  (Annually, Monthly, Quarterly) with grand totals

**Slicers:** Policy Name · Payment Frequency · Policy Status · Tenure Band ·
Region · Purchase Date

---

## 🗂️ Data Model
| Table | Type | Records |
|---|---|---|
| FCT_Insurance_Policy_Table | Fact | ~10,000 |
| DM_Customer_Detail_Table | Dimension | Unique customers |
| DM_Insurance_Agent_Table | Dimension | 22 agents |
| DM_Policy_Protection_Plan | Dimension | Policy products |
| DM_Policy_Type | Dimension | 3 types |
| DM_Regional_Manager | Dimension | 5 regions |
| DM_Zonal_Manager | Dimension | 3 zonal managers |

---

## ⚙️ Key DAX Measures

```dax
Total Policies = COUNTROWS(FCT_Insurance_Policy_Table)

Active Policies = 
    CALCULATE(COUNTROWS(FCT_Insurance_Policy_Table),
    FCT_Insurance_Policy_Table[Policy Status] = "Active")

Maturity Amount =
SUMX('FCT_Insurance_Policy_Table',
    VAR Premium    = 'FCT_Insurance_Policy_Table'[Premium Amount]
    VAR Tenure     = 'FCT_Insurance_Policy_Table'[Tenure (Years)]
    VAR ReturnRate = SWITCH(TRUE(),
        Tenure <= 5,  0.04, Tenure <= 10, 0.06,
        Tenure <= 15, 0.08, Tenure <= 20, 0.10, 0.12)
    RETURN Premium * Tenure * (1 + ReturnRate))

Premium YOY Growth % =
    VAR LatestYear = MAXX(ALL('FCT_Insurance_Policy_Table'),
                     'FCT_Insurance_Policy_Table'[Purchase Year])
    VAR CY = CALCULATE([Total Premium Collected],
              'FCT_Insurance_Policy_Table'[Purchase Year] = LatestYear)
    VAR PY = CALCULATE([Total Premium Collected],
              'FCT_Insurance_Policy_Table'[Purchase Year] = LatestYear - 1)
    RETURN DIVIDE(CY - PY, PY, BLANK())
```

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Power BI Desktop | Dashboard development |
| Power Query | Data cleaning and transformation |
| DAX | Measures and calculated columns |
| Bing Maps | Geographic visuals |

---

## 📁 How to Use

1. Clone or download this repository
2. Open `TruSecure_Dashboard.pbix` in Power BI Desktop
3. If data doesn't load, go to **Transform Data → Data Source Settings**
   and repoint to the CSV files in the `/data` folder
4. Publish to Power BI Service for sharing and scheduled refresh

---

## 📌 Notes

- Map visuals show a "retiring soon" warning — replace with the newer
  Azure Maps visual in Power BI for future-proofing
- RLS (Row Level Security) can be configured for Region and Zonal Manager
  level access before publishing to Power BI Service


Star schema with 1 Fact table and 6 Dimension tables.
