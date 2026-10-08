# Clinical-Healthcare-Analytics

This is a brief overview of my project
Tool used: Excel, Power Query, Power Pivot, Pivot Tables, Pivot Charts, KPIs, and Interactive Slicers and Dashboards.....

Brief Overview:
This project focuses on analyzing clinical visit data to provide meaningful insights into:Patient visits Clinical cost Treatment type Diagnoses Doctor performance
Patient volume
End-to-end healthcare analytics solution transforming raw clinical data into actionable insights using Excel, Power Query, Power Pivot, DAX, and interactive dashboards. Features data cleaning, KPI development, trend analysis, dynamic reporting, and data-driven performance insights.

Data Dictionary- Doctors Dataset
Here is a **GitHub README-ready Markdown** version you can paste directly into your repository:

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


Data Dictionary- Patient_Mapping Dataset
Field Name	Data Type	Description	Example	Business / Analytical Purpose
patient_id	Text / String	Unique identifier assigned to each patient.	P2503	Serves as the unique patient key and links patient records to clinical visits, treatments, and other healthcare data.
patient_name	Text / String	Full name of the patient.	Maurice Habyarimana	Identifies the patient for administrative and reporting purposes.
health_score	Integer / Numeric	Numerical score representing the patient's recorded health status.	89	Used to assess and analyze overall patient health levels and identify patterns across demographic or clinical groups.
insurance_type	Text / Categorical	Type of health insurance or coverage associated with the patient.	Private	Enables analysis of patient coverage, insurance distribution, and healthcare utilization by insurance category.
primary_care_physician_id	Text / String	Unique identifier of the physician designated as the patient's primary care physician.	DOC0020	Establishes the relationship between patients and their primary care physicians and supports physician-level analysis.
blood_type	Text / Categorical	Patient's recorded blood group and Rh factor.	A+	Supports clinical classification and healthcare analysis where blood-group information is relevant.
city	Text / Categorical	City where the patient is recorded as residing or receiving care.	Gisenyi	Enables geographic analysis of patient distribution, healthcare utilization, and clinical trends.

Data Dictionary- Patient_records_fact Dataset

Field Name	Data Type	Description	Example	Business / Analytical Purpose
patient_id	Text / String	Unique identifier of the patient associated with the visit.	P2831	Links each visit to the patient dimension and enables patient-level visit and utilization analysis.
visit_date	Date	Calendar date on which the patient visit occurred.	16/05/2010	Supports time-series analysis, monthly/annual trends, date filtering, and clinical cost analysis over time.
visit_time	Time	Exact time at which the patient visit was recorded.	15:57:06	Enables analysis of visit patterns by time of day and operational activity.
age	Whole Number	Age of the patient at the time of the recorded visit.	39	Supports demographic segmentation and analysis of healthcare utilization by age group.
gender	Text / Categorical	Gender recorded for the patient.	F	Enables analysis of visits, diagnoses, and costs by gender.
diagnosis	Text / Categorical	Primary medical condition or diagnosis associated with the visit.	Migraine	Supports disease prevalence, visit-volume, treatment, and cost analysis by diagnosis.
cost	Decimal / Currency	Recorded clinical cost associated with the patient visit.	63.05	Used to calculate total clinical cost, average cost per visit, cost trends, and cost comparisons across diagnoses and treatments.
doctor_id	Text / String	Unique identifier of the doctor responsible for the visit.	DOC0002	Links visits to the doctor dimension and supports physician-level performance and workload analysis.
treatment_type	Text / Categorical	Type of healthcare service or treatment provided during the visit.	Emergency	Enables comparison of visit volume and clinical costs across Checkup, Outpatient, and Emergency services.

Data Dictionary- wellnes_activity Dataset

Field Name	Data Type	Description	Example	Business / Analytical Purpose
patient_id	Text / String	Unique identifier assigned to each patient participating in a wellness activity.	P0785	Links wellness activity records to the patient dimension and enables patient-level wellness analysis.
activity_type	Text / Categorical	Type of wellness or lifestyle activity performed by the patient.	Meditation	Enables comparison of participation and outcomes across activities such as Meditation, Sleep, Exercise, Yoga, and Checkup.
activity_date	Date	Calendar date on which the wellness activity occurred.	29/02/2024	Supports time-series analysis, monthly trends, and date-based dashboard filtering.
activity_time	Time	Time at which the wellness activity was recorded or performed.	02:42:37	Enables analysis of activity patterns by time of day.
duration_minutes	Whole Number	Total duration of the recorded activity in minutes.	45	Measures activity engagement and supports analysis of wellness outcomes based on activity duration.
wellness_status	Text / Categorical	Overall wellness classification associated with the recorded activity.	Average	Enables comparison of wellness outcomes across patients, activities, locations, and time periods.
Percentage_good	Integer / Binary Indicator	Indicator showing whether the recorded wellness status is classified as Good. 1 represents Good, while 0 represents a non-Good status.	1	Supports KPI calculations such as percentage of activities/patients achieving a Good wellness status.
City	Text / Categorical	City associated with the patient's wellness activity.	Kigali	Enables geographic analysis of wellness activities and outcomes across locations.

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
