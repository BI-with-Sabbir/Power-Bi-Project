# Power BI Project: Financial Reporting In Power BI

## 📊 Watch Live Dashboard  
<p> https://app.powerbi.com/view?r=eyJrIjoiNzRhYjc4MTEtNjVhYi00MWVmLTllODAtNzcxYjYxOGQzOGM1IiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D  </a>
</p>

## 📌 Project Overview  
The goal of this project is to develop an interactive financial reporting dashboard using Microsoft Power BI. The dashboard will help organizations analyze, track, and visualize their financial data, enabling them to make data-driven business decisions efficiently.
Real-time Insights: Revenue, expenses, budget, and cash flow tracking
Customizable Dashboards: Dynamic filters for tailored reports
Core Reports: Income Statement, Balance Sheet, Cash Flow, Aged Accounts, Turnover Summary
Accessibility: Web & mobile-friendly for easy access
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

This dataset consists of multiple tables that are structured to provide insights into financial performance, customer transactions, and business operations. Below is a breakdown of key tables and their attributes:

The dataset consists of multiple tables, structured to provide insights into financial performance, customer transactions, and business operations.

1. Core Financial Tables:

Income Statement, Closing Date, Items, Value, Balance Sheet Data, Balance Sheet Type, Category, Sub Category, Value, Cash Flow Data, Cash Flow Category,Cash Flow Sub Category, Cash Flow Type, Cash Flow, Company Expenses, Expense Category, Expense Name, Expense Month, Value.

2. Transaction & Sales Tables

Invoice, Closing Date, Customer Index, Due Date, Invoice Number, Value, Sales, Outstand, Customer Code, Customer Name Index, Delivery Region Index, Size Total, Order Quantity, Order Number, Product Description Index.

3. Supporting Data Tables

Regions, City, Country, Full Name, Standard Country Code, Customers, Customer Index, Customer Name, Products, Gross Sales, Product Group, product Group Index

4.Date Table

Date, Day of Week,Day & Month

5. Report & Visualization Templates

Income Statement Template
Sources - Normalized
Tax Index

Summary:
Balance Sheet Template
Balance Sheet Items
Balance Value Normalized
Cash Flow Template
Cash Flow Item
Cash Flow Normalized

**Value:

Aged Debtor Groups, Sale Detail, Sale Order, Min & Max, Channel Revenue, Category, Expense Name, Month & Year

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

### Steps Taken:  
1. Enter All Data in Power Query and the ETL process starts.
2. Correct Data types are applied in every column.
3. Create a Parameter Table for All incoming data. 
4.```DAX
      = (StartDate as date, EndDate as date, FYStartMonth as number) as table =>
     let
    DayCount = Duration.Days(Duration.From(EndDate - StartDate)),
    Source = List.Dates(StartDate,DayCount,#duration(1,0,0,0)),
    TableFromList = Table.FromList(Source, Splitter.SplitByNothing()),   
    ChangedType = Table.TransformColumnTypes(TableFromList,{{"Column1", type date}}),
    RenamedColumns = Table.RenameColumns(ChangedType,{{"Column1", "Date"}}),
    InsertYear = Table.AddColumn(RenamedColumns, "Year", each Date.Year([Date]),type text),
    InsertYearNumber = Table.AddColumn(RenamedColumns, "YearNumber", each Date.Year([Date])),
    InsertQuarter = Table.AddColumn(InsertYear, "QuarterOfYear", each Date.QuarterOfYear([Date])),
    InsertMonth = Table.AddColumn(InsertQuarter, "MonthOfYear", each Date.Month([Date]), type text),
    InsertDay = Table.AddColumn(InsertMonth, "DayOfMonth", each Date.Day([Date])),
    InsertDayInt = Table.AddColumn(InsertDay, "DateInt", each [Year] * 10000 + [MonthOfYear] * 100 + [DayOfMonth]),
    InsertMonthName = Table.AddColumn(InsertDayInt, "MonthName", each Date.ToText([Date], "MMMM"), type text),
    InsertCalendarMonth = Table.AddColumn(InsertMonthName, "MonthInCalendar", each (try(Text.Range([MonthName],0,3)) otherwise [MonthName]) & " " & Number.ToText([Year])),
    InsertCalendarQtr = Table.AddColumn(InsertCalendarMonth, "QuarterInCalendar", each "Q" & Number.ToText([QuarterOfYear]) & " " & Number.ToText([Year])),
    InsertDayWeek = Table.AddColumn(InsertCalendarQtr, "DayInWeek", each Date.DayOfWeek([Date])),
    InsertDayName = Table.AddColumn(InsertDayWeek, "DayOfWeekName", each Date.ToText([Date], "dddd"), type text),
    InsertWeekEnding = Table.AddColumn(InsertDayName, "WeekEnding", each Date.EndOfWeek([Date]), type date),
    InsertWeekNumber= Table.AddColumn(InsertWeekEnding, "Week Number", each Date.WeekOfYear([Date])),
    InsertMonthnYear = Table.AddColumn(InsertWeekNumber,"MonthnYear", each [Year] * 10000 + [MonthOfYear] * 100),
    InsertQuarternYear = Table.AddColumn(InsertMonthnYear,"QuarternYear", each [Year] * 10000 + [QuarterOfYear] * 100),
    ChangedType1 = Table.TransformColumnTypes(InsertQuarternYear,{{"QuarternYear", Int64.Type},{"Week Number", Int64.Type},{"Year", type text},{"MonthnYear", Int64.Type}, 
    {"DateInt", Int64.Type}, {"DayOfMonth", Int64.Type}, {"MonthOfYear", Int64.Type}, {"QuarterOfYear", Int64.Type}, {"MonthInCalendar", type text}, {"QuarterInCalendar", 
    type text}, {"DayInWeek", Int64.Type}}),
    InsertShortYear = Table.AddColumn(ChangedType1, "ShortYear", each Text.End(Text.From([Year]), 2), type text),
    AddFY = Table.AddColumn(InsertShortYear, "FY", each "FY"&(if [MonthOfYear]>=FYStartMonth then Text.From(Number.From([ShortYear])+1) else [ShortYear]))
     in
    AddFY```

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/46b14e2f-5b6e-4345-b457-1fadd5c5efda)


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  
### Income Analysis  
```DAX
Revenues = 
CALCULATE( [Financial Values], 'Income Statement'[Type] = "Revenues" )```

```DAX
Revenue LY = CALCULATE( [Revenues], DATEADD( Dates[Date], -1, YEAR ) )```

```DAX
Net Profit Margin LY = CALCULATE( [Net Profit Margin], DATEADD( Dates[Date], -1, YEAR ) )```

```DAX
Net Profit Margin = 
DIVIDE( [Net Profit], [Revenues], 0 )
```
```DAX
Actuals (,000) = 
VAR Revenue = CALCULATE( [Financial Values], 'Income Statement'[Type] = "Revenues" )
VAR Expense = CALCULATE( [Financial Values], 'Income Statement'[Type] = "Expenses" ) * -1

RETURN
DIVIDE( 
IF( SELECTEDVALUE( 'Income Statement'[Type] ) = "Revenue" ,
    Revenue,
        IF( SELECTEDVALUE( 'Income Statement'[Type] ) = "Expenses" ,
            Expense,
                Revenue + Expense ) ), 1000, 0 )) 
```

```DAX
Actuals LY (,000) = 
CALCULATE( [Actuals (,000)], DATEADD( Dates[Date], -1, YEAR ) )) 
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
Annual %s = 
VAR CurrentItem = SELECTEDVALUE( 'Income Statement Template'[Summary] )
VAR CurrentSummary = SELECTEDVALUE( 'Income Statement Template'[Summary] )

RETURN
SWITCH( TRUE(),
    CurrentSummary = "Total Revenues", 1,
    CurrentSummary = "Total COGS", DIVIDE( [COGS], [Revenues], 0 ),
    CurrentSummary = "Total Gross Profit", DIVIDE( [Gross Profit], [Revenues], 0 ),
    CurrentSummary = "Total Other Expenses", DIVIDE( [Expenses], [Revenues], 0 ),
    CurrentSummary = "Total Net Profit", DIVIDE( [Net Profit], [Revenues], 0 ),
    //CurrentSummary = "Net Profit %", [Net Profit Margin], (not using)
        DIVIDE( CALCULATE( [Actuals (,000)]  * 1000,
            FILTER( 'Income Statement', 'Income Statement'[Items] = CurrentItem ) ), [Revenues], 0 ))```
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
### Cash Flow Analysis:
```DAX
Net Cash Flow - Operations = 
[Cash Reciepts From - Operations] - [Cash Paid For - Operations]```

```DAX
Net Cash Flow - Investing = 
[Cash Reciepts From - Investing] - [Cash Paid For - Investing]```

```DAX
Net Cash Flow - Financing = 
[Cash Reciepts From - Financing] - [Cash Paid For - Financing]```

```DAX
Cash Reciepts From - Operations = 
CALCULATE( [Cash Flow Values],
        FILTER( 'Cash Flow Data',
        'Cash Flow Data'[Cash Flow Category] = "Cash receipts from" &&
        'Cash Flow Data'[Cash Flow Type] = "Operations" ))```

```DAX
Cash Reciepts From - Investing = 
CALCULATE( [Cash Flow Values],
        FILTER( 'Cash Flow Data',
        'Cash Flow Data'[Cash Flow Category] = "Cash receipts from" &&
        'Cash Flow Data'[Cash Flow Type] = "Investing" ))```

```DAX
Cash Paid For - Operations = 
CALCULATE( [Cash Flow Values],
        FILTER( 'Cash Flow Data',
        'Cash Flow Data'[Cash Flow Category] = "Cash paid for" &&
        'Cash Flow Data'[Cash Flow Type] = "Operations" ))```
```DAX
C/F Values = 
VAR CurrentItem = SELECTEDVALUE( 'Cash Flow Template'[Cash Flow Normalized] )

RETURN
SWITCH( TRUE(),
    CurrentItem = "Net Cash Flow from Operations", [Net Cash Flow - Operations],
    CurrentItem = "Net Cash Flow from Investing Activities", [Net Cash Flow - Investing],
    CurrentItem = "Net Cash Flow from Financing Activities", [Net Cash Flow - Financing],
    CurrentItem = "Net Increase in Cash", [Net Cash Flow - Operations] + [Net Cash Flow - Investing] + [Net Cash Flow - Financing],
    CurrentItem = "Cash at Beginning of Year", [Cash A Start Of Year],
    CurrentItem = "Cash at End of Year", [Cash A Start Of Year] - ( [Net Cash Flow - Operations] + [Net Cash Flow - Investing] + [Net Cash Flow - Financing] ),
        CALCULATE( [Cash Flow Values], FILTER( 'Cash Flow Data', 'Cash Flow Data'[Cash Flow Sub Category] = CurrentItem )  ))```

### Balanced Sheet Analysis:

```DAX
Total Liabilities = 
[Current Liabilites] + [Long-Term Liabilities]```
```DAX
Total Assets = [Current Assets] + [Fixed Assets] + [Other Assets]```
```DAX
Owners Equity = 
CALCULATE( [BS Values], 'Balance Sheet Data'[Category] = "Owner's Equity" )```

```DAX
BS Values = 
CALCULATE( SUM( 'Balance Sheet Data'[Value] ),
    TREATAS( VALUES( Dates[Year] ), 'Balance Sheet Data'[Year] ) )```
```DAX
B/S Values = 
VAR CurrentItem = SELECTEDVALUE( 'Balance Sheet Template'[Balance Sheet Normalized] )

RETURN
SWITCH( TRUE(),
    CurrentItem = "Total current assets", [Current Assets],
    CurrentItem = "Total fixed assets", [Fixed Assets],
    CurrentItem = "Total Other assets", [Other Assets],
    CurrentItem = "Total Assets", [Total Assets],
    CurrentItem = "Total current liabilities", [Current Liabilites],
    CurrentItem = "Total long-term liabilities", [Long-Term Liabilities],
    CurrentItem = "Total owner's equity", [Owners Equity],
    CurrentItem = "Total Liabilities and Owner's Equity", [Liabilities and Owners Equity],
    CurrentItem = "Debt Ratio (Total Liabilities / Total Assets)", FORMAT( DIVIDE( [Current Liabilites] + [Long-Term Liabilities], [Total Assets], 0 ), "0.00" ),
    CurrentItem = "Current Ratio (Current Assets / Current Liabilities)", FORMAT( DIVIDE( [Total Liabilities], [Total Assets], 0 ), "0.00" ),
    CurrentItem = "Working Capital (Current Assets - Current Liabilities)", FORMAT( [Current Assets] - [Current Liabilites], "0" ),
    CurrentItem = "Assets-to-Equity Ratio (Total Assets / Owner's Equity)", FORMAT( DIVIDE( [Total Assets], [Owners Equity], 0 ), "0.00" ),
    CurrentItem = "Debt-to-Equity Ratio (Total Liabilities / Owner's Equity)", FORMAT( DIVIDE( [Total Liabilities], [Owners Equity], 0 ), "0.00" ),
        CALCULATE( [BS Values], FILTER( 'Balance Sheet Data', 'Balance Sheet Data'[Sub Category] = CurrentItem )  ))```
### Annualized Analysis
```DAX
TY vs PY Actuals % = 
VAR CurrentItem = SELECTEDVALUE( 'Income Statement Template'[Items - Normalized] )

RETURN
SWITCH( TRUE(),
    CurrentItem = "Total Revenues", DIVIDE( [TY vs PY Actuals], [Revenue LY], 0 ),
    CurrentItem = "Total COGS", DIVIDE( [TY vs PY Actuals], [COGS LY], 0 ),
    CurrentItem = "Total Gross Profit", DIVIDE( [TY vs PY Actuals], [Gross Profit LY], 0 ),
    CurrentItem = "Total Other Expenses", DIVIDE( [TY vs PY Actuals], [Expenses LY], 0 ),
    CurrentItem = "Total Net Profit", DIVIDE( [TY vs PY Actuals], [Net Profit LY], 0 ),
    CurrentItem = "Gross Profit %", [Gross Profit Margin] - [Gross Margin LY],
    CurrentItem = "Net Profit %", [Net Profit Margin] - [Net Profit Margin LY],
        DIVIDE( [TY vs PY Actuals],
            ABS( CALCULATE( CALCULATE( [Actuals (,000)],
            FILTER( 'Income Statement', 'Income Statement'[Items] = CurrentItem ) ),
                DATEADD( Dates[Date], -1, YEAR ) ) ) ) )```
```DAX
TY vs PY Actuals = 
VAR CurrentItem = SELECTEDVALUE( 'Income Statement Template'[Items - Normalized] )

RETURN
SWITCH( TRUE(),
    CurrentItem = "Total Revenues", DIVIDE( [Revenues] - [Revenue LY], 1000, 0 ),
    CurrentItem = "Total COGS", DIVIDE( [COGS] - [COGS LY], 1000, 0 ),
    CurrentItem = "Total Gross Profit", DIVIDE( [Gross Profit] - [Gross Profit LY], 1000, 0 ),
    CurrentItem = "Total Other Expenses", DIVIDE( [Expenses] - [Expenses LY], 1000, 0 ),
    CurrentItem = "Total Net Profit", DIVIDE( [Net Profit] - [Net Profit LY], 1000, 0 ),
        CALCULATE( [Actuals (,000)], FILTER( 'Income Statement', 'Income Statement'[Items] = CurrentItem ) )  ) - 
        CALCULATE( CALCULATE( [Actuals (,000)], FILTER( 'Income Statement', 'Income Statement'[Items] = CurrentItem ) ), DATEADD( Dates[Date], -1, YEAR ) )```
```DAX
Selected Year Actuals = 
VAR CurrentItem = SELECTEDVALUE( 'Income Statement Template'[Items - Normalized] )

RETURN
SWITCH( TRUE(),
    CurrentItem = "Total Revenues", DIVIDE( [Revenues], 1000, 0 ),
    CurrentItem = "Total COGS", DIVIDE( [COGS], 1000, 0 ),
    CurrentItem = "Total Gross Profit", DIVIDE( [Gross Profit], 1000, 0 ),
    CurrentItem = "Gross Profit %", FORMAT( [Gross Profit Margin], "0.00%" ),
    CurrentItem = "Total Other Expenses", DIVIDE( [Expenses], 1000, 0 ),
    CurrentItem = "Total Net Profit", DIVIDE( [Net Profit], 1000, 0 ),
    CurrentItem = "Net Profit %", FORMAT( [Net Profit Margin], "0.00%" ),
        CALCULATE( [Actuals (,000)],
            FILTER( 'Income Statement', 'Income Statement'[Items] = CurrentItem ) ) )```
### Accounts Receiivable:

```DAX
Receivables Per Group = 
CALCULATE( [Invoice Values],
    FILTER( Invoices,
        COUNTROWS(
            FILTER( 'Aged Debtor Groups',
                [Days Left] >= 'Aged Debtor Groups'[Min] &&
                [Days Left] <= 'Aged Debtor Groups'[Max] ) ) > 0 ) )```

```DAX
Outstanding Invoices = 
COUNTROWS( 
    FILTER( Invoices,
        [Days Left] > 0 ) )```

```DAX
Due Date = MIN( Invoices[Due Date] )```

###Time Comparison:


```DAX
Cash Paid For - Operations = 
CALCULATE( [Cash Flow Values],
        FILTER( 'Cash Flow Data',
        'Cash Flow Data'[Cash Flow Category] = "Cash paid for" &&
        'Cash Flow Data'[Cash Flow Type] = "Operations" ))```

```DAX
Cash Paid For - Operations = 
CALCULATE( [Cash Flow Values],
        FILTER( 'Cash Flow Data',
        'Cash Flow Data'[Cash Flow Category] = "Cash paid for" &&
        'Cash Flow Data'[Cash Flow Type] = "Operations" ))```

[🔼 Back to Table of Contents](#-table-of-contents)

---
## 📈 Dashboard & Visualizations  

### 📌 Income Statement 
![image](https://github.com/user-attachments/assets/a5911a11-987c-40d2-b578-ce0172bc6f84)


### 📌 Finantial Details  
![image](https://github.com/user-attachments/assets/937d0077-8d52-4e6f-9369-5071f686a1d7)


### 📌 Balance Sheet  
![image](https://github.com/user-attachments/assets/3058ddd9-926d-46f0-9037-810e4d92b346)


### 📌 Cash Flow Statement
![image](https://github.com/user-attachments/assets/16a05cc7-398d-4b7b-80ce-abc0ad56e5fe)


### 📌 Cash FLow
![image](https://github.com/user-attachments/assets/44c4406b-335d-42cd-9523-13443b90827d)


### 📌 Aged Account  
![image](https://github.com/user-attachments/assets/0d8661f3-1ad1-4a9d-a142-7717495d83d6)


### 📌 Turnover Summary 
![image](https://github.com/user-attachments/assets/d3844b74-3018-4085-b6a3-6b0fa68dd66c)


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Key Findings  
- **Sales Trend Over Time:** Total sales show fluctuations over time, with notable peaks around Feb 2021..  
- **Top Performing Customers:** The top 5 customers by total sales include Eayo Company, Holston Company, Dazzleshape Corp, Skyvu Group, and Natures          Group. Eayo Company appears to be the highest revenue-generating customer.  
- **Top Cities by Sales:** Orange, Lake Macquarie, Albury, Nowra, and Adelaide are the top-performing cities in terms of sales. Orange leads in sales volume, followed by Lake Macquarie 
- **City & Customer-Level Insights:** The South Australia region, particularly Adelaide, contributes significantly to total sales. Some companies, such as SUPERVALU Ltd and Unit Ltd, maintain high-profit margins despite varying total sales.  
- **Profitability & Margin Analysis:**Profit margins vary, with some companies achieving margins as high as 60% (e.g., Skippad Ltd), while others have lower profitability. There is a mix of high-revenue but low-margin and low-revenue but high-margin customers.  
- **Sales Distribution & Rolling Average:**The total sales graph indicates spikes in sales, but the rolling average (blue line) provides a smoother trend.
There are several outlier peaks, indicating large sales transactions or promotions during specific periods.
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🏥 Recommendations  
✅ Focus on High-Value Customers: Prioritize key customers such as Eayo Company, Holston Company, and Dazzleshape Corp, ensuring continued engagement and loyalty programs.
✅ Customer Segmentation: Classify customers based on spending patterns and profitability, offering tailored discounts or service enhancements to repeat buyers and high-margin clients.
✅ Optimize Product Pricing: Adjust pricing strategies for low-margin but high-volume products, ensuring sustainable profits while maintaining competitive pricing.
✅ Expense Management: Track and analyze company expenses, optimizing budget allocation to maximize ROI.
✅ Automate Financial Reporting: Utilize Power BI dashboards for real-time monitoring of revenue, expenses, and cash flow, reducing manual reporting efforts.
✅ Implement Predictive Analytics: Use historical trends to forecast future sales, demand, and potential revenue fluctuations.
✅ Regularly Monitor Financial Health: Track key financial KPIs such as profit margins, revenue trends, cash flow position, and budget adherence to ensure business stability.
✅ Sales Strategy Optimization: Replicate successful sales strategies that drove peak performance in February 2021 to maintain consistent growth.
✅ A/B Testing for Sales Tactics: Test different pricing models, promotional offers, and marketing campaigns to determine the most effective strategies.

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🚀 Project Impact  
✅ Saves time by automating financial analysis
✅ Improves decision-making with real-time financial data
✅ Enhances clarity into an organization’s financial health
✅ Enables effective expense management and profitability tracking
