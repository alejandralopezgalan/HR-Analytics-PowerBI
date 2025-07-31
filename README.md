# HR Metrics Monitoring and Attrition Analysis

**A guided project from DataCamp´s career track: [Data Analyst in Power BI](https://app.datacamp.com/learn/career-tracks/data-analyst-in-power-bi)**


![HR](assets/img/project1-HRportada.png)

Image from [Freepik](https://www.freepik.com/search?format=search&last_filter=query&last_value=HR&query=HR)


## Table Of Contents

- [Executive Summary](#executive-summary)
  - [Primary Goal](#primary-goal)
  - [Key Findings and Recommendations](#key-findings-and-recommendations)

- [Introduction](#introduction)
  - [Business Problem](#business-problem)
  - [Goals](#goals)
- [Methodology](#methodology)
  - [Data Source](#data-source)
  - [Tools and Techniques](#tools-and-techniques)
  - [Data Understanding](#data-understanding)
  - [Data Cleaning](#data-cleaning)
  - [Data Transformation and Modelling](#data-transformation-and-modelling)
  - [DAX measures](#dax-measures)
  - [Data Analysis](#data-analysis)
  - [Data Visualisation](#data-visualisation)
- [Results and Implications](#results-and-implications)
  - [Demographics](#demographics)
  - [Attrition](#attrition)
  - [Performance](#performance)
  - [Implications](#implications)
- [Power BI Report](#power-bi-report)


## Executive Summary
### Primary Goal
Analyse Human Resources (HR) metrics and employee performance to identify attrition drivers.

### Key Findings and Recommendations 

<table>
    <thead>
        <tr>
            <th>Finding</th>
            <th>Recommendation</th>
            <th>Plot</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The company has 1,470 employees, with 1,233 active and 237 inactive.</td>
            <td>Review inactive staff profiles to understand the underlying reasons and consider redeployment or re-engagement initiatives to maximise workforce efficiency.</td>
            <td><img src="assets/img/project1_fiding1.png"></img></td>
        </tr>
        <tr>
            <td>The Technology department is the largest, followed by Sales and HR.</td>
            <td>Prioritise initiatives in Technology and foster collaboration with Sales and HR to align efforts and boost performance.</td>
            <td><img src="assets/img/project1_fiding2.png"></img></td>
        </tr>
        <tr>
	          <td>Attrition rate was 16.1%, with Sales Representatives experiencing the highest turnover (39.8%).</td>	
            <td>Examine the reasons behind the 39.8% attrition rate in Sales and introduce retention strategies such as enhanced pay structures, professional development initiatives, or improved workload management.</td>
            <td><img src="assets/img/project1_fiding3.png"></img></td>
        </tr>
	       <tr>
            <td rowspan=2>Frequent travelers, those hired in 2020, and employees working overtime have higher attrition rates.</td>
	          <td> Consider flexible working arrangements or travel incentives to reduce the 24.9% attrition rate among employees who travel frequently.</td>
            <td><img src="assets/img/project1_fiding4.png"></img> </td>
        </tr>
	       <tr>
            <td> High attrition among employees required to work overtime (30.5%) suggests a need for better work-life balance measures, such as adjusted overtime pay or additional leave benefits.</td>
            <td><img src="assets/img/project1_fiding5.png"></img> </td>
        </tr>
	       <tr>
            <td rowspan=2>The workforce is predominantly young (20-29), mostly married, and female-dominated.</td>
	          <td>Focus on career progression opportunities and mentorship schemes to encourage long-term retention among young employees.</td>
            <td><img src="assets/img/project1_fiding6.png"></img> </td>
        </tr>
        <tr>
	          <td>With a female-majority workforce, assess leadership development programmes to ensure equal growth opportunities for all employees.</td>
            <td><img src="assets/img/project1_fiding7.png"></img> </td>
        </tr>
          <tr>
            <td>White employees form the largest ethnic group, earning an average salary of $115K.</td>
	          <td>While white employees form the majority, further investment in diversity, equity, and inclusion (DEI) initiatives could strengthen workplace culture.</td>
            <td><img src="assets/img/project1_fiding8.png"></img> </td>
        </tr>
    </tbody>
</table> 

## Introduction

### Business Problem
Atlas Labs is facing a significant challenge with employee turnover, which is negatively impacting productivity, morale, and the bottom line. Understanding the underlying causes of attrition is crucial for developing effective retention strategies.

### Goals
1. Identify the primary factors contributing to employee turnover.
2. Develop targeted retention strategies to address these factors.
3. Improve overall employee satisfaction and engagement.


## Methodology

### Data Source
The analysis is based on HR data from Atlas Labs, covering demographics, tenure, salary, performance reviews, and attrition information for 1470 employees. Datasets were obtained through [DataCamp](https://www.datacamp.com/users/sign_in?redirect=http%3A%2F%2Fapp.datacamp.com%2Flearn)

### Tools and Techniques
Power BI, DAX functions, Data Modelling, and Exploratory Data Analysis (EDA).

### Data Understanding
tlas Labs is a fictitious software company. To perform the analysis, I used HR records that consisted of five tables: `EducationLevel`, `Employee`, `RatingLevel`, `SatisfiedLevel`, and `PerformanceRating`. For more details on each dataset, please review the [Metadata](assets/datasets/metadata.md) page.

### Data Cleaning
I initiated a new Power BI report and imported the five CSV datasets. To clarify the table roles, I prepended 'Fact' or 'Dim' to each table name, designating them as either fact or dimension tables. I then ensured that the columns were correctly formatted according to the [Metadata](assets/datasets/metadata.md) information. 

### Data Transformation and Modelling
I created a new calculated date table using the DAX code from the [DimDate.txt](assets/datasets/DimDate.txt) file.

![Renaming the columns](assets/img/project1-rename-columns.png)


<br/>Once the data was loaded and cleaned, I generated a data model to establish the relatioships between tables. This image shows the initial data model.

![Initial model](assets/img/project1-initial-model.png)


<br/>Final data model: The image below illustrates the final data model used for the analysis.

![The final model](assets/img/project1-model-hr-powerbi.png)

### DAX measures
I created a table containing all the necessary measures for the analysis, utilising DAX formulas to generate these measures.

**1. `TotalEmployees`** <br />
Calculates the overall number of employees currently working at the company.
```sql  
TotalEmployees =
  DISTINCTCOUNT(DimEmployee[EmployeeID])
```

**2. `ActiveEmployees`** <br />
Shows the count of employees who are currently employed and active in the company.
```sql  
ActiveEmployees =
  CALCULATE(COUNT(DimEmployee[EmployeeID]),
    FILTER(DimEmployee, DimEmployee[Attrition] = "No"))
```

**3. `InactiveEmployees`** <br />
Counts the number of employees who have left the company.
```sql  
InactiveEmployees =
  CALCULATE(COUNT(DimEmployee[EmployeeID]),
    FILTER(DimEmployee, DimEmployee[Attrition] = "Yes"))
```

**4. `% Attrition Rate`** <br />
Calculates the percentage of employees who have left the company relative to the total number of employees.
```sql  
% Attrition Rate =
  DIVIDE([InactiveEmployees], [TotalEmployees])
```

**5. `TotalEmployeesDate`** <br />
Displays the total count of employees on specific dates.
```sql  
TotalEmployeesDate =
  CALCULATE([TotalEmployees],
    USERELATIONSHIP(DimDate[Date], DimEmployee[HireDate]))
```

**6. `AverageSalary`** <br />
Provides the average salary of all employees in the company.
```sql  
AverageSalary =
  AVERAGE(DimEmployee[Salary])
```

**7. `FullName`** <br />
Combines first names and last names to get the full name of each employee.
```sql  
FullName =
  CONCATENATE(DimEmployee[FirstName], CONCATENATE(" ", DimEmployee[LastName]))
```

**8. `LastReviewDate`** <br />
Displays the date of the most recent performance review for a selected employee.
```sql  
LastReviewDate =
  IF (MAX (FactPerformanceRating[ReviewDate]) = BLANK(),
    "No Review Yet", MAX(FactPerformanceRating[ReviewDate]))
```

**9. `NextReviewDate`** <br />
Calculates the date for the next performance review, which should be 365 days after the `LastReviewDate`.
```sql  
NextReviewDate =
  VAR review = IF(MAX(FactPerformanceRating[ReviewDate]) = BLANK (),
    MAX(DimEmployee[HireDate]), MAX(FactPerformanceRating[ReviewDate] ))

  RETURN review + 365
```

**10. `JobSatisfaction`** <br />
Shows the highest level of satisfaction employees have with their job roles.
```sql  
JobSatisfaction =
  MAX(FactPerformanceRating[JobSatisfaction])
```

**11. `EnvironmentSatisfaction`** <br />
Shows the highest rating of employees’ satisfaction with their work environment.
```sql  
EnvironmentSatisfaction =
  CALCULATE(MAX(FactPerformanceRating[EnvironmentSatisfaction]),
    USERELATIONSHIP(FactPerformanceRating[EnvironmentSatisfaction],
      DimSatisfiedLevel[SatisfactionID]))
```

**12. `RelationshipSatisfaction`** <br />
Estimates the highest level of satisfaction employees have with their relationships at work.
```sql  
RelationshipSatisfaction =
  CALCULATE(MAX(FactPerformanceRating[RelationshipSatisfaction]),
    USERELATIONSHIP(FactPerformanceRating[RelationshipSatisfaction], DimSatisfiedLevel[SatisfactionID]))
```

**13. `WorkLifeBalance`** <br />
Measures the highest level of satisfaction employees have with their work-life balance.
```sql  
WorkLifeBalance =
  CALCULATE(MAX(FactPerformanceRating[WorkLifeBalance]),
    USERELATIONSHIP(FactPerformanceRating[WorkLifeBalance], DimSatisfiedLevel[SatisfactionID])
```

**14. `SelfRating`** <br />
Calculates the highest rating of employee performance based on their own self-assessment.
```sql  
SelfRating =
  CALCULATE(MAX (FactPerformanceRating[SelfRating]),
    USERELATIONSHIP(FactPerformanceRating[SelfRating], DimRatingLevel[RatingID]))
```

**15. `ManagerRating`** <br />
Calculates the highest rating of employee performance based on their manager’s assessment.
```sql  
ManagerRating =
  CALCULATE(MAX(FactPerformanceRating[ManagerRating]),
    USERELATIONSHIP(FactPerformanceRating[ManagerRating], DimRatingLevel[RatingID]))
```

**16. `InactiveEmployeesDate`** <br />
Quantifies the number of inactive employees on specific dates.
```sql  
InactiveEmployeesDate =
  CALCULATE([InactiveEmployees],
    USERELATIONSHIP(DimDate[Date], DimEmployee[HireDate]))
```

**17. `%AttritionRateDate`** <br />
Calculates the attrition rates based on the number of inactive employees on specific dates.
```sql  
%AttritionRateDate =
  DIVIDE([Inactive_Employees_Date], [TotalEmployeesDate])
```

### Data Analysis
- Exploratory Data Analysis (EDA): I used descriptive statistics and data visualisation to identify trends and patterns in the data.
- Key Metrics: Analysed employee satisfaction, turnover rates, diversity indexes, and hiring trends.


### Data Visualisation
I created a report in Power BI to showcase the results of the analysis.

_Overview_ provides a high-level summary of key metrics related to attrition at the company, including:
- Total Employees: The organisation currently comprises 1,470 employees in total.
- Active and Inactive Employees: Among the workforce, 1,233 employees are actively engaged, while 237 are classified as inactive, providing a clear picture of current operational capacity.
- Employees by Department and Role: The distribution of active employees across departments highlights the dominance of the Technology department, with 828 employees. This is followed by Sales (354 employees) and Human Resources (51 employees), emphasising the structural composition of the workforce.
- Attrition Rate: The current attrition rate stands at 16.1%, reflecting the proportion of employees leaving the organisation.
- Hiring Trend by Year: An analysis of hiring trends from 2012 to 2022 reveals key insights into employee retention. Among all hiring years, 2020 saw the highest number of employees who left the company, highlighting its unique attrition pattern.
  
![overview](assets/img/project1-overview.png)

The interactive elements of the Overview page in my Power BI report enable users to explore various metrics and trends related to employee attrition. These include:
- Viewing the total number of employees and their status (active/inactive).
- Exploring the distribution of employees by department and role.
- Analysing the attrition rate and observing hiring trends over the years.

![overview_gif](assets/img/project1_overview_video.gif)


<br/> _Demographics_ page includes key statistics such as:
- Age Distribution: Employees range in age from 18 to 51 years, with the majority (874 employees) falling within the 20-29 age group. This highlights a workforce predominantly comprised of younger professionals.
- Marital Status: The largest demographic by marital status is married (624 employees). This is followed by single employees (549) and those who are divorced (297).
- Gender Distribution: The company’s workforce is predominantly female, with a significant concentration of women aged between 30 and 39 years. Gender representation across other age groups is evenly distributed.
- Ethnicity: The majority of employees identify as white, with an average salary of approximately $115K. Other ethnic groups contribute to the company’s diversity, each with their respective salary averages.
  
![demographics](assets/img/project1-demographics.png)

The interactive features of the Demographics dashboard allow users to explore various demographic metrics in depth, providing detailed insights into employee data.

![demographics_gif](assets/img/project1_demographics_video.gif)

<br/> _Performance Tracker_ is a page designed to visualise individual employee performance scores based on yearly performance reviews. The dashboard includes various elements such as:
- KPIs: Key Performance Indicators of individual scores on different criteria such as job satisfaction, environment satisfaction, relationship satisfaction, and work-life balance across multiple years.
- Review Records: Individual records including the start date (when the employee began working at the company), the date of the last review, and the due date for the next review
- Comparison Charts: Visual comparisons of self-performance metrics and those evaluated by their managers.

![performance-tracker](assets/img/project1-performance.png)

Users can select an individual employee to view detailed performance scores and review records.

![performance-gif](assets/img/project1_performance_video.gif)

<br/> _Attrition_ is a page that displays in-depth plots related to the company's attrition rate. The dashboard includes various elements such as:
- Overall Attrition Rate: The overall percentage of employees leaving the company, which is 16.1%.
- Atrition by Job Role: Attrition rates segmented by different departments and job roles. Sales Representatives have the highest attrition rate at 39.8%.
- Attrition by Travel Frequency: Attrition rates based on employees' travel frequency. Frequent travellers have the highest attrition rate at 24.9%.
- Attrition by Hire Date: Analysis of attrition rates by employee hire dates. The highest attrition rate was in 2020 at 22.0%, followed by 2016 with 21.1%.
- Attrition by Overtime Requirement: Attrition rates related with overtime work requirements. Employees with overtime requirements have the highest attrition rate at 30.5%.
- Attrition by Tenure: Breakdown of attrition rates based on employees' length of tenure at the company. The majority of attrition occurs among employees with less than two years of tenure.

![attrition](assets/img/project1-attrition.png)

Users can interact with the dashboard to delve into these metrics and gain comprehensive insights into employee turnover.

![attrition_gif](assets/img/project1_attrition_video.gif)

## Results and Implications
### Demographics
- The workforce is relatively young, with the majority of employees aged 20-29.
- Despite a gender imbalance, the company has a diverse workforce in terms of ethnicity.
- Employees identifying as white have the highest average salary, whereas those from mixed or multiple ethnic groups have one of the lowest average salaries.

### Attrition:
- The overall attrition rate is 16.1%, which is higher than industry benchmarks ([Onsight Global](https://insightglobal.com/blog/employee-attrition-rate-how-to-calculate-improve/)).
- Sales Representative roles experience significantly higher turnover rates, indicating potential issues with job satisfaction, work-life balance, or management.
- Frequent travellers and employees who work overtime are the most likely to resign, as they have the highest attrition rates.

### Performance:
- There is a discrepancy between managerial ratings and self-performance ratings, suggesting potential misalignment in expectations or performance evaluation processes.

### Implications:
- High turnover rates can lead to increased costs, decreased productivity, and a negative impact on company culture.
- Addressing the root causes of attrition is essential for improving retention and creating a positive work environment.

## PowerBi Report
To review the analysis in detail, you can download the Power BI Report [here](assets/report/Report-HR-analytics-in-PowerBI.pbix)

