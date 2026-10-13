# Medical Record ROI Operations Analysis

An Excel-based operations analytics project built on a dataset of **10,000 Release of Information (ROI) requests**. ROI is the process by which healthcare organizations fulfil requests for patient medical records from insurers, attorneys, physicians, patients, and legal parties. The workbook tracks request volume, turnaround time (TAT), SLA compliance, quality, and associate productivity.

---

## Project Contents

| File | Description |
|------|-------------|
| `Medical_Record_ROI_Operations_Dataset.xlsx` | Raw dataset plus an EDA dashboard sheet with pivot tables |
| `README.md` | This document |

### Workbook structure

| Sheet | Purpose |
|-------|---------|
| `Medical_Record_ROI_Operations_D` | Raw data, 10,000 rows × 18 columns (14 input fields + 4 formula-derived fields) |
| `EDA Analysis` | KPI summary and 6 pivot tables for exploratory analysis |

---

## Dataset Overview

- **Records:** 10,000 (one row per ROI request, unique `Request_ID`)
- **Request dates:** 1 Jan 2026 – 30 Jun 2026
- **Completion dates:** 2 Jan 2026 – 10 Jul 2026
- **Missing values:** none
- **Unique patients:** 9,481 (some patients have multiple requests)

### Data dictionary

| Column | Type | Description |
|--------|------|-------------|
| `Request_ID` | Text | Unique request identifier (`ROI00001` …) |
| `Patient_ID` | Text | Patient identifier (`P#####`) |
| `Request_Date` | Date | Date the request was received |
| `Completion_Date` | Date | Date the request was fulfilled |
| `Request_Type` | Category | Requester: Attorney, Insurance, Legal, Patient, Physician |
| `Record_Type` | Category | Consultation Note, Discharge Summary, Lab Report, Operative Note, Progress Notes, Radiology Report |
| `Department` | Category | Cardiology, Emergency, General Medicine, Neurology, Oncology, Orthopedics, Pediatrics, Radiology |
| `Priority` | Category | Normal, High, Urgent |
| `Status` | Category | Completed, In Progress, Pending |
| `Assigned_Associate` | Category | Amit, Karan, Neha, Pooja, Priya, Rahul, Sneha, Vikas |
| `Pages_Processed` | Integer | Pages handled for the request (20–500) |
| `Errors_Found` | Integer | Errors identified during processing (0–5) |
| `Quality_Score` | Decimal | Quality score out of 100 (92.5–100) |
| `TAT_Days` | *Formula* | `=Completion_Date − Request_Date` |
| `SLA_Target` | *Formula* | Fixed target of **5 days** |
| `SLA_Status` | *Formula* | `Met` if `TAT_Days <= SLA_Target`, otherwise `Breached` |
| `Error_Rate` | *Formula* | `(Errors_Found / Pages_Processed) × 100` |
| `Pages_Per_Day` | *Formula* | `Pages_Processed / TAT_Days` |

---

## Analysis (`EDA Analysis` sheet)

**KPI summary (formula-driven)**
- Total requests
- Average TAT
- Average quality score
- Total pages processed
- Total errors

**Pivot tables**
1. **Request volume by type**: count of requests per requester category
2. **Record type analysis**: count of requests per record type
3. **Department workload**: count of requests per department
4. **Turnaround time analysis**: average TAT per department
5. **SLA performance**: count of requests Met vs. Breached
6. **Associate productivity**: requests handled, total pages processed, and average quality score per associate

### Headline results

| Metric | Value |
|--------|-------|
| Total requests | 10,000 |
| Average TAT | 5.49 days |
| SLA met | 5,052 (50.5%) |
| SLA breached | 4,948 (49.5%) |
| Average quality score | 97.50 |
| Total pages processed | ~2.60 million |
| Total errors | ~24,846 |

**Observations**
- Workload is spread fairly evenly across departments (1,201–1,306 requests each) and requester types (1,970–2,021 each).
- Emergency has the highest average TAT (≈5.64 days); Cardiology the lowest (≈5.30 days).
- Associate quality scores are tightly clustered (≈97.4–97.6), with Rahul slightly highest.
- Roughly half of all requests breach the 5-day SLA, which is the main performance concern surfaced by the data.

---

## How to Use

1. Open `Medical_Record_ROI_Operations_Dataset.xlsx` in Microsoft Excel (or a compatible tool).
2. Browse the raw data in `Medical_Record_ROI_Operations_D`.
3. Open `EDA Analysis` for the KPIs and pivot tables.
4. If you edit or add data, use **Data → Refresh All** to update the pivot tables. Note that the KPI formulas on the EDA sheet reference rows 2–10001 for some metrics and entire columns for others, so adjust ranges if the row count changes.

---

## Data Notes & Known Limitations

- **Completion dates on open requests:** `Completion_Date` is populated for every row, including those with `Status` of *Pending* or *In Progress*. TAT and SLA results are therefore calculated for all requests, regardless of status. Consider filtering to `Status = Completed` for strict SLA reporting.
- **Uniform SLA target:** all requests use a 5-day target regardless of priority (Normal / High / Urgent). Priority-based targets would give a more realistic view.
- **Pivot tables are static snapshots** until refreshed.
- **Label typo:** the "Department Workload" heading on the EDA sheet is spelled "Deaprtment".
- The dataset appears to be synthetic/sample data. It contains patient-style identifiers, so follow your organization's privacy policies if you adapt it for real PHI.

---

## Possible Next Steps

- Add charts and slicers (department, priority, status) to the EDA dashboard
- Analyze SLA breach rate by priority, request type, and associate
- Track monthly request volume and TAT trends
- Define priority-specific SLA targets
- Explore the relationship between page volume, error rate, and quality score
- Recreate the analysis in Python (pandas) or Power BI / Tableau

---

## Tools

- Microsoft Excel (formulas, pivot tables)
