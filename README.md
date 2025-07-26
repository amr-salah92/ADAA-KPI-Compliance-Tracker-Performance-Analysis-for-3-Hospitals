# ADAA KPI Compliance Tracker: Performance Analysis for 3 Hospitals

## Table of Contents
1. [Project Background](#project-background)  
2. [Project Goals](#project-goals)  
3. [Insights and Recommendations](#insights-and-recommendations)  
4. [Data Collection and Sources](#data-collection-and-sources)  
5. [Formal Data Governance](#formal-data-governance)  
6. [Regulatory Reporting](#regulatory-reporting)  
7. [Methodology](#methodology)  
8. [Data Structure & Initial Checks](#data-structure--initial-checks)  
9. [Documenting Issues](#documenting-issues)  
10. [Executive Summary](#executive-summary)  
11. [Insights Deep Dive](#insights-deep-dive)  
12. [Recommendations](#recommendations)  
13. [Future Work](#future-work)  
14. [Technical Details](#technical-details)  

---

### 1. Project Background <a name="project-background"></a>
The National Center for Performance Measurement (ADAA), established by Royal Decree M/5 in 2015, is mandated to elevate government performance across Saudi Arabia as part of Vision 2030. ADAA develops, monitors, and reports on Key Performance Indicators (KPIs) across ministries and public entities, driving transparency, citizen satisfaction, and data-driven governance. 

Within healthcare, the **Ada’a Health Program** (launched 2017) evaluates hospital performance on critical metrics including wait times, CSAT, average length of stay, and bed occupancy rate. This analysis supports the Ministry of Health’s ADAA KPI compliance monitoring across 3 major facilities. Analysis encompasses **over 50,000 distinct data points** spanning fiscal year 2023–2025.

---


### 2. Project Goals <a name="project-goals"></a>
- **Detect Underperformance**: Identify real-time trends breaching ADAA thresholds for metrics like ALOS, BOR%, ICD10 descriptions, CPT descriptions, and  wait time.  
- **Enable Data-Driven Decisions**: Provide department heads with visual insights to understand performance drivers.  
- **Ensure Regulatory Compliance**: Generate auditable documentation demonstrating adherence to MOH and ADAA requirements.  


---

### 3. Insights and Recommendations <a name="insights-and-recommendations"></a>
#### Category 1: Capacity & Occupancy Analysis
| Hospital          | BOR% Range | Status       |
|-------------------|------------|--------------|
| Specialty Jeddah  | 74%-75.7%  | Compliant    |
| Al Madina Hospital| 73.8%-75.8%| Compliant    |
| Specialty Abha    | ~75%       | Compliant    |

**Recommendation**: Sustain occupancy through proactive bed management and optimize staffing during peak seasons.

#### Category 2: Patient Flow & Wait Time Analysis
| Hospital          | Wait Time (Last Quarter) | ADAA Target |
|-------------------|--------------------------|-------------|
| Specialty Jeddah  | 3.07 hrs                 | 2 hrs       |
| Al Madina         | 3.15 hrs                 | 2 hrs       |
| Specialty Abha    | 3.05 hrs                 | 2 hrs       |

**Recommendation**: Implement fast-track triage systems and increase staffing during peak hours.

#### Category 3: Clinical Quality & Diagnostic Trends
**Top Diagnoses/Procedures**:
- **Specialty Jeddah**: Headache (332 cases), Heart Disease (340 cases), Type 2 Diabetes (338 cases)  
- **Al Madina**: Heart Disease (334), Low Back Pain (330), MRI Lumbar Spine (185 cases)  
- **Specialty Abha**: Atrial Fibrillation (335), Hyperlipidemia (337), Chest X-rays (192 cases)  

**Recommendation**: Focus preventative care on high-incidence chronic conditions.

#### Category 4: Patient Satisfaction (CSAT) Analysis
| Hospital          | Last Quarter CSAT | ADAA Target |
|-------------------|-------------------|-------------|
| Specialty Jeddah  | 4.51              | ≥4.5        |
| Al Madina         | 4.49              | ≥4.5        |
| Specialty Abha    | 4.51 (avg)        | ≥4.5        |

**Recommendation**: Deploy empathy training and service recovery protocols.

---

### 4. Data Collection and Sources <a name="data-collection-and-sources"></a>

| Source System         | Table Name             | Update Frequency | Record Count | Key Data Elements                                                                 | Purpose                                                                 |
|-----------------------|------------------------|------------------|--------------|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------|
| **Enterprise EHR**    | `fact_admissions`      | Daily (CSV)      | 50,000+      | AdmissionID, LengthOfStay, WaitTimeHours, BedOccupancyRate, CSAT_Score            | Core transactional data for patient flow and satisfaction metrics        |
| **Master Data Mgmt**  | `dim_hospitals`        | Quarterly        | 3            | HospitalID, HospitalName, TotalCapacity, AccreditationStatus                      | Hospital attributes and capacity tracking                               |
| **Master Data Mgmt**  | `dim_departments`      | Quarterly        | 2            | DepartmentID, HospitalID, DepartmentName, ServiceLine, DeptCapacity               | Departmental structure and resource allocation                           |
| **Date Warehouse**    | `dim_dates`            | Static           | 2017         | DateKey, FullDate, Year, Quarter, MonthName, IsWeekend                            | Time-based analysis and cohort comparisons                               |
| **Clinical Coding**   | `dim_icd10_codes`      | Monthly          | 20           | ICD10Code, Description                                                            | Standardized diagnosis code mapping (e.g., E78.5 = Hyperlipidemia)       |
| **Clinical Coding**   | `dim_cpt_codes`        | Monthly          | 20           | CPTCode, Description                                                              | Procedure code mapping (e.g., 99232 = Subsequent hospital care)          |
| **Patient Experience**| `dim_csat_stakeholders`| Annually         | 6            | StakeholderType                                                                   | Categorizes CSAT feedback providers (Patient, Family, Physician, etc.)  |
| **Patient Experience**| `dim_csat_categories`  | Annually         | 6            | FeedbackCategory                                                                  | Classifies CSAT feedback topics (Timeliness, Communication, etc.)       |
---

### 5. Formal Data Governance <a name="formal-data-governance"></a>
- **Governance Council**: Led by Chief Data Officer + Hospital Data Stewards  
- **Standards**: DAMA-DMBOK framework for patient identifiers, code sets (ICD-10, CPT), quality metrics  
- **Quality Reviews**: Quarterly audits tracking completeness, consistency, lineage  

---

### 6. Regulatory Reporting <a name="regulatory-reporting"></a>
- **Compliance**: GDPR (EU expatriates), HIPAA (domestic PHI)  
- **Security**: Encryption (at rest/in transit), role-based access controls  
- **Audits**: Monthly log reviews + Privacy Impact Assessments  

---

### 7. Methodology <a name="methodology"></a>
**Descriptive Analytics**: Time-series analysis of occupancy/wait times using Power BI  

---

## 9. Data Structure & Initial Checks
**Overview**: Four primary tables totaling 60,532 records

### Table 1: `dim_hospitals`
| Column              | Description                         | Key Points                          | Usage Considerations          |
|---------------------|-------------------------------------|-------------------------------------|-------------------------------|
| HospitalID (PK)     | Unique hospital identifier          | No nulls                            | Join on HospitalID for location-based analysis |
| HospitalName        | Hospital name                       | Capacity range: 200–1,000 beds      |                               |
| City                | Location                            | CSAT target ±0.2 of actual          |                               |
| TotalCapacity       | Total bed capacity                  |                                     |                               |
| AccreditationStatus | Accreditation status                |                                     |                               |
| CSAT_30DayAvg       | 30-day CSAT average                 |                                     |                               |
| CSAT_Target         | Target CSAT score                   |                                     |                               |

**Represents**: Master list of hospital attributes

### Table 2: `dim_departments`
| Column         | Description                   | Key Points                     | Usage Considerations          |
|----------------|-------------------------------|--------------------------------|-------------------------------|
| DepartmentID (PK) | Unique department ID       | 2 departments per hospital   | Filter by ServiceLine         |
| HospitalID (FK) | Foreign key to hospitals   | Staff ratio: 0.8 staff per bed |                               |
| DepartmentName | Department name               |                                |                               |
| ServiceLine    | Service category              |                                |                               |
| DeptCapacity   | Department capacity           |                                |                               |
| CurrentStaff   | Current staff count           |                                |                               |
| CSAT metrics   | Satisfaction metrics          |                                |                               |

**Represents**: Departmental structure within hospitals

### Table 3: `dim_dates`
| Column       | Description               | Key Points                | Usage Considerations          |
|--------------|---------------------------|---------------------------|-------------------------------|
| DateKey (PK) | Unique date identifier    | Covers 2020-2025          | Time cohort analysis          |
| FullDate     | Full date (YYYY-MM-DD)    | Fiscal year shifts in July| Period-over-period analysis   |
| Year         | Calendar year             | Weekends flagged          |                               |
| Quarter      | Calendar quarter          |                           |                               |
| MonthName    | Month name                |                           |                               |
| FiscalPeriod | Fiscal period             |                           |                               |
| Week         | Week number               |                           |                               |
| DayName      | Day of week               |                           |                               |
| IsWeekend    | Weekend indicator         |                           |                               |

**Represents**: Date dimension for time-based joins


### Table 4: `fact_admissions`

| Column                   | Description                       | Key Points                     | Usage Considerations          |
|--------------------------|-----------------------------------|--------------------------------|-------------------------------|
| **AdmissionID (PK)**     | Unique admission identifier       | 50,000+ records                | Ensure joins on dimensional keys; primary key for fact table |
| **PatientID**            | Patient identifier                |                                | Link to patient dimension table; anonymized format (e.g., 137C9570D5E6) |
| **HospitalID**           | Hospital foreign key              | CSAT survey 1-14 days post-d/c | Join to hospital dimension; impacts satisfaction metrics |
| **DepartmentID**         | Department foreign key            |                                | Join to department dimension; indicates care unit |
| **AdmissionDateKey**     | Admission date surrogate key      | YYYYMMDD integer format        | Date dimension join for admission timing |
| **DischargeDateKey**     | Discharge date surrogate key      | YYYYMMDD integer format        | Date dimension join; validate ≥ AdmissionDateKey |
| **LengthOfStay**         | Duration in days                  | Integer (e.g., 1)              | Calculate from date keys; watch same-day discharges |
| **WaitTimeHours**        |  wait time pre-admission        | Decimal (e.g., 4.8)            | Track efficiency; may have outliers |
| **BedOccupancyRate**     | Utilization percentage            | Decimal 0.00-1.00 (e.g., 0.83)| Snapshotted at admission; impacts resource analysis |
| **PrimaryDiagnosisCode** | Primary ICD-10 code               | Standardized (e.g., E78.5)     | Join to diagnosis dimension; clinical grouping |
| **PrimaryDiagnosisDesc** | Diagnosis description             | Human-readable (e.g., Hyperlipidemia) | Reporting use; denormalized from code |
| **ProcedureCodes**       | Performed procedure codes         | CPT/HCPCS (e.g., 99232)        | Consider array storage; impacts billing analytics |
| **AdmissionSource**      | Point of origin                   | Categorical (e.g., )         | Flow analysis;  vs. referral differences |
| **InsuranceType**        | Payer category                    | Categorical (e.g., Private)    | Reimbursement analysis; impacts financial facts |
| **DischargeDisposition** | Post-care destination             | Categorical (e.g., Hospice)    | Readmission risk indicator; care continuity |
| **CSAT_Score**           | Satisfaction score (1-5 scale)    | Decimal (e.g., 4.6)            | Collected 1-14d post-discharge; voluntary bias |
| **CSAT_Stakeholder**     | Feedback provider role            | Categorical (e.g., Insurance Provider) | Stratify analysis by stakeholder type |
| **CSAT_FeedbackCategory**| Feedback topic                    | Categorical (e.g., Billing Clarity) | Root cause analysis; trend monitoring |
| **CSAT_Comment**         | Free-text feedback                | Unstructured (e.g., "Very satisfied with care") | NLP required; PII scrubbing needed |

**Represents**: Detailed admissions and satisfaction data
Here are the tables rewritten in the requested Markdown format:

### Table 5: `dim_icd10_codes`
| Column          | Description                            | Key Points                                                          | Usage Considerations                                                |
|-----------------|----------------------------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|
| ICD10Code (PK)  | Unique ICD-10 diagnosis code           | Master list of medical diagnosis codes and descriptions             | Join to fact_admissions via PrimaryDiagnosisCode or SecondaryDiagnosisCodes |
| Description     | Description of diagnosis               | Contains 20 common diagnosis codes                                 | Used to decode diagnoses in fact_admissions                        |

### Table 6: `dim_cpt_codes`
| Column      | Description                        | Key Points                                                          | Usage Considerations                                     |
|-------------|------------------------------------|---------------------------------------------------------------------|----------------------------------------------------------|
| CPTCode (PK)| Unique CPT procedure code          | Master list of medical procedure codes and descriptions             | Join to fact_admissions via ProcedureCodes field         |
| Description | Description of procedure           | Contains 20 common procedure codes                                 | Maps to procedures in fact_admissions                   |

### Table 7: `dim_csat_stakeholders`
| Column             | Description                                   | Key Points                                                          | Usage Considerations                          |
|--------------------|-----------------------------------------------|---------------------------------------------------------------------|-----------------------------------------------|
| StakeholderType (PK)| Type of CSAT feedback provider               | Lookup table for CSAT feedback provider types                       | Decodes CSAT_Stakeholder in fact_admissions   |
|                    |                                               | 6 distinct types (e.g., Patient, Physician, Staff)                 |                                               |

### Table 8: `dim_csat_categories`
| Column             | Description                                   | Key Points                                                          | Usage Considerations                          |
|--------------------|-----------------------------------------------|---------------------------------------------------------------------|-----------------------------------------------|
| FeedbackCategory (PK)| Category of CSAT feedback                   | Lookup table for CSAT feedback categories                           | Decodes CSAT_FeedbackCategory in fact_admissions |
|                    |                                               | 6 distinct categories (e.g., Timeliness, Communication)             |                                               |



---

## 10. Documenting Issues
| Table            | Column             | Issue Description                                                                 | Magnitude | Solvable | Resolution                                                                 |
|------------------|--------------------|-----------------------------------------------------------------------------------|-----------|----------|----------------------------------------------------------------------------|
| fact_admissions  | Various Columns    | ~3% NULLs in key metrics (WaitTimeHours & CSAT_Score)    | High      | Yes      | Impute numeric nulls with department-month medians |
| fact_admissions | Admission_Date      | Multiple date formats (ISO '2024-07-07', EU '07/07/2024', US '07-07-2024') causing parsing errors | High      | Yes      | Standardize to YYYY-MM-DD using datetime conversion                        |
| fact_admissions | Discharge_Date      | Inconsistent formats mixed with Admission_Date; US format inappropriate for KSA   | High      | Yes      | Convert to ISO 8601; validate Discharge ≥ Admission                        |
| fact_admissions | LengthOfStay          | Incorrect calculation               | High      | Yes      | corrected using : **LOS = Discharge_Date - Admission_Date**       |
| fact_admissions |  SecondaryDiagnosisCodes     | Null values   | High      | Yes      | used AI to Know the description of each ICD10 Code                        |



**Handling NULL Values**:
- **Assessment**: Missingness confirmed as Missing At Random (MAR)
- **Numeric Fields**: Replaced NULLs with department-month medians
- **Categorical Fields**: Filled with 'Unknown' or most frequent value (>50% frequency)
- **Validation**: Post-imputation percentiles aligned within ±1% of original
- **Prevention**: Added data-quality checks with configurable thresholds (<5% missingness)

---

## 11. Executive Summary
**For Chief Operations Officer** (Q4 2025):
- ✅ **Bed Occupancy**: All hospitals sustained 75–85% ADAA range (±3% variability)
- ⚠️ **Emergency Wait Times**:
  - Specialty Jeddah: 3.07hr
  - Al Madina: 3.15hr
  - Specialty Abha: 3.05hr 
  *(All above 2hr ADAA target)*
- 📊 **Patient Satisfaction**:
  - Specialty Jeddah: 4.51 (exceeded 4.5 target)
  - Al Madina: 4.49 
  - Specialty Abha: 4.51
- 🔥 **Clinical Hotspots**: Neurology/Oncology show highest chronic condition loads


![Screenshot_26-7-2025_133515_chat deepseek com](https://github.com/user-attachments/assets/78e02201-0264-4280-90d5-ae1fcffc8de9)

---

## 12. ### 12. Insights Deep Dive  

#### **A. Capacity & Occupancy**  
| Hospital          | ALOS (Days) | Target | BOR Range       | Key Findings |  
|-------------------|-------------|--------|-----------------|--------------|  
| Specialty Jeddah  | **8.02**    | 7      | 75.4%           | ICU/Oncology bottlenecks persist (Oncology ALOS: 8.11 days). Highest BOR: **75.7%** (Jan 2023) |  
| Al Madina         | **7.99**    | 7      | **73.8–75.8%**  | ICU ALOS: **8.02 days**; Oncology ALOS: **8.05 days**. Highest BOR: **75.8%** (Apr 2025) |  
| Specialty Abha    | **7.98**    | 7      | **72.9–75.9%**  | ICU ALOS: **8.04 days**; Oncology ALOS: **7.83 days**. Highest BOR: **75.9%** (Jul 2024) |  
> **Critical Issue**: All hospitals exceed ALOS target (7 days), indicating systemic discharge inefficiencies.  

#### **B. Patient Flow & Wait Times**  
- **All hospitals exceeded 2hr ADAA target**:  
  - **Specialty Jeddah**: 3.07hr (ICU highest: **3.29hr** Oct 2023; Oncology lowest: **2.77hr** Aug 2024)  
  - **Al Madina**: 3.15hr (ICU: **3.17hr** Jul 2024; Oncology lowest: **2.68hr** Jul 2024)  
  - **Specialty Abha**: 3.05hr (Oncology highest: **3.21hr** Feb 2024/2025)  
- **CSAT-Wait Time Correlation**:  
  - Longest waits (e.g., **3.25hr** at Abha, May/Jun 2025) correlate with lower CSAT (**4.53**)  
  - Shortest waits (e.g., **2.64hr** at Jeddah ICU, Mar 2025) boost CSAT (**4.59**)  

#### **C. Clinical Quality**  
| Hospital          | Department | Top Diagnoses/Cases |  
|-------------------|------------|---------------------|  
| Specialty Jeddah  | Oncology   | Heart disease (**958 cases**), Chest X-ray single view (**525**) |  
|                   | ICU        | Headache (**332 cases**), Comprehensive metabolic panel (**181**) |  
| Al Madina         | Oncology   | Anxiety (**317 cases**), MRI lumbar spine w/o contrast (**185**) |  
|                   | ICU        | Heart disease (**320**), Diabetes Type 2 (**323**) |  
| Specialty Abha    | Oncology   | Hyperlipidemia (**337**), X-ray single view (**192**) |  
|                   | ICU        | Hyperlipidemia (**322**), UTI (**322**), High-severity ED visits (**172**) |  

#### **D. Patient Satisfaction (CSAT)**  
| Hospital          | CSAT Range | Key Trends |  
|-------------------|------------|------------|  
| Specialty Jeddah  | **4.44–4.59** | Highest: **4.59** (ICU, Feb 2025; wait: **2.8hr**). Lowest: **4.44** (ICU, May 2025) |  
| Al Madina         | **4.45–4.56** | Lowest: **4.45** (Oncology, Oct 2024; wait: **2.82hr**). ICU consistency: **4.54** (Jul 2024) |  
| Specialty Abha    | **4.43–4.57** | Highest: **4.57** (Oncology, Nov 2023; wait: **2.99hr**). ICU volatility: **4.43–4.54** |  

#### **E. Correlation Analysis**  
| Metric Pair               | Correlation | Interpretation |  
|---------------------------|-------------|----------------|  
| **ALOS ↔ Wait Time**      | -0.00016    | Negligible linear relationship |  
| **ALOS ↔ BOR**            | -0.0018     | Minimal impact |  
| **Wait Time ↔ CSAT**      | -0.0059     | Weak negative correlation |  
| **Readmission ↔ CSAT**    | -0.00052    | No significant link |  
> **Key Insight**: Weak correlations suggest ALOS drivers are **non-linear** (e.g., case complexity, care coordination).  

#### **F. Predictive Readmission Analytics**  
- **Model**: RandomForest (Accuracy: **93.94%**, ROC AUC: **0.9636**)  

```python
Best params: {'max_depth': None, 'min_samples_split': 2, 'n_estimators': 300}
Accuracy: 0.9394 • ROC AUC: 0.9636
Confusion Matrix: 
[[8866  139]  # True Negatives: 8866, False Positives: 139
 [ 950 8029]] # False Negatives: 950, True Positives: 8029
```
### 📊 Model Performance Metrics

| Metric      | Score  | Insight                                  |
|-------------|--------|------------------------------------------|
| **Precision** | 0.94   | 94% of predicted readmissions are correct |
| **Recall**    | 0.94   | Identifies 94% of actual high-risk patients |
| **F1-Score**  | 0.94   | Balanced precision-recall performance |

> **Key Correlation Insight**: Bed occupancy shows stronger readmission link (r = -0.0026) than wait times (r = -0.004)

---

### 13. Recommendations

1. **Reduce ALOS** (Priority: **ICU/Oncology**):  
 - Launch **discharge readiness teams** for Jeddah Oncology (ALOS: 8.11 days) and Al Madina ICU (ALOS: 8.02 days)  
 - Address **non-linear drivers** (weekend staffing, care coordination) using Random Forest analysis  

2. **Optimize Wait Times**:  
 - Deploy **predictive queue management** during peak hours (e.g., Abha Oncology Feb: 3.21hr wait)  
 - Scale Al Madina’s **2.68hr wait model** (July 2024 Oncology) hospital-wide  

3. **Chronic Disease Management**:  
 - Establish clinics for **heart disease** (Jeddah: 958 cases) and **hyperlipidemia** (Abha: 337 cases)  

4. **Elevate CSAT**:  
 - **Real-time feedback kiosks** in volatile departments (e.g., Abha ICU: CSAT **4.43–4.54**)  
 - **Empathy training** during low-CSAT periods (e.g., Al Madina Oncology: **4.45** in Oct 2024)  

5. **Leverage Predictive Analytics**:  
 - Integrate **readmission model** to reduce **950 false negatives**  
 - Conduct **non-linear analysis** to identify hidden ALOS drivers  

6. **Dynamic Bed Management**:  
 - **Cross-departmental dashboards** to stabilize BOR (e.g., prevent Abha’s **72.9%** Jul 2025 dip)  
 - **Discharge algorithms** to maintain BOR within **75–85%**  

> **Success Metrics**: ALOS ≤7 days, wait times ≤2.5hr, CSAT ≥4.55, 30-day readmissions ↓10%.  

---

## 14. Future Work

- Real-time dashboard with predictive alerts
- Integrate outpatient clinic data
- Expand ML model to forecast CSAT
- ML-based readmission prediction system

---

## 15. Technical Details
**Tools Used**: 
Power BI • Power Query
Methods: Time-series analysis • Random Forest modeling
