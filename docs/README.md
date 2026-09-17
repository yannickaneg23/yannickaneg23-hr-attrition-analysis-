# Documentation

  # Methodology

**1. Data cleaning (Excel)**
- Converted raw data to an Excel Table
- Dropped 3 constant-value columns with no analytical value: EmployeeCount, Over18, StandardHours
- Verified data types (numeric columns confirmed as numbers, not text)
- Verified zero nulls and zero duplicate rows
- Added a derived `TenureBand` column to group YearsAtCompany into ranges

**2. Analysis (SQL / MySQL)**
- Imported cleaned data into MySQL Workbench
- Wrote queries to answer each business question: attrition rate by department, job role, overtime status, tenure band, and an income/distance comparison
- Ran a follow-up overlap query (JobRole × OverTime) after the single-factor queries revealed overtime as a strong driver — this surfaced the strongest finding in the dataset

**3. Visualization (Power BI)**
- Built a 2-page interactive dashboard: Overview (KPIs, department/role breakdown) and Risk Drivers (overtime, tenure, role×overtime matrix)
- Used conditional formatting on the matrix to visually highlight the highest-risk combination
