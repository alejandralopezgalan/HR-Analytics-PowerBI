# HR Metrics Monitoring and Attrition Analysis

**A guided project from DataCamp´s career track: [Data Analyst in Power BI](https://app.datacamp.com/learn/career-tracks/data-analyst-in-power-bi)**


![HR](https://github.com/user-attachments/assets/429b7a45-d183-40f9-8207-8caa972cafee)

Image from [Freepik](https://www.freepik.com/search?format=search&last_filter=query&last_value=HR&query=HR)


---
## Table Of Contents

- [Executive Summary](#executive-summary)
  - [Primary Goal](#primary-goal)
  - [Key Findings](#key-findings)
  - [Recommendations](#recommendations)
- [Introduction](#introduction)
  - [Business Problem](#business-problem)
  - [Goals](#goals)
 - [Methodology](#methodology)
   - [Data Source](#data-source)
    - [Tools and Techniques](#tools-and-techniques)
    - [Data Understanding](#data-understanding)
    - [Data Cleaning](#data-cleaning)
    - [Data Transformation](#data-transformation)
    - [DAX measures](#dax-measures)
    - [Data Analysis](#data-analysis)
    - [Data Visualization](#data-visualization)
- [Results and Implications](#results-and-implications)


---
## Executive Summary
### Primary Goal
To analyse HR metrics and employee performance to identify attrition drivers.

### Key Findings
- __Demographics:__ The majority of employees are between 20 and 29 years old, with a slightly higher proportion (2.7%) of women than men.
- __Attrition:__ The overall attrition rate is 16.1% with Sales Representative roles experiencing the highest turnover.
- __Performance:__ Managerial rating level and self-performance level do not always align. 

### Recommendations

- __Targeted retention programmes:__ Implement specific retention initiatives for departments with high turnover, such as Sales.
- __Employee Satisfaction Initiatives:__ Enhance employee satisfaction through recognition programs, flexible work arrangements, and improved communication.
- __Managerial Development:__ Provide training and support to managers to improve their leadership skills and create a positive work environment.
- __Compensation and Benefits Review:__ Regularly review compensation and benefits packages to ensure they remain competitive.

By implementing these recommendations, Atlas Labs can significantly reduce employee turnover, improve morale, and enhance overall organizational performance.

---

## Introduction

### Business Problem
Atlas Labs is facing a significant challenge with employee turnover, which is negatively impacting productivity, morale, and the bottom line. Understanding the underlying causes of attrition is crucial for developing effective retention strategies.

### Goals
1. Identify the primary factors contributing to employee turnover.
2. Develop targeted retention strategies to address these factors.
3. Improve overall employee satisfaction and engagement.

---
## Methodology

### Data Source
The analysis is based on HR data from Atlas Labs, covering demographics, tenure, salary, performance reviews, and attrition information for 1470 employees. Datasets were obtained through [DataCamp](https://www.datacamp.com/users/sign_in?redirect=http%3A%2F%2Fapp.datacamp.com%2Flearn)

### Tools and Techniques
Power BI, DAX functions, Data Modelling, and Exploratory Data Analysis (EDA).

### Data Understanding
Atlas Labs is a fictitious software company. To perform the analysis I used HR records that consisted of five tables:  `EducationLevel`, `Employee`, `RatingLevel`, `SatisfiedLevel`, and `PerformanceRating`. For more details of each dataset review the [Metadata page](metadata.md).

### Data Cleaning
I initiated a new Power BI report and imported the five CSV datasets. To clarify the table roles, I prepended 'Fact' or 'Dim' to each table name, designating them as either fact or dimension tables. Next, I ensured that the columns were correctly formatted following the [Metadata information](metadata.md). 

### Data Transformation
I created a new date calculated table using the DAX code from [DimDate.txt](DimDate.txt) file.

![Renaming the columns](https://github.com/user-attachments/assets/02f48e75-bc60-4993-9c19-d1053a691830)


<br/>Once the data was loaded and cleaned, I generated a data model to establish the relatioships between tables. This image shows the initial data model.

![Initial model](https://github.com/user-attachments/assets/04fbeee3-acca-46d0-bb47-e3673feb7366)


<br/>This image shows the final data model used for the analysis.

![The final model ](https://github.com/user-attachments/assets/cdd1d69a-a91d-40c3-a34c-f40762366476)

### DAX measures
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

#### Data Analysis
- Exploratory Data Analysis (EDA): I used descriptive statistics and data visualization to identify trends and patterns in the data.
- Key Metrics: Employee satisfaction, turnover rates, diversity indexes, and hiring trends were analyzed.


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

---

### Results and Implications
#### Demographics
- The workforce is relatively young, with a majority of employees aged 20-29.
- While there is a gender imbalance, the company has a diverse workforce in terms of ethnicity.
- Employees who identify as white have the highest average salary, whereas mixed or multiple ethnic groups have one of the lowest average salaries

#### Attrition:
- The overall attrition rate of 16.1% is higher than industry benchmarks ([Onsight Global](https://insightglobal.com/blog/employee-attrition-rate-how-to-calculate-improve/)).
- Sales and Sales Representative roles have significantly higher turnover rates, indicating potential issues with job satisfaction, work-life balance, or management.
- Frequent travelers and overtime workers are most likely to resign, as they have the highest attrition rate.

#### Performance:
- A discrepancy between managerial ratings and self-performance ratings suggests potential misalignment in expectations or performance evaluation processes.

#### Implications:
- High turnover rates can lead to increased costs, decreased productivity, and a negative impact on company culture.
- Addressing the root causes of attrition is essential for improving retention and creating a positive work environment.

