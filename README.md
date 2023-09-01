# AI VARIANT DATA ANALYST INTERNSHIP PROJECT: BANK ANALYTICS

## Table of Contents

1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Key Performance Indicators (KPIs)](#key-performance-indicators-kpis)
4. [Data Collection and Preprocessing](#data-collection-and-preprocessing)
    - [Data Sources](#data-sources)
    - [Data Cleaning and Transformation](#data-cleaning-and-transformation)
5. [Dashboard Creation](#dashboard-creation)
    - [Excel Dashboard](#excel-dashboard)
    - [Power BI Dashboard](#power-bi-dashboard)
    - [Tableau Dashboard](#tableau-dashboard)
6. [SQL Queries and Data Analysis](#sql-queries-and-data-analysis)
    - [Purpose of SQL Queries](#purpose-of-sql-queries)
    - [Query 1: Year-wise Loan Amount Statistics](#query-1-year-wise-loan-amount-statistics)
    - [Query 2: Grade and Sub-grade-wise Revol_bal](#query-2-grade-and-sub-grade-wise-revol_bal)
    - [Query 3: Total Payment for Verified vs. Non-Verified Status](#query-3-total-payment-for-verified-vs-non-verified-status)
    - [Query 4: State-wise and Month-wise Loan Status](#query-4-state-wise-and-month-wise-loan-status)
    - [Query 5: Home Ownership vs. Last Payment Date Statistics](#query-5-home-ownership-vs-last-payment-date-statistics)
    - [SQL Code and Explanation](#sql-code-and-explanation)
    - [Insights from SQL Analysis](#insights-from-sql-analysis)
7. [Usage Instructions](#usage-instructions)
8. [Conclusion](#conclusion)
9. [Contact Information](#contact-information)

## Introduction

Welcome to the AI Variant Data Analyst Internship Project: Bank Analytics! This project was completed during an internship at AI Variant as part of the Data Analyst course at ExcelR. The project involves the analysis of bank loan data and the creation of interactive dashboards using tools like Excel, Power BI, Tableau, and SQL. The primary datasets used are Finance_1.xlsx and Finance_2.xlsx, each containing over 39,000 records of bank loans.

## Project Overview

- **Domain:** Finance
- **Project:** Bank Loan Analysis
- **Datasets:** Finance_1.xlsx & Finance_2.xlsx
- **Dataset Type:** Excel Data
- **Dataset Size:** Each Excel file has 39k+ records

## Key Performance Indicators (KPIs)

The project focuses on five key performance indicators (KPIs) to gain insights from the bank loan data:

1. Year-wise Loan Amount Statistics.

2. Grade and Sub-grade-wise Revol_bal.

3. Total Payment for Verified vs. Non-Verified Status.

4. State-wise and Month-wise Loan Status.

5. Home Ownership vs. Last Payment Date Statistics.

## Data Collection and Preprocessing

### Data Sources

The datasets (Finance_1.xlsx and Finance_2.xlsx).

### Data Cleaning and Transformation

The datasets underwent thorough data preprocessing, including handling missing values, data type conversions, and removing duplicates. 

## Dashboard Creation

### Excel Dashboard

#### Purpose and Objectives

The Excel dashboard was designed to provide an easily accessible platform for visualizing key financial insights from the bank loan data, enabling quick decision-making for stakeholders.

#### Design and Layout

The Excel dashboard features a clean and organized layout, prioritizing clarity and ease of understanding. Color choices were made to enhance visual hierarchy and aid comprehension.

#### Data Visualization Components

The dashboard includes bar charts, line graphs, and pivot tables to represent KPIs such as loan amount trends and payment behaviors. These visualizations were selected for their effectiveness in conveying data patterns.

#### PivotTables and Charts

PivotTables were employed to dynamically summarize and aggregate loan data by various attributes. Charts were linked to PivotTables for interactive updates as filters change.

#### User Interaction Features

The Excel dashboard offers dropdown menus for filtering data by attributes like year and state. Buttons enable users to refresh data or perform specific calculations.

### Power BI Dashboard

#### Purpose and Objectives

The Power BI dashboard was designed to provide an immersive and interactive data exploration experience, aiding in uncovering insights and trends within the bank loan dataset.

#### Design and Layout

The dashboard layout is intuitive, featuring a balanced distribution of visualizations and filtering options. The design prioritizes visual clarity and responsiveness.

#### Data Visualization Components

Visualizations include treemaps, line charts, and slicers, showcasing KPIs like loan status distribution and payment comparisons. They're arranged for easy comprehension.

#### Interactive Filters and Slicers

Users can filter data using slicers for attributes like year, grade, and state. Cross-filtering enables instant updates across visuals based on user selections.

#### Drill-through Features

Users can drill through specific data points to access more detailed information. Clicking on elements like states or categories allows for deeper exploration.

### Tableau Dashboard

#### Purpose and Objectives

The Tableau dashboard aims to provide an interactive platform for comprehensive analysis of bank loan data, facilitating a deeper understanding of loan behaviors and patterns.

#### Design and Layout

The dashboard design follows a clean and modern approach, with strategically positioned visuals for a cohesive data exploration experience.

#### Data Visualization Components

The dashboard integrates scatter plots, bar charts, and a geographic map to depict KPIs such as credit grade relationships and loan status distribution.

#### Interactive Dashboard Features

Users can interact with filters to dynamically adjust visualizations. Hovering over data points reveals detailed tooltips, enhancing user comprehension.

#### Dashboard Actions and Filters

Actions like filter actions and highlight actions enable users to explore correlations and details by interacting with one visualization to affect others.

## SQL Queries and Data Analysis

### Purpose of SQL Queries

SQL queries played a crucial role in deriving valuable insights from the bank loan datasets, enabling the extraction and transformation of data for KPI calculation.

### Query 1: Year-wise Loan Amount Statistics

**SQL Query:**
```sql
     CREATE VIEW YearlyLoanStats AS
     SELECT 
			year(issue_d) as Year,
			FORMAT(sum(loan_amnt) / 1000000, 0) as loan_amount_in_millions
     FROM 
			finance1 
     GROUP BY 
			year
     ORDER BY 
			year;
```
**SQL Query to Print:**
```sql
     #USING VIEWS FOR KPI 1
     SELECT * FROM yearlyloanstats;
```

### Query 2: Grade and Sub-grade-wise Revol_bal
**SQL Query:**
```sql
      CREATE VIEW GradeSubgrade_Revol_Bal AS 
      SELECT 
			f1.grade,
			f1.sub_grade,
			FORMAT(SUM(f2.revol_bal) / 1000000, 0) AS revol_bal_in_millions
      FROM 
			finance1 AS f1
      JOIN 
			finance2 AS f2 ON f2.id = f1.id
      GROUP BY 
			1,2
      ORDER BY 
			1,2;
```

**SQL Query to Print:**
```sql
      #USING VIEWS FOR KPI 2
      SELECT * FROM gradesubgrade_revol_bal;
```

### Query 3: Total Payment for Verified vs. Non-Verified Status
**SQL Query:**

```sql
      CREATE VIEW Total_Payment_Verification_Status_Wise AS
      SELECT 
			verification_status, 
			FORMAT(SUM(total_pymnt) / 1000000, 2) AS Total_payment_in_millions
      FROM 
			finance1 AS f1 
      INNER JOIN 
			finance2 AS f2 ON f1.id = f2.id
      WHERE 
			verification_status IN ( "Not Verified","Verified")
      GROUP BY 
			verification_status
      ORDER BY 
			Total_payment_in_millions;
```

**SQL Query to Print:**
```sql
     #USING VIEWS For KPI 3
     SELECT * FROM Total_Payment_Verification_Status_Wise;
```

### Query 4: State-wise and Month-wise Loan Status
**SQL Query:**
```sql
     CREATE VIEW State_wise_and_month_wise_loan_status AS
     SELECT
			addr_state AS State,
			month(issue_d) as Month,
			MONTHNAME(issue_d) AS Month_Name, 
                        COUNT(loan_status) AS Loan_Status
     FROM 
			finance1
     GROUP BY 
			State,
                        Month,
                        Month_Name
     ORDER BY 
			State,
                        Month;
```

**SQL Query to Print:**
```sql
     #USING VIEWS FOR KPI 4
     SELECT * FROM State_wise_and_month_wise_loan_status;
```

### Query 5: Home Ownership vs. Last Payment Date Statistics
**SQL Query:**

```sql
      CREATE VIEW  Home_ownership_Vs_last_payment_date_stats AS
      SELECT 
			f1.home_ownership, 
			YEAR(f2.last_pymnt_d) AS last_pymnt_year,
			COUNT(f1.id) AS countofid
      FROM 
			finance1 AS f1
      JOIN 
			finance2 AS f2 ON f2.id = f1.id
      GROUP BY 
			1,2
      ORDER BY 
			1,2;
```

**SQL Query to Print:**
```sql
     #USING VIEWS FOR KPI 5
     SELECT * FROM Home_ownership_Vs_last_payment_date_stats;
```


## SQL Code and Explanation
Each query's SQL code is designed to retrieve, aggregate, and calculate specific metrics. Explanation accompanying the code provides clarity on the logic applied and the importance of the query within the project's context.

## Insights from SQL Analysis
The SQL analysis reveals trends such as changing loan amounts over the years, the connection between credit grades and revolving balances, payment behavior differences between verified and non-verified statuses, geographical and temporal loan status patterns, and insights into payment behaviors linked to home ownership.

## Usage Instructions
### Cloning the Repository:
Clone the project repository using the command `git clone https://github.com/your-username/bank-analytics-internship.git` in your preferred terminal. This creates a local copy of the project files on your machine.

### Setting Up Data Sources:
To work with the dashboards and SQL queries, ensure `Finance_1.xlsx` and `Finance_2.xlsx` are available in the designated folders. Open the respective dashboard tools and link these datasets to your project.

### Accessing and Interacting with Dashboards:
1. Open the Excel dashboard by double-clicking `Bank_Analytics_Dashboard.xlsx`.
2. In Power BI, open `Bank_Analytics.pbix`.
3. In Tableau, open `Bank_Analytics.twb`.
4. Navigate through tabs and interact with charts by selecting dropdowns, buttons, and filters.

### Running SQL Queries:
Install a SQL environment MySQL. Copy each SQL query from the documentation and execute it in the SQL environment. Observe the query output for insights.

### Troubleshooting and FAQs:
If dashboards don't load properly, ensure the required software is installed. For SQL query issues, check SQL syntax and ensure data sources are accurately linked. For more FAQs, refer to the documentation.

## Conclusion
### Project Summary
The project involved analyzing bank loan data using Excel, Power BI, and Tableau dashboards, as well as SQL queries. Key insights were derived to understand lending patterns, payment behaviors, and geographic trends.

### Achievements and Insights
The project successfully created interactive dashboards showcasing critical KPIs, revealing trends like year-wise loan amounts, credit grade associations, and payment disparities. Insights contributed to informed decision-making.

### Lessons Learned
Challenges included dataset cleaning and tool-specific nuances. The project highlighted the importance of clear visualization design and thorough data understanding for meaningful analysis.

### Future Enhancements
Future work could involve predictive modeling for loan defaults, advanced SQL analysis, or integration with live data sources to maintain real-time insights.

## Contact Information
- **[Name]:** Devesh Gaonkar
- **[Email Address]:** gaonkardevesh@gmail.com
- **[LinkedIn Profile]:** [Link to Your LinkedIn Profile]
- **[GitHub Repository]:** [Link to the GitHub Repository for This Project]
