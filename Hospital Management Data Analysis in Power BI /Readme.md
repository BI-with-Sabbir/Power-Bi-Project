# Power BI Project: Hospital Inpatient Discharge Healthcare Analysis

## 📊 Watch Live Dashboard  
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiM2M3ODFhMzMtOTE3OS00M2NkLWI0MTMtNzQwMDc1MTI4NTBlIiwidCI6IjQxYjQ2M2RkLTg1ZWItNGE1NS1iYTZmLTVhMWFjYWMyYjA5YyIsImMiOjEwfQ%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/Click%20Here-Power%20BI-blue?style=for-the-badge" alt="View Dashboard">
  </a>
</p>

### 📌 Project Overview  
The project overview highlights the Healthcare Analytics BI solution, which helps hospitals and healthcare organizations make data-driven decisions using Microsoft Power BI. The goal is to simplify healthcare analytics by converting raw data into actionable insights, improving efficiency, and reducing time spent on manual data analysis. 

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
- **Hospital_Doctor_Table:** Doctor_ID, Specialization, Fees, Full Name, Monthly Salary, Facility ID.
                              
- **Hospital_Facility_Table:** Facility ID, Facility Name, Division, Total Beds, Region, System, Open Date, Contact Information, Toatl ICU Beds, Total Ventilators, Dengue 
                              Unit, DU, ED, GW, ICU, MW, NICU, OPD, SU, Facility Rent Fee.
                              TrainingOpportunitiesTaken

- **Salary:** EmployeeID, FirstName, LastName, Gender, Age, BusinessTravel, Department, DistanceFromHome (KM), State, Ethnicity, Education, EducationField, JobRole, 
                           MaritalStatus, Salary, StockOptionLevel, OverTime, HireDate, Attrition, YearsAtCompany, YearsInMostRecentRole, YearsSinceLastPromotion, 
                           YearsWithCurrManager, AgeBins, Salary Bin. 

- **Hospital_Nurse_Table:** Nurse_ID, First_Name, Last_Name, Department, Monthly Salary,Facility ID.

- **Hospital_Patient Details:** Patient ID, Date_of_Birth, Full Name, Gender, Visit Date, Admission Date, Discharge Date, Facility_ID, Unit Type, Unit_Type_Short_name, Bed 
                            Type, Type of Visit, Doc_ID, Nurse_ID, Blood Required, Blood Group, Dengue Status, Care Type, Gen, Age, Age Group, Length of Stay, Treatment 
                            Cost, Wait Time, Admission/Readmission.

- **Patient_Satisfactioon_Category:** Negative\Positive, Satisfaction, Satisfaction Name, Status, Star Retting.
   
- **Medical_equipment_supply:** Facility_ID, Item_ID, Item_Name, Total, In Use, Available, Supply_date.
 
- **Patient_Survey:** Pat_id, Survey Date, Facility Id, Date of Bith, Questions No, Score, Supply_date.

 - **Survey_Questation:** Negative\Positive, Satisfaction, Satisfaction Name, Status, Star Retting.

 -  **Patient_TreatmentCost:** Negative\Positive, Satisfaction, Satisfaction Name, Status, Star Retting.  


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

- **Promote Headers:** Ensure the first row is used as column headers.  
- **Change Data Types:** Convert Transaction_ID to Int64, and other relevant columns to text or date.  
- **Remove Errors:** Eliminate rows with errors in Transaction_ID.  
- **Drop Unnecessary Columns:** Remove unused columns.  
- **Replace Null Values:** Fill missing values in Agent_ID and Merchant_ID with "Not found".  

### Steps Taken:  
1. Created Measured Table:  
   ```DAX
   Avg WT/ LOS = {
    ("WT", NAMEOF('_Measure Contoso'[Avg Waiting Time]), 0),
    ("LOT", NAMEOF('_Measure Contoso'[Avg Length Of Stay]), 1)
}
   ```
2. Care type Function:  
  ```DAX
     Care Type = 
      SWITCH(
    TRUE(),
    'C_Patient Details'[Unit_Type_Short_name] IN {"ICU", "ED", "SU", "NICU"}, "Acute Care",
    'C_Patient Details'[Unit_Type_Short_name] IN {"OPD", "DU", "MW","GW","Dengue Uint"}, "Non Acute Care",
    "Other"
)
   ```
3. Filtered data for hip replacement procedures:  
   ```DAX
      Day Type = 
       IF(
       WEEKDAY(C_DateTable[Day of Week], 2) IN {5, 6}, 
       "Weekend", 
       "Weekday"
      )
   ```
4. Created a `surgical_program_volume_summary` column:  
   ```DAX
      Year_Month_Formatted = 
      FORMAT(DATE('C_DateTable'[Year], 'C_DateTable'[Month], 1), "MMM yy")

   ```

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![image](https://github.com/user-attachments/assets/e6aab382-e3aa-42b0-9baa-edc1a64e76d7)


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  

### General Measures  
```DAX
% Readmission Rate = 
VAR _ADD_READD = COUNT('C_Patient Details'[Admission/Readmission])
return
DIVIDE([Total Readmission],_ADD_READD,0)
```
```DAX
Average Treatment Cost = 
DIVIDE([Treatment Cost],[Unique Patient])
```
```DAX
Avg Rating = AVERAGE(C_Survey[Score])
```
```DAX
Avg Treatment Cost = AVERAGE('C_Patient Details'[Treatment Cost])
```
```DAX
Avg Waiting Time = AVERAGE('C_Patient Details'[Wait Time])
```
```DAX
LY Total Patients = CALCULATE([Total Patient],SAMEPERIODLASTYEAR('C_DateTable'[Date]))
```
```DAX
Total Admission = CALCULATE(COUNT('C_Patient Details'[Admission/Readmission]),'C_Patient Details'[Admission/Readmission] = "Admission")
```
```DAX
Total Length of Stay = SUM('C_Patient Details'[Length of Stay])
```
```DAX
Total Readmission = CALCULATE(COUNT('C_Patient Details'[Admission/Readmission]),'C_Patient Details'[Admission/Readmission] = "Readmission")
```
```DAX
Avg Waiting Time = AVERAGE('C_Patient Details'[Wait Time])
```
### Beds Measures  

```DAX
Available Beds = [Total Beds] - [Beds in Use]
```
```DAX
Beds in Use = 
CALCULATE(
    COUNT('C_Patient Details'[Patient ID]),
    'C_Patient Details'[Admission Date] <> BLANK(),
    'C_Patient Details'[Discharge Date] = BLANK()
)

```
```DAX
Occupancy Rate = 
DIVIDE([Beds in Use], [Total Beds], 0)
```
```DAX
Total Beds = SUM(C_Facility[Total Beds])
```
### Cost measure:

```DAX
Facility Fee = SUM(C_Facility[Facility Rent Fee])
```
```DAX
Profit/Loss = [Total Revenue/Charge]- [Total Cost/ Operation Cost]
```
```DAX
Total Cost/ Operation Cost = [Facility Fee]+[Total Doctor Salary]+ [Total Nurse Salary]
```
```DAX
Total Doctor Fee = SUM(C_TreatmentCost[Doc_fee])
```
```DAX
Total Revenue/Charge = [Total Doctor Fee] + [Total Test Fee]
```
### Equipment measure:

```DAX
ICU_BEDS_IN_USE = 
CALCULATE(
    DISTINCTCOUNT('C_Patient Details'[Patient ID]),
    'C_Patient Details'[Admission Date] <> BLANK(),
    'C_Patient Details'[Discharge Date] = BLANK(),
    'C_Patient Details'[Bed Type] = "ICU Bed"
)
```
```DAX
Total ICU Beds = SUM(C_Facility[Toatl ICU Beds])
```
```DAX
Total Ventilator = SUM(C_Facility[Total Ventilators])
```

### KPI Visual:

```DAX
MaxDate = 
CALCULATE(
    MAX(C_DateTable[Date]),
    REMOVEFILTERS(C_DateTable[Date]))
```
```DAX
MinDate = 
CALCULATE(
    MIN(C_DateTable[Date]),
    REMOVEFILTERS(C_DateTable[Date]))
```
```DAX
Selected Min Date = 
Var MaxDate = [2_MaxDate] +1
VAR MinDate = [2_MinDate] -1
RETURN

SWITCH([3_Selected Dates],
    "1W",MaxDate -7,
    "1M",EDATE(MaxDate, -1),
    "3M",EDATE(MaxDate, -3),
    "TY",DATE(YEAR(MaxDate), 1, 1),
    "1Y",EDATE(MaxDate, -12),
    "ALL",MinDate )
```
```DAX
Selected Min Date Past Period = 
Var MaxDate = [3_Selected Min Date] -1
VAR MinDate2 = [2_MinDate] -1
RETURN

SWITCH([3_Selected Dates],
    "1W",MaxDate -7,
    "1M",EDATE(MaxDate, -1),
    "3M",EDATE(MaxDate, -3),
    "TY",DATE(YEAR(MaxDate), 1, 1),
    "1Y",EDATE(MaxDate, -12),
    "ALL",MinDate2 )
```
```DAX
SM Date Param = 
CALCULATE(
    [3_Selected Measure],
    KEEPFILTERS(
        DATESBETWEEN(
            'C_DateTable'[Date],                 
            [3_Selected Min Date],
            [2_MaxDate]
         ))
    )
```
```DAX
SM Date Param Past Period = 
CALCULATE(
    [3_Selected Measure],
    KEEPFILTERS(
        DATESBETWEEN(
            'C_DateTable'[Date],       
            [3_Selected Min Date Past Period],                      
            [3_Selected Min Date]
         ))
    )
```
```DAX
Moving Average Selection = 
//get the selected moving average value from the disconnected table

SELECTEDVALUE('Moving Average'[Days])
```
```DAX
SM Moving Average = 
    
    AVERAGEX(                                   
    DATESINPERIOD(                          //the dates from:                     
        'C_DateTable'[Date],                       //Current Date
        LASTDATE(                           //Current Date - #Selected Months
            'C_DateTable'[Date] ), 
            -[5_Moving Average Selection], 
            DAY                           
            ),                              
         [3_Selected Measure]               //For the selected measure
         )
         
```
```DAX
SM Moving Average Date Param = 
if(ISBLANK([3_Selected Measure]), blank(),
CALCULATE(
    [5_SM Moving Average],
    KEEPFILTERS(
        DATESBETWEEN(
            'C_DateTable'[Date],                 
            [3_Selected Min Date],
            [2_MaxDate]
         ))
    )
)
```
```DAX
SM Past Period Reference Label = 


var _Current = [4_SM Date Param]
var _LY = [4_SM Date Param Past Period]
var _Growth = DIVIDE(_Current - _LY, _LY)
VAR NumOfDigits =
    IFERROR(
            LEN( 
                CONVERT( 
                        INT( 
                           [4_SM Date Param])
                        , STRING ))
            ,0)
VAR Suffix =
    ".0" &
    SWITCH(
        TRUE( ),
        NumOfDigits >= 13,  "T",
        NumOfDigits >= 10,  "B",
        NumOfDigits >= 7,   "M",
        NumOfDigits >= 4,   "K"
    )
VAR Commas =
    REPT(",",
        SWITCH(
            TRUE( ),
            NumOfDigits >= 13,  "4",
            NumOfDigits >= 10,  "3",
            NumOfDigits >= 7,   "2",
            NumOfDigits >= 4,   "1"
        )
    )
var _Format =     "#,##0" & Commas & Suffix & ";-#,##0" & Commas &Suffix 
return
IF (_Growth <> 0, 
    
"" &FORMAT(_Current-_LY, _Format)
    &  
        FORMAT(_Growth," | 0.0% ▲; | 0.0% ▼;")& ""," ")
```






[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  

### 📌 Overview of Hospital Management:
![image](https://github.com/user-attachments/assets/13b7ff5f-82f2-4f6f-a9d0-25f0f6a2be23)

### 📌Details: 
![image](https://github.com/user-attachments/assets/1ebde5b8-0ab9-4cef-902e-7ac16238382f)

### 📌 Financial: 
![image](https://github.com/user-attachments/assets/2e2439ed-aad0-4324-b751-1d349f06c60b)

### 📌 Bed Management:
![image](https://github.com/user-attachments/assets/3fb18aa8-4d89-41b9-8f18-2bf4b4e66fc3)

### 📌Division: 
![image](https://github.com/user-attachments/assets/7b5043bc-9154-4b49-83cc-fc45acac9363)

### 📌 Patient Overview: 
![image](https://github.com/user-attachments/assets/1568ac15-c1a7-4e70-9795-54ce384db362)

### 📌 Doctors Overview: 
![image](https://github.com/user-attachments/assets/f5682993-bd7f-4496-9e5b-7213e162cc7d)

### 📌Satisfaction: 
![image](https://github.com/user-attachments/assets/2515c477-0ef4-4a19-9d81-35bf0632c9ee)


### 📌AI Insights: 
![image](https://github.com/user-attachments/assets/4361a0d0-d65a-4ba5-ac4c-f36fd1fdf956)


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Key Findings  

### Patient Information
-80 Census Patients (Currently admitted patients)
-266 Total Dengue Patients
-3,498 Total Unique Patients

###  Supply Information
- Critical Supplies:
- Oxygen: 80K available, 39.7% in use
- IV Bags: 78K available, 45.1% in use
- Needles: 5K available, 39.4% in use (very low stock)
- Other supplies like Diagnostic Kits, Tubes, Gloves, Masks, and Bandages have 30%+ usage.
- 
###  Treatment Cost
- Total Treatment Cost: 807K
-Monthly Change: -12.7K (-1.5%) compared to the previous month (819.4K).
-Daily cost trend shows fluctuations with a slight downward trend.

###  Bed Management
- 415 Total Beds
- 80 Beds in Use (19% Occupancy Rate)
- Dhaka South Hospital has 16% occupancy, while Nagar Hospital has 19% occupancy.
- 
###  Equipment Information
- 75 Total ICU Beds.
- 43 Ventilators.
- 67 ICU Beds in Use (High ICU occupancy).
- 
###  Staffing Information
- 20 Total Doctors
- 25 Total Nurses
- 7.2% Nurse to Patient Ratio
- Port City Hospital has 61 long-stay patients, with an 8.2% nurse-to-patient ratio.
- Peoples Care Hospital has 71 long-stay patients with 7.0% nurse-to-patient ratio.
- Some non-acute care facilities have a very high nurse-to-patient ratio (above 70%), indicating staffing inefficiencies.
- 
[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🏥 Recommendations  

✅ Stable Bed Occupancy: Low occupancy (19%), but ICU utilization is high, requiring better ICU resource allocation.
⚠️ Critical Supply Management Needed: Needles stock is very low; procurement needed. IV Bags and Diagnostic Kits usage is high, so stock needs continuous monitoring.
📉 Treatment Costs Declining: Slight 1.5% drop in expenses, indicating cost control efforts, but further analysis needed to ensure quality care isn’t affected.
👨‍⚕️ Staffing Imbalance: Some non-acute care sections have excess nurses, while critical areas need better allocation.

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🚀 Project Impact  

🚀 Faster Patient Care → Reduced wait times & better treatment outcomes
📉 Lower Operational Costs → Smarter resource allocation & waste reduction
💰 Increased Revenue → Data-driven financial planning
📊 Informed Decision-Making → AI-driven predictions & real-time monitoring
---

