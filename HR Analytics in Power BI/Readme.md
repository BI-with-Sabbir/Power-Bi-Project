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
- **Parameter Create:** 1.Did not Met Expectation & Emerging Potential = GENERATESERIES(0, 0.25, 0.01)
                        2.Did not Met Expectation & High Potential = GENERATESERIES(0, 0.25, 0.01)
                        3.Did not Met Expectation & Low Potential = GENERATESERIES(0, 0.25, 0.01)
                        4.Create_Dim_Data table Using Dax Formula.
                        5.Exceeded Expectations & Emerging Potential = GENERATESERIES(0, 0.25, 0.01)
                        6.Exceeded Expectations & High Potential = GENERATESERIES(0, 0.25, 0.01)
                        7.Exceeded Expectations & Low Potential = GENERATESERIES(0, 0.25, 0.01)
                        8.Met Expectations & Emerging Potential = GENERATESERIES(0, 0.25, 0.01)
                        9.Met Expectations & High Potential = GENERATESERIES(0, 0.25, 0.01)
                       10. Salary and Tenure measure: SALARY & TENURE = {
                                                                   ("TENURE", NAMEOF('DimEmployee'[YearsAtCompany]), 0),
                                                                   ("SALARY", NAMEOF('DimEmployee'[Salary Bin]), 1),
                                                                   ("AGE", NAMEOF('DimEmployee'[AgeBins]), 2)
                                                                    }
#### **Dim Date Table Preprocessing:**  
- Define Date Range from MinOrderDate to MaxOrderDate.  
- Generate a continuous date list.  
- Extract Date Components (Year, Month, Quarter, Start of Month).  
- Format Date Information (Short Month Names, Quarter Formatting).  
- Classify Days as Weekend or Weekday.  
- Sort by Date.  

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/c638bf4a-db61-425e-bee7-69e0ccd044a0)

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### General Measures  
```DAX
Transaction Volume by agents = 
SUMX(
    FILTER(Transaction_Fact, Transaction_Fact[Channel] = "Agent"),
    RELATED(Revenue_Table[Revenue_Amount])
)
```
```DAX
Total Transactions by Agents = 
CALCULATE(
    COUNT(Transaction_Fact[Transaction_ID]),
    Transaction_Fact[Channel] = "Agent"
)
```
```DAX
Total transaction volume = SUMX('Transaction_fact', RELATED('Revenue_table'[Revenue_Amount]))
```
```DAX
Total Transaction = DISTINCTCOUNT(Transaction_fact[Transaction_ID])
```
```DAX
Success Rate = 
DIVIDE(
    COUNTROWS(
        FILTER(
            'Transaction_fact',
            'Transaction_fact'[Transaction_Status] = "Successful"
        )
    ),
    COUNTROWS('Transaction_fact'),
    0
) 
```
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  
### 📌 Business Performance  
![image](https://github.com/user-attachments/assets/024f48b4-3b58-498b-8ff9-20346a6aade1)

### 📌 Agent Network Optimization  
![image](https://github.com/user-attachments/assets/c795d3e1-f843-4f42-8f04-3b4f725eac1e)

### 📌 Customer Insights  
![image](https://github.com/user-attachments/assets/880becc0-048f-41bb-a9a7-372f6c0bba79)

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Key Findings  
- **Transaction Volume:** 22.0bn – Strong financial activity.  
- **Transaction Success Rate:** 88.5% – Needs improvement in failed transactions.  
- **Top Channel:** Mobile App (42.36%) – Digital adoption is high.  
- **Cash In Dominance:** 7.2bn – Large amounts being deposited into accounts.  
- **Geographical Trends:** Dhaka leads in fintech adoption.  

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🏥 Recommendations  
✔ Expand agent networks in underserved regions.  
✔ Increase female customer engagement through financial inclusion campaigns.  
✔ Enhance digital service usage with promotions.  
✔ Optimize failed transactions for better user experience.  

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🚀 Project Impact  
✔ Optimized failed transactions.  
✔ Targeted regional fintech growth.  
✔ Promoted digital spending.  
✔ Leveraged seasonal transaction trends for business insights.  

