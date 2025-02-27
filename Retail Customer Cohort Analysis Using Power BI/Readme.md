# Power BI Project: Cohort Analysis
Watch Live Dashboard: Click here <p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiMDI2NjZjYzgtODUwNC00MzQ3LTg4Y2ItZWFhYzE5ZGVlNjIxIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="Click Here">
  </a>
</p>

The goal of this project is to analyze customer churn and perform cohort analysis using transactional data from an online retail store. The dataset contains records from two periods: 2009-2010 and 2010-2011. By leveraging cohort analysis, the project aims to identify customer retention trends, understand churn behavior, and provide actionable insights to improve customer retention strategies.

## Table of Contents
1. [Dataset Details](#dataset-details)
2. [Data Preprocessing Steps](#data-preprocessing-steps)
3. [Additional Tables Created](#additional-tables-created)
   - [DimCustomer Table](#dimcustomer-table)
   - [DimDate Table](#dimdate-table)
4. [Data Modeling](#data-modeling)
5. [DAX Measures](#dax-measures)
   - [General Measures](#general-measures)
   - [Cohort Measures](#cohort-measures)
6. [Dashboard and Visualization](#dashboard-and-visualization)
   - [Customers by Cohort and Months after First Purchase](#customers-by-cohort-and-months-after-first-purchase)
   - [Churned Customer Trend Analysis](#churned-customer-trend-analysis)
   - [Lost Customer and Recovered Customer Analysis](#lost-customer-and-recovered-customer-analysis)
7. [Project Impact](#project-impact)

## Dataset Details

### Columns in the Dataset:
- **InvoiceNo**: A 6-digit integral number uniquely assigned to each transaction. If the code starts with 'C', it indicates a cancellation.
- **StockCode**: A 5-digit integral number uniquely assigned to each distinct product.
- **Description**: The name of the product/item.
- **Quantity**: The quantity of each product/item per transaction (numeric).
- **InvoiceDate**: The date and time when each transaction was generated.
- **UnitPrice**: The unit price of the product, in sterling.
- **CustomerID**: A 5-digit integral number uniquely assigned to each customer.
- **Country**: The name of the country where each customer resides.

[🔼 Back to Table of Contents](#table-of-contents)

## Data Preprocessing Steps
1. Removed canceled orders (invoices starting with 'C') from `InvoiceNo`.
2. Removed empty values from `CustomerID`.
3. Created a `sales` column by multiplying `Quantity` and `UnitPrice`.
4. Created a `month since first transaction` column:
   ```DAX
   month since first transaction = 
   DATEDIFF(RELATED(DimCustomer[First Transaction Month]),
   RELATED(DimDate[Date]), MONTH)
   ```
5. Created a `week since first transaction` column:
   ```DAX
   week since first transaction = 
   DATEDIFF(RELATED(DimCustomer[First Transaction Week]),
   RELATED(DimDate[Date]), WEEK)
   ```
```DAX
Rev growth % = 
VAR up_arrow = UNICHAR(129137)
VAR down_arrow = UNICHAR(129139)
VAR Current_revenue = [Revenue]
VAR pm_revenue = [previous month revenue]
VAR MOM_growth = DIVIDE(Current_revenue - pm_revenue, pm_revenue)
RETURN
IF(MOM_growth < 0, ROUND(MOM_growth * 100, 1) & "% " & down_arrow,
ROUND(MOM_growth * 100, 1) & "% " & up_arrow)
```
```DAX
previous month revenue = CALCULATE([Revenue], DATEADD(DimDate[Date], -1, MONTH))
```
```DAX
previous month Orders = CALCULATE([Total Order], DATEADD(DimDate[Date], -1, MONTH))
```

### Cohort Measures
```DAX
Active Customer = COUNTROWS(VALUES(FactSales[Customer ID]))
```
```DAX
Churned Customer = SWITCH(TRUE(), ISBLANK([Retention Rate]), BLANK(), [New Customer] - [Cohort Performance])
```
```DAX
Churned Rate = [Churned Customer] / [New Customer]
```
```DAX
Cohort Performance = 
VAR mindate = MIN(DimDate[Start of Month])
VAR maxdate = MAX(DimDate[Start of Month])
RETURN
CALCULATE([Active Customer], REMOVEFILTERS(DimDate[Start of Month]),
RELATEDTABLE(DimCustomer), DimCustomer[First Transaction Month] >= mindate &&
DimCustomer[First Transaction Month] <= maxdate)
```
```DAX
Difference = [Active Customer] - [Validation]
```
```DAX
Lost Customer = 
VAR currentmonth_customer = VALUES(FactSales[Customer ID])
VAR previousmonth_customer = CALCULATETABLE(VALUES(FactSales[Customer ID]), PREVIOUSMONTH(DimDate[Start of Month]))
VAR lost_customer = EXCEPT(previousmonth_customer, currentmonth_customer)
RETURN COUNTROWS(lost_customer)
```
```DAX
New Customer = CALCULATE([Active Customer], FactSales[month since first transaction] = 0)
```
```DAX
Recovered Customers = 
VAR _CustomersThisMonth = VALUES(FactSales[Customer ID])
VAR _CustomersLastMonth = CALCULATETABLE(VALUES(FactSales[Customer ID]), PREVIOUSMONTH(DimDate[Start of Month]))
VAR _NewCustomers = CALCULATETABLE(VALUES(FactSales[Customer ID]), FactSales[Month Since First Transaction] = 0)
VAR _RecoveredCustomers = EXCEPT(EXCEPT(_CustomersThisMonth, _CustomersLastMonth), _NewCustomers)
RETURN COUNTROWS(_RecoveredCustomers)
```
```DAX
Retained Customer = 
VAR currentmonth_customer = VALUES(FactSales[Customer ID])
VAR previousmonth_customer = CALCULATETABLE(VALUES(FactSales[Customer ID]), PREVIOUSMONTH(DimDate[Start of Month]))
VAR retain_customer = INTERSECT(currentmonth_customer, previousmonth_customer)
RETURN COUNTROWS(retain_customer)
```
```DAX
Retention Rate = DIVIDE([Cohort Performance], [New Customer])
```
```DAX
Validation = [New Customer] + [Retained Customer] + [Recovered Customers]
```

[🔼 Back to Table of Contents](#table-of-contents)

## Additional Tables Created

### DimCustomer Table
- Contains `CustomerID`, `First Transaction Month`, and `First Transaction Week`.

### DimDate Table
- Contains all unique dates from 12/1/2010 to 12/9/2011.
- Includes `Start of Month` and `Start of Week` attributes.

[🔼 Back to Table of Contents](#table-of-contents)

## Data Modeling
![Data Modeling](https://github.com/Tanim-code/power-Bi-project/assets/86589317/ed9e61bb-1c8d-4954-b298-0c561b009733)

[🔼 Back to Table of Contents](#table-of-contents)

## DAX Measures

### General Measures
```DAX
Avg Order Value = DIVIDE([Revenue], [Total Order])
```
```DAX
Avg Revenue per Customer = DIVIDE([Revenue], [Total Customer])
```
```DAX
Unit Sold = SUM(FactSales[Quantity])
```
```DAX
Total Order = DISTINCTCOUNT(FactSales[Invoice])
```
```DAX
Total Customer = COUNTROWS(DimCustomer)
```
```DAX
Revenue = SUMX(FactSales, FactSales[Quantity] * FactSales[Price])
```

### Cohort Measures
```DAX
Active Customer = COUNTROWS(VALUES(FactSales[Customer ID]))
```
```DAX
Churned Customer = 
SWITCH(TRUE(),
    ISBLANK([Retention Rate]), BLANK(),
    [New Customer] - [Cohort Performance]
)
```
```DAX
Churned Rate = [Churned Customer] / [New Customer]
```

[🔼 Back to Table of Contents](#table-of-contents)

## Dashboard and Visualization

### Customers by Cohort and Months after First Purchase
![image](https://github.com/user-attachments/assets/fae91010-329b-4a17-9425-1f2df00ddbbf)

### Churned Customer Trend Analysis
![image](https://github.com/user-attachments/assets/46199aa7-25bf-4b3f-bd2f-d928e779e9c4)

### Lost Customer and Recovered Customer Analysis
![image](https://github.com/user-attachments/assets/c865aaa1-d53d-4199-8a35-5a4b55cb7d26)

[🔼 Back to Table of Contents](#table-of-contents)

## Project Impact
This project provides stakeholders with actionable insights derived from cohort analysis. The results enable data-driven decision-making to:
- Enhance customer retention.
- Optimize marketing strategies.
- Maximize revenue generation.

[🔼 Back to Table of Contents](#table-of-contents)

