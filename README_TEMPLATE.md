# [Project Title]
> # HR Employee Attrition Analysis

Analyzed 1,470 employee records to identify what's actually driving attrition — finding that overtime, not department or compensation, is the strongest predictor, with rates nearly 3x higher among employees working overtime.

---

## ⚙️ Project Type Flags

- [x] Exploratory Data Analysis (EDA)
- [x] SQL Analysis / Querying
- [x] Dashboard / Data Visualization
- [x] Data Cleaning / Wrangling
- [x] End-to-End (multiple of the above)

---

# HR Employee Attrition Analysis

Analyzed 1,470 employee records to identify what's actually driving attrition — finding that overtime, not department or compensation, is the strongest predictor, with rates nearly 3x higher among employees working overtime.

## ⚙️ Project Type Flags

- [x] Exploratory Data Analysis (EDA)
- [x] SQL Analysis / Querying
- [x] Dashboard / Data Visualization
- [x] Data Cleaning / Wrangling
- [x] End-to-End (multiple of the above)

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [Key Insights](#8-key-insights)
9. [Recommendations](#9-recommendations)
10. [Assumptions & Limitations](#10-assumptions--limitations)
11. [Future Enhancements](#11-future-enhancements)
12. [Deliverables](#12-deliverables)
13. [Author](#13-author)

---

## 1. Project Overview

HR wants to understand why employees leave — but "why do people quit" is too broad to act on. This project breaks that question into specific, testable sub-questions and answers each one with SQL, then presents the findings as an interactive Power BI dashboard.

The dataset: IBM's public HR Analytics Employee Attrition & Performance dataset — 1,470 employees, 35 original attributes, no missing values.

## 2. Objectives

This analysis was framed around five questions an HR director would actually ask:

1. Which departments and job roles have the highest attrition rate?
2. Does overtime correlate with attrition, and by how much?
3. Is attrition concentrated in early tenure (0-2 years) or spread evenly?
4. Does income level or distance-from-home show a pattern with attrition?
5. If HR could only fix one thing, what would move the needle most?

## 3. Project Scope & Tools

| Stage | Tool |
|---|---|
| Data cleaning | Excel |
| Analysis | SQL (MySQL Workbench) |
| Visualization | Power BI |

Scope: single dataset, single point-in-time snapshot (not longitudinal). No predictive modeling — this is a diagnostic, not a forecasting exercise.

## 4. Repository Structure

```
hr-attrition-analysis/
├── README.md
├── data/                  # raw and cleaned datasets
├── sql/
│   ├── exploratory/       # ad-hoc checks during initial exploration
│   ├── transformation/    # cleaning/reshaping queries
│   └── final/             # presentation-ready analysis queries
├── dashboard/             # Power BI .pbix file
├── images/                # dashboard screenshots
├── docs/
│   ├── business-questions.md
│   ├── data-dictionary.md
│   └── methodology.md
└── reports/               # final deliverables (if any)
```

## 5. Data Workflow

**1. Clean (Excel)**
- Converted raw data to an Excel Table
- Dropped 3 constant-value columns with no analytical value: `EmployeeCount`, `Over18`, `StandardHours`
- Verified data types and confirmed zero nulls, zero duplicate rows
- Added a derived `TenureBand` column (0-2 / 3-5 / 6-10 / 10+ years)

**2. Analyze (SQL)**
- Imported the cleaned data into MySQL Workbench
- Wrote queries answering each business question: attrition by department, job role, overtime status, tenure band, and an income/distance comparison
- Ran a follow-up overlap query (JobRole × OverTime) once overtime emerged as a strong single-factor driver — this surfaced the project's strongest finding

**3. Visualize (Power BI)**
- Built a 2-page dashboard: **Overview** (KPIs, department/role breakdown) and **Risk Drivers** (overtime, tenure, role × overtime matrix)
- Used conditional formatting on the matrix to visually flag the highest-risk combination

Full detail: [`docs/methodology.md`](docs/methodology.md)

## 6. Data Model & Schema

Single flat table — no joins or relational modeling required. Key fields:

| Column | Type | Description |
|---|---|---|
| Attrition | text | Yes/No — whether the employee left |
| Department | text | Sales, R&D, or Human Resources |
| JobRole | text | 9 distinct roles |
| OverTime | text | Yes/No |
| MonthlyIncome | int | Monthly salary |
| DistanceFromHome | int | Distance from home to workplace |
| YearsAtCompany | int | Tenure in years |
| TenureBand | text | Derived: 0-2 / 3-5 / 6-10 / 10+ yrs |

Full dictionary: [`docs/data-dictionary.md`](docs/data-dictionary.md)

## 7. Analysis & Metrics

| Metric | Result |
|---|---|
| Overall attrition rate | 16.1% (237 of 1,470) |
| Attrition — Sales dept | 20.6% |
| Attrition — Sales Representative role | 39.8% |
| Attrition — overtime workers | 30.5% |
| Attrition — non-overtime workers | 10.4% |
| Attrition — 0-2 yrs tenure | 29.8% |
| Attrition — 10+ yrs tenure | 8.1% |
| Attrition — Sales Rep + overtime | 66.7% |

Queries: [`sql/final/`](sql/final)

## 8. Key Insights

1. **Overtime is the dominant attrition driver, not department or pay.** Employees working overtime leave at 30.5% vs. 10.4% for those who don't — and this gap holds across nearly every job role (Sales Rep: 66.7%, Lab Technician: 50.0%, HR: 38.5%), not just one team.
2. **43% of all attrition happens in an employee's first 2 years.** Attrition declines steadily with tenure (29.8% → 13.8% → 12.3% → 8.1%).
3. **Department-level numbers can mask role-level problems.** R&D's department rate (13.8%) looks healthy, but it contains Laboratory Technician at 23.9% — the single largest source of leavers by raw count.
4. Leavers earn ~30% less and live ~19% farther from work on average — a weaker, supporting pattern rather than a headline driver.

## 9. Recommendations

- **Review overtime policy first.** It has the largest, most consistent effect across roles and is the most actionable lever in this dataset.
- **Strengthen onboarding/early-tenure support**, since attrition risk is front-loaded into the first two years.
- **Investigate Sales Representative and Laboratory Technician roles specifically** — both show elevated attrition independent of their department's overall rate.

## 10. Assumptions & Limitations

- This is a public, point-in-time dataset — not a live company export. Findings describe patterns *in this dataset*, not causal proof for any specific organization.
- The standout 66.7% Sales Rep + overtime figure is based on a small subgroup (n=24) — directionally strong, but a few different outcomes would shift the percentage meaningfully.
- No time dimension — the dataset can't show whether attrition is rising, falling, or seasonal.

## 11. Future Enhancements

- Add a predictive model (e.g. logistic regression) to estimate individual attrition risk scores.
- Automate the Excel → SQL → Power BI refresh as a repeatable pipeline if applied to live, recurring data.
- Extend the analysis with a second dataset (e.g. exit-survey text data) to explain *why*, not just *who*.

## 12. Deliverables

- Interactive Power BI dashboard: [`dashboard/`](dashboard)
- SQL analysis queries: [`sql/final/`](sql/final)
- Dashboard screenshots: [`images/`](images)
- Supporting docs: [`docs/`](docs)

## 13. Author

**Aneg Yannick**
Data Analyst | SQL · Power BI · Excel · Python
Buea, Cameroon
[[LinkedIn](https://www.linkedin.com/in/aneg-yannick-19692a432/)] · [[GitHub](https://github.com/yannickaneg23)]
