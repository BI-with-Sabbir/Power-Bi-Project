# Shwapno Chain Retail Store Analytics – Power BI Project
## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiMGNhNWRhZjgtYzAyMS00YTBiLWI0OTEtNWMyMjJiNzgwMDY5IiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

## 📌 Project Overview  
![image](https://github.com/user-attachments/assets/258206ff-06b2-4daf-b78c-77cf493ad6e5)

 
This Power BI dashboard project presents a **comprehensive retail business analysis** for a fictionalized version of **Shwapno**, a leading Bangladeshi chain retail store. The dashboard is built to help retail managers, finance officers, and business strategists monitor **financial performance**, **product profitability**, and **daily operational metrics**.
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

### Dataset:  


The project integrates multiple datasets into a star-schema model including:

**Fact_table:** Contains transactional records (Qty, UnitPrice, OrderDate, ProductID, StoreID, CustomerID)

**Customers:** Customer demographics (ID, Age, Name, Email)

**Product:** Product metadata (CategoryID, Marketing Cost, Rent)

**Category & Subcategory:** Hierarchical product classification

**Date:** Full date dimension with breakdowns

**Stores:** Store-wise location and name

**Marketing_Spend:** Offline and online monthly marketing expenses

**RFM Table & RFM_Segmented:** Customer segmentation based on recency, frequency, and monetary values

**RawProductsOrders:** Intermediate table to track product orders

**And Product:** Composite key for drill-down hierarchy

 
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

- Cleaned null and duplicate entries

- Standardized date formats

- Created calculated columns for month, year, and day types

- Calculated product-level net profit (Revenue - Cost - Marketing Expense)

- Derived customer lifetime value (CLV) and net profit contributions

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/17e31be3-34f2-4439-8932-9e185aafff8d)



[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### Basket Analysis  
```Support basket = 

VAR prod1 = 'Basket Analysis'[Product1]
VAR prod2 = 'Basket Analysis'[Product2]

VAR prod1transaction =
        SELECTCOLUMNS(FILTER('Fact_table','Fact_table'[ProductID]=prod1),
        "OrderID", Fact_table[OrderID])


VAR prod2transaction =
        SELECTCOLUMNS(FILTER('Fact_table','Fact_table'[ProductID]=prod2),
        "OrderID", Fact_table[OrderID])

VAR BothTransaction = INTERSECT(prod1transaction,prod2transaction)

RETURN
    COUNTROWS(BothTransaction)/[Total transaction]

```

```DAX
Product2 Name = 
LOOKUPVALUE(
    'Product'[Product_name], 
    'Product'[Product_ID], 
    'Basket Analysis'[Product2]
)

```

```DAX
Product1 Name = 
LOOKUPVALUE(
    'Product'[Product_name], 
    'Product'[Product_ID], 
    'Basket Analysis'[Product1]
)

```

```DAX
Lift = 
VAR prod1 = 'Basket Analysis'[Product1]
VAR prod2 = 'Basket Analysis'[Product2]


VAR supportprod1 = COUNTROWS(FILTER(Fact_table,Fact_table[ProductID] = prod1))/[Total transaction]
VAR supportprod2 = COUNTROWS(FILTER(Fact_table,Fact_table[ProductID] = prod2))/[Total transaction]

RETURN
'Basket Analysis'[Support basket]/(supportprod1*supportprod2)
```

```DAX
Confidence of product2 = 
VAR prod2 = 'Basket Analysis'[Product2]

VAR supportprod2 = COUNTROWS(FILTER(Fact_table,Fact_table[ProductID] = prod2))/[Total transaction]
RETURN
'Basket Analysis'[Support basket]/supportprod2
```

```DAX
Confidence of product1 = 
VAR prod1 = 'Basket Analysis'[Product1]

VAR supportprod1 = COUNTROWS(FILTER(Fact_table,Fact_table[ProductID] = prod1))/[Total transaction]
RETURN
'Basket Analysis'[Support basket]/supportprod1

```
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  
### 1. **📍 Daily Performance**
![image](https://github.com/user-attachments/assets/2ad9ee29-d2f6-423a-9b07-32b0ee2165be)




### 2. **📍 Financial Overview**
![image](https://github.com/user-attachments/assets/6b8f59da-7202-4924-94eb-b0355dde1f6f)



#### 3. **📦 Product Analysis**
![image](https://github.com/user-attachments/assets/e7b8a181-ffc0-4355-b7fa-bce3191e30e4)


### 4 . **📍 Basket Analysis**
![image](https://github.com/user-attachments/assets/0d911591-2db8-443a-9312-f3d56bcc73fe)



### 5. **📍 RFM Segmentation**
![image](https://github.com/user-attachments/assets/130262ec-7b6d-45bc-983f-cbceb1a9e192)


#### 6. **📦 Regional Performance**
![image](https://github.com/user-attachments/assets/a4f8410d-4628-49ec-a1bd-869c752e6f07)




[🔼 Back to Table of Contents](#-table-of-contents)

---
## 📈 Key Business Insights

### 1. 📍 Financial Overview

A comprehensive snapshot of the company’s **overall financial health** over time.

#### 📃 Key Metrics:

* **Total Transactions**: 13.9K (↑ 42% YoY)
* **Total Revenue**: 26.7M৳ (↑ 73%)
* **Net Profit**: 1.9M৳ (↑ 76%)
* **Total Cost**: 24.8M৳ (↑ 73%)

#### 🗓️ Monthly Breakdown Includes:

* Product Purchasing Cost
* Marketing Cost (Online & Offline)
* Operational Cost (Salaries, Rent, Utility Bills)
* Net Profit Calculation

✅ **Use Case**: Monitor key financial KPIs to assess trends, performance, and cost control effectiveness.

---

### 2. 📦 Product Analysis

In-depth analysis of **product profitability** across categories and subcategories.

#### 📈 Key Visuals:

* **Net Profit Trend (Last 180 Days)**: 369.72K৳ (↑ 9%)
* **Top Products by Net Profit**:

  * Powder Milk 500gm – 13.37K৳
  * Cumin Powder 200gm – 11.31K৳
  * Coffee Mate 400gm – 9.72K৳

#### 🧪 Net Profit Decomposition:

* Drill-down: Category → Subcategory → Product
* Example: Drinks & Beverages → Drinking Water → Drinking Water 5L (2.8K৳)

✅ **Use Case**: Identify top-performing products for pricing, promotion, and inventory planning.

---

### 3. 🗕️ Daily Performance Overview *(Additional Page)*

Tracks **daily operational performance** to ensure business consistency.

#### 📌 Metrics:

* Daily Revenue
* Daily Transactions
* Daily Net Profit

#### 🗓️ Time Filters:

* Last 7, 15, 30, 180 days or Full Year
* Toggle: **Top** or **Bottom** Performing Products

---

### 4. 🧺 Basket Analysis (Market Basket Insights)

Market Basket Analysis reveals product pairing patterns for effective cross-selling.

#### 📈 Key Visuals:

* **Lift vs Support Scatter Plot**:

  * Powder Milk 500gm + Baking Powder 110gm
  * White Bread (400g) + Jelly (500gm)
  * Condensed Milk 397gm + Whipped Cream 150gm

* **Network Graph** (Sum of Lift):

  * Powder Milk 500gm (Lift: 24.1)
  * With: Baking Powder 110gm (15.0), Whipped Cream 150gm (12.3)

* **Top Product Pairs Table**:

  * Powder Milk + Baking Powder:

    * Support: 0.4%, Confidence: 16.5%, Lift: 6.5
  * White Bread + Jelly: Lift: 6.3
  * Miniket Rice + Moshur Dal: Lift: 5.2

#### 🧬 Business Impact:

* Identified key cross-selling combinations
* Informed product bundling and promotional strategies

#### 🛠️ Tools Used:

* Power BI Visuals
* Apriori-style metrics (Lift, Confidence, Support)
* Matrix & Graph Visuals

---

### 5. 👥 RFM Segmentation (Customer Value Analysis)

RFM segmentation to uncover **high-value customer groups** and tailor retention strategies.

#### 📊 Segment Distribution:

* Loyal Customers: 26%
* At Risk: 24%
* About to Sleep: 22%
* Potential Loyalists: 20%
* Champions: 8%

#### 🔹 Definitions:

* **Recency**: Last purchase date
* **Frequency**: Number of purchases
* **Monetary**: Spending amount

#### 📊 Visuals:

* Bubble Chart: Recency vs Frequency (size = Monetary)
* Customer Detail Table (Q4 2024):

  * Example: Hasan Islam — RFM: 544, Avg Revenue: 1,278৳, Total: 11,500৳
  * Nadia Chowdhury — RFM: 534, Avg Revenue: 1,163৳, Total: 10,465৳

✅ **Use Case**: Target segments with customized campaigns and loyalty programs.

---

### 6. 🏪 Store Performance Dashboard (Regional Analysis – Bangladesh)

Performance analysis across **eight divisions of Bangladesh** for 2024.

#### 🌍 Key Features:

* **Heatmap**: Total revenue by division

  * Dhaka leads with ৳6.27M, followed by Khulna and Barisal

* **Monthly Revenue Trend**: Peaks in May and November

* **Location Summary Table**:
  \| Location | Transactions | Total Revenue | Total Cost | Net Profit |
  \|----------|--------------|----------------|------------|-------------|
  \| Dhaka | 3,953 | ৳6,274,780 | ৳5,823,633 | ৳451,147 |
  \| Khulna | 1,995 | ৳3,374,945 | ৳3,128,671 | ৳246,271 |
  \| Barisal | 1,929 | ৳2,743,180 | ৳2,550,883 | ৳192,297 |
  \| Mymensingh | 1,045 | ৳1,747,240 | ৳1,610,346 | ৳136,804 |
  \| Sylhet | 836 | ৳1,482,851 | ৳1,366,165 | ৳116,686 |
  \| Chittagong | 723 | ৳1,164,695 | ৳1,008,048 | ৳156,647 |
  \| Rangpur | 666 | ৳1,096,400 | ৳1,008,633 | ৳87,767 |
  \| Rajshahi | 675 | ৳889,976 | ৳803,630 | ৳86,346 |
  \| **Total** | **9,822** | **৳18.76M** | **৳17.38M** | **৳1.38M** |

#### 💼 Business Insights:

* **Top Region**: Dhaka – Over 33% of total revenue
* **Low Margin Zones**: Chittagong & Sylhet

#### 🚀 Growth Strategy:

* Boost marketing in Rajshahi & Rangpur
* Control costs in Dhaka and Khulna

  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 💡 Business Recommendations

## ✅ Business Recommendations

Based on the comprehensive analysis of financial, product, customer, basket, and regional data, the following strategic recommendations are proposed for enhancing business performance:

---

### 1. Optimize Product Pricing & Stock Based on Profitability

* Prioritize inventory and promotions for high-profit products:

  * **Powder Milk 500gm**
  * **Cumin Powder 200gm**
  * **Coffee Mate 400gm**
* Reevaluate or discontinue low-margin or underperforming products to maximize shelf efficiency and profitability.

---

### 2. Enhance Cross-Selling Strategy Using Basket Analysis

* Leverage identified product pairs for bundling:

  * Powder Milk + Baking Powder
  * White Bread + Jelly
  * Condensed Milk + Whipped Cream
* Use strategic product placement and discount combinations to drive higher basket values.

---

### 3. Personalized Customer Retention Strategy via RFM Segmentation

* **Loyal Customers & Champions**:

  * Provide exclusive rewards and early access to new products or sales.
* **At-Risk & About-to-Sleep Segments**:

  * Launch targeted win-back campaigns with personalized offers.
* **Potential Loyalists**:

  * Engage with loyalty programs or incentives to increase frequency.

---

### 4. Region-Specific Strategy for Store Growth

* **High-Growth Areas (e.g., Dhaka, Khulna)**:

  * Increase stock variety and regional promotions.
* **Underperforming Regions (e.g., Rajshahi, Rangpur)**:

  * Strengthen marketing efforts and local supplier engagement.
* Monitor cost-heavy regions for operational optimization (e.g., Dhaka, Khulna).

---

### 5. Improve Operational Efficiency

* Review marketing and operational expenditures monthly to identify cost-saving opportunities.
* Expand automation in analytics and reporting using Power BI to enhance decision-making speed and accuracy.

---

### 6. Campaign Planning Based on Seasonal Trends

* Plan promotions and product launches aligned with peak seasons (e.g., May, November).
* Scale inventory and staff planning according to demand cycles to maximize revenue and customer satisfaction.

---

These recommendations aim to boost profitability, enhance customer retention, and support sustainable regional expansion using data-driven insights.

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🌟 Project Impact

## 📊 Project Impact

This business intelligence project delivered significant value across multiple dimensions of the retail operation, enabling more informed and agile decision-making:

### 💰 Financial Performance Monitoring

* Delivered comprehensive financial KPIs in a single dashboard.
* Enabled real-time tracking of revenue, costs, and profitability trends.
* Improved decision-making around operational spending and cost optimization.

### 🧾 Product Portfolio Optimization

* Identified top-performing products and categories based on net profit trends.
* Helped remove or revise underperforming products, reducing inventory waste.
* Provided data-backed support for product bundling and promotional strategies.

### 📆 Operational Efficiency

* Daily performance tracking ensured real-time operational oversight.
* Revealed daily sales spikes and low-performance periods for better workforce planning.
* Allowed leadership to respond rapidly to anomalies or emerging trends.

### 🛒 Customer Insight & Retention

* RFM segmentation facilitated targeted marketing to key customer segments.
* Helped design loyalty programs and customer win-back campaigns.
* Improved retention of high-value customers, boosting long-term revenue.

### 🛍️ Cross-Selling & Basket Value Enhancement

* Market Basket Analysis surfaced strong product affinities.
* Empowered the business to implement data-driven cross-sell campaigns.
* Increased average basket value and encouraged complementary product purchases.

### 🏪 Regional Expansion & Store Strategy

* Store-wise revenue analysis identified high- and low-performing regions.
* Helped strategize location-based promotions and resource allocation.
* Supported expansion planning in underperforming but potential-rich regions.

### 📈 Overall Business Impact

* Delivered actionable insights at all business levels — from operations to strategy.
* Supported data-driven decisions that increased net profit by 76% YoY.
* Built a foundation for a performance-focused, scalable, and customer-centric growth strategy.

---

✅ **This project showcases the power of Power BI in transforming raw transactional data into high-impact strategic intelligence.**







