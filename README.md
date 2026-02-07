# Bank Loan Analysis - Data Analytics Project

## 📊 Project Overview

This project presents an end-to-end analysis of a bank loan dataset containing **38,600+ loan applications** from 2021. The analysis provides actionable insights into loan performance metrics, customer behavior, and risk management strategies.

### Key Highlights

- **86.2%** Good Loan Rate
- **$435.8M** Total Funded Amount  
- **13.33%** Average DTI Ratio
- **38,600+** Loan Applications Analyzed
- **3** Interactive Dashboards Created

---

## 🎯 Objectives

1. **Monitor Loan Portfolio Performance** - Track KPIs and trends across the loan portfolio
2. **Risk Assessment** - Identify and classify good vs bad loans
3. **Customer Segmentation** - Analyze loan distribution across demographics
4. **Trend Analysis** - Understand temporal patterns in loan applications
5. **Data-Driven Insights** - Provide stakeholders with interactive dashboards

---

## 📂 Dataset Information

### Source Details
| Property | Value |
|----------|-------|
| **Dataset** | financial_loan.csv |
| **Records** | 38,600+ loan applications |
| **Time Period** | January 2021 - December 2021 |
| **Database** | Microsoft SQL Server |

---

## 📈 Key Performance Indicators

### Primary Metrics

| Metric | Value | MoM Change |
|--------|-------|------------|
| **Total Loan Applications** | 38.6K | +6.9% ↑ |
| **Total Funded Amount** | $435.8M | +13.0% ↑ |
| **Total Amount Received** | $473.1M | +15.8% ↑ |
| **Average Interest Rate** | 12.05% | +3.5% ↑ |
| **Average DTI Ratio** | 13.33% | +3.5% ↑ |

### Good vs Bad Loan Analysis

#### 1. Good Loans (Current + Fully Paid)
- **Percentage**: 86.2%
- **Applications**: 33,200
- **Funded Amount**: $370.2M
- **Amount Received**: $435.8M

#### 2. Bad Loans (Charged Off)
- **Percentage**: 13.8%
- **Applications**: 5,300
- **Funded Amount**: $65.5M
- **Amount Received**: $37.3M

---

## 🔍 Detailed Analysis & Findings

### 1. Loan Status Distribution

| Loan Status | Applications | Funded Amount | Amount Received | Avg Interest Rate | Avg DTI |
|-------------|--------------|---------------|-----------------|-------------------|---------|
| **Fully Paid** | 32,145 (83.3%) | $35.6B | $41.2B | 11.64% | 13.17% |
| **Charged Off** | 5,333 (13.8%) | $6.6B | $3.7B | 13.88% | 14.00% |
| **Current** | 1,098 (2.8%) | $1.9B | $2.4B | 15.10% | 14.72% |

**Key Insight**: 83.3% of loans are fully paid, demonstrating strong collection performance.

### 2. Monthly Trend Analysis

The funded amount shows consistent growth throughout 2021:

- **January**: $25M  
- **March**: $29M (+16%)  
- **June**: $34M (+17%)  
- **September**: $41M (+21%)  
- **December**: $54M (+32% - Peak)

**Growth**: 116% increase from January to December

### 3. Loan Distribution by Term

| Term | Funded Amount | Percentage |
|------|---------------|------------|
| **60 Months** | $273.0M | 62.7% |
| **36 Months** | $162.7M | 37.3% |

**Insight**: Borrowers prefer longer terms for lower monthly payments.

### 4. Top Loan Purposes

| Purpose | Funded Amount | Percentage |
|---------|---------------|------------|
| Debt Consolidation | $0.23B | 52.7% |
| Credit Card | $0.08B | 18.4% |
| Home Improvement | $0.03B | 6.9% |
| Other | $0.03B | 6.9% |
| Small Business | $0.02B | 4.6% |

**Key Finding**: Over half of all loans are for debt consolidation.

### 5. Employment Length Distribution

| Employment Length | Total Funded |
|-------------------|--------------|
| 10+ years | $116M (26.6%) |
| 2 years | $65M (14.9%) |
| < 1 year | $44M (10.1%) |
| 3 years | $44M (10.1%) |

**Insight**: Longer employment tenure correlates with higher approval rates.

### 6. Home Ownership Impact

| Ownership Type | Funded Amount | Percentage |
|----------------|---------------|------------|
| MORTGAGE | $219.3M | 50.3% |
| RENT | $185.8M | 42.6% |
| OWN | $30.9M | 7.1% |

---

## 🛠️ Technical Implementation

### Technology Stack

```
Database:       Microsoft SQL Server
Visualization:  Power BI Desktop
Analysis:       SQL Queries + DAX
Documentation:  MS Word, PowerPoint
Data Modeling:  Star Schema
```

### SQL Implementation

#### Example 1: MTD Total Funded Amount
```sql
SELECT SUM(loan_amount) AS MTD_Total_Funded_Amount
FROM bank_loan_data
WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021;
```

#### Example 2: Month-over-Month Growth
```sql
WITH CurrentMonth AS (
    SELECT SUM(loan_amount) AS current_amount
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 12
),
PreviousMonth AS (
    SELECT SUM(loan_amount) AS previous_amount
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 11
)
SELECT 
    ((current_amount - previous_amount) / previous_amount * 100) AS MoM_Percentage
FROM CurrentMonth, PreviousMonth;
```

#### Example 3: Good vs Bad Loan Classification
```sql
-- Good Loan Percentage
SELECT 
    (COUNT(CASE WHEN loan_status IN ('Fully Paid', 'Current') 
           THEN id END) * 100.0) / COUNT(id) AS Good_Loan_Percentage
FROM bank_loan_data;

-- Bad Loan Percentage
SELECT 
    (COUNT(CASE WHEN loan_status = 'Charged Off' 
           THEN id END) * 100.0) / COUNT(id) AS Bad_Loan_Percentage
FROM bank_loan_data;
```

### Power BI Dashboards

#### 📊 Dashboard 1: Summary
- High-level KPI cards with MoM comparisons
- Good vs Bad loan donut charts
- Loan status breakdown table

#### 📊 Dashboard 2: Overview
- Monthly trend analysis (line chart)
- Loan term distribution (donut chart)
- Employee length analysis (bar chart)
- Purpose-based segmentation
- Home ownership distribution

#### 📊 Dashboard 3: Details
- Granular loan-level data table
- Interactive filters (Purpose, Grade, State)
- Drill-through capabilities for detailed metrics

### DAX Measures

```dax
// MTD Total Loan Applications
MTD_Applications = 
CALCULATE(
    COUNT(loan_data[id]),
    DATESMTD(date_table[Date])
)

// Month-over-Month Percentage Change
MoM_Applications = 
DIVIDE(
    [MTD_Applications] - [PMTD_Applications],
    [PMTD_Applications],
    0
)

// Good Loan Percentage
Good_Loan_% = 
DIVIDE(
    CALCULATE(
        COUNT(loan_data[id]),
        loan_data[loan_status] IN {"Fully Paid", "Current"}
    ),
    COUNT(loan_data[id])
)
```

---

## 💡 Key Insights & Recommendations

### Strategic Insights

1. ** Strong Portfolio Health**
   - 86.2% good loan rate indicates robust underwriting standards
   - Consistent MoM growth demonstrates business expansion

2. ** Positive Growth Trajectory**
   - 13% MoM increase in funded amounts
   - 116% YoY growth from Jan to Dec 2021

3. ** Market Opportunity**
   - Debt consolidation (52.7%) presents upsell opportunities
   - Potential for specialized products targeting this segment

4. ** Risk Management**
   - 13.8% charged-off rate requires attention
   - Higher DTI in charged-off loans (14.00% vs 13.17%)

### Business Recommendations

#### 1. Risk Mitigation
- Implement early warning systems for at-risk loans
- Tighten underwriting for high-risk segments
- Monitor loans with DTI > 15% more closely

#### 2. Product Development
- Create specialized debt consolidation products
- Incentivize 36-month terms to improve portfolio turnover
- Design products for stable, long-term employees

#### 3. Geographic Expansion
- Analyze state-level performance for expansion opportunities
- Focus marketing in high-performing regions

#### 4. Pricing Strategy
- Implement risk-based pricing more granularly
- Consider DTI-adjusted interest rates

#### 5. Customer Retention
- Develop loyalty programs for "Fully Paid" customers
- Create re-engagement campaigns for past borrowers

---

## 📁 Repository Structure

```
bank-loan-analysis/
│
├── data/
│   └── financial_loan.csv                    # Raw dataset
│
├── sql_queries/
│   ├── kpi_calculations.sql                  # KPI metrics
│   ├── good_bad_loan_analysis.sql            # Loan classification
│   └── trend_analysis.sql                    # Temporal analysis
│
├── power_bi/
│   └── bank_loan_analysis.pbix               # Complete dashboard
│
├── documentation/
│   ├── BANK_LOAN_REPORT_QUERY_DOCUMENT.docx  # SQL documentation
│   └── Bank_Loan_PPT_Power_BI.pptx           # Presentation
│
├── screenshots/
│   ├── summary_dashboard.png
│   ├── overview_dashboard.png
│   └── details_dashboard.png
│
├── README.md                                 # This file
```

## 📊 Dashboard Previews

### Summary Dashboard
<img width="1919" height="1019" alt="Screenshot 2026-02-07 202848" src="https://github.com/user-attachments/assets/7f99e27f-9377-44fd-b2a5-9fdef15796af" />

### Overview Dashboard
<img width="1919" height="1018" alt="Screenshot 2026-02-07 202923" src="https://github.com/user-attachments/assets/6119d6be-4d4d-409f-a672-3ad387a3dd1f" />

### Details Dashboard
<img width="1914" height="1014" alt="Screenshot 2026-02-07 202940" src="https://github.com/user-attachments/assets/e103f4a0-2b93-47cc-b48d-0e83a15f8447" />

---

## 🎓 Learning Outcomes

- Data cleaning and preprocessing
- Advanced SQL query writing and optimization
- Complex DAX calculations
- Data modeling (Star Schema)
- Interactive dashboard design
- Business intelligence concepts
- KPI definition and tracking
- Financial domain knowledge
- Data visualization best practices
- Stakeholder communication


---

## 👤 Author

**Shashi Ranjan**  
📍 EX-Intern at HFFC  
💼 Exploring roles in Business Analytics & Data Analysis  
📧 www.linkedin.com/in/shashi-ranjan-7b6097282

---
