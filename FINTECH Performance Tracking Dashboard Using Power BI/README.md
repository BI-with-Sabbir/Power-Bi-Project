# Power BI Project: FINTECH Performance Tracking Dashboard

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiNmZjNTA5YjEtN2JmMy00NmM4LWE2ZTgtZThlNWU4ZjViMjRiIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

## 📌 Project Overview  
This Fintech Performance Dashboard is designed to provide a comprehensive view of your organization's operational health and growth potential. Tailored for leaders and decision-makers, the dashboard delivers actionable insights into critical areas, helping you achieve efficiency, transparency, and excellence.

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
- **Category_Table:** Category_ID, Category_Name  
- **Complaint_Table:** Complaint_ID, Complaint_Type, Customer_ID, Resolution Min, Resolution_Date, Resolution_Status  
- **Revenue_Table:** Revenue_Amount, Revenue_ID, Source_of_Revenue  
- **Customer_Table:** Age, Age_Group, Customer_ID, Gender, Location, Name, Other_Demographics  
- **Dim_Date:** Date, Day Name, Day Type, Month, Month Name, Quarter, Start of Month, Year  
- **Merchant_Table:** Business_Type, Merchant_ID, Merchant_Location, Merchant_Name, Total_Transactions  
- **Agent_Table:** Active_Status, Agent_ID, Agent_Name, Incentive_Plan, Location, Number_of_Transactions, Total_Revenue_Generated  
- **Transaction_Fact:** Agent_ID, Category_ID, Channel, Customer_ID, Gender, Location, Merchant_ID, Revenue_ID, Transaction_Date, Transaction_ID  

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
- **Sort Transactions:** Arrange by Transaction_Date in ascending order.  
- **Standardize Data Types:** Convert ID fields to text.  
- **Add Index Column:** Create an index starting from 489434.  
- **Join with Customer Table:** Merge to bring Location and Gender.  
- **Fill Missing Data:** Replace null values with default values.  

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
