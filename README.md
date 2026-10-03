# Automated Hospital MIS Report Generator

Python + Excel automation that cleans raw hospital admission data and generates an 11-sheet monthly MIS report with KPIs, month-on-month analysis, charts, auto-written insights and formula-based validation.

## Problem
Monthly MIS reports are often built manually in Excel: copy data, clean it, build pivots, write commentary. This is slow and error-prone. This project automates the whole flow. Change one line (the report month), run the notebook, and the full report is ready.

## Dataset
Healthcare Dataset (Kaggle, synthetic data): 55,500 admission records, 15 columns.
Download: (paste Kaggle dataset link here)

The dataset is synthetic, so the insights demonstrate the automation and not real clinical findings.

## What the pipeline does
1. **Cleans the data**
   - Removes 534 duplicate rows
   - Removes 106 rows with negative billing
   - Validates dates (discharge cannot be before admission)
   - Fixes name formatting in Hospital and Doctor columns (case, commas, "And" fragments, titles like Mr./Jr.)
   - Drops patient names from the output (privacy)
2. **Handles incomplete months automatically**: months with less than 80% of the median admissions (2019-05 and 2024-05) are excluded from MoM comparison and the trend chart
3. **Calculates KPIs**: admissions, total billing, average billing per admission, average length of stay, emergency admission %, abnormal test result %
4. **Reports MoM change correctly**: percentage change for volume KPIs, percentage-point (pp) change for ratio KPIs
5. **Generates Key Insights** as text from the data (computed each run, not hard-coded)
6. **Builds an 11-sheet Excel report** with charts and formatting
7. **Validates the report** with live Excel formulas (report values vs source data)

## Excel report sheets
| Sheet | Content |
|---|---|
| Summary | KPI table, MoM change, Key Insights |
| Medical_Condition | Admissions, billing, average LOS by condition + chart |
| Admission_Type | Emergency / Urgent / Elective split |
| Insurance | Billing by insurance provider |
| Age_Group | Admissions and billing by age group |
| Medication | Admissions and billing by medication |
| Abnormal_by_Condition | Abnormal test result % by condition |
| Monthly_Trend | Admissions per complete month + line chart |
| Data_Quality | Cleaning log (rows removed at each step) |
| Clean_Data_CurrentMonth | Audit data for the report month |
| Validation | Excel formulas comparing report totals with source data |

## Screenshots
### Summary (2024-04)
![Summary April](summary.png)

### Summary (2024-03): same code, one line changed
![Summary March](summary_march.png)

### Billing by medical condition
![Condition](condition.png)

### Monthly admissions trend
![Trend](trend.png)

### Validation (report vs source data)
![Validation](validation.png)

## How to run
1. Open the notebook in Google Colab
2. Run Cell 1 to install XlsxWriter
3. Run Cell 2 and upload `healthcare_dataset.csv` (download link above)
4. Run Cells 3 to 5 (load, inspect, clean)
5. In Cell 6, the report month defaults to the last complete month. To pick another month, uncomment and edit this line:
   `report_month = '2024-03'`
6. Run Cells 6 to 9. The Excel report is generated and downloaded

Sample outputs for 2024-04 and 2024-03 are included in this repo to show month-switching automation.

## Key design decisions
- **Incomplete months are excluded**: comparing a full month with a partial month gives misleading MoM numbers
- **Percentage points for ratio KPIs**: Emergency share moving from 32.00% to 35.14% is +3.14 pp, not +9.8%
- **Privacy**: patient names are removed before any output is created
- **Validation inside Excel**: formulas recalculate the totals from the source sheet, so anyone can audit the report without running Python

## Tools
Python, Pandas, NumPy, XlsxWriter, Excel, Google Colab

## Limitations
- Synthetic dataset, so the business insights are illustrative only
- Hospital and doctor names in the dataset are randomly generated, so hospital-level and doctor-level rankings are not meaningful and are not included

## Author
Abinayaa . J | linkedin.com/in/abinaya-j-69549227b
