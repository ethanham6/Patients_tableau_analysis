# Patient Visit Analysis Dashboard

An end-to-end analysis of clinic patient visit data using **Excel, MySQL, and Tableau**. The project answers four questions a clinic team might ask about its patients, then presents the results in an interactive dashboard.

## Business Questions

1. What does the clinic's patient population look like by age?
2. What are the top diagnoses overall?
3. How many events does the average patient have, and who are the high utilizers?
4. Which procedures (CPT codes) are performed most often?

## Tools Used

| Tool | Purpose |
|------|---------|
| Excel | Calculated patient age from date of birth and grouped patients into age brackets |
| MySQL | Counted and ranked diagnoses and procedures, calculated per-patient utilization |
| Tableau | Built the interactive dashboard |

## Dataset

A **synthetic sample dataset** of 200 diagnosis records for 42 patients. It contains no real patient information.

| Column | Description |
|--------|-------------|
| `visit_id` | Visit identifier |
| `patient_id` | Patient identifier |
| `visit_date` | Date of the visit |
| `date_of_birth` | Patient date of birth |
| `patient_age` | Patient age as recorded in the file |
| `patient_sex` | Male or Female |
| `icd_code` | Diagnosis code (ICD-10) |
| `cpt_code` | Procedure code (CPT) |

## Approach

1. **Age brackets (Excel):** calculated each patient's age from date of birth and grouped patients into 0-17, 18-39, 40-64, and 65+.
2. **Diagnosis analysis (MySQL):** counted how many times each ICD code appears and ranked the top 10.
3. **Utilization (MySQL):** counted diagnosis events per patient, averaged them across patients, and flagged high utilizers (4 or more events).
4. **Procedures (MySQL):** ranked CPT codes by frequency.
5. **Dashboard (Tableau):** exported each query result to CSV and built KPI tiles and charts, with a patient sex filter.

### Example queries

```sql
-- Average diagnosis events per patient
SELECT COUNT(*) / COUNT(DISTINCT patient_id) AS avg_events_per_patient
FROM patient_visits;

-- High utilizers (4+ diagnosis events)
SELECT patient_id, COUNT(*) AS diagnosis_events
FROM patient_visits
GROUP BY patient_id
HAVING COUNT(*) >= 4
ORDER BY diagnosis_events DESC;

-- Top 10 diagnoses
SELECT icd_code, COUNT(*) AS diagnoses
FROM patient_visits
GROUP BY icd_code
ORDER BY diagnoses DESC
LIMIT 10;

-- Most common procedures
SELECT cpt_code, COUNT(*) AS procedures
FROM patient_visits
GROUP BY cpt_code
ORDER BY procedures DESC
LIMIT 10;
```

## Key Findings

- **42 patients** and **200 diagnosis events**, an average of **4.76 events per patient**.
- **I10 (essential hypertension) is the most common diagnosis** with 19 records, well ahead of N39.0 and J45.909 (11 each).
- **20 of 42 patients are high utilizers** with 4 or more events. Eighteen have 9 events and two have 8, so about half of all patients account for most of the 200 events.
- **Patients aged 40-64 are the largest age group** in the clinic.
- **CPT 99213 is the most frequently performed procedure** (7 records), followed by 99214 and 99204 (6 each).

## Dashboard

The dashboard includes:

- **KPI tiles:** total patients, average events per patient, and total diagnoses
- **Most Common Diagnosis:** top 10 ICD codes
- **Patients by Age Bracket:** patient count in each age group
- **Highest Utilizers (4+):** patients with four or more diagnosis events
- **Patient Sex filter** to focus on female or male patients


## Limitations

- The data is synthetic and small (42 patients), so the findings are illustrative, not clinical.
- Counts reflect diagnosis events, not unique clinical encounters.
- The dashboard does not break down diagnoses by age bracket and sex, which could be a next step.

## Possible Next Steps

- Break down top diagnoses by age bracket and sex
- Add a procedures chart (CPT codes) to the dashboard
- Analyze visit trends over time by month
- Clean the conflicting patient records and rebuild the age brackets


## What I Learned

- Using Excel, SQL, and Tableau together in one workflow, from raw data to a finished dashboard
- Checking my numbers against the raw data and finding data quality problems before presenting results
- Defining a metric clearly (diagnosis events versus visits) before reporting it
