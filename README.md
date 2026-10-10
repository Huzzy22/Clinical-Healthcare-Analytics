# Clinical-Healthcare-Analytics

This is a brief overview of my project
Tool used: Excel, Power Query, Power Pivot, Pivot Tables, Pivot Charts, KPIs, and Interactive Slicers and Dashboards.....

Brief Overview:
This project focuses on analyzing clinical visit data to provide meaningful insights into:Patient visits Clinical cost Treatment type Diagnoses Doctor performance
Patient volume
End-to-end healthcare analytics solution transforming raw clinical data into actionable insights using Excel, Power Query, Power Pivot, DAX, and interactive dashboards. Features data cleaning, KPI development, trend analysis, dynamic reporting, and data-driven performance insights.


## 📋 Data Dictionary — Doctor Dataset

The **Doctor Dataset** contains reference information about healthcare providers, including their unique identifiers, names, and medical specialties. It serves as a **doctor dimension table (`Dim_Doctor`)** for linking physician information to clinical visit records and supporting provider-level healthcare analytics.

| Field Name    | Data Type   | Description                                                    | Example                  | Analytical Purpose                                                                                        |
| ------------- | ----------- | -------------------------------------------------------------- | ------------------------ | --------------------------------------------------------------------------------------------------------- |
| `doctor_id`   | String      | Unique identifier assigned to each doctor.                     | `DOC0009`                | Serves as the primary key and establishes relationships with clinical visit and patient-related datasets. |
| `doctor_name` | String      | Full name of the doctor, including professional title.         | `Dr.Benjamin Manirakiza` | Identifies healthcare providers for reporting, filtering, and physician-level analysis.                   |
| `specialty`   | Categorical | Medical specialty or area of clinical expertise of the doctor. | `Cardiology`             | Enables analysis of clinical activity, patient visits, costs, and performance by medical specialty.       |

### 🔑 Data Model Role

* **Table Name:** `Dim_Doctor`
* **Primary Key:** `doctor_id`
* **Relationship:** `doctor_id` → `Fact_Clinical_Visits[doctor_id]`
* **Table Type:** Dimension / Reference Table

### 🧹 Data Quality Considerations

| Field         | Data Quality Consideration                                            | Recommended Treatment                                                  |
| ------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `doctor_id`   | Contains alphanumeric identifiers with leading zeros.                 | Store as **Text** and ensure every ID is unique.                       |
| `doctor_name` | Some records contain inconsistent spacing and capitalization.         | Apply **Trim, Clean, and Proper Case** transformations in Power Query. |
| `specialty`   | Some values contain inconsistent spacing, e.g., `General   Practice`. | Standardize spacing and capitalization before analysis.                |

### 📊 Analytical Use Cases

This dataset supports:

* Doctor-level visit volume analysis
* Clinical cost analysis by doctor
* Performance comparison across specialties
* Top doctors by patient/visit volume
* Specialty-level healthcare trends
* Interactive dashboard filtering using doctor slicers

> **Data Governance Note:** `doctor_id` should remain stable and unique because it functions as the primary key for the doctor dimension and as a foreign key in related clinical datasets.


## 📋 Data Dictionary — Patient_Mapping Dataset

The **Clinical Visits Dataset** contains transactional records of patient healthcare encounters. Each row represents a clinical visit and captures key demographic, diagnostic, treatment, physician, date/time, and cost information. This dataset serves as the primary **fact table (`Fact_Clinical_Visits`)** for healthcare analytics and dashboard reporting.

### Data Dictionary

| Field Name       | Data Type          | Description                                                                   | Example      | Analytical Purpose                                                                                   |
| ---------------- | ------------------ | ----------------------------------------------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------------- |
| `patient_id`     | String             | Unique identifier assigned to the patient associated with the clinical visit. | `P2831`      | Links each visit to the patient dimension and supports patient-level utilization analysis.           |
| `visit_date`     | Date               | Calendar date on which the clinical visit occurred.                           | `16/05/2010` | Supports time-series analysis, monthly/annual trends, and date-based dashboard filtering.            |
| `visit_time`     | Time               | Time at which the clinical visit was recorded.                                | `15:57:06`   | Enables analysis of healthcare utilization by time of day.                                           |
| `age`            | Integer            | Age of the patient at the time of the recorded visit.                         | `39`         | Supports demographic segmentation and analysis of healthcare utilization across age groups.          |
| `gender`         | Categorical        | Gender recorded for the patient.                                              | `F`          | Enables comparison of visit volumes, diagnoses, and costs across gender categories.                  |
| `diagnosis`      | Categorical        | Primary medical condition or diagnosis associated with the clinical visit.    | `Migraine`   | Supports diagnosis prevalence, visit-volume, treatment, and cost analysis.                           |
| `cost`           | Decimal / Currency | Clinical cost associated with the recorded patient visit.                     | `63.05`      | Used to calculate total clinical cost, average cost per visit, and cost trends.                      |
| `doctor_id`      | String             | Unique identifier of the doctor responsible for the clinical visit.           | `DOC0002`    | Links visits to the doctor dimension and supports physician-level workload and performance analysis. |
| `treatment_type` | Categorical        | Type of healthcare service or treatment provided during the visit.            | `Emergency`  | Enables comparison of healthcare utilization and costs across treatment categories.                  |

### 🔑 Data Model Role

* **Table Name:** `Fact_Clinical_Visits`
* **Table Type:** Fact / Transactional Table
* **Primary Transaction:** Clinical Patient Visit
* **Patient Relationship:** `patient_id` → `Dim_Patient[patient_id]`
* **Doctor Relationship:** `doctor_id` → `Dim_Doctor[doctor_id]`
* **Date Relationship:** `visit_date` → `Dim_Calendar[date]`

### 🧹 Data Quality Considerations

| Field            | Data Quality Consideration                         | Recommended Treatment                                                                                                                    |
| ---------------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `patient_id`     | Alphanumeric identifiers such as `P2831`.          | Store as **Text** and validate against the patient dimension.                                                                            |
| `visit_date`     | Dates are provided in `DD/MM/YYYY` format.         | Convert to a proper **Date** data type in Power Query.                                                                                   |
| `visit_time`     | Time values include hours, minutes, and seconds.   | Store as **Time** and validate the time format.                                                                                          |
| `age`            | Numeric age values.                                | Store as **Whole Number** and validate against reasonable age boundaries.                                                                |
| `gender`         | Values such as `F` and `M` are abbreviated.        | Standardize to `Female` and `Male` for user-friendly reporting where appropriate.                                                        |
| `diagnosis`      | Medical diagnosis is categorical.                  | Standardize spelling, capitalization, and naming conventions.                                                                            |
| `cost`           | Contains decimal values and some records with `0`. | Store as **Decimal/Currency**. Investigate whether zero represents a genuine zero charge, waived cost, coverage, or missing information. |
| `doctor_id`      | Alphanumeric physician identifiers.                | Store as **Text** and validate against `Dim_Doctor[doctor_id]`.                                                                          |
| `treatment_type` | Categorical treatment classifications.             | Standardize categories such as `Checkup`, `Outpatient`, and `Emergency`.                                                                 |

### 📊 Key Analytical Metrics

This dataset supports the development of key healthcare KPIs, including:

* **Total Visits**
* **Total Clinical Cost**
* **Average Cost per Visit**
* **Distinct Patients**
* **Visits by Diagnosis**
* **Visits by Treatment Type**
* **Clinical Cost Trend by Month**
* **Top Doctors by Visit Volume**
* **Clinical Cost by Doctor**
* **Clinical Cost by Diagnosis**
* **Visits by Gender and Age Group**

### 🎯 Analytical Purpose

The dataset provides the transactional foundation for analyzing **patient utilization, clinical activity, treatment patterns, physician performance, and healthcare costs**. When integrated with the patient, doctor, wellness, and calendar dimensions, it enables an interactive **Power Pivot healthcare analytics model and dashboard** for data-driven decision-making.

> **Data Governance Note:** `patient_id`, `doctor_id`, and `visit_date` should be maintained as reliable relationship keys to ensure accurate filtering, aggregation, and KPI calculations throughout the Power Pivot data model.


## 📋 Data Dictionary — Patient_records_fact Dataset

The **Patient_records_fact Dataset** contains transactional records of patient healthcare encounters. Each row represents an individual clinical visit and captures demographic, diagnostic, treatment, physician, date/time, and financial information. The dataset serves as the primary **fact table (`Patient_records_fact`)** within the healthcare analytics data model.

### Data Dictionary

| Field Name       | Data Type          | Description                                                                    | Example      | Analytical Purpose                                                                                     |
| ---------------- | ------------------ | ------------------------------------------------------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------ |
| `patient_id`     | String             | Unique identifier assigned to the patient associated with the visit.           | `P2831`      | Links clinical encounters to the patient dimension and enables patient-level utilization analysis.     |
| `visit_date`     | Date               | Calendar date on which the clinical encounter occurred.                        | `16/05/2010` | Enables temporal analysis, trend identification, and date-based dashboard filtering.                   |
| `visit_time`     | Time               | Time at which the clinical encounter was recorded or occurred.                 | `15:57:06`   | Supports analysis of patient visits and healthcare activity by time of day.                            |
| `age`            | Integer            | Age of the patient at the time of the clinical encounter.                      | `39`         | Enables demographic segmentation and analysis of healthcare utilization across age groups.             |
| `gender`         | Categorical        | Gender recorded for the patient.                                               | `F`          | Supports analysis of visit volume, diagnoses, and clinical costs by gender.                            |
| `diagnosis`      | Categorical        | Primary medical condition or diagnosis associated with the clinical encounter. | `Migraine`   | Enables disease-pattern analysis, diagnosis frequency, treatment analysis, and cost comparisons.       |
| `cost`           | Decimal / Currency | Recorded clinical cost associated with the patient encounter.                  | `63.05`      | Supports calculation of total clinical expenditure, average cost per visit, and cost trends.           |
| `doctor_id`      | String             | Unique identifier of the physician responsible for the clinical encounter.     | `DOC0002`    | Links clinical visits to the doctor dimension and enables physician workload and performance analysis. |
| `treatment_type` | Categorical        | Type of healthcare service or treatment provided during the encounter.         | `Emergency`  | Enables comparison of utilization and clinical costs across treatment categories.                      |

### 🔑 Data Model Structure

| Attribute       | Definition                            |
| --------------- | ------------------------------------- |
| **Table Name**  | `Patient_records_fact`                |
| **Table Type**  | Fact / Transactional Table            |
| **Grain**       | One record per clinical patient visit |
| **Patient Key** | `patient_id`                          |
| **Doctor Key**  | `doctor_id`                           |
| **Date Key**    | `visit_date`                          |

### 🔗 Relationships

```text
Dim_Patient
    │
    │ patient_id
    ▼
Fact_Clinical_Visits
    ▲
    │ doctor_id
    │
Dim_Doctor

Dim_Calendar
    │
    │ visit_date
    ▼
Fact_Clinical_Visits
```

### 🧹 Data Quality & Standardization

| Field            | Consideration                                       | Recommended Treatment                                                                       |
| ---------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `patient_id`     | Alphanumeric identifiers with leading zeros.        | Store as **Text** and validate against `Dim_Patient`.                                       |
| `visit_date`     | Dates are presented in `DD/MM/YYYY` format.         | Convert to a proper **Date** data type using Power Query.                                   |
| `visit_time`     | Contains hours, minutes, and seconds.               | Store as **Time** and validate the time format.                                             |
| `age`            | Numeric patient age.                                | Store as **Whole Number** and validate for reasonable age ranges.                           |
| `gender`         | Uses abbreviated categories such as `F` and `M`.    | Standardize to `Female` and `Male` where appropriate for reporting.                         |
| `diagnosis`      | Categorical medical classification.                 | Standardize spelling, capitalization, and category names.                                   |
| `cost`           | Contains decimal values, including some `0` values. | Store as **Decimal/Currency** and investigate the business meaning of zero-cost encounters. |
| `doctor_id`      | Physician identifiers are alphanumeric.             | Store as **Text** and validate against `Dim_Doctor`.                                        |
| `treatment_type` | Categorical treatment/service classification.       | Standardize categories such as `Checkup`, `Outpatient`, and `Emergency`.                    |

### 📊 Key Analytical Metrics

The dataset supports the development of healthcare performance KPIs, including:

* **Total Visits**
* **Total Clinical Cost**
* **Average Cost per Visit**
* **Distinct Patients**
* **Visits by Diagnosis**
* **Visits by Treatment Type**
* **Clinical Cost Trend by Month**
* **Top Doctors by Visit Volume**
* **Clinical Cost by Doctor**
* **Clinical Cost by Diagnosis**
* **Visits by Gender**
* **Visits by Age Group**

### 🎯 Analytical Purpose

The `Patient_records_fact` table provides the transactional foundation for analyzing **patient utilization, clinical activity, treatment patterns, physician performance, and healthcare expenditure**.

When integrated with the `Dim_Patient`, `Dim_Doctor`, and `Dim_Calendar` tables, it forms a structured **Power Pivot analytical data model** that supports interactive dashboards, KPI reporting, trend analysis, and data-driven healthcare insights.

> **Data Governance Note:** `patient_id`, `doctor_id`, and `visit_date` should be maintained as reliable relationship keys to ensure accurate aggregation, filtering, and KPI calculations across the Power Pivot data model.


## 📋 Data Dictionary — Wellness Activity Dataset

The **Wellness Activity Dataset** captures patient wellness and lifestyle activities, including activity type, date and time, duration, wellness classification, performance indicator, and location. It supports analysis of wellness engagement, activity patterns, and patient well-being across different periods and locations.

### Data Dictionary

| Field Name         | Data Type        | Description                                                                                                                  | Example      | Analytical Purpose                                                                                                                  |
| ------------------ | ---------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| `patient_id`       | String           | Unique identifier assigned to the patient associated with the wellness activity.                                             | `P0785`      | Links wellness records to the patient dimension and enables patient-level wellness analysis.                                        |
| `activity_type`    | Categorical      | Type of wellness or lifestyle activity performed or recorded.                                                                | `Meditation` | Enables comparison of participation and wellness outcomes across activities such as Meditation, Sleep, Exercise, Yoga, and Checkup. |
| `activity_date`    | Date             | Calendar date on which the wellness activity was performed or recorded.                                                      | `29/02/2024` | Supports time-series analysis, trend identification, and date-based dashboard filtering.                                            |
| `activity_time`    | Time             | Time at which the wellness activity was performed or recorded.                                                               | `02:42:37`   | Supports analysis of wellness activity patterns by time of day.                                                                     |
| `duration_minutes` | Integer          | Total duration of the recorded wellness activity in minutes.                                                                 | `45`         | Measures activity engagement and enables analysis of wellness outcomes based on activity duration.                                  |
| `wellness_status`  | Categorical      | Overall wellness classification associated with the recorded activity.                                                       | `Average`    | Enables analysis of wellness conditions and comparison across activities, patients, cities, and time periods.                       |
| `Percentage_good`  | Binary / Integer | Indicator identifying whether the wellness status is classified as Good. `1` represents Good, while `0` represents Not Good. | `1`          | Supports calculation of the Good Wellness Rate and other wellness performance KPIs.                                                 |
| `City`             | Categorical      | City associated with the patient's wellness activity.                                                                        | `Kigali`     | Enables geographic comparison of wellness activity and outcomes across locations.                                                   |

### 🔑 Dataset Structure

| Attribute                  | Definition                                 |
| -------------------------- | ------------------------------------------ |
| **Table Name**             | `Wellness_activity`                        |
| **Table Type**             | Wellness Activity / Analytical Table       |
| **Primary Entity**         | Patient Wellness Activity                  |
| **Patient Identifier**     | `patient_id`                               |
| **Date Field**             | `activity_date`                            |
| **Categorical Dimensions** | `activity_type`, `wellness_status`, `City` |
| **Key Measures**           | `duration_minutes`, `Percentage_good`      |

### 🧹 Data Quality & Standardization

| Field              | Data Quality Consideration                                                  | Recommended Treatment                                                                                                                                     |
| ------------------ | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `patient_id`       | Contains alphanumeric patient identifiers.                                  | Store as **Text** and validate against `Dim_Patient[patient_id]`.                                                                                         |
| `activity_type`    | Contains categories such as Meditation, Sleep, Exercise, Yoga, and Checkup. | Standardize spelling and capitalization across all records.                                                                                               |
| `activity_date`    | Dates are presented in `DD/MM/YYYY` format.                                 | Convert to a proper **Date** data type using Power Query.                                                                                                 |
| `activity_time`    | Many records contain `00:00:00`, while others contain specific times.       | Store as **Time** and investigate whether midnight represents an actual recorded time or an unavailable/missing time value.                               |
| `duration_minutes` | Numeric activity duration measured in minutes.                              | Store as **Whole Number** and validate that values are non-negative and within reasonable limits.                                                         |
| `wellness_status`  | Contains categories such as Good, Average, and Poor.                        | Standardize category names and ensure consistent classification.                                                                                          |
| `Percentage_good`  | Contains binary values `0` and `1`.                                         | Treat as an indicator where `1 = Good` and `0 = Not Good`. Calculate the overall rate rather than treating the field as an already-calculated percentage. |
| `City`             | Geographic categorical field.                                               | Standardize spelling and capitalization for consistent geographic analysis.                                                                               |

### 📊 Key Analytical Metrics

The dataset can support the following healthcare and wellness KPIs:

* **Total Wellness Activities**
* **Average Activity Duration**
* **Total Activity Duration**
* **Good Wellness Rate**
* **Activities by Activity Type**
* **Wellness Status Distribution**
* **Wellness Trend Over Time**
* **Wellness Performance by City**
* **Average Duration by Activity Type**
* **Good Wellness Rate by Activity Type**
* **Patient Wellness Activity Frequency**

### 🔗 Integration with the Healthcare Data Model

The `Wellness_activity` table can be connected to the broader healthcare analytics model through `patient_id`:

```text
Dim_Patient
     │
     │ patient_id
     ▼
Wellness_activity
```

It can also be analyzed alongside:

```text
Dim_Doctor
      │
      ▼
Fact_Clinical_Visits
      │
      │ patient_id
      ▼
Dim_Patient
      │
      ▼
Wellness_activity
```

This enables a broader analysis of **clinical activity and patient wellness**, including relationships between healthcare utilization, patient characteristics, wellness activities, and wellness outcomes.

### 🎯 Analytical Purpose

The `Wellness_activity` dataset provides a structured foundation for analyzing **patient wellness engagement, activity duration, wellness outcomes, geographic patterns, and activity trends**.

When integrated with the project's patient and clinical datasets, it contributes to a comprehensive **healthcare analytics solution** capable of transforming operational healthcare data into meaningful, data-driven insights.

> **Data Governance Note:** `patient_id` should be maintained as a consistent relationship key. The `Percentage_good` field should be interpreted as a binary indicator (`1/0`) and converted into a percentage KPI through aggregation rather than being treated as a pre-calculated percentage.

## Business Questions

The project aims to help healthcare leadership understand clinical performance, patient wellness engagement, and resource utilization through an integrated, interactive Excel dashboard.

Key business questions include:

1. **Clinical Performance:** How do clinical visit volumes, diagnoses, treatment types, and costs vary over time?
2. **Cost Analysis:** What are the total clinical costs, average cost per visit, and factors associated with higher healthcare spending?
3. **Wellness Engagement:** How frequently do patients participate in wellness activities, and how do activity duration and wellness status vary over time and across cities?
4. **Patient Health:** How do patient health scores relate to clinical costs and visit frequency across different cities and insurance categories?
5. **Doctor Workload:** Which medical specialties handle the highest patient volumes, and how is workload distributed among doctors?
6. **Data-Driven Planning:** How can clinical and wellness insights guide staffing decisions, improve patient engagement, and support more effective healthcare resource allocation?

**Project Goal:** Transform raw healthcare data into actionable insights that support evidence-based clinical planning, cost monitoring, and wellness program improvement.


