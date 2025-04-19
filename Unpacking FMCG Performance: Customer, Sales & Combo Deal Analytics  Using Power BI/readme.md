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
- **aligned_sales_targets:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/aligned_sales_targets.csv)
- **calendar:**   [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/calendar.csv)
- **customers:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/customers.csv)
- **products:**[Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/products.csv)
- **promotions:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/promotions.csv)
- **regions:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/regions.csv)
- **sales_with_promotions:** [Click here](https://github.com/BI-with-Sabbir/Power-Bi-Project/blob/main/FMCG%20Sales%20%26%20Marketing%20BI/sales_with_promotions.csv) 


  
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
![image](https://github.com/user-attachments/assets/d8917d02-c745-4478-97ad-7ebd74471521)



[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### General Measures  
```DAX
Total Sales = sum(Fact_Sales[Sales_Amount]) 
```

```DAX
Total Quantity Sold = SUM(Fact_Sales[Quantity_Sold])

```

```DAX
Total Profit = [Total Sales]-[Total Cost]
)
```
```DAX
Post Promotion Sales = CALCULATE([Total Sales],Dim_Promotions[Promotion_Type]="No Promotion",Fact_Sales[Date]>max(Dim_Promotions[End_Date]))/CALCULATE(DISTINCTCOUNT(Fact_Sales[Date]),
Dim_Promotions[Promotion_Type]="No Promotion",Fact_Sales[Date]>max(Dim_Promotions[End_Date]))
```

```DAX
Pre Promotion Sales = CALCULATE([Total Sales],Dim_Promotions[Promotion_Type]="No Promotion",Fact_Sales[Date]<min(Dim_Promotions[Start_Date]))/CALCULATE(DISTINCTCOUNT(Fact_Sales[Date]),Dim_Promotions[Promotion_Type]="No Promotion",Fact_Sales[Date]<min(Dim_Promotions[Start_Date]))
```

```DAX
Pre Year Order = CALCULATE(COUNT(Fact_Sales[Transaction_ID]),Dim_Date[Year]=MAX(Dim_Date[Year])-1) 
```

```DAX
Previous Year Cost = CALCULATE([Total Cost],Dim_Date[Year]=MAX(Dim_Date[Year])-1) 
```
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  
### 📌 Financial Analysis 
![image](https://github.com/user-attachments/assets/30646fb2-f8e3-4792-9476-060a372a689e)



### 📌 Customer Analysis by RFM Sales
![image](https://github.com/user-attachments/assets/586bce8a-44b5-478d-9b7e-7ffc7c4f3a65)



### 📌 Marketing Analysis on individual product
![image](https://github.com/user-attachments/assets/90f39e60-493d-4540-8c4a-b02ca3d0e547)


[🔼 Back to Table of Contents](#-table-of-contents)

---
## 📈 Key Business Insights

🔍 Key Business Insight: Promotions Boost Sales but Fail to Drive Long-Term Loyalty
📊 Sales & Promotions Analysis
- Bundle Offers generated the highest daily sales (152.9 units/day), far outperforming other promotion types.

- Sales quantity increased from 1,191 (Pre-Promo) to 1,395 (Post-Promo) — a 17% uplift.

- However, YoY Sales Quantity and Revenue still dropped by 22.4% and 21.3% respectively, signaling issues beyond promotions.

💸 Profitability Insight
- Despite promotions, Profit % remains flat at 24.8% (vs 24.9% last year).

- Total cost decreased by 21.2%, yet revenue dropped at the same pace, indicating inefficient revenue generation.

- High-cost categories like Snacks and Personal Care contribute most to sales, but have moderate profit margins (~23–37%).

👥 Customer Behavior Insight
Top 2 segments by volume:

- Loyal Customers: 45%

- Potential Loyalists: 31%

🔼 Promotions impact is highest among:

- Potential Loyalists (+2638 units)

- At-Risk Customers (+2825 units)

But At Risk & Lost Customers form ~24% of the base, with minimal post-promotion retention.
  
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 💡 Business Recommendations

1. 🎯 Targeted Loyalty Campaigns
Why: 45% of customers are already Loyal Customers, and 31% are Potential Loyalists.
What to do:

- Launch exclusive loyalty programs for these segments (e.g., early access, personalized offers).

- Use recency and frequency scores to automate follow-up campaigns.

2. 📈 Optimize Promotional Strategy
Why: Bundle Offers yield the highest daily sales, but overall YoY revenue and profit dropped.
What to do:

- Focus on high-performing promo types (like Bundle Offers).

- Avoid generic discounts; instead, align promotions with high-margin categories.

- Track promotion ROI per customer segment (especially for “At Risk” and “Potential Loyalists”).

3. 🛒 Reassess Product-Cost Structure
Why: Snacks and Personal Care dominate sales volume but have average profit margins.
What to do:

- Identify low-margin, high-cost products for potential delisting or repositioning.

- Explore product bundling to increase sales of lower-performing, high-margin items.

4. 🌍 Regional Sales Focus
Why: Revenue heatmap shows significant regional disparity.
What to do:

- Prioritize underperforming regions with localized campaigns.

- Invest in region-specific market research to understand barriers.

5. 🔄 Customer Re-engagement Strategy
Why: 23% of customers are “At Risk” and many fall into “Lost Customer” category.
What to do:

- Deploy email/SMS remarketing focused on their previously purchased items.

- Offer personalized win-back discounts to encourage reactivation.

6. 📊 Sales Channel Diversification
Why: Sales are split across Departmental Store (33.9%), Super Shop (33.4%), and Online (32.6%).
What to do:

- Enhance digital presence with stronger online campaigns and UX improvements.

- Incentivize omnichannel behavior through click & collect or app-based offers.

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🌟 Project Impact

🌟 Project Impact
✅ Improved Customer Segmentation & Retention Strategy
Leveraged RFM analysis to classify customers into actionable segments (Loyal, At Risk, Lost, etc.), enabling targeted campaigns and reducing churn.

✅ Identified Top Revenue & Underperforming Products
Clear visuals of category-wise sales and profitability helped focus on high-margin products and optimize inventory for low performers.

✅ Enhanced Promotion Effectiveness Tracking
Revealed that Bundle Offers had the highest sales impact per day, guiding smarter promotional planning and increasing ROI.

✅ Boosted Regional Sales Strategy
Geo insights highlighted region-wise performance, supporting localized campaigns and improving distribution efficiency.

✅ Empowered Sales & Marketing Teams with Self-Service Dashboards
Interactive, intuitive Power BI dashboards allowed non-technical users to explore KPIs, trends, and customer behavior independently.

✅ Supported Goal Setting with Target Fill Tracking
Visualizing annual target fill (currently at 59.8%) enabled leadership to proactively adjust strategy and allocate resources effectively.






