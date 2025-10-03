
# 📘 HR Analytics Project Documentation - Atlas Labs

This document explains the data model, analyses, and key performance indicators (KPIs) for the Atlas Labs HR Analytics Dashboard.

## 1\. Data Model & Structure

### 1.1 Data Loading & Preparation

  - **Datasets:** We imported six HR datasets: Employee, Performance, Ratings, Education, Satisfaction, and Dates.
  - **Date Table:** We created a `DimDate` table with Year, Month, Day, and Week fields. This table is marked as the official date table for time-based analysis.
  - **Power Query Transformation (AgeBins):** In Power Query, we created a new column called **AgeBins** to group employees by age into categories: `<20`, `20-29`, `30-39`, `40-49`, `50+`.

### 1.2 Schema Design

  - **Overall Structure:** The data model uses a **Star Schema** with a small "snowflake" part because Education data links through the Employee table.
  - **Fact Table:** `FactPerformanceRating` stores all performance reviews, ratings, satisfaction scores, training details, and review dates.
  - **Dimension Tables:** These tables provide context: `DimEmployee`, `DimEducationLevel`, `DimRatingLevel`, `DimSatisfiedLevel`, and `DimDate`.

### 1.3 Relationships

  - **Core Connections:** The `FactPerformanceRating` table is linked to all main dimension tables.
  - **Inactive Relationships:** We set up inactive relationships to handle different dates (like Hire Date vs. Review Date) when needed for specific calculations.
  - **Performance Relations:** Multiple active relationships were set up from `FactPerformanceRating` to `DimSatisfiedLevel` to analyze different satisfaction measures (Environment, Relationship, WorkLifeBalance).
  -
  - <img width="746" height="736" alt="image" src="https://github.com/user-attachments/assets/3a4c0b1b-c7e5-4dbe-bc25-7f6d9324373e" />


-----

## 2\. Key Measures (DAX KPIs & Advanced Calculations)

| Measure | What it does (Formula Description) | Why it's useful |
| :--- | :--- | :--- |
| **Total Employee** | Simple count of all employees (Initial: **1470**). | Shows the total number of people working at Atlas Labs. |
| **Attrition Rate** | Percentage of inactive employees out of total employees (Initial: **16.12%**). | Measures how many employees leave the company. |
| **TotalEmployeesDate** | `CALCULATE([Total Employee], USERELATIONHIP(DimDate[Date], DimEmployee[HireDate]))` | Counts employees based on their **Hire Date** to see hiring trends over time. |
| **Next Review Date** | `IF(NOT ISBLANK([LastReviewDate]), [LastReviewDate] + 365, [HireDate] + 365)` | Calculates the date of the next annual performance review. (Format: `mm/dd/yyyy`). |

-----

## 3\. Dashboard Pages & Analysis

The dashboard is organized into four main pages (tabs) to cover all aspects of HR analysis logically.

### 3.1 📊 Overview Page

  - **Goal:** To give a quick summary of the workforce status and hiring trends.
  - **Screenshot:**
  - <img width="1248" height="709" alt="image" src="https://github.com/user-attachments/assets/fc266e29-15ef-45e4-9791-497230750cff" />


  - **Key Metrics:**
      - **KPI Cards:** Total Employee (1470), Active Employee (1233), Inactive Employees (237), Attrition Rate (16.12%).
  - **Key Analysis:**
      - **Employee Hiring Trend (Stacked Bar Chart):** Shows how many employees were hired each year, split by whether they stayed or left (Attrition Yes/No).
      - **Active Employee by Department & Job Role (Bar Chart & Treemap):** Displays current employee count by department (Technology has the most) and a detailed breakdown by job role.

### 3.2 👤 Demographics Page

  - **Goal:** To understand the employee population based on age, gender, ethnicity, and marital status.
  - **Screenshot:**
  - <img width="1249" height="713" alt="image" src="https://github.com/user-attachments/assets/90544e23-4094-463d-abbb-4b59a7d9c9f4" />

  - **Key Analysis:**
      - **Age & Gender:** How employees are distributed across **AgeBins** (most are 20-29) and by gender percentage.
      - **Marital Status (Donut Chart):** Shows the percentage of married, single, or divorced employees.
      - **Ethnicity & Salary (Column & Line Chart):** Compares employee numbers and average salaries across different ethnic groups.
  - **Key Insights Uncovered:**
    1.  Most employees are between 20-29 years old.
    2.  Atlas Labs employs 2.7% more women than men.
    3.  Employees who identify as Non-binary make up 8.5% of total employees.
    4.  White employees have the highest average salary.

### 3.3 📉 Attrition Page

  - **Goal:** To find out why employees leave and which groups are most likely to leave, helping us create better strategies to keep them.
  - **Screenshot:**
  - <img width="1243" height="710" alt="image" src="https://github.com/user-attachments/assets/0ee9e82a-1dea-4d3e-a906-fc448429188b" />

  - **Key Analysis:**
      - **Attrition by Job & Department:** Shows which job roles and departments have the highest attrition rates (e.g., Sales Representative and Recruiter are high-risk).
      - **Attrition by Environmental Factors:** Looks at how **Travel Frequency** (more travel = higher attrition) and **Overtime** (more overtime = much higher attrition) affect attrition.
      - **Attrition by Tenure:** Shows that **newer employees (under 2-3 years)** are the most likely to leave.

### 3.4 📈 Performance Tracker Page

  - **Goal:** To let managers and HR track an individual employee's performance and satisfaction trends over time.
  - **Screenshot:**
  - <img width="1250" height="713" alt="image" src="https://github.com/user-attachments/assets/b285f20b-4ff7-4111-9250-f760f0209f8a" />

  - **Key Features:** An **Employee Selector** to pick any employee, and **Date Cards** showing Start Date, Last Review, and the calculated **Next Review Date**.
  - **Key Visualizations (Line Charts):** Tracks yearly trends for six metrics:
      - **Performance:** Self Rating, Manager Rating.
      - **Satisfaction:** Job, Relationship, Environment, and Work Life Balance.

-----

### 4\. Final Design Note

  - **Consistency:** The dashboard has a clean, modern look with a consistent color scheme (using Atlas Labs' green and blue branding).
  - **Hierarchy:** The top row always shows **KPI cards** for quick executive summaries.

-----
