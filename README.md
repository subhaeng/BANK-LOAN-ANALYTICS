# Bank Loan Dashboard (`bloandash.pbix`)

A Power BI dashboard for analyzing a bank's loan portfolio — tracking loan applications, funded amounts, repayments, and the split between good and bad (defaulted/charged-off) loans across two report pages: **Summary** and **Overview**.

## 📊 Overview

This report is built on a single fact table, `financial_loan`, containing loan-level records (issue date, borrower details, loan terms, payment status, etc.). It gives lenders/analysts a quick read on portfolio health — how much has been lent, how much has come back, and where risk is concentrated.

## 📁 Data Model

**Table:** `financial_loan`

| Column | Type | Description |
|---|---|---|
| `ID` | Integer | Loan record ID |
| `MEMBER_ID` | Integer | Borrower ID |
| `ISSUE_DATE` | Date | Date the loan was issued |
| `LOAN_AMOUNT` | Integer | Amount funded |
| `INT_RATE` | Decimal | Interest rate |
| `INSTALLMENT` | Decimal | Monthly installment amount |
| `TERM` | Text | Loan term (e.g., 36/60 months) |
| `GRADE` / `SUB_GRADE` | Text | Lender-assigned credit grade |
| `PURPOSE` | Text | Reason for the loan |
| `HOME_OWNERSHIP` | Text | Borrower's home ownership status |
| `ANNUAL_INCOME` | Decimal | Borrower's annual income |
| `DTI` | Decimal | Debt-to-income ratio |
| `EMP_TITLE` / `EMP_LENGTH` | Text | Borrower's employment info |
| `ADDRESS_STATE` | Text | Borrower's state (used for map visual) |
| `VERIFICATION_STATUS` | Text | Income verification status |
| `LOAN_STATUS` | Text | Current status (e.g., Fully Paid, Charged Off) |
| `Good_vs_Bad_Loan` | Text | Derived flag: "Good" or "Bad" loan |
| `TOTAL_PAYMENT` | Integer | Total amount repaid so far |
| `TOTAL_ACC` | Integer | Total number of credit accounts |
| `LAST_PAYMENT_DATE`, `NEXT_PAYMENT_DATE`, `LAST_CREDIT_PULL_DATE` | Date | Payment/credit history dates |

Power BI's automatic date hierarchy tables (`LocalDateTable_*`, `DateTableTemplate_*`) are also present to support built-in date drill-down/time intelligence.

## 🧮 Key DAX Measures

| Measure | DAX |
|---|---|
| `Total Loan Application` | `COUNT(financial_loan[ID])` |
| `TOTAL FUNDED AMOUNT` | `SUM(financial_loan[LOAN_AMOUNT])` |
| `Total Amount Recieved` | `SUM(financial_loan[TOTAL_PAYMENT])` |
| `Average Interest Rate` | `AVERAGE(financial_loan[INT_RATE])` |
| `Average DTI Ratio` | `AVERAGE(financial_loan[DTI])` |
| `Good Loan Application` | `CALCULATE([Total Loan Application], financial_loan[Good_vs_Bad_Loan]="Good")` |
| `Good Loan %` | `DIVIDE([Good Loan Application], [Total Loan Application])` |
| `Bad Loan Application` | `CALCULATE([Total Loan Application], financial_loan[Good_vs_Bad_Loan]="Bad")` |
| `Bad Loan %` | `DIVIDE([Bad Loan Application], [Total Loan Application])` |

## 🖥️ Report Pages

### 1. Summary
The main KPI page, giving a high-level snapshot of loan performance:
- KPI cards for total applications, funded amount, and amount received
- Donut charts: **Good Loan Application** vs **Bad Loan Application**
- Column charts: **Funded Amount vs Repayment**, **Loan Application** trends, **Average Interest Rate**, and **Average DTI Ratio**
- Slicers for filtering the report

### 2. Overview
A deeper, exploratory view of the loan book:
- **Monthly Loan Application** trend (line chart)
- **Loan Application by Home Ownership** (bar chart)
- **Loan Application by Purpose** (treemap)
- **Loan Application by State** (map)
- Donut chart and KPI card for additional breakdowns

Both pages share a common navigation bar (page navigator + action buttons) for switching between Summary and Overview.

## 🛠️ Requirements

- [Power BI Desktop](https://www.microsoft.com/en-us/power-platform/products/power-bi/desktop) (latest version recommended) to open and edit `bloandash.pbix`
- No external data source connections are required beyond what's embedded in the file — the data model is fully contained in the `.pbix`

## 🚀 Getting Started

1. Clone or download this repository.
2. Open `bloandash.pbix` in Power BI Desktop.
3. Use the slicers on each page to filter by date, state, purpose, grade, etc.
4. Explore the **Summary** page for a quick portfolio health check, and the **Overview** page for trend and segment-level analysis.

## 📌 Notes

- `Good_vs_Bad_Loan` is a derived classification column used to separate healthy loans from defaulted/charged-off ones — this drives the "Good Loan %" and "Bad Loan %" measures that anchor the risk-analysis view.
- The dataset appears to be based on a lending-club-style loan dataset (columns like `GRADE`, `SUB_GRADE`, `DTI`, `EMP_LENGTH` are characteristic of that schema).

##Dashboard
<img width="1918" height="796" alt="Dashboard" src="https://github.com/subhaeng/BANK-LOAN-ANALYTICS/blob/f3f7bcbe58092984866197b9f5020bbc6494620f/Screenshot%202026-08-19%20184024.png"/>

<img width="1918" height="796" alt="Dashboard" src="https://github.com/subhaeng/BANK-LOAN-ANALYTICS/blob/f3f7bcbe58092984866197b9f5020bbc6494620f/Screenshot%202026-08-19%20184038.png"/>
