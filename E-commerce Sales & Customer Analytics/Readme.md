# Power BI Project: E-commerce Sales and Customer Analytics 

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiYmQ4MGFmNzEtMGU1Yi00MzI4LTg3ZmUtZmY5ZTBiOTBmMzRmIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

## 📌 Project Overview  
![image](https://github.com/user-attachments/assets/2681ea51-5006-4550-be23-7df84ad02827)


This Power BI-powered reporting solution empowers e-commerce businesses with real-time visibility into sales trends, customer behavior, and regional performance. It enables smarter decision-making through interactive and customizable dashboards.

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
- **Customer_Lookup:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Customer%20Lookup.csv)
- **Date_Lookup:**   [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Date%20Lookup.csv)
- **Customer Group:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Customer%20Group.csv)
- **Product Lookup:**[Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Product%20Lookup.csv)
- **Product Subcategories Lookup:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Product%20Subcategories%20Lookup.csv)
- **RFM Table:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/RFM%20Segment.csv)
- **Return_Table:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Return.csv) 
- **Sales Lookup:**[Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Sales.csv)
- **Territory_Table:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/E-commerce%20Sales%20%26%20Customer%20Analytics/Territory%20Lookup.csv)

  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

### Steps Taken:  
#### **Calculate some Groups of table which is help in our analysis project:**  
- **Customer Information Table:**
  ```DAX
    Customer Information = {
    ("Expenditure Analysis", NAMEOF('Measures Table'[Sales Per Customer]), 0),
    ("Demographic Analysis", NAMEOF('Measures Table'[Active Customers]), 1)
  }
```

- **Customer/Sales Table:**

```DAX
    Customer/sales = {
    ("Active Customers", NAMEOF('Measures Table'[Active Customers]), 0),
    ("Sales Per Customer", NAMEOF('Measures Table'[Sales Per Customer]), 1)
   }
```
- **KY kpi:**
```DAX
  CY KPI = {
    ("#Transaction", NAMEOF('Measures Table'[Total Transaction]), 0),
    ("Total Revenue", NAMEOF('Measures Table'[Total Revenue]), 1),
    ("Profit", NAMEOF('Measures Table'[Profit]), 2),
    ("Profit Margin", NAMEOF('Measures Table'[Profit Margin]), 3),
    ("Return Rate", NAMEOF('Measures Table'[Return Rate (%)]), 4)
}
```
- **Product Split:**
```DAX
    Product Split = CROSSJOIN(
    {"A+", "Others"},
    SELECTCOLUMNS('Product Lookup', "Product Name", 'Product Lookup'[Product Name]))
```
- **RFM value:**
- ```DAX
  RFM Table =
  SUMMARIZE( 'Customer Lookup' , 'Customer Lookup'[CustomerKey], "R Value" , [R Value],"F Value" , [Frequency] , "M Value" , [Monetary])
  ```
  

#### **Data Cleaning and  Preprocessing:**  
- Promote Headers.
- Change Data Types
- Remove Errors
- Drop Unnecessary Columns
- Replace Null Values


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/d6123ff5-c475-443f-9969-947afffefc65)


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### General Measures  
```DAX
Avg Sales Excluding January and Last Month of Year = 
VAR LastYear = MAX('Date Lookup'[Year])
VAR LastMonth = 
    CALCULATE(
        MAX('Date Lookup'[Month]),
        FILTER(
            ALL('Date Lookup'),
            'Date Lookup'[Year] = LastYear
        )
    )
VAR ValidMonths = 
    FILTER(
        'Date Lookup',
        'Date Lookup'[Year] = LastYear &&
        'Date Lookup'[Month] <> 1 &&          // Exclude January
        'Date Lookup'[Month] <> LastMonth     // Exclude the last month
    )
VAR SelectedMonth = MAXX(ValidMonths, 'Date Lookup'[Month])
RETURN
CALCULATE(
    MIN([Sales Per Customer], [PM Avg Salesper customers])
,
    FILTER(
        'Date Lookup',
        'Date Lookup'[Year] = LastYear &&
        'Date Lookup'[Month] = SelectedMonth
    )
)
```
```DAX
Avg Sales Last Month = 
VAR LastYear = MAX('Date Lookup'[Year])
VAR LastMonth =
    CALCULATE(
        MAX('Date Lookup'[Month]),
        FILTER(
            ALL('Date Lookup'),
            'Date Lookup'[Year] = LastYear
        )
    )
RETURN
CALCULATE(
    IF(
        SELECTEDVALUE('Date Lookup'[Month]) = 1,
        [Sales Per Customer], // January's value from the current year
        IF(
            SELECTEDVALUE('Date Lookup'[Month]) = LastMonth,
            MINX(
                { [Sales Per Customer], [PM Avg Salesper customers] },
                [Value] // Evaluates the minimum between the current and previous month measures
            )
        )
    ),
    FILTER(
        'Date Lookup',
        'Date Lookup'[Year] = LastYear &&
        (
            'Date Lookup'[Month] = 1 || // Include January
            'Date Lookup'[Month] = LastMonth // Include the last month condition
        )
    )
)


```
```DAX
Base Avg Sales = CALCULATE(
    MIN([Sales Per Customer], [PM Avg Salesper customers])
)
```DAX
Decrease Avg Sales = 
VAR CY = [Sales Per Customer]
VAR PY = [PM Avg Salesper customers]

RETURN
IF(PY>CY, PY - [Base Avg Sales])
```
```DAX
Decrease Avg Sales <> Jan = CALCULATE([Decrease Avg Sales], 'Date Lookup'[Month]<>1)
```
```DAX
Increase Avg Sales = 
VAR CY = [Sales Per Customer]
VAR PY = [PM Avg Salesper customers]

RETURN
IF(
    MONTH(MAX('Date Lookup'[Date])) , 
    IF(CY > PY, [Sales Per Customer] - [Base Avg Sales]


))
```
```DAX
Increase Avg Sales <> Jan = CALCULATE([Increase Avg Sales], 'Date Lookup'[Month]<>1)```
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  
### 📌 Customer Analysis 
![image](https://github.com/user-attachments/assets/1db076c3-ec1d-4d31-a05a-7f4c429d1ddf)


### 📌 Product Analytics by Pareto Analysis
![image](https://github.com/user-attachments/assets/174a0440-8939-430e-bc5b-080b796f55b2)


### 📌 Territory Analysis
![image](https://github.com/user-attachments/assets/564ba4c1-bf7f-400b-a4c8-f14ebc6cd3ee)


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





