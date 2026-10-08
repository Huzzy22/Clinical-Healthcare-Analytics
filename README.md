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

Business Questions 
1. The Scenario
You've been hired as a data analyst for a regional health network that runs both a clinical service (doctor visits) and a patient wellness program (meditation, exercise, sleep tracking, and more). Leadership wants one dashboard that brings both sides together, so they can see how clinical costs, doctor workload, and patient wellness engagement relate to each other — and use it to plan staffing and expand the wellness program.
The data has never been cleaned or connected before. You're given four raw exports pulled straight from different systems. Your job is to clean them, model them properly, and turn them into a dashboard leadership can actually use.
2. The Data
All four tables are in Healthcare_Wellness_Student_Data.xlsx, each on its own sheet, formatted as an Excel Table. Do not edit this file directly — import each table into Power Query and do all cleaning there, so your steps are documented and repeatable.
Table	Rows	What it holds
patient_records_fact	24,928	One row per clinical visit: patient, visit date, age, gender, diagnosis, cost, attending doctor, treatment type.
wellness_activity	49,776	One row per logged wellness activity: patient, activity type, date, duration (minutes), wellness status.
doctors	26	One row per doctor: doctor ID, name, specialty.
patient_mapping	3,243	One row per patient: name, health score, insurance type, primary care physician ID, blood type, city.

How the tables connect
•	patient_mapping.patient_id → patient_records_fact.patient_id (one patient, many visits)
•	patient_mapping.patient_id → wellness_activity.patient_id (one patient, many activities)
•	doctors.doctor_id → patient_records_fact.doctor_id (one doctor, many visits)
•	doctors.doctor_id and patient_mapping.primary_care_physician_id look like the same kind of value, but they answer different questions — one is “who did the patient see today”, the other is “who is the patient's assigned physician.” Decide deliberately how (or whether) to relate them, and be ready to explain your choice.
3. Task 1 — Clean the Data (Power Query)
Every table has real-world problems. Investigate each column before you touch it — don't assume a fix until you've profiled the data (use Column Quality / Column Distribution in Power Query). For every fix, choose an approach you can defend, and leave yourself a trail (clear query step names) so someone else could follow your logic.
patient_records_fact
•	age: mixed with non-numeric entries and some biologically impossible values. Decide how to handle each case — don't silently delete rows without a rule.
•	gender: several different spellings/codes for the same categories, plus at least one value that doesn't belong in this column at all.
•	cost: not stored consistently — some values are text, not numbers. This has to be numeric before it can be summed or charted.
•	visit_date: check the range. Some dates are far outside anything plausible for an active patient record — decide a cutoff and a rule.
•	Duplicate rows exist — find and remove them.
wellness_activity
•	Duplicate rows exist here too — don't assume the same rule that worked on the other table is enough; check this table on its own terms.
doctors
•	doctor_name: inconsistent capitalization. Standardize to one consistent format.
patient_mapping
•	patient_name: stray leading/trailing spaces and inconsistent capitalization.
•	insurance_type: several spellings/abbreviations for what should be two categories, plus missing values — decide how to treat blanks (a new category, or something else) and justify it.
Load every cleaned query to the Data Model (not just a worksheet) — that's what makes it available to Power Pivot.
4. Task 2 — Build the Data Model (Power Pivot)
1.	In Power Pivot → Manage, confirm all four cleaned tables loaded to the model.
2.	Add a Date table — follow the steps below.
•	Power Query → New Source → Blank Query.
•	In the formula bar, paste: = List.Dates(#date(2020,1,1), 3000, #duration(1,0,0,0)) — this generates 3,000 consecutive days starting Jan 1, 2020, comfortably covering every date in your cleaned data.
•	Right-click the list → To Table, then rename the column Date.
•	Add Column → Date → Year, and Add Column → Date → Month Name, so you have something to group charts by later.
•	Close & Load To… → Only Create Connection → Add to Data Model.
•	In Power Pivot, select the table → Design → Mark as Date Table → choose your Date column.
3.	Create the relationships listed in Section 2. Relate the Date table to both visit_date and activity_date — only one relationship per fact table can be active at a time, so pick the one your dashboard needs most (you can always build a second Date table if you genuinely need both active).
4.	Arrange the Diagram View into a star schema: fact tables in the middle, dimension tables (patient_mapping, doctors, Date) around the outside. Take a screenshot for your submission.
5. Task 3 — KPI Cards & PivotTable Calculations (No DAX)
Every KPI card and chart on your dashboard must come from a PivotTable or PivotChart connected to the Data Model — never a typed-in number. The trick is knowing which aggregation to pick, and when a straightforward aggregation isn't enough.
How PivotTables calculate without DAX
Drag a field into the Values area, then right-click it → Value Field Settings → Summarize Values By, and choose Sum, Count, Average, Min, Max, or — because your tables live in the Data Model — Distinct Count. That single choice is doing the same job a DAX measure would.
KPI card	Value Field setting	Source & watch-outs
Total Visits	Count of patient_id	PivotTable from patient_records_fact.
Distinct Patients (Clinical)	Distinct Count of patient_id	Only shows up as an option because the table is in the Data Model — right-click the value → Value Field Settings → Summarize by → Distinct Count.
Total Clinical Cost	Sum of cost	PivotTable from patient_records_fact.
Average Cost per Visit	Average of cost	The average of a per-row column already equals total ÷ count — you don't need a separate ratio.
Average Patient Age	Average of age	Pull this from patient_records_fact so every visit counts once.
Total Wellness Activities	Count of activity_type	PivotTable from wellness_activity.
Average Activity Duration	Average of duration_minutes	PivotTable from wellness_activity.
% Good Wellness Status	Average of an IsGood helper column	Add a Power Query Conditional Column: 1 if wellness_status = “Good”, else 0. Averaging that column and formatting it as a percentage gives you the KPI — no formula needed on the dashboard.
Average Health Score	Average of health_score — pivoted from patient_mapping, not from a fact table	This is the trap: pivoting health_score from patient_records_fact averages it once per visit, so patients with more visits count more. Build this card from patient_mapping (one row per patient) instead.
Cost per Patient	Sum of cost ÷ Distinct Count of patient_id	There's no single Value Field setting for this. Build a small PivotTable with both values, then a separate cell that divides one GETPIVOTDATA reference by the other — see the technique below.
Turning a PivotTable value into a KPI card
1.	Build a tiny PivotTable with nothing in Rows or Columns — just one field in Values. That single cell is your KPI.
2.	Turn on File → Options → Formulas → “Generate GetPivotData functions”. Now, if you type “=” in another cell and click a value inside any PivotTable, Excel writes the reference for you as a GETPIVOTDATA formula, e.g. =GETPIVOTDATA("Sum of cost",$A$3).
3.	Put that formula on your dashboard sheet, in a cell that isn't part of a pivot. Format it as the “card”: large bold font, a fill or border, a short label above or below it. This is what keeps your dashboard from looking like a spreadsheet of raw pivot tables.
4.	For a two-step KPI like Cost per Patient, use two GETPIVOTDATA references in one formula: =GETPIVOTDATA("Sum of cost",$A$3) / GETPIVOTDATA("Distinct Count of patient_id",$A$3).
5.	Connect the same slicers to the PivotTable(s) feeding your cards (right-click slicer → Report Connections). The GETPIVOTDATA formula recalculates automatically whenever the pivot does, so your KPI cards stay live.
Challenge yourself further
•	Add conditional formatting to a KPI card — an icon or color that changes based on a threshold you set (e.g. green if Average Cost per Visit is below a target, red if above).
•	Build a period-over-period KPI: two PivotTables filtered to different date ranges, then a card that shows the % change between them.
•	Calculate Cost per Patient a second, different way (e.g. from a pivot built on patient_mapping with a related-table lookup) and confirm both methods agree — a good way to catch a modeling mistake before your instructor does.

 6. Task 4 — The Dashboard (3 pages)
Build all three pages in the same workbook as separate sheets, using PivotTables/PivotCharts connected to the Data Model. Use one consistent color theme and font across all three pages. Every page needs a clear title and at least one slicer, and slicers that make sense on more than one page should be synced across those pages (Slicer → Report Connections).
Page 1 — Clinical Overview
•	KPI cards: Total Visits, Total Clinical Cost, Average Cost per Visit, Distinct Patients
•	Visits by diagnosis (bar or column chart)
•	Clinical cost trend over time (line chart, by month)
•	Visits by treatment type (Checkup / Outpatient / Emergency)
•	Top doctors by visit volume
•	Slicers: date range, treatment type, doctor specialty
Page 2 — Wellness & Engagement
•	KPI cards: Total Wellness Activities, Average Duration, % Good Wellness Status
•	Activities by type (Meditation, Sleep, Exercise, Yoga, Checkup, Hydration, Diet)
•	Wellness status breakdown (Good / Average / Poor)
•	Activity volume over time (trend)
•	Activities or average duration by city
•	Slicers: date range, activity type, city
Page 3 — Patient & Doctor Deep Dive
•	Patient count by insurance type and by blood type
•	Average health score by city and by insurance type
•	Doctor headcount and visit share by specialty
•	A combined view relating health score to clinical cost or visit frequency per patient — this is the chart that should directly support your key insight
•	Slicers: city, insurance type, specialty
7. Deliverables
•	Cleaned workbook: one Excel file containing your Power Query steps, the Data Model, and all three dashboard pages
•	Relationship diagram: a screenshot of your Diagram View, included on a sheet or in your write-up
•	Insights write-up: half a page to one page: 3–5 specific findings from your dashboard and one recommendation for leadership, referencing actual numbers
8. Grading Rubric
Component	Points	What we're looking for
Data cleaning in Power Query	20	Every issue identified and fixed with an appropriate, justified method; queries are named clearly and load correctly.
Data model (Power Pivot)	15	Correct relationships, working date table, sensible key choices, no unintended many-to-many or circular relationships.
KPI cards & PivotTable calculations	20	Every KPI uses the correct aggregation (or technique, for the trickier ones), pulls from the right source table, and updates live from a PivotTable — no typed-in numbers.
3-page dashboard	30	Clear layout, correct visuals for each question, working slicers, consistent design, all three pages complete.
Insights write-up	15	Findings are specific, supported by the dashboard, and lead to a concrete recommendation.
Total	100	
9. Getting Started
1.	Open Healthcare_Wellness_Student_Data.xlsx and read the Start Here sheet.
2.	Data tab → Get Data → From File → From Workbook, point it at this file, and load each table into Power Query (not directly into the worksheet).
3.	Profile each column before changing anything: Home → Data Preview → Column quality / distribution.
4.	Fix one issue at a time, naming each query step so your logic is traceable.
5.	Close & Load To… → Only Create Connection → Add to Data Model, for each of the four tables.
6.	Build the Date table, set up relationships, then build your KPI PivotTables before building any charts.

  	Insights, Findings and Recommendations

    Section	Finding / Recommendation
Finding 1	From the dasboards you can tell that Clinical costs keeps climbing while visit volume drops. The Clinical Overview dashboard shows monthly clinical cost trending upwards toward roughly $90,000+ near the end of the period (from a low near $63,900), even as the line's shape suggests fewer, costlier visits rather than more visits overall. Across all 2,671 visits, total clinical cost reached $927,492.05, averaging $347.25 per visit.
Finding 2	Hypertension, asthma, and diabetes dominate diagnoses. These three conditions account for the top visit volumes (420, 411, and 389 respectively) in the Clinical Overview dashboard, well ahead of migraine (311) and bronchitis (303) — suggesting chronic condition management is the primary clinical workload.
Finding 3	The Wellness & Engagement dashboard logged 49,776 total activities averaging 67.5 minutes each, with 40.3% rated "Good." However, activity volume over time slides from 4,343 in the earliest month to roughly 3,812 by the latest, a decline of about 12%.
Finding 4	Doctor workload is uneven across specialties. Neurology (7.0) and Cardiology (6.0) carry the highest headcount-to-visit ratios, while Endocrinology (3.0) is comparatively understaffed.
Finding 5	Visit frequency and cost move together, and both have a weak negative link to health score — not insurance type. The correlation analysis on the Patient & Doctor dashboard shows visit frequency and clinical cost are strongly associated with each other (r = 0.937), while each has only a weak negative relationship with health score (r = -0.352 for cost, r = -0.258 for visits). In other words, patients who visit more often also tend to spend more
Recommendation	"Staff around actual demand. Put more resources behind the highest-volume diagnoses (hypertension, asthma, diabetes) and the specialties carrying the heaviest caseloads (Neurology, Cardiology), while reviewing whether Endocrinology is understaffed relative to demand.
Target wellness outreach using utilization patterns, not insurance type. Patients with rising visits and cost are the ones showing early signs of declining health 
Reverse the drop in wellness activity. Engagement has fallen about 12% over the period tracked. Re-engagement efforts should focus first on patients already flagged as high-utilization, since they have the most to gain from staying active."



