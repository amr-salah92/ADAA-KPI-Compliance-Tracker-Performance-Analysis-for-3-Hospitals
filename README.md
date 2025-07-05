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
- **Detect Underperformance**: Identify real-time trends breaching ADAA thresholds for metrics like ALOS, BOR%, ICD10 descriptions, CPT descriptions, and ER wait time.  
- **Enable Data-Driven Decisions**: Provide department heads with visual insights to understand performance drivers.  
- **Ensure Regulatory Compliance**: Generate auditable documentation demonstrating adherence to MOH and ADAA requirements.  
- **Establish Data Foundation**: Implement foundational data governance practices and access controls (RLS) within Power BI.  

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
| Source                     | Description                                  | Update Frequency |
|----------------------------|----------------------------------------------|------------------|
| Enterprise Data Warehouse  | EHR system (admissions, discharges, surveys) | Daily            |
| dim_dates                  | Date dimension (2020-2025)                   | Static           |
| dim_hospitals              | Hospital master data                         | Quarterly        |
| fact_admissions            | Transactional admission records              | Daily (CSV)      |

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
| DepartmentID (PK) | Unique department ID       | 5–8 departments per hospital   | Filter by ServiceLine         |
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

### Additional Dimension Tables
| Table                     | Columns                     | Key Points                         | Usage Considerations                     |
|---------------------------|-----------------------------|------------------------------------|------------------------------------------|
| `dim_icd10_codes`         | ICD10Code (PK), Description | 20 common diagnosis codes          | Join to fact_admissions via diagnosis codes |
| `dim_cpt_codes`           | CPTCode (PK), Description   | 20 common procedure codes          | Join to fact_admissions via procedure codes |
| `dim_csat_stakeholders`   | StakeholderType (PK)        | 6 stakeholder types                | Decodes CSAT_Stakeholder                 |
| `dim_csat_categories`     | FeedbackCategory (PK)       | 6 feedback categories              | Decodes CSAT_FeedbackCategory            |

### Table 4: `fact_admissions`
| Column              | Description                       | Key Points                     | Usage Considerations          |
|---------------------|-----------------------------------|--------------------------------|-------------------------------|
| AdmissionID (PK)    | Unique admission ID               | 50,000+ records                | Ensure joins on dimensional keys |
| PatientID           | Patient identifier                | 10% readmission rate           |                               |
| HospitalID          | Hospital foreign key              | CSAT survey 1-14 days post-d/c |                               |
| DepartmentID        | Department foreign key            |                                |                               |
| AdmissionDateKey    | Admission date key                |                                |                               |
| DischargeDateKey    | Discharge date key                |                                |                               |
| WaitTimeHours       | Wait time in hours                |                                |                               |
| LengthOfStay        | Length of stay                    |                                |                               |
| ReadmissionFlag     | Readmission indicator             |                                |                               |
| BedOccupancyRate    | Bed occupancy rate                |                                |                               |
| AdmissionSource     | Admission source                  |                                |                               |
| InsuranceType       | Insurance type                    |                                |                               |
| Diagnoses           | Diagnosis codes                   |                                |                               |
| Procedures          | Procedure codes                   |                                |                               |
| CSAT fields         | Satisfaction metrics              |                                |                               |

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
| fact_admissions  | Various Columns    | ~3% NULLs in key metrics (WaitTimeHours, SecondaryDiagnosisCodes, CSAT_Score)    | High      | Yes      | Impute numeric nulls with department-month medians; categoricals → 'Unknown' |

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

---

## 12. Insights Deep Dive
### Capacity & Occupancy
| Hospital          | Bed Occupancy | Key Findings                                                                 |
|-------------------|--------------|------------------------------------------------------------------------------|
| Specialty Jeddah  | 75.4%        | ALOS 8.02hrs indicates ICU/Oncology bottlenecks                              |
| Al Madina         | 73.8–75.8%   | High-severity neurology admissions (334 heart disease cases)                |
| Specialty Abha    | 75%          | ALOS 7.98hrs; Oncology recorded 192 Chest X-rays                            |

### Patient Flow & Wait Times
All hospitals exceeded 2hr ADAA target:
- Specialty Jeddah: 3.07hr (range: 2.92–3.18hr)
- Al Madina: 3.15hr (range: 2.93–3.18hr)
- Specialty Abha: 3.05hr (range: 2.94–3.15hr)

### Clinical Quality
| Hospital          | Department  | Top Diagnoses/Cases                                  |
|-------------------|-------------|------------------------------------------------------|
| Specialty Jeddah  | ICU         | 332 headache cases                                   |
|                   | Oncology    | 340 heart disease cases                              |
| Al Madina         | Neurology   | 183 high-severity ED visits + heart disease          |
| Specialty Abha    | Neurology   | 335 atrial fibrillation cases                        |

### Patient Satisfaction (CSAT)
| Hospital          | CSAT Range  | Last Quarter | Status                           |
|-------------------|-------------|--------------|----------------------------------|
| Specialty Jeddah  | 4.47–4.54   | 4.51         | Exceeded target (4.5)            |
| Al Madina         | 4.47–4.54   | 4.49         | Below target                     |
| Specialty Abha    | 4.47–4.53   | 4.51         | Needs patient experience focus   |

---

## 13. Recommendations
| Goal                        | Action                                                                 |
|-----------------------------|------------------------------------------------------------------------|
| Reduce ALOS                 | Root cause analysis in ICU/Oncology (Jeddah/Abha)                      |
| Optimize Wait Times         | Implement fast-track triage + pre-registration systems                 |
| Address Chronic Diseases    | Launch disease management clinics                                     |
| Elevate CSAT                | Implement service recovery protocols + empathy training               |
| Monitor Diagnoses/Procedures| Use predictive analytics for staffing/resource allocation              |
| Sustain Bed Utilization     | Dynamic scheduling + cross-departmental bed management                |

---

## 14. Future Work
- Develop real-time dashboard with predictive occupancy alerts
- Integrate outpatient clinic data for comprehensive flow analysis
- Expand ML model to forecast CSAT from operational metrics
- Implement ML-based readmission prediction system

---

## 15. Technical Details
**Tools Used**: 
- Power BI 
- Power Query
