# Shwapno Chain Retail Store Analytics – Power BI Project
## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiMGNhNWRhZjgtYzAyMS00YTBiLWI0OTEtNWMyMjJiNzgwMDY5IiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

## 📌 Project Overview  

 
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

Fact_table: Contains transactional records (Qty, UnitPrice, OrderDate, ProductID, StoreID, CustomerID)

Customers: Customer demographics (ID, Age, Name, Email)

Product: Product metadata (CategoryID, Marketing Cost, Rent)

Category & Subcategory: Hierarchical product classification

Date: Full date dimension with breakdowns

Stores: Store-wise location and name

Marketing_Spend: Offline and online monthly marketing expenses

RFM Table & RFM_Segmented: Customer segmentation based on recency, frequency, and monetary values

RawProductsOrders: Intermediate table to track product orders

And Product: Composite key for drill-down hierarchy

 
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

Cleaned null and duplicate entries

Standardized date formats

Created calculated columns for month, year, and day types

Calculated product-level net profit (Revenue - Cost - Marketing Expense)

Derived customer lifetime value (CLV) and net profit contributions
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/d6123ff5-c475-443f-9969-947afffefc65)


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
![image](https://github.com/user-attachments/assets/1db076c3-ec1d-4d31-a05a-7f4c429d1ddf)


### 2. **📍 Financial Overview**
![image](https://github.com/user-attachments/assets/174a0440-8939-430e-bc5b-080b796f55b2)


#### 3. **📦 Product Analysis**
![image](https://github.com/user-attachments/assets/564ba4c1-bf7f-400b-a4c8-f14ebc6cd3ee)

### 4 . **📍 Basket Analysis**
![image](https://github.com/user-attachments/assets/1db076c3-ec1d-4d31-a05a-7f4c429d1ddf)


### 5. **📍 RFM Segmentation**
![image](https://github.com/user-attachments/assets/174a0440-8939-430e-bc5b-080b796f55b2)


#### 6. **📦 Regional Performance**
![image](https://github.com/user-attachments/assets/564ba4c1-bf7f-400b-a4c8-f14ebc6cd3ee)



[🔼 Back to Table of Contents](#-table-of-contents)

---
## 📈 Key Business Insights

### 1. **📍 Financial Overview**

A complete snapshot of the company’s **overall financial health** across months.

#### 🔢 Key Metrics:

* **Total Transactions**: 13.9K (↑ 42% YoY)
* **Total Revenue**: 26.7M৳ (↑ 73%)
* **Net Profit**: 1.9M৳ (↑ 76%)
* **Total Cost**: 24.8M৳ (↑ 73%)

#### 📆 Monthly Breakdown Includes:

* Product Purchasing Cost
* Marketing Cost (Online & Offline)
* Operational Cost (Salaries, Rent, Utility Bills)
* Net Profit Calculation

✅ **Use Case**: Monitor high-level financial KPIs to evaluate monthly trends, performance, and cost management.

---

### 2. **📦 Product Analysis**

Drills into **profitability by product**, category, and subcategory, focusing on recent trends.

#### 📈 Key Visuals:

* **Net Profit Trend (180 Days)**: 369.72K৳ (↑ 9% from prior 180 days)
* **Top Products by Net Profit**:

  * Powder Milk 500gm – 13.37K৳
  * Cumin Powder 200gm – 11.31K৳
  * Coffee Mate 400gm – 9.72K৳

#### 🧠 Net Profit Decomposition:

* Drill-down from Category → Subcategory → Product
* Example: Drinks & Beverages → Drinking Water → Drinking Water 5L (2.8K৳)

✅ **Use Case**: Identify best-performing products, manage pricing, optimize stock.

---

### 3. **📅 Daily Performance Overview** *(Additional Page)*

Tracks **day-to-day business performance**, helping to monitor operational consistency.

#### 📌 Metrics Tracked Daily:

* Revenue
* Transactions
* Net Profit

#### 📅 Time Filters:

* Last 7, 15, 30, 180 days or Full Year
* Toggle between **top** or **bottom** performing products

4.🧺 Basket Analysis (Market Basket Insights)
Basket analysis was performed to uncover product affinity patterns, enabling more effective cross-selling strategies.

📌 Key Visuals:
Lift vs Support by Basket Name:
A scatter plot showing frequently co-purchased item pairs. Notable combinations include:

Powder Milk 500gm + Baking Powder 110gm

White Bread (400g) + Jelly (500gm)

Condensed Milk 397gm + Whipped Cream 150gm

Sum of Lift by Product Basket (Network Graph):
Highlights the most influential products in driving basket value.

Powder Milk 500gm shows the highest lift (24.1), often bought with:

Baking Powder 110gm (Lift: 15.0)

Whipped Cream 150gm (Lift: 12.3)

Top Product Pairs Table:
Ranked by lift, support, and confidence. Key metrics include:

Powder Milk 500gm + Baking Powder 110gm:

Support: 0.4%

Confidence: 16.5%

Lift: 6.5

White Bread + Jelly: Lift: 6.3

Miniket Rice + Moshur Dal: Lift: 5.2

🧠 Business Impact:
Helped identify strong cross-selling opportunities.

Guided product bundling decisions (e.g., Milk + Cream).

Revealed complementary item relationships for promotional planning.

🧰 Tools & Techniques:
Power BI: For visualizing basket patterns.

Market Basket Analysis (Apriori-style metrics: Lift, Confidence, Support).

Graph and matrix visuals for network-style product association.

5. 👥 RFM Segmentation (Customer Value Analysis)
This section identifies and profiles customer segments using Recency, Frequency, and Monetary (RFM) metrics to drive personalized marketing and retention strategies.

📊 Key Segments:
Loyal Customers: 26%

At Risk: 24%

About to Sleep: 22%

Potential Loyalists: 20%

Champions: 8%

🔹 Definitions:

Recency: How recently a customer made a purchase

Frequency: How often the customer purchases

Monetary: How much the customer spends

📈 Visual Insights:
Bubble Chart (Recency vs Frequency)

Size represents Monetary Value

Clusters reveal high-value customers (lower recency, higher frequency)

Red-orange bubbles show vulnerable segments (low frequency, high recency)

Customer Detail Table (Q4 2024):

Top customers by RFM score, average and total revenue

Example:

Hasan Islam:

RFM: 544

Avg Revenue: 1,278৳

Total Revenue: 11,500৳

Nadia Chowdhury:

RFM: 534

Avg Revenue: 1,163৳

Total Revenue: 10,465৳

6. 🏪 Store Performance Dashboard (Regional Analysis – Bangladesh)
This dashboard provides a comprehensive view of store-wise performance across eight major divisions of Bangladesh in 2024, helping business stakeholders make data-driven decisions.

🌍 Key Features:
Location-Based Performance Map

Visualizes total revenue by division on a heatmap of Bangladesh.

Dhaka leads with ৳6.27M in revenue, followed by Khulna and Barisal.

Monthly Revenue Trend (2024)

Revenue shows steady growth, peaking in May and November.

Trendline indicates seasonal fluctuations and strategic growth.

Transaction Summary by Location

Location	Transactions	Total Revenue	Total Cost	Net Profit
Dhaka	3,953	৳6,274,780	৳5,823,633	৳451,147
Khulna	1,995	৳3,374,945	৳3,128,671	৳246,271
Barisal	1,929	৳2,743,180	৳2,550,883	৳192,297
Mymensingh	1,045	৳1,747,240	৳1,610,346	৳136,804
Sylhet	836	৳1,482,851	৳1,366,165	৳116,686
Chittagong	723	৳1,164,695	৳1,008,048	৳156,647
Rangpur	666	৳1,096,400	৳1,008,633	৳87,767
Rajshahi	675	৳889,976	৳803,630	৳86,346
Total	9,822	৳18.76M	৳17.38M	৳1.38M

💼 Business Insights:
Top-Performing Region: Dhaka — contributes over 33% of total revenue.

High Revenue–Low Margin: Some areas like Chittagong and Sylhet offer high revenue but relatively lower profit margins.

Growth Strategy:

Boost marketing and supply chain in Rajshahi & Rangpur to increase volume.

Monitor cost control in high-expense regions like Dhaka and Khulna.

## 📈 Key Business Insights keypoints:

ADD You Chat Gpt 
  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 💡 Business Recommendations

ADD REcommebdations 

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🌟 Project Impact

Add Project Recommentation.






