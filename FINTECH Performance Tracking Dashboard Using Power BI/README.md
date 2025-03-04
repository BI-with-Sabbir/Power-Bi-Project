# Power BI Project: FINTECH Performance Tracking Dashboard

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiNmZjNTA5YjEtN2JmMy00NmM4LWE2ZTgtZThlNWU4ZjViMjRiIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

### 📌 Project Overview  
This Fintech Performance Dashboard is Designed to provide a comprehensive view of your Organization's Operational Health and 
Growth Potential. Tailored for Leaders and Decision-makers, the Dashboard Delivers Actionable Insights into critical areas, helping you 
achieve Efficiency, Transparency, and Excellence.

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
Category_Table:

Category_ID, Category_Name, 

Complaint_Table:

Complaint_ID, Complaint_Type, Customer_ID, Resolution Min, Resolution_Date, 
Resolution_Status

Revenue_table:
Revenue_Amount,Revenue_ID,Source_of_Revenue

Customer_Table:
Age,Age_Group,Customer_ID,Gender,Location,Name,Other_Demographics

Dim_date:

Date,Day Name,Day Type,Month,Month Name,Quarter,Start of Month,Year

Merchant_Table:

Business_Type,Merchant_ID,Merchant_Location,Merchant_Name,Total_Transactions

Agent_Table:

Active_Status, Agent_ID,Agent_Name,Incentive_Plan,Location,Number_of_Transactions,Total_Revenue_Generated

Transaction_fact:

Agent_ID,Category_ID,Channel,Customer_ID,Gender,Location,Merchant_ID,Revenue_ID,Transaction_Date,Transaction_ID

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

### Steps Taken:  
Transaction Fact Table Preprocessing:
Promote Headers:
Ensure the first row of the dataset is used as column headers.
Change Data Types

Convert Transaction_ID to Int64.
Convert Customer_ID, Transaction_Type, Category_ID, Revenue_ID, Channel, Agent_ID to Text.
Convert Transaction_Date to Date.
Convert Merchant_ID to Any type.
Remove Errors

Remove any rows containing errors in the Transaction_ID column.
Remove Unnecessary Columns

Drop column Column10, which is not needed.
Replace Null Values

Replace null values in Agent_ID with "Not found".
Replace null values in Merchant_ID with "Not found".
Sort Transactions by Date

Order the data in ascending order based on Transaction_Date.
Standardize Data Types

Convert Merchant_ID and Transaction_ID to Text type.
Add an Index Column

Create an Index column starting from 489434 and incrementing by 1.
Reorder Columns

Arrange the columns in the following order:
Transaction_ID, Index, Customer_ID, Transaction_Date, Transaction_Type, Category_ID, Revenue_ID, Channel, Merchant_ID, Agent_ID.
Rename Index as Transaction_ID

Remove the existing Transaction_ID column and rename Index to Transaction_ID.
Join with Customer Table

Merge with Customer_Table on Customer_ID to bring Location.
Fill Missing Location Data

Replace null values in Location with "Dhaka".
Join with Customer Table Again

Merge with Customer_Table again to bring Gender.
Rename Gender Column

Rename Customer_Table.Gender to Gender.
Fill Missing Gender Data

Replace null values in Gender with "Female".
Final Column Reordering

Arrange the final columns as:
Transaction_ID, Transaction_Date, Customer_ID, Location, Gender, Transaction_Type, Category_ID, Revenue_ID, Channel, Merchant_ID, Agent_ID.
Dim Date Table Preprocessing
Define Date Range

Extract the minimum and maximum Transaction_Date from the Transaction_fact table.
Generate Date List

Create a continuous date list from MinOrderDate to MaxOrderDate.
Convert Date List to Table

Convert the list into a tabular format.
Set Data Type for Date Column

Ensure the Date column is of type Date.
Extract Date Components

Extract Year, Month, Quarter, Start of Month from the Date column.
Extract Month Name

Get full month names from the Start of Month column.
Format Quarter Information

Prefix Quarter values with "Q", e.g., "Q1", "Q2", "Q3", "Q4".
Shorten Month Names

Convert month names to their first three letters (e.g., "Jan", "Feb", "Mar").
Extract Day Name

Get the full day name for each date.
Create Day Type Column

Define Friday and Saturday as "weekend", and other days as "weekday".
Rename Day Type Column

Rename "weekend" column to "Day Type".
Set Data Types

Ensure the correct data types for all columns:
Date (Date), Year (Int64), Month (Int64), Quarter (Text), Start of Month (Date), Month Name (Text), Day Name (Text).
Sort by Date

Sort the entire table in ascending order based on the Date column.

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
Total Revenue Agents = 
SUMX(
    FILTER(Transaction_Fact, Transaction_Fact[Channel] = "Agent"),
    RELATED(Agent_Table[Total_Revenue_Generated])
)
```
```DAX
Total Agents = DISTINCTCOUNT(Transaction_fact[Agent_ID])
```
```DAX
Top_Agents_By_Region_Revenue = 
SUMX(
    FILTER(Transaction_Fact, Transaction_Fact[Channel] = "Agent"),
    RELATED(Agent_Table[Total_Revenue_Generated])
)
```
```DAX
successful transactions = CALCULATE(COUNT(Transaction_fact[Transaction_ID]),Transaction_fact[Transaction_Status] = "successful")
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
```DAX
Count of Merchant IDs (Filtered) = 
CALCULATE(
    DISTINCTCOUNT('Transaction_fact'[Merchant_ID]),
    'Transaction_fact'[Merchant_ID] <> "Not Found"
)
```
```DAX
Count of Agent IDs (Filtered) = 
CALCULATE(
    DISTINCTCOUNT('Transaction_fact'[Agent_ID]),
    'Transaction_fact'[Agent_ID] <> "Not Found"
)
```
```DAX
Average Transaction = [Total transaction volume]/[Total Transaction]
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

### 🔹Transaction Overview
Total Transaction Volume: 22.0bn – Indicates a strong financial activity in the fintech ecosystem.
Total Transactions: 455.8K – Shows a high level of customer engagement.
Average Transaction Size: 48.3K – Suggests relatively large transactions per user.
Success Rate: 88.5% – A high success rate, but 11.5% of transactions failed, which may need investigation (e.g., system errors, user mistakes, or insufficient funds).
2. Transaction Trends & Seasonal Patterns
The Transaction Volume Trend shows fluctuations across quarters.
Peaks in Q3 2023 and Q4 2024 suggest seasonal variations or promotional campaigns influencing higher transactions.
A decline in Q1 2023 and Q1 2024 could indicate post-festive slowdowns or economic factors.
3. Customer Behavior & Preferred Channels
Most Used Channels:
Mobile App (42.36%) is the most preferred, indicating high adoption of digital payments.
Agents (34.62%) are still significant, suggesting reliance on physical locations for transactions.
USSD (23.02%) has a smaller share, possibly used by customers with limited internet access.
The high app usage suggests an opportunity to enhance digital banking services.
4. Transaction Type Analysis
Cash In (7.2bn) dominates, meaning a large amount of money is being deposited into accounts.
Merchant Payments (5.1bn) indicate strong digital commerce adoption.
Send Money (3.5bn) shows high peer-to-peer transactions.
Cash Out (2.0bn) is relatively lower, meaning users prefer to keep funds within the system.
Remittances (0.8bn) contribute a smaller share, indicating potential for growth in cross-border transactions.
**Insights:**  
- Strong Memorial Hospital has the highest average LOS, which may indicate more complex cases or inefficiencies.  
- The Unity Hospital of Rochester has the lowest LOS, possibly reflecting higher efficiency in patient turnover.  

Agent Network Optimization (First Dashboard)
Total Agents & Transactions:

There are 68.7K agents handling 181.9K transactions with a total volume of 10.31bn.
The total revenue from these transactions is 1.75bn.
Geographical Agent Distribution:

The map shows agent distribution across Bangladesh, with high concentrations in some areas.
Dhaka, Chattogram, and Khulna have the most active agents with top agents handling 471.61M in transactions.
Dhaka leads with 106.38M, followed by Khulna (103.13M) and Rajshahi (96.92M).
Agent Performance:

The presence of inactive agents (based on the Active Status filter) could indicate the need for network expansion or optimization.
Some regions might be underserved despite high transaction volumes in neighboring areas.
Customer Insights (Second Dashboard)
Age Group & Transaction Behavior:

Seniors (35.1K transactions) and Young Adults (29.8K transactions) are the most active transaction groups.
Teenagers have the lowest transaction count, indicating potential market expansion opportunities.
Gender-Based Insights:

81% of unique customers are male, while only 19% are female.
This suggests an opportunity to attract more female customers through targeted marketing and financial services.
Transaction Categories & Trends:

Cash In & Cash Out transactions dominate across all age groups.
Loan Repayment and Mobile Recharge are less frequent, indicating potential for promoting these services.
Total Transaction Volume & Averages:

Senior and Young Adult groups have higher transaction volumes, meaning fintech adoption is strong among them.
The average transaction value is increasing across age groups, especially for Adults and Seniors.



[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🏥 Recommendations  

Expand Agent Networks in Underserved Regions

Target high-transaction volume areas with fewer agents.
Reduce inactivity by optimizing agent incentives.
Increase Female Customer Engagement

Launch campaigns focused on women’s financial inclusion.
Promote mobile banking and micro-loans for female entrepreneurs.
Enhance Digital Service Usage

Encourage Loan Repayments, Donations, and Mobile Recharges through promotions or cashback incentives.
Improve awareness about fintech solutions in lower-utilization segments.
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🚀 Project Impact  

✔ Optimize failed transactions – Identify reasons for failure and improve user experience.
✔ Target regional growth – Expand fintech adoption in non-Dhaka cities with localized campaigns.
✔ Promote digital spending – Incentivize merchant payments and reduce reliance on cash.
✔ Leverage seasonal trends – Plan offers during high-transaction periods for better revenue. 

---

