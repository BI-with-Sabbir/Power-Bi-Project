# Power BI Project: Sales Optimization & FMCG Market Intelligence

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiNWM0Mjk1OWQtMTAxMS00OTlkLTg5NzctZDBmNmViYTYxNmRmIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

## 📌 Project Overview  
![image](https://github.com/user-attachments/assets/29ead28c-df86-4935-aaa7-2b68cb879c8c)




In the highly competitive Fast-Moving Consumer Goods (FMCG) sector, understanding customer preferences and optimizing sales strategies are crucial for driving growth. This project focuses on delivering actionable insights through comprehensive analysis of sales data, customer behavior,RFM segmentation and combo (bundle) sales performance.

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

### Belongs data in the Dataset:  
[Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/Unpacking%20FMCG%20Performance%3A%20Customer%2C%20Sales%20%26%20Combo%20Deal%20Analytics%20%20Using%20Power%20BI/FMCG.xlsx)) 

- **dim_customer** 
- **dim_date**  
- **dim_Products** 
- **dim_target_orders**
- **fact_Order_line** 
- **fact_Orders** 
- **RFM Segmentation** 
- **RFM Table** 
  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  
#### **Data Cleaning and  Preprocessing:**  
- Promote Headers.
- Change Data Types
- Remove Errors
- Drop Unnecessary Columns
- Replace Null Values


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/b8bd1f8f-52fd-419f-ba5a-1ce57308152c)



[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### General Measures  
```DAX
Total orders quantity shipped = SUM(fact_Order_line[delivery_qty]) 
```

```DAX
Total Orders quantity = SUM(fact_Order_line[order_qty]) 
```

```DAX
Total Order Lines = COUNT(fact_Order_line[delivery_qty])
```
```DAX
Total Number of Orders = DISTINCTCOUNT(fact_Orders[order_id])
```

```DAX
Rev = SUM(fact_Order_line[Sales Amount])
```

```DAX
OT % = DIVIDE([Orders Delivered On Time], [Total Number of Orders], 0)
```

```DAX
OTIF % = DIVIDE([Orders Delivered In Full and On Time], [Total Number of Orders], 0)
```

```DAX
VOFR % = DIVIDE([Total orders quantity shipped], [Total Orders quantity], 0) 
```

```DAX
VOFR % = DIVIDE([Total orders quantity shipped], [Total Orders quantity], 0) 
```

```DAX
IF % = DIVIDE([Orders Delivered In Full], [Total Number of Orders],0) 
```
```DAX
LIFR % = DIVIDE([LIFR], [Total Order Lines], 0) 
```
```DAX
_1 Total Revenue = 
CALCULATE(
    [Revenue],
    ALLSELECTED(fact_Order_line)
)

```
```DAX
_1 Revenue GT% = 
DIVIDE(
    [Revenue],
    [_1 Total Revenue],
    0
)
```

```DAX
_1 Rank = 
IF(
    ISINSCOPE(dim_Customers[customer_name]),
    RANKX(
        ALLSELECTED(dim_Customers[customer_name]),
        [Revenue]
    )
)
```

```DAX
_1 Cutoff Revenue GT% = 
VAR ClosestCustomer = [_1 Cutoff Customer f]
VAR RevenueValue = 
    IF(
        SELECTEDVALUE(dim_Customers[customer_name]) = ClosestCustomer,
        [Revenue],
        BLANK()
    )
RETURN
IF(
    ISBLANK(RevenueValue) || RevenueValue = 0,
    BLANK(), -- Avoid division by zero or blank values
    RevenueValue / 70000000

)
```

```DAX
_1 Cutoff Revenue % = 
VAR ClosestCustomer = [_1 Cutoff Customer f]
RETURN
IF(
    SELECTEDVALUE(dim_Customers[customer_name]) = ClosestCustomer,
    [_1 Cumulative Revenue %],
    BLANK()
)
```

```DAX
_1 Cutoff Rank = 
VAR ClosestCustomer = [_1 Cutoff Customer f]
RETURN
IF(
    SELECTEDVALUE(dim_Customers[customer_name]) = ClosestCustomer,
    [_1 Rank],
    BLANK()
)
```

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  
### 📌 Revenue Analysis by Pareto Chart
![image](https://github.com/user-attachments/assets/ca55a845-919f-4b17-a6cb-378ac467976c)




### 📌 Customer Analysis by RFM Sales
![image](https://github.com/user-attachments/assets/4366ac41-1b16-43ca-9eaf-07ab56d33d9a)




### 📌 Supply chain analysis By On time(OT), In Full(IF), OTIF(On Tine & In Full)
![image](https://github.com/user-attachments/assets/7bca7d63-43ac-4854-8440-e85207d8aa14)


### 📌 Order Analysis
![image](https://github.com/user-attachments/assets/6825dabe-2a71-4e7b-9a28-79d02b388624)


### 📌 Order line Analysis
![image](https://github.com/user-attachments/assets/13b57a2e-630b-4c47-8249-0c74a0df99c2)


### 📌 Delivery Analysis 
![image](https://github.com/user-attachments/assets/a23fd260-4479-4c17-a702-da15adb01283)


### 📌 Trend Analysis 
![image](https://github.com/user-attachments/assets/7bcac441-4de9-463a-bfef-6299448058e1)


### 📌 Scorecard Analysis
![image](https://github.com/user-attachments/assets/e0e8f399-d4b7-4805-b3b6-7a4c6fe1b0bb)


[🔼 Back to Table of Contents](#-table-of-contents)

---
## 📈 Key Business Insights

🔍 Key Business Insight: Promotions Boost Sales but Fail to Drive Long-Term Loyalty
This document presents the core findings and observations based on the FMCG sales, customer behavior, and supply chain performance data.

---

## 🧾 1. Revenue Concentration (Pareto Analysis)
- **56% of total revenue** comes from **top 5 customers**.
- Major clients include: `City Mart`, `Royal Mart BD`, `Horizon Retail`, etc.
- Indicates strong dependency on a few key customers, crucial for retention and growth strategies.

---

## 🛒 2. Top Product & Manager Performance
- **Top 3 products** by sales:
  - `Licon`, `Nihar Lovely`, and `Parachute Baby Care` — each contributes ~33-34% of their category revenue.
- **Top Sales Managers**:
  - `Md Sabit`, `Shanta Aktar`, `Foiz Khan` — each drove over **24M+** in sales.

---

## 🧍‍♂️ 3. Customer Segmentation (RFM Analysis)
- **Customer Categories**:
  - 20% Champions
  - 25% Loyal Customers
  - 13-15% At Risk / Need Attention
- Opportunity to re-engage dormant segments and nurture potential loyalists.

---

## 🚚 4. Supply Chain Performance (OTIF Metrics)
- **On-Time (OT%)**: 59% vs Target 84%
- **In-Full (IF%)**: 53% vs Target 76%
- **OTIF% Combined**: 29% vs Target 67%
- Key bottlenecks identified: Delivery delays, partial shipments, supply issues.

---

## 📦 5. Order & Delivery Satisfaction
- **Total Orders:** 31,729
- **Total Revenue:** 470.8M
- **Average Delivery Satisfaction:** 3.7/5
- Gap between order experience and delivery experience implies logistics improvement is needed.

---

## 🌍 6. Regional Revenue Distribution
- **Top Performing Cities**:
  - Barishal, Dhaka, Rangpur, Khulna.
- **Top Clients by Region**:
  - Barishal: `Delta Store`, `Horizon Retail`
  - Dhaka: `Royal Mart BD`, `Loyal Trade House`
- Recommended: Focused logistics improvement and marketing for high-potential regions.

---

## 📉 7. Performance Scorecard Summary
- **Targets Missed**:
  - OTIF%, IF%, OT%, Delivery Satisfaction
  - Quantity Delivered vs Ordered
- Recommendation: Investigate internal fulfillment, delivery scheduling, and stock optimization processes.

---


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🌟 Project Impact

🌟 Project Impact
## ✅ Recommendations
- Strengthen relationships with top revenue-generating customers.
- Address supply chain gaps to improve OTIF and delivery satisfaction.
- Focus retention strategies on at-risk and potential loyal customers.
- Prioritize regions with high sales contributions for promotional activities.






