# HR Metrics Monitoring and Attrition Analysis

**A guided project from DataCamp´s career track: [Data Analyst in Power BI](https://app.datacamp.com/learn/career-tracks/data-analyst-in-power-bi)**


![HR](https://github.com/user-attachments/assets/429b7a45-d183-40f9-8207-8caa972cafee)

Image from [Freepik](https://www.freepik.com/search?format=search&last_filter=query&last_value=HR&query=HR)


---
## Table Of Contents

- [Executive Summary](#executive-summary)
  - [Key Findings](#key-findings)
  - [Recommendations](#recommendations)
- [Introduction](#introduction)
  - [Business Problem](#business-problem)
  - [Goals](#goals)
  - [Data Source](#data-source)
  - [Tools and Techniques](#tools-and-techniques)
- [Methodology](#methodology)
  - [Data Exploration and Cleaning](#data-exploration-and-cleaning)
    - [Data Understanding](#data-understanding)
    - [Data Cleaning](#data-cleaning)
    - [Data Transformation](#data-transformation)
  - [Data Analysis](#data-analysis)
    - [Key Metrics and KPIs](#key-metrics-and-kpis)
    - [Data Visualization](#data-visualization)
- [Results and Implications](#results-and-implications)


---
## Executive Summary
The primary goal of this project was to establish a comprehensive system for monitoring key HR metrics and employee performance. Secondary to this, the project aimed to identify the primary factors influencing employee attrition within the organisation.

By analysing various employee and company data points, I was able to uncover significant insights into the drivers of turnover.

### Key Findings

- The majority of employees are between 20 and 29 years old, and currently the company, Atlas Labs, employs 2.7% more women than men, with a total of 1233 active employees.
- Employees who identify as non-binary make up 8.5% of total employees.
- Employees who identify as white have the highest average salary, whereas mixed or multiple ethnic groups have one of the lowest average salaries. 
- The attrition rate is 16.1% and the department and job role with the highest attrition rate is Sales and Sales Representative, respectively.
- Departmental disparities: Sale consistently experienced higher turnover rates than other departments.
- Managerial rating level and self-performance level do not always align. 

### Recommendations

- Targeted retention programmes: Implement specific retention programmes for departments with high turnover rates.
- Employee satisfaction initiatives: Focus on improving employee satisfaction through various initiatives, such as recognition programmes and flexible work arrangements.
- Managerial development: Provide training and support to managers to enhance their leadership skills and create a positive work environment.
- Compensation and benefits review: Regularly review compensation and benefits packages to ensure they remain competitive and attractive to employees.

By implementing these recommendations, the organisation can significantly improve employee retention and reduce the associated costs of turnover, while also ensuring that HR metrics are effectively monitored and managed.

---

## Introduction

### Business Problem
Atlas Labs is a fictitious software company that is concerned about their employee turnover rates but lacks specific data on the extent of the issue. The key questions to address are: what are the primary reasons for employee turnover? Are there specific departments or job roles with higher turnover rates? How does employee satisfaction correlate with turnover? And, are there any trends or patterns in employee turnover over time?

### Goals
- Monitor HR Metrics: Implement a system to track key HR metrics, such as employee satisfaction, turnover rates, and performance reviews.
- Understand Attrition Factors: Identify the primary factors contributing to employee turnover, including job satisfaction, compensation, management style, and work-life balance.

### Data Source
Data was taken from [DataCamp website](https://app.datacamp.com/learn/courses/case-study-hr-analytics-in-power-bi). It consisted of employee data such as information on demographics, tenure, salary, and performance reviews from 1470 employees.

### Tools and Techniques
Power BI and DAX, Data Modelling, and Exploratory Data Analysis (EDA).

---
## Methodology

###  Data Exploration and Cleaning
#### Data Understanding
Atlas Labs is a fictitious software company. To perform the analysis I used HR records that consisted of five tables:  `EducationLevel`, `Employee`, `RatingLevel`, `SatisfiedLevel`, and `PerformanceRating`. For more details of each dataset review the [Metadata page](metadata.md).

#### Data Cleaning
I initiated a new Power BI report and imported the five CSV datasets. To clarify the table roles, I prepended 'Fact' or 'Dim' to each table name, designating them as either fact or dimension tables. Next, I ensured that the columns were correctly formatted following the [Metadata information](metadata.md). 

#### Data Transformation
I created a new date calculated table using the DAX code from [DimDate.txt](DimDate.txt) file.
![Renaming the columns](https://github.com/user-attachments/assets/b20dcb79-c69c-4187-a0e8-1f70c925edc3)


<br/>Once the data was loaded and cleaned, I generated a data model to establish the relatioships between tables. This image shows the initial data model.
![Initial model](https://github.com/user-attachments/assets/38db1b21-02cc-480a-bea1-857af4841be2)

<br/>This image shows the final data model used for the analysis.
![The final model ](https://github.com/user-attachments/assets/404b18c9-f94a-494a-b5f1-e42d99c90d5e)


#### DAX measures
I created a table with all the measures required for the analysis, and I used DAX formulas to create these measures.

| Measure | Description | DAX code |
| :--- | --- | :--- |
| `TotalEmployees` | The total count of the employees in the company | `TotalEmployees = DISTINCTCOUNT(DimEmployee[EmployeeID])` |
| `ActiveEmployees` | Active employees in the company | `ActiveEmployees = CALCULATE(COUNT(DimEmployee[EmployeeID]), FILTER(DimEmployee, DimEmployee[Attrition] = "No"))` |
| `InactiveEmployees` | Employees that left the company | `InactiveEmployees = CALCULATE(COUNT(DimEmployee[EmployeeID]), FILTER(DimEmployee, DimEmployee[Attrition] = "Yes"))` |
| `% Attrition Rate` | Percentage of Attrition Rate of the company | `% Attrition Rate = DIVIDE([InactiveEmployees], [TotalEmployees])` |
| `TotalEmployeesDate` | Count of total employees by date | `TotalEmployeesDate = CALCULATE([TotalEmployees], USERELATIONSHIP(DimDate[Date], DimEmployee[HireDate]))` |
| `AverageSalary` | Average salary for all employees | `AverageSalary = AVERAGE(DimEmployee[Salary])` |
| `FullName` | To get the full name of each employee | `CONCATENATE(DimEmployee[FirstName], CONCATENATE(" ", DimEmployee[LastName]))` |
| `LastReviewDate` | Displays the last performance review for the selected employee | `LastReviewDate = IF ( MAX ( FactPerformanceRating[ReviewDate] ) = BLANK(), "No Review Yet", MAX ( FactPerformanceRating[ReviewDate] ))` |
| `NextReviewDate` | Calculates when the next performance is due. It should be 365 days after the `LastReviewDate` | `NextReviewDate = VAR review = IF ( MAX ( FactPerformanceRating[ReviewDate] ) = BLANK (), MAX ( DimEmployee[HireDate] ),  MAX ( FactPerformanceRating[ReviewDate] )) RETURN review + 365` |
| `JobSatisfaction` | Gets the max level of employees' satisfaction with their job role. | `JobSatisfaction = MAX(FactPerformanceRating[JobSatisfaction])` |
| `EnvironmentSatisfaction` | Calculates the max rating of employees' satisfaction with their environment | `EnvironmentSatisfaction = CALCULATE ( MAX ( FactPerformanceRating[EnvironmentSatisfaction] ), USERELATIONSHIP ( FactPerformanceRating[EnvironmentSatisfaction], DimSatisfiedLevel[SatisfactionID] ) )` |
| `RelationshipSatisfaction` | Estimates the max level of employees' satisfaction with their relationships at work | `RelationshipSatisfaction = CALCULATE ( MAX (FactPerformanceRating[RelationshipSatisfaction] ), USERELATIONSHIP ( FactPerformanceRating[RelationshipSatisfaction] , DimSatisfiedLevel[SatisfactionID] ) )` |
| `WorkLifeBalance` | Measures the max level of employees' satisfaction with their work-life balance | `WorkLifeBalance = CALCULATE ( MAX (FactPerformanceRating[WorkLifeBalance]), USERELATIONSHIP ( FactPerformanceRating[WorkLifeBalance], DimSatisfiedLevel[SatisfactionID] ) ` |
| `Self_Rating` | Calculates the max rating of employees' performance based on their own view | `Self_Rating = CALCULATE ( MAX (FactPerformanceRating[SelfRating] ), USERELATIONSHIP ( FactPerformanceRating[SelfRating] , DimRatingLevel[RatingID] ))` |
| `Manager_Rating` | Estimates the max rating of employees' performance based on their manager’s view | `Manager_Rating = CALCULATE ( MAX (FactPerformanceRating[ManagerRating]), USERELATIONSHIP ( FactPerformanceRating[ManagerRating] , DimRatingLevel[RatingID] ))` |
| `Inactive_Employees_Date` | Quantifies the number of inactive employees by date | `Inactive_Employees_Date = CALCULATE( [InactiveEmployees], USERELATIONSHIP ( DimDate[Date], DimEmployee[HireDate] ))` |
| `% Attrition Rate Date` | Calculates the rates of attrition base on inactive employees by date | `% Attrition Rate Date = DIVIDE([Inactive_Employees_Date], [TotalEmployeesDate])` |





### Data Analysis
I used a EDA with descriptive statistics to calculate average turnover rates, tenure, and other relevant metrics.

#### Key Metrics and KPIs
Some of the metrics and KPIs I used for this analysis were:
- Employee satisfaction with various aspects of the employee experience, such as job satisfaction, work-life balan, and management.
- Diversity indexes to measure the diversity of the workforce in terms of gender, race, age, and ethnicity.
- Turnover rates to monitor the rate at which employees leave the company.
- Hiring trends over time by calculating the total number of employees by year.


#### Data Visualization
I created a report in Power BI to showcase the results of the analysis. 

_Overview_ describes the high-level metrics about attrition at the company, including total, active and inactive employees, active employees by department and role, attrition rate, and employee hiring trend by year.
![overview](https://github.com/user-attachments/assets/412efe7f-5a8c-42bc-9355-c76717d15257)

<br/> _Demographics_ include plots and metrics related to the demographics, diversity and inclusion of employees
![demographics](https://github.com/user-attachments/assets/4277dd2e-42fc-4204-b166-aca2dae62712)

<br/> _Performance tracker_ is a page to visualise individual employee's performance scores based on yearly performance reviews, 
![performance-tracker](https://github.com/user-attachments/assets/e04c65b2-80fa-4895-a99d-75b5199c69af)

<br/> _Attrition_ displays more in-depth plots related to the attrition rate for the company.
![attrition](https://github.com/user-attachments/assets/021ec719-3e66-4ebd-955a-c9354269a3d4)


### Results and Implications
#### Overview 
- Atlas Labs has a total of 1470 employees, but only 1233 are active employees.
- The attrition rate of Atlas Lab is 16.1%. According to [Onsight Global](https://insightglobal.com/blog/employee-attrition-rate-how-to-calculate-improve/), while it is difficult to define a “good” attrition rate, businesses should generally aim for an attrition rate of 10% or lower. Therefore, it is Atlas Labs should look for strategies to reduce their attrityion rate below 10%
- The hiring trend over time remains stable, between 2012 and 2022, Atlas Labs hired 90 to 130 employees.
- The deparment with the largest number of employees is Technology with 828 employees, followed by Sles with 354, and Human Resoruces with 51.

#### Demographics
- The youngest employee is 18 years old, whereas the oldest is 51.
- Most employees (874) has between 20 to 29 years old.
- Most employees are married (42.5%), followed by single (37.3%), and divorced (20.2%).
- Atlas Labs has more female than male employees.
- The majority of Atlas Labs employees identify as White, and they have the higher salaries among all ethhic groups within the company.

#### Performance tracker
- We can 
- Most employees (874) 
