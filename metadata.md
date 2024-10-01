# Metadata

## EducationLevel
| Column | Description | Datatype |
| :--- | --- | --- |
| EducationLevelID | A unique ID that connects to Education in DimEmployee | Number |
| EducationLevel | Provides meaning to the education level: Doctorate, Masters, Bachelors, High School, and No Formal Qualification | Text |

---
## Employee
| Column | Description | Datatype |
| :--- | --- | --- |
| EmployeelID | A unique ID that identifies an employee | Text |
| FirstName | First name of an employee | Text |
| LastName | Last name of an employee | Text |
| Gender | Self-defined employee gender identity | Text |
| Age | Current age of an employee | Number |
| BusinessTravel | Frequency of business travel - three categories: Frequent Traveller, Some Travel, and No Travel | Text |
| Department | Department an employee works in - three categories: Technology, HR, and Sales | Text |
| DistanceFromHome | Kilometer distance between an employee’s home and their office | Text |
| State | State where the employee lives | Text |
| Ethnicity | Self-defined employee ethnicity | Text |
| Education | Education level for employees'. Connects to DimEducationLevel | Number |
| EducationField | Employee field of study | Text |
| JobRole | Current/latest employee job role | Text |
| MaritalStatus | Current/latest employee marital status | Text |
| Salary | Current/latest employee salary | Number |
| StockOptionLevel | The banding level for stock options that the employee has | Number |
| Overtime | Contains "Yes" and "No" to indicate whether an employee is expected to work overtime in their role | Text |
| HireDate | Date the employee joined the company | Date |
| Attrition | Contains "Yes" and "No" to indicate whether an employee has left the organisation | Text |
| YearsAtCompany | Number of years since the employee joined the organisation | Number |
| YearsInMostRecentRole | Number of years the employee has been in their most recent role | Number |
| YearsSinceLastPromotion | Number of years since the employee last got promoted | Number |
| YearsWithCurrManager | Number of years the employee has been with their current manager | Number |

---
## PerformanceRating
| Column | Description | Datatype |
| :--- | --- | --- |
| PerformancelID | A unique ID that identifies an individual performance review | Text |
| EmployeeID | A unique ID that identifies an employee. Connects to DimEmployee | Text |
| ReviewDate | Date an employee’s review took place | Date |
| EnvironmentSatisfaction | Rating for employees' satisfaction with their environment. Connects to DimSatisfiedLevel | Text |
| JobSatisfaction | Rating for employees' satisfaction with their job role. Connects to DimSatisfiedLevel | Number |
| RelationshipSatisfaction | Rating for employees' satisfaction with their relationships at work. Connects to DimSatisfiedLevel | Number |
| WorkLifeBalance | Rating for employees' satisfaction with their work-life balance. Connects to DimSatisfiedLevel | Number |
| SelfRating | Rating for employees' performance based on their own view. Connects to DimRatingLevel | Number |
| ManagerRating | Rating for employees' performance based on their manager’s view. Connects to DimRatingLevel | Number |
| TrainingOpportunitiesWithinYear | Number of training opportunities offered in the last 12 months | Number |
| TrainingOpportunitiesTaken | Number of training opportunities taken | Number |

---
## RatingLevel
| Column | Description | Datatype |
| :--- | --- | --- |
| RatingID | A unique ID that connects to SelfRating and ManagerRating in FactPerformanceRating | Number |
| RatingLevel | Provides meaning to the rating level: Above and Beyond, Exceeds Expectation, Meets Expectation, Needs Improvement, and Unacceptable | Text |

---
## SatisfiedLevel
| Column | Description | Datatype |
| :--- | --- | --- |
| SatisfactionID | A unique ID that connects to EnvironmentSatisfaction, JobSatisfaction, RelationshipSatisfaction, and Work-Life Balance in FactPerformanceRating | Number |
| SatisfactionLevel | Provides meaning to the satisfaction level: Very Satisfied, Satisfied, Neutral, Dissatisfied, and Very Dissatisfied | text |

