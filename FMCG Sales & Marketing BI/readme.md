# Power BI Project: FMCG Sales & Marketing Analysis Using power BI

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiYjRhMDljZWEtZmI0YS00ZWMyLWEzNDgtNmQ0ZmRjYTAxNGNkIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

## 📌 Project Overview  
![image](https://github.com/user-attachments/assets/f2c980bb-0e3a-4dc8-9d98-9a469cbec5d3)



This Business Intelligence (BI) Solution, built with Power BI, provides a comprehensive analysis of FMCG( Fast-Moving Consumer Goods) Sales Performance, Financial Health, Customer Behavior, and Marketing Effectiveness. It empowers stakeholders to make data-driven decisions and optimize strategies for sustainable business growth.

---

## 📖 Table of Contents  
- [Dataset Details](#-dataset-details)  
- [Data Preprocessing](#-data-preprocessing)  
- [Data Modeling](#-data-modeling)  
- [DAX Measures](#-dax-measures)  
- [Dashboard & Visualizations](#-dashboard--visualizations)  
- [Key Business Insights](#-key-business-insights)  
- [Business Recommendations](#-business-recommendations)  
- [Project Impact](#-project-impact)  

---

## 📂 Dataset Details  

### Columns in the Dataset:  
- **Customer_Lookup:** [Click here]()
- **Date_Lookup:**   [Click here]()
- **Customer Group:** [Click here]()
- **Product Lookup:**[Click here]()
- **Product Subcategories Lookup:** [Click here]()
- **RFM Table:** [Click here]()
- **Return_Table:** [Click here]() 
- **Sales Lookup:**[Click here]()
- **Territory_Table:** [Click here]()

  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

### Steps Taken:  
#### **Calculate some Groups of table which is help in our analysis project:**  
**Customer Information Table:**
```DAX
Customer Information = {
    ("Expenditure Analysis", NAMEOF('Measures Table'[Sales Per Customer]), 0),
    ("Demographic Analysis", NAMEOF('Measures Table'[Active Customers]), 1)
  }
```

**Customer/Sales Table:**

```DAX
    Customer/sales = {
    ("Active Customers", NAMEOF('Measures Table'[Active Customers]), 0),
    ("Sales Per Customer", NAMEOF('Measures Table'[Sales Per Customer]), 1)
   }
```

**KY KPI:**
```DAX
  CY KPI = {
    ("#Transaction", NAMEOF('Measures Table'[Total Transaction]), 0),
    ("Total Revenue", NAMEOF('Measures Table'[Total Revenue]), 1),
    ("Profit", NAMEOF('Measures Table'[Profit]), 2),
    ("Profit Margin", NAMEOF('Measures Table'[Profit Margin]), 3),
    ("Return Rate", NAMEOF('Measures Table'[Return Rate (%)]), 4)
}
```

**Product Split:**
```DAX
    Product Split = CROSSJOIN(
    {"A+", "Others"},
    SELECTCOLUMNS('Product Lookup', "Product Name", 'Product Lookup'[Product Name]))
```

**RFM value:**
```DAX
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
Increase Avg Sales <> Jan = CALCULATE([Increase Avg Sales], 'Date Lookup'[Month]<>1)
```
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
## 📈 Key Business Insights

📌 Executive Summary Dashboard – E-commerce Sales & Customer Analytics
This dashboard offers a high-level overview of key performance indicators (KPIs) for e-commerce businesses, combining sales, profit, and customer return behavior into one interactive view.

## 🔍 Highlights:
- Total Transactions: 10.7K (📈 +307% YoY)

- Revenue: $9.3M (📈 +46% YoY)

- Profit: $4.0M (📈 +52% YoY)

- Profit Margin: 42.5%

- Return Rate: 2.1% (📉 -35% YoY)

## 📊 Key Visuals:
- Revenue Trend & Forecast: A clear time-series line chart showing revenue growth throughout 2021.

- Return Rate by Month and Category: A stacked column chart comparing return rates for Accessories, Bikes, and Clothing.

- Top 15 Subcategories by Return Rate: Detailed view of subcategories like Tires, Road Bikes, Helmets, etc., including YoY change.

## 🧩 Interactive Features:
- Category selector (Accessories, Bikes, Clothing)

- Scrollable subcategory list

- Real-time comparison between current and previous year (CY vs. PY)

### 🧮 Product Analysis Dashboard – Performance by Product
This dashboard dives into individual product-level performance to help businesses identify their top revenue-generating items, track sales trends, and monitor return behavior.

## 🔍 Highlights:
- Dynamic Revenue Target Slider: Users can filter products based on a custom revenue contribution target (e.g., Top 20%).

- Product Segmentation:

- A+ Group: Top 3 performers contributed $1.76M+ in revenue, each with strong profit margins and relatively low return rates.

## 📊 Example Insights:
Mountain-200 Black, 46 generated $616,779 in revenue with a return rate of 3.7%, while Mountain-200 Silver, 46 had the lowest return rate at 1.8% among A+ products.

Some smaller products like HL Road Tire had a higher return rate (4.6%) despite lower revenue.

## 🧩 Interactive Filters:
Country, Category, and Year selectors

The revenue Target slider dynamically updates the table

### 👥 Customer Analysis Dashboard – Demographics & Behavioral Insights
This dashboard delivers a comprehensive overview of customer profiles, spending behavior, and segmentation to help drive targeted marketing and retention strategies.

🔎 Key Areas:
##📊 1. Demographic Analysis:
Visual distribution by:

- Gender

- Marital Status

- Parenthood

- Home Ownership

- Tabs to explore deeper by:

- Education Level

- Income Group

- Occupation

- Age Group

##💸 2. Expenditure Analysis:
- Customer Count: 9.1K in 2021, a +247% increase from 2020

- Average Spending: $1,021 in 2021, a -58% drop from the prior year

- Monthly Trend: Notable dip in August (-66%), recovery in December (+16%)

## 🧠 3. RFM-Based Customer Segmentation:
- Promising Customers: 35.5%

- About to Sleep: 28.6%

- Loyal Customers: 16.3%

- Champions: 8.5%

- Needs Attention: 9.8%


## 🧠 Insights for Business:
Although customer acquisition surged, average spending dropped, suggesting a need for loyalty and re-engagement campaigns.

- RFM segmentation reveals that nearly 1 in 3 customers are at risk of churn (About to Sleep), while Champions remains a small but valuable segment.



## 📈 Key Business Insights keypoints:

- **Customer Growth**: 247% increase in total customers YoY
- **Spending Drop**: Despite more customers, average spending fell by 58%
- **Top Products**: Mountain bikes (e.g., Mountain-200 series) are best-sellers with high profits and low return rates
- **Return Rate Watch**: Some accessories like "All-Purpose Bike Stand" have higher return rates (4.2%+)
- **Customer Segments**: 35.5% of customers are "Promising", while 28.6% are "About to Sleep" — indicating opportunities and risks
- **Seasonality**: High spending in Jan and Dec; major drop in Aug (−66%)
  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 💡 Business Recommendations

1. **Target High-Value Segments**  
   Focus marketing on Loyal Customers and Champions to increase retention and repeat purchases.

2. **Re-engage At-Risk Customers**  
   Create campaigns for "About to Sleep" and "Needs Attention" segments to reduce churn.

3. **Boost Average Spending**  
   Introduce loyalty programs and bundle offers to increase per-customer revenue.

4. **Improve Underperforming Products**  
   Investigate high-return rate items to improve quality or adjust marketing strategies.

5. **Optimize Inventory for Seasonal Trends**  
   Prepare for low sales periods (e.g., August) and capitalize on high-spending months like December and January.

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🌟 Project Impact

- ✅ Improved customer segmentation and retention strategy
- ✅ Highlighted top revenue-generating and underperforming products
- ✅ Enabled data-driven seasonal marketing and product planning
- ✅ Helped visualize the link between demographics and spending behavior
- ✅ Empowered business users with self-service analytics dashboards






