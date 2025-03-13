# Power BI Project: HR Analytics In Power BI

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiYzMyNTIzYTMtOTgyYy00YmE1LWIxNDktYTI1ZTkyMDcxYmIyIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>


## 📌 Project Overview  
The Project interface for HR Analytics highlights various analytical features powered by Microsoft Power BI. It emphasizes customizable dashboards, real-time monitoring, and data-driven decision-making in HR management.
---

## 📖 Table of Contents  
- [Dataset Details](#-dataset-details)  
- [Data Preprocessing](#-data-preprocessing)  
- [Data Modeling](#-data-modeling)  
- [DAX Measures](#-dax-measures)  
- [Dashboard & Visualizations](#-dashboard--visualizations)  
- [Key Findings](#-key-findings)  
- [Recommendations](#-recommendations)  
- [Project Impact](#-project-impact)  

---

## 📂 Dataset Details  

### Columns in the Dataset:  
- **Fact_Performance_Table:** Performance_ID, Employee_ID, Review_Date, Environment_Satisfaction, Job_Satisfaction, Relationship_Satisfaction, 
                              Training_Opportunities_Within_Year,   Training_Opportunities_Taken, Work_LifeBalance, Self_Rating, ManagerRating, 
- **Fact_Performance_Rating_upvote:** Performance_ID, Employee_ID, Review_Date, Satisfaction Type, Satisfaction Score, Training_Opportunities_WithinYear, 
                              TrainingOpportunitiesTaken
- **Salary:** EmployeeID, FirstName, LastName, Gender, Age, BusinessTravel, Department, DistanceFromHome (KM), State, Ethnicity, Education, EducationField, JobRole, 
                           MaritalStatus, Salary, StockOptionLevel, OverTime, HireDate, Attrition, YearsAtCompany, YearsInMostRecentRole, YearsSinceLastPromotion, 
                           YearsWithCurrManager, AgeBins, Salary Bin. 
- **Dim_Education_Level:** EducationLevel_ID, EducationLevel.
- **CapexComponents:** Component, Amount, Department.
- **LeaveRecords:** EmployeeLeave, Leave_Start_Date, Leave_End_Date, Leave_Type, Leave_Duration  

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

### Steps Taken:  
#### **Transaction Fact Table Preprocessing:**  
- **Promote Headers:** Ensure the first row is used as column headers.  
- **Change Data Types:** Convert Transaction_ID to Int64, and other relevant columns to text or date.  
- **Remove Errors:** Eliminate rows with errors in Transaction_ID.  
- **Drop Unnecessary Columns:** Remove unused columns.  
- **Replace Null Values:** Fill missing values in Agent_ID and Merchant_ID with "Not found".  
- **Sort Transactions:** Arrange by Date in ascending order.  
- **Standardize Data Types:** Convert ID fields to text.  
- **Fill Missing Data:** Replace null values with default values.  
- **Parameter Create:** 
1.Did not Met Expectation & Emerging Potential = GENERATESERIES(0, 0.25, 0.01)

2.Did not Met Expectation & High Potential = GENERATESERIES(0, 0.25, 0.01)

3.Did not Met Expectation & Low Potential = GENERATESERIES(0, 0.25, 0.01)
                       
 4.Create_Dim_Data table Using Dax Formula.
                       
 5.Exceeded Expectations & Emerging Potential = GENERATESERIES(0, 0.25, 0.01)
                        
6.Exceeded Expectations & High Potential = GENERATESERIES(0, 0.25, 0.01)
                        
7.Exceeded Expectations & Low Potential = GENERATESERIES(0, 0.25, 0.01)
                        
8.Met Expectations & Emerging Potential = GENERATESERIES(0, 0.25, 0.01)
                       
 9.Met Expectations & High Potential = GENERATESERIES(0, 0.25, 0.01)
                      
 10. Salary and Tenure measure:
```DAX
      SALARY & TENURE = {
      ("TENURE", NAMEOF('DimEmployee'[YearsAtCompany]), 0),
      ("SALARY", NAMEOF('DimEmployee'[Salary Bin]), 1),
      ("AGE", NAMEOF('DimEmployee'[AgeBins]), 2)
       }
```

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/5925ea14-2317-4edc-a215-fb669bbebf63)


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### General Measures  
```DAX
% Attrition Rate = DIVIDE([InactiveEmployees], [TotalEmployees])
)
```
```DAX
% Attrition Rate Date = DIVIDE([InactiveEmployeesDate], [TotalEmployeesDate])
```
```DAX
2 Dynamic Variance % = 
//this data is static, so we get the latest values by getting the max in the data table. Date tables are really useful for their "relative" columns to keep things dynamic - this calculation would auto-update as time passed if fresh data was coming in.
//WEEK

var formattedWeekAttr = IF([2 WOW % Attrition rate] < 1, "▼ " & FORMAT([2 WOW % Attrition rate] * -1, "0.00%"), "▲" & FORMAT([2 WOW % Attrition rate], "0.0%")) & " vs previous week"

//MONTH

var formattedMonthAttr = IF([2 MOM % Attrition rate] < 1, "▼ " & FORMAT([2 MOM % Attrition rate] * -1, "0.00%"), "▲" & FORMAT([2 MOM % Attrition rate], "0.0%")) & " vs previous month"

//QUARTER

var formattedQuarterAttr = IF([2 QOQ % Attrition rate] < 1, "▼ " & FORMAT([2 QOQ % Attrition rate] * -1, "0.00%"), "▲" & FORMAT([2 QOQ % Attrition rate], "0.0%")) & " vs previous quarter"

//YEAR

var formattedYearAttr = IF([2 YOY % Attrition rate]< 1, "▼ " & FORMAT([2 YOY % Attrition rate] * -1, "0.00%"), "▲" & FORMAT([2 YOY % Attrition rate], "0.00%")) & " vs previous year"


return SWITCH(
        TRUE(),
            SELECTEDVALUE('DATE'[DATE  Order]) = 0, formattedWeekAttr,  //0 is the first field parameter row, for some reason it will give an error when trying to use the text W/M/Q/Y column instead
            SELECTEDVALUE('DATE'[DATE  Order]) = 1, formattedMonthAttr,
            SELECTEDVALUE('DATE'[DATE  Order]) = 2, formattedQuarterAttr,
            SELECTEDVALUE('DATE'[DATE  Order]) = 3, formattedYearAttr,
            BLANK()
)
```
```DAX
3 Dynamic Variance % = 
//this data is static, so we get the latest values by getting the max in the data table. Date tables are really useful for their "relative" columns to keep things dynamic - this calculation would auto-update as time passed if fresh data was coming in.
//WEEK
var latestWeek = CALCULATE(MAX('DIMDate'[WeekRelative]), 'DIMDate'[Date] = MAX(DimEmployee[HireDate]))
var priorWeekAttr = CALCULATE([% Attrition Rate Date], 'DIMDate'[WeekRelative] = latestWeek - 7)
var latestWeekAttr = CALCULATE([% Attrition Rate Date], 'DIMDate'[WeekRelative] = latestWeek)
var weekAttrChange = DIVIDE(latestWeekAttr - priorWeekAttr, priorWeekAttr)
var formattedWeekAttr = IF(weekAttrChange < 1, "▼ " & FORMAT(weekAttrChange * -1, "0.00%"), "▲" & FORMAT(weekAttrChange, "0.00%")) & " vs previous week"

//MONTH
var latestMonth = CALCULATE(MAX('DimDate'[MonthRelative]), 'DIMDate'[Date] = MAX(DimEmployee[HireDate]))
var priorMonthAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[MonthRelative] = latestMonth - 1)
var latestMonthAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[MonthRelative] = latestMonth)
var monthAttrChange = DIVIDE(latestMonthAttr - priorMonthAttr, priorMonthAttr)
var formattedMonthAttr = IF(monthAttrChange < 1, "▼ " & FORMAT(monthAttrChange * -1, "0.00%"), "▲" & FORMAT(monthAttrChange, "0.00%")) & " vs previous month"

//QUARTER
var latestQuarter = CALCULATE(MAX('DimDate'[QuarterRelative]), 'DIMDate'[Date] = MAX(DimEmployee[HireDate]))
var priorQuarterAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[QuarterRelative] = latestQuarter - 1)
var latestQuarterAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[QuarterRelative] = latestQuarter)
var quarterAttrChange = DIVIDE(latestQuarterAttr - priorQuarterAttr, priorQuarterAttr)
var formattedQuarterAttr = IF(quarterAttrChange < 1, "▼ " & FORMAT(quarterAttrChange * -1, "0.00%"), "▲" & FORMAT(quarterAttrChange, "0.00%")) & " vs previous quarter"

//YEAR
var latestYear = CALCULATE(MAX('DimDate'[YearRelative]), 'DIMDate'[Date] = MAX(DimEmployee[HireDate]))
var priorYearAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[YearRelative] = latestYear - 1)
var latestYearAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[YearRelative] = latestYear)
var yearAttrChange = DIVIDE(latestYearAttr - priorYearAttr, priorYearAttr)
var formattedYearAttr = IF(yearAttrChange < 1, "▼ " & FORMAT(yearAttrChange * -1, "0.00%"), "▲" & FORMAT(yearAttrChange, "0.00%")) & " vs previous year"


return SWITCH(
        TRUE(),
            SELECTEDVALUE('DATE'[DATE  Order]) = 0, formattedWeekAttr,  //0 is the first field parameter row, for some reason it will give an error when trying to use the text W/M/Q/Y column instead
            SELECTEDVALUE('DATE'[DATE  Order]) = 1, formattedMonthAttr,
            SELECTEDVALUE('DATE'[DATE  Order]) = 2, formattedQuarterAttr,
            SELECTEDVALUE('DATE'[DATE  Order]) = 3, formattedYearAttr,
            BLANK()
)
```
```DAX
4 Dynamic Variance % = 

// WEEK


// MONTH
var latestMonth = CALCULATE(MAX('DimDate'[MonthRelative]), 'DimDate'[Date] = MAX(DimEmployee[HireDate]))
var priorMonthAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[MonthRelative] = latestMonth - 1)
var latestMonthAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[MonthRelative] = latestMonth)
var monthAttrChange = DIVIDE(latestMonthAttr - priorMonthAttr, priorMonthAttr)
var formattedMonthAttr = IF(monthAttrChange < 0, "▼ " & FORMAT(ABS(monthAttrChange), "0.00%"), "▲ " & FORMAT(monthAttrChange, "0.00%")) & " vs previous month"

// QUARTER
var latestQuarter = CALCULATE(MAX('DimDate'[QuarterRelative]), 'DimDate'[Date] = MAX(DimEmployee[HireDate]))
var priorQuarterAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[QuarterRelative] = latestQuarter - 1)
var latestQuarterAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[QuarterRelative] = latestQuarter)
var quarterAttrChange = DIVIDE(latestQuarterAttr - priorQuarterAttr, priorQuarterAttr)
var formattedQuarterAttr = IF(quarterAttrChange < 0, "▼ " & FORMAT(ABS(quarterAttrChange), "0.00%"), "▲ " & FORMAT(quarterAttrChange, "0.00%")) & " vs previous quarter"

// YEAR
var latestYear = CALCULATE(MAX('DimDate'[YearRelative]), 'DimDate'[Date] = MAX(DimEmployee[HireDate]))
var priorYearAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[YearRelative] = latestYear - 1)
var latestYearAttr = CALCULATE([% Attrition Rate Date], 'DimDate'[YearRelative] = latestYear)
var yearAttrChange = DIVIDE(latestYearAttr - priorYearAttr, priorYearAttr)
var formattedYearAttr = IF(yearAttrChange < 0, "▼ " & FORMAT(ABS(yearAttrChange), "0.00%"), "▲ " & FORMAT(yearAttrChange, "0.00%")) & " vs previous year"

// FINAL RETURN
return SWITCH(
        TRUE(),
             // Use the field parameter value for switching
            SELECTEDVALUE('DATE'[DATE  Order]) = 0, formattedMonthAttr,
            SELECTEDVALUE('DATE'[DATE  Order]) = 1, formattedQuarterAttr,
            SELECTEDVALUE('DATE'[DATE  Order]) = 2, formattedYearAttr,
            BLANK()
)

```
```DAX
WorkLifeBalance = 
CALCULATE (
    MAX ( FactPerformanceRating[WorkLifeBalance] ),
    USERELATIONSHIP ( FactPerformanceRating[WorkLifeBalance], DimSatisfiedLevel[SatisfactionID] )
)
```
```DAX
TotalTrainingCost = SUM(DimEmployee[TrainingCost])
```
```DAX
LastReviewDate = 
IF (
    MAX ( FactPerformanceRating[ReviewDate] ) = BLANK (),
    "No Review Yet",
    MAX ( FactPerformanceRating[ReviewDate] )
)
)
```
```DAX
EnvironmentSatisfaction = 
CALCULATE (
    MAX ( FactPerformanceRating[EnvironmentSatisfaction] ),
    USERELATIONSHIP ( FactPerformanceRating[EnvironmentSatisfaction], DimSatisfiedLevel[SatisfactionID] )
)
```
```DAX
Employee Multirow Details = 

"Attrition date: " & FORMAT(CALCULATE(MIN('DimEmployee'[HireDate]),DimEmployee[Attrition] = "Yes"), "DD MMM, YYYY") & UNICHAR(10) &
"Avg. Satisfaction Score: " &ROUND(AVERAGE('FactPerformanceRatingUnpivot'[Satisfaction Score]), 2) & UNICHAR(10) & 
"Performance Rating: " & MIN(FactPerformanceRating[ManagerRating]) & UNICHAR(10) &
"Monthly Income: " & MIN(DimEmployee[Salary]) & UNICHAR(10) &
"Education Level: " & MIN(DimEducationLevel[EducationLevel]) & UNICHAR(10) &
"Total Stay: " & MIN(Salary[YearsAtCompany]) & UNICHAR(10) 
 // "Salary Hike: " & MIN('HR Attrition'[Percent Salary Hike]) & UNICHAR(10)
)
```
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  
### 📌OVERVIEW Of Dashboard
![image](https://github.com/user-attachments/assets/23f30c14-470b-4d35-ab4a-bc52f2dbe53b)


### 📌 DEMOGRAPHICS Analysis 
![image](https://github.com/user-attachments/assets/b9ffcff1-965f-4e81-b643-7943ef29059e)


### 📌 9 BOX  GRID
![image](https://github.com/user-attachments/assets/20a58151-8fe6-40a3-9b52-ff478bb6107c)

### 📌 Incriment 
![image](https://github.com/user-attachments/assets/df015de8-17d6-4c7f-9ba0-f7c17582af45)


### 📌 Admin Page 
![image](https://github.com/user-attachments/assets/c70e375c-4c9c-46ec-8906-fa3bbc556f13)


### 📌 Attrition
![image](https://github.com/user-attachments/assets/c17b619d-623c-4665-897d-2d1a0bd0d4a6)


### 📌 Satisfaction
![image](https://github.com/user-attachments/assets/5d030622-25b5-42fd-a955-e7c23356841b)

### 📌 Leave
![image](https://github.com/user-attachments/assets/bad4d9d4-f8f6-46b0-9ef6-4dc62a66fa53)



[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Key Findings  
- **Overall Employee Statistics:**

✔ Total Employees: 1,471
✔ Active Employees: 1,233
✔ Inactive Employees: 237  
✔ Attrition Rate: 16.1% 

- **Employee Attrition Distribution by Department:** 
✔ Sales & Marketing has the highest attrition (310 left, 72 stayed).
✔ Production follows closely (286 left, 64 stayed).
✔ Transport has the lowest attrition (90 left, 21 stayed).

- **Employee Hiring Trends:** 
✔ Hiring has been fluctuating over the years.
✔ A noticeable decline occurred in 2016 and 2020.
✔ The latest hiring data (2022) shows 130 retained employees and 25 left.

- **OPEX and CAPEX Trends:**
✔ Operational Expenditure (OpEx) shows a declining trend from 2013 to around 2018, then starts rising again post-2020.
✔ Capital Expenditure (CapEx) follows a fluctuating trend but starts increasing significantly after 2020.

- **Attrition by Tenure, Salary & Age:**
✔ Tenure: Employees with 1-2 years of experience have the highest attrition rates (31.6%–34.5%).
✔ Tenure attrition decreases as experience increases, reaching 0.8% for employees with 10 years of tenure. 


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🏥 Recommendations  
✔ High Attrition in Sales & Marketing and Production: Possible causes could be job stress, lack of career growth, or poor job satisfaction. Employee engagement strategies, better incentives, or workload management could help.
✔ New Employees Leaving Quickly: The highest attrition rate is among employees with 1-2 years of tenure, suggesting issues with onboarding, job expectations, or career progression. 
✔ Hiring Trends: Despite ongoing hiring, attrition remains high, suggesting a need to focus on retention strategies rather than just recruitment.  
✔ Financial Expenditure: OpEx saw a decline but is rising again post-2020, possibly indicating increased investment in employee-related expenses or operational improvements.

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🚀 Project Impact  
✔ Better Decision-Making:

1. Provides HR teams and management with data-driven insights into employee attrition, hiring trends, and financial expenditures.
2. Helps identify departments with high turnover rates (e.g., Sales & Marketing, Production), allowing for targeted retention strategies.

✔ Cost Optimization:

1. Analyzing OpEx and CapEx trends enables businesses to optimize operational expenses and forecast future spending needs.
2. Reducing high turnover can lead to lower recruitment and training costs.
   
✔ Employee Retention & Engagement:

1. Insights into why employees leave (tenure-based attrition analysis) help HR implement better retention strategies.
2. Addressing concerns for employees in their first two years can significantly improve retention rates.
   
✔Improved Workforce Planning:

1. Forecasting hiring trends and attrition rates enables companies to plan recruitment efforts proactively rather than reactively.
2. Ensures critical roles remain filled, reducing operational disruptions.

## Business Benefits:
✅ Reduced Employee Turnover → Lower hiring costs and improved stability.
✅ Data-Backed HR Strategies → Align workforce planning with company goals.
✅ Financial Savings → Optimized OpEx and CapEx management.
✅ Higher Employee Satisfaction → Improved engagement and retention.

