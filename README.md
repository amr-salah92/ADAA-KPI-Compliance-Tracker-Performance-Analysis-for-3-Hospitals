# ADAA KPI Compliance Tracker: Eastern Province Hospitals Performance Analysis

## Table of Contents
1. [Project Name](#1-project-name)
2. [Project Background](#2-project-background)
3. [Project Goals](#3-project-goals)
4. [Insights](#4-insights)  
   4.1 [Category 1: Bed Utilization](#41-category-1-bed-utilization)  
   4.2 [Category 2: Appointment Attendance](#42-category-2-appointment-attendance)  
   4.3 [Category 3: ER Wait Times](#43-category-3-er-wait-times)  
   4.4 [Category 4: Data Quality & Governance](#44-category-4-data-quality--governance)  
   4.5 [Category 5: Average Length of Stay (ALOS)](#45-category-5-average-length-of-stay-alos)
5. [Data Collection and Sources](#5-data-collection-and-sources)
6. [Formal Data Governance](#6-formal-data-governance)
7. [Regulatory Reporting](#7-regulatory-reporting)
8. [Methodology](#8-methodology)
9. [Data Structure & Initial Checks](#9-data-structure--initial-checks)
10. [Documenting Issues](#10-documenting-issues)
11. [Executive Summary](#11-executive-summary)
12. [Insights Deep Dive](#12-insights-deep-dive)  
    12.1 [Category 1: Bed Utilization (BOR)](#121-category-1-bed-utilization-bor)  
    12.2 [Category 2: Appointment Attendance (No-Show Rate)](#122-category-2-appointment-attendance-no-show-rate)  
    12.3 [Category 3: ER Wait Times](#123-category-3-er-wait-times)  
    12.4 [Category 4: Data Quality & Governance](#124-category-4-data-quality--governance)  
    12.5 [Category 5: Average Length of Stay (ALOS)](#125-category-5-average-length-of-stay-alos)
13. [Recommendations](#13-recommendations)
14. [Future Work](#14-future-work)
15. [Technical Details](#15-technical-details)
16. [Assumptions and Caveats](#16-assumptions-and-caveats)

---

### 1. Project Name
ADAA KPI Compliance Tracker: Performance Analysis for Eastern Province Hospitals (KFHU, Dammam Central, Qatif PHC, AlKhobar Gen)

---

### 2. Project Background
* The National Center for Performance Measurement (ADAA), established by Royal Decree M/5 in 2015, is mandated to elevate government performance across Saudi Arabia as part of Vision 2030
* ADAA develops, monitors, and reports on Key Performance Indicators (KPIs) across ministries and public entities, driving transparency, citizen satisfaction, and data-driven governance
* Within healthcare, the Ada’a Health Program (launched 2017) evaluates hospital performance on critical metrics including Emergency Room (ER) wait times, appointment efficiency, and patient experience
* ADAA's analytics division, formed Q1 2022, centralizes performance insights. As a data analyst embedded in this unit since Q3 2024, core responsibilities include monitoring Ministry of Health ADAA KPI compliance across 4 major Eastern Province facilities
* Analysis encompasses over 5,000 distinct data points spanning fiscal year 2024-2025

---

### 3. Project Goals
* **Detect Underperformance:** Identify real-time trends breaching ADAA thresholds for metrics like ALOS, BOR%, No-Show Rate, and ER Wait Time
* **Enable Data-Driven Decisions:** Provide department heads with visual insights to understand performance drivers
* **Ensure Regulatory Compliance:** Generate auditable documentation demonstrating adherence to MOH and ADAA requirements
* **Establish Data Foundation:** Implement foundational data governance practices and access controls (RLS) within Power BI

---

### 4. Insights

#### 4.1 Category 1: Bed Utilization
**Key Observations**  
1. Significant monthly declines in BOR% across departments/hospitals  
2. Widespread ADAA non-compliance (only Radiology met standards)  
3. Strong positive correlation (r = 0.71) between Beds Available and Occupied  
4. Negligible correlation (r ≈ 0.007) between BOR% and Length of Stay  
 

#### 4.2 Category 2: Appointment Attendance
**Key Observations**  
1. Hospital variation (Qatif PHC: 23.4% vs KFHU: 22.1% avg no-show)  
2. Cardiology/Outpatient had highest rates (~23%)  
3. Temporal improvements identified (e.g., Qatif PHC Feb 2025)  
4. Critical absolute volumes in Surgery (KFHU: 6,850 no-shows)  
 

#### 4.3 Category 3: ER Wait Times
**Key Observations**  
1. Dammam Central consistently met ADAA target (29.65 mins)  
2. Recurring June breaches (2024: 33 mins, 2025: 31.36 mins)  
3. 47.85% of ER visits waited 31-60 minutes  
4. Radiology/Cardiology contributed to severe delays (61-120 mins)  


#### 4.4 Category 4: Data Quality & Governance
**Key Observations**  
1. Inconsistent `Region` values and redundant field  
2. 16.2% missing ICD-10 codes; 16.5% invalid CPT codes  
3. Moderate duplication risk (1 per 500 records)  
4. Code normalization reduced invalid records by 85%  
 

#### 4.5 Category 5: Average Length of Stay (ALOS)
**Key Observations**  
1. Strong LOS correlations with Discharges (r=0.78) and Admissions (r=0.73)  
2. All departments breached thresholds; Outpatient/ER/Radiology top contributors  
3. Even hospital distribution of breaches (~25% each)  
4. Critical breach rates in core departments (>89%)  


---

### 5. Data Collection and Sources
* Synthetically generated data simulating 2024 operations
* Simulated fields: Patient visits, ER logs, admissions, discharges, bed counts, appointment statuses
* Deliberate error injections in ICD-10/CPT codes to replicate real-world challenges

---

### 6. Formal Data Governance
**Current State:**  
Inconsistent nomenclature, incomplete fields, schema gaps, duplication risk  

**Proposed Model (DAMA-DMBOK):**  
- **Metadata Management:** Central KPI definitions in SharePoint  
- **Business Glossary:** Standardized term definitions  
- **Data Quality Scoring:** Matrix for completeness/accuracy/consistency/timeliness  
- **Patient_ID Governance:** UUID implementation for HIPAA/GDPR compliance  

---

### 7. Regulatory Reporting
* Directly supports MOH ADAA KPI compliance
* Resolved inconsistencies via:  
  - Clinical code imputation/mapping  
  - Deduplication (UUID+date+hospital)  
  - Removal of inconsistent fields  
* RLS in Power BI ensures auditability and access control

---

### 8. Methodology
* **Descriptive Statistics:** Core KPI calculations
* **Correlation Analysis:** Relationships between key metrics
* **Data Cleaning:** Power Query transformations
* **DAX Measures:**  
  ```dax
  BOR_% = DIVIDE(
      [Beds_Occupied],
      [Beds_Available],
      0
  )
  ``` 
```dax
  No_show_Percent = 
  IF(
      NOT ISBLANK([No_show_Appointments]) &&
      NOT ISBLANK([Appointments_Booked]),
      DIVIDE(
          [No_show_Appointments],
          [Appointments_Booked],
          0
      ),
      BLANK()
  )
```

- Visualization: Line/bar charts, KPI cards, decomposition trees
- Security: Row-Level Security (RLS) implementation

**Visualization:**  
• Line/bar charts for trend analysis and comparisons  
• KPI cards for real-time metric monitoring  

 ![Screenshot_4-7-2025_1338_chat deepseek com](https://github.com/user-attachments/assets/e06ab42c-34d9-473b-965c-f5a9248e1431)


**Security:**  
• Row-Level Security (RLS) implementation in Power BI to restrict data access by:
  - Hospital affiliation
  - Departmental responsibility
  - User role (Clinician vs. Administrator)

---

### 9. Data Structure & Initial Checks
**Database Schema:**  
8 relational tables with surrogate keys and complex relationships  

| Table             | Purpose                          | Key Columns/Issues                     |
|-------------------|----------------------------------|----------------------------------------|
| Diagnosis         | Clinical documentation           | `ordering_dysplasia` (typo), FK conflicts |
| Lab_Tests         | Diagnostic results               | 30% column naming inconsistencies      |
| A_Data_Codes      | Test code registry               | Undefined purpose, naming errors       |
| C_Get_Codes       | OPT code registry                | 16.5% invalid/blank codes              |
| Hospitals         | Facility metadata                | Solves regional inconsistency          |
| Patients          | Demographic registry             | PII requiring GDPR/HIPAA compliance    |
| Appointments      | Scheduling system                | `notrewiring` (likely "no_show")       |
| Visit_Records     | Encounter tracking               | `average_length_of_carry` (typo for "stay") |

![Screenshot_4-7-2025_0859_dbdiagram io](https://github.com/user-attachments/assets/b8e516c7-b68f-4cb8-9dfa-312165c651ca)

---

### 10. Documenting Issues
| Table             | Column              | Issue Description                                                                 | Magnitude | Resolutions                                                                                               |
|-------------------|---------------------|-----------------------------------------------------------------------------------|-----------|-----------------------------------------------------------------------------------------------------------|
| ADAA_KPI_Eastern  | Region              | Inconsistent values ("Eastern", "eastern", "EASTERN", blank/null)                 | High      | Remove column                                                                                             |
| ADAA_KPI_Eastern  | ICD10_Code          | 810 missing records (16.2%)                                                       | High      | Mandatory dropdowns at source + CPT-based imputation                                                      |
| ADAA_KPI_Eastern  | CPT_Code            | 827 invalid/blank records (16.5%)                                                 | High      | Droping the null in ICD10 Code & Invalid CPT code or FHIR validation + ICD10-based imputation             |
| ADAA_KPI_Eastern  | Entire Table        | ~1 duplicate per 5000 records                                                     | Medium    | Droping Duplicate                                                                                         |
| ADAA_KPI_Eastern  | `[Hospital_Name]`   | Inconsistent values With Hospital_ID                                              | High      | Exclude from analysis                                                                                     |

---

### 11. Executive Summary  
**(Audience: Director of Quality & Compliance)**   
• **Operational bottlenecks** in discharge processes and bed management  
• **47.85% of ER patients** wait 31-60 minutes  
• **Data quality gaps** compromise regulatory reporting  
• **Systemic ALOS breaches** across all departments  

---

### 12. Insights Deep Dive  
*(Visualizations embedded: Line/Bar charts, KPI cards, Distribution charts)*  

**12.1 Category 1: Bed Utilization (BOR)**
**Insight 1: Radiology Department Benchmark:** Radiology was the *only* department maintaining BOR% within ADAA standards consistently throughout FY2024-2025, demonstrating effective capacity management. This contrasts sharply with persistent breaches in Outpatient (Avg ~75%), Emergency (Avg ~73%), Cardiology (Avg ~41%), and Surgery (Avg ~38%).
**Insight 2: Reactive Bed Strategy:** The strong positive correlation (r = 0.71, p<0.01) between `Beds_Available` and `Beds_Occupied` indicates beds are primarily opened *in response* to rising occupancy, rather than proactively forecasted. This contributes to periods of both over-occupancy (ER, Outpatient) and underutilization (Cardiology, Surgery).
**Insight 3: No Link Between Beds & Stay Duration:** Analysis found no meaningful correlation (r ≈ 0.007) between `Bed Occupancy Rate (BOR%)` and `Length_of_Stay (LOS)`. High bed occupancy does not predict longer patient stays, nor does low occupancy predict shorter stays, suggesting bed availability is not a primary driver of LOS.
**Insight 4: Seasonal & Departmental Lows:** Significant monthly BOR% lows were identified: System-wide low in Jan-2025; Radiology (Mar-2025); Emergency (Apr-2025); Outpatient (Aug-2024); Surgery (Jun-2025). Hospital lows: Dammam Central (Jan-2025); Qatif PHC & AlKhobar Gen (Jun-2024); KFHU (Jun-2025). These patterns highlight specific times/departments needing targeted capacity review.

**12.2 Category 2: Appointment Attendance (No-Show Rate)**
**Insight 1: Cardiology & Outpatient Disengagement:** Cardiology and Outpatient departments consistently reported the highest *average* no-show rates (~23% across all hospitals) in FY2024-2025, significantly higher than Surgery (22.4%). This indicates specific challenges in patient engagement for non-urgent, chronic, or follow-up care.
**Insight 2: Hospital-Level Variation:** Qatif PHC had the highest overall average no-show rate (23.4%), followed by AlKhobar Gen (23.2%), Dammam Central (22.5%), and KFHU (22.1%). This 1.3% difference highlights varying effectiveness in appointment management and patient reminder systems across the region.
**Insight 3: High Absolute Volumes Despite Lower Rates:** Surgery departments, while having the lowest *average rate* (22.4%), generated the highest *absolute* number of no-shows at KFHU (6,850). Similarly, Emergency Departments at KFHU (6,781 no-shows) and Cardiology at Qatif PHC (6,739 no-shows) represent substantial operational burdens requiring process review.
**Insight 4: Temporal Improvement Opportunities:** Distinct monthly periods showed significant drops in no-show rates: Qatif PHC (Feb-2025: Lowest Rate), KFHU (Dec-2024: Lowest Rate), Dammam Central (May-2025: Improvement), AlKhobar Gen (Jun-2025: Improvement). These periods warrant investigation to identify replicable operational changes or campaign impacts.

**12.3 Category 3: ER Wait Times**
**Insight 1: Dammam Central & Emergency Dept Leadership:** Dammam Central Hospital consistently achieved the ADAA ER wait time target (Avg 29.65 mins < 30 mins). Within departments, the core Emergency Department itself had the best overall average wait time (29.93 mins), demonstrating efficient intake/triage.
**Insight 2: Recurring June Performance Deterioration:** The most significant ER wait time breaches occurred in June 2024 (33.00 mins avg) and June 2025 (31.36 mins avg). This recurring pattern strongly suggests seasonal capacity strain, potentially linked to summer vacations, holidays, or regional events impacting staffing or patient volume.
**Insight 3: Critical Mid-Tier Wait Time Prevalence:** 47.85% of all ER visits (4,095 out of 8,557 analyzed visits) experienced wait times between 31-60 minutes. This represents a substantial cohort just exceeding the compliance threshold, where targeted efficiency gains could yield significant compliance improvements. Distribution across hospitals was remarkably even (KFHU 25.35%, Qatif PHC 25.25%, Dammam Central 24.71%, AlKhobar Gen 24.69%).
**Insight 4: Diagnostic Department Bottlenecks:** While Radiology and Cardiology are not core ER departments, they contributed significantly to the most severe delays (61-120 mins: Radiology 24.72%, Cardiology 21.35%). This points to potential bottlenecks in diagnostic processes (e.g., imaging, lab) delaying ER patient clearance and overall throughput.

**12.4 Category 4: Data Quality & Governance**
**Insight 1: Clinical Coding Gaps Impede Operations:** High rates of missing ICD-10 codes (16.2%, 810 records) and invalid/blank CPT codes (16.5%, 827 records) directly hinder accurate clinical reporting, operational analysis, and create financial risk (insurance claim rejections). Distribution showed Emergency, Surgery, and Radiology departments had the highest counts of invalid records (26 each).
**Insight 2: Mapping & Imputation Effective:** The applied two-way clinical code mapping (ICD-10 ↔ CPT) and ICD-10 normalization (e.g., adding decimals) successfully reduced the volume of invalid records by over 85% (from ~800+ to 117 per code type), demonstrating a viable interim solution while source system fixes are implemented.
**Insight 3: Duplication Risk Moderate:** Identification of an average of 1 duplicate record per 500 entries indicates a moderate but tangible risk of double-counting patient encounters, potentially inflating volume metrics and distorting per-patient calculations like ALOS.
**Insight 4: Redundant & Inconsistent Fields:** The `Region` field was entirely redundant (all records pertain to Eastern Province) but contained inconsistent values and nulls, complicating filtering. The existence of the `` field with values inconsistent with `Hospital_Name` necessitated its exclusion to ensure accurate facility-level reporting.

**12.5 Category 5: Average Length of Stay (ALOS)**
**Insight 1: Discharge Delays Drive LOS:** The strong positive correlation (r = 0.78, p<0.01) between `Length_of_Stay (LOS)` and `Discharges` indicates that delays in discharging patients are a primary constraint on patient flow, directly extending stays for others.
**Insight 2: Admission Surges Exacerbate LOS:** A strong positive correlation (r = 0.73, p<0.01) between `Admissions` and `LOS` shows that periods of high admission volume lead to longer average stays, suggesting capacity management struggles during surges.
**Insight 3: Short-Stay Departments Top Breach Contributors:** Despite having the lowest ALOS threshold (1 day), Outpatient (22.51% of breaches), Radiology (22.31%), and Emergency (22.21%) departments were the top three sources of ADAA ALOS breaches. This is particularly concerning given the expectation of very short stays, pointing to severe bottlenecks or misclassification.
**Insight 4: Systemic High Breach Rates in Core Areas:** Specific department-hospital combinations exhibited alarmingly high ALOS breach rates, predominantly in Outpatient, ER, and Radiology: AlKhobar Gen (ER 93.8%, Outpatient 92.5%, Rad 89.6%), Dammam Central (Rad 91.9%, Outpatient 91.0%, ER 90.9%), KFHU (Outpatient 93.6%, Rad 92.7%, ER 92.3%), Qatif PHC (ER 93.9%, Outpatient 93.7%, Rad 91.5%). This indicates widespread patient flow issues in fundamental service areas.

---

### 13. Recommendations
**(For: Hospital Operations Leadership, Quality & Compliance, IT/Data Governance Teams)**

1.  **Implement Proactive Bed Management:** Utilize historical admission/discharge patterns to forecast required bed openings *before* occupancy spikes (Category 1, Obs 2), moving away from reactive practices. *Action: Develop predictive model; Pilot in Surgery/Cardiology.*
2.  **Expand & Target Patient Reminder Systems:** Deploy enhanced SMS/WhatsApp reminders, prioritizing high no-show hospitals (Qatif PHC, Dammam Central) and departments (Cardiology, Outpatient), learning from KFHU's lower rates (Category 2, Obs 1,2,4). *Action: IT to implement system; Ops to design department-specific protocols.*
3.  **Optimize June ER Resource Allocation:** Analyze and adjust staffing, triage protocols, and diagnostic department coordination specifically for June, based on recurring severe wait time breaches (Category 3, Obs 2). *Action: Form June Surge Task Force; Pre-plan resource shifts.*
4.  **Enforce Clinical Coding Standards:** Mandate dropdown lists and FHIR validation for ICD-10/CPT codes at point of entry to eliminate missing/invalid data (Category 4, Obs 1). *Action: IT system modification; Clinician training.*
5.  **Streamline Discharge Processes:** Implement rapid discharge pathways, especially in Outpatient, Radiology, and ER, and develop predictive triggers for early discharge activation during high admission periods (Category 5, Obs 1,2,3,4). *Action: Process redesign teams; Develop & deploy early discharge algorithm.*
6.  **Deploy Real-Time Departmental KPI Dashboards:** Provide department heads with Power BI dashboards featuring ALOS, BOR%, No-Show Rate, and ER Wait Times, including automated threshold breach alerts (All Categories). *Action: Finalize & deploy dashboards with RLS; Train managers.*
7.  **Adopt DAMA-DMBOK Governance Framework:** Formalize metadata management (SharePoint), create a business glossary, implement data quality scoring, and enforce UUIDs for `Patient_ID` (Category 4, Obs 1,3,4; Formal Governance). *Action: Establish Data Governance Council; Implement framework components.*
8.  **Conduct Root-Cause Analysis in Short-Stay Departments:** Investigate specific bottlenecks causing high ALOS breaches in Outpatient, Radiology, and ER (e.g., report waits, staffing gaps, bed assignment delays) (Category 5, Obs 3,4). *Action: Departmental process mapping workshops; Staffing pattern review.*
9.  **Establish ER-Diagnostic Department SLAs:** Define and monitor Service Level Agreements between ER and Radiology/Cardiology capping turnaround times for diagnostics on ER patients to reduce overall ER wait times (Category 3, Obs 4). *Action: Define SLAs; Implement joint monitoring.*
10. **Replicate Radiology Bed Management:** Document and share Radiology's successful BOR% management strategies with Cardiology, Surgery, Outpatient, and Emergency departments (Category 1, Obs 1). *Action: Best practice documentation; Cross-departmental workshops.*

---

### 14. Future Work
• Integrate real EHR data via HL7/FHIR  
• Expand to Western/Central provinces  
• Correlate staffing levels with performance metrics  
• National benchmarking using ADAA datasets  
• Develop ML models for no-show/ER wait time prediction  
• Incorporate patient satisfaction (CAHPS) data  

---

### 15. Technical Details
**Primary Tools:**  
• Power BI (Visualization/DAX/RLS)  
• PostgreSQL (Data cleaning/analysis)  
• Excel (Synthetic data generation)  
• SharePoint (Documentation hosting)  

**Python Queries:**  
•

**Power bi :** [Interactive Dashboard]()  

---

### 16. Assumptions and Caveats
1. All `Region` variations refer to Eastern Province  
2. Non-attended booked appointments = "No-Shows"  
3. Simulated codes reflect real-world complexity  
4. Single missing bed count imputed using hospital mode  
5. Unrealistic LOS values treated as data errors  
6. Synthetic data validity for operational patterns  
7. Improvement periods correlate with initiatives (unverified)  
