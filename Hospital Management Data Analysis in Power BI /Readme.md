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


[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🔄 Data Preprocessing  

### Steps Taken:  
1. Promoted headers:  
   ```DAX
   = Table.PromoteHeaders(Source, [PromoteAllScalars=true])
   ```
2. Transformed column types:  
   ```DAX
   = Table.TransformColumnTypes(#"Promoted Headers", {{"health_service_area", type text}})
   ```
3. Filtered data for hip replacement procedures:  
   ```DAX
   = Table.SelectRows(#"Changed Type", each ([ccs_procedure_description] = "HIP REPLACEMENT,TOT/PRT"))
   ```
4. Created a `surgical_program_volume_summary` column:  
   ```DAX
   Surgical Program Size = 
   IF(surgical_program_volume_summary[Total Discharges (bins)] == 0, "<200",
   IF(surgical_program_volume_summary[Total Discharges (bins)] == 200, "200 - 399",
   IF(surgical_program_volume_summary[Total Discharges (bins)] == 400, "400 - 599",
   ">=600")))
   ```

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Data Modeling  
![Data Model](https://github.com/user-attachments/assets/b2421ade-d56e-43eb-818d-7bce8d2fb7d6)

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🧮 DAX Measures  

### General Measures  
```DAX
Total Hospitals = DISTINCTCOUNT(hospital_discharges[facility_name])
```
```DAX
Total Surgeons = DISTINCTCOUNT(hospital_discharges[operating_provider_license_number])
```
```DAX
Total Discharges = COUNTROWS(hospital_discharges)
```
```DAX
Average LOS Days = AVERAGE(hospital_discharges[length_of_stay])
```
```DAX
Average Cost Per Discharge = AVERAGE(hospital_discharges[total_costs])
```

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📈 Dashboard & Visualizations  

### 📌 LOS (Length of Stay) Comparison  
![LOS Comparison](https://github.com/user-attachments/assets/7e13565c-ca0c-4040-98ee-e8e17e3335d0)

### 📌 Cost Comparison  
![Cost Comparison](https://github.com/user-attachments/assets/79947642-3e5e-4942-91b1-267cd662599d)

### 📌 Hospital Profile  
![image](https://github.com/user-attachments/assets/2a0e7625-9711-475a-b084-487b75322729)

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 📊 Key Findings  

### 🔹 LOS (Length of Stay) Analysis  
- **Top 3 Hospitals with Highest LOS:**  
  - Strong Memorial Hospital – 4.86 days  
  - Corning Hospital – 3.13 days  
  - Geneva General Hospital – 3.08 days  

**Insights:**  
- Strong Memorial Hospital has the highest average LOS, which may indicate more complex cases or inefficiencies.  
- The Unity Hospital of Rochester has the lowest LOS, possibly reflecting higher efficiency in patient turnover.  

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🏥 Recommendations  

### ✅ Optimize High-LOS Hospitals  
- Investigate reasons for extended stays at Strong Memorial Hospital and others.  
- Implement efficiency strategies to reduce hospital congestion.  

### ✅ Cost-Effectiveness Measures  
- Compare hospitals with high vs. low treatment costs and analyze the quality of care.  
- Negotiate better cost structures or improve resource allocation.  

[🔼 Back to Table of Contents](#-table-of-contents)

---

## 🚀 Project Impact  

✔ Improve patient care efficiency  
✔ Reduce unnecessary hospital costs  
✔ Optimize healthcare resource utilization  
✔ Enhance hospital performance benchmarking  

---

