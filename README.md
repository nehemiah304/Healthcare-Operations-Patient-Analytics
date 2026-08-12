# Healthcare-Operations-Patient-Analytics
This project performs exploratory data analysis (EDA) on hospital patient data using Python. Ten analytical questions were developed to uncover patterns in patient demographics, condition prevalence, billing behavior, insurance usage, length of stay, test outcomes, admission trends, and prescribing patterns.

The goal is to surface operational and cost patterns that could inform hospital resourcing, insurance strategy, and data-quality practices — while explicitly separating invalid billing records from the analysis rather than letting them distort the results.

## 📌 Objectives

Load and inspect the healthcare dataset to understand its structure and quality
Clean the data: standardize text fields, remove duplicates, fix date types, flag invalid values
Engineer a Length of Stay field from admission and discharge dates
Build a dedicated billing subset that excludes invalid (negative) billing amounts
Answer 10 business questions covering demographics, conditions, cost, insurance, admissions, and outcomes
Summarize findings into clear, actionable takeaways

## ❓ Business Questions

- What does the age distribution of patients look like?
- Which medical conditions are most common?
- How does average billing amount vary by medical condition?
- Which insurance providers are used most, and how does billing compare across them?
- Does admission type (Emergency, Urgent, Elective) affect length of stay?
- What proportion of test results come back Normal, Abnormal, or Inconclusive?
- Are certain medical conditions more likely to have Abnormal test results?
- How has the number of admissions changed over time?
- Which medications are prescribed most often, and does that vary by condition?
- Is there a relationship between patient age and billing amount?

## 📚 Dataset Description Source: 
healthcare_dataset.csv — 55,500 records, 15 columns. Columns: Name, Age, Gender, Blood Type, Medical Condition, Date of Admission, Doctor, Hospital, Insurance Provider, Billing Amount, Room Number, Admission Type, Discharge Date, Medication, Test Results. No missing values were present in any column at load.

## 🛠 Tools & Technologies Used

- Python
- Pandas — data loading, cleaning, aggregation
- NumPy — numerical operations
- Matplotlib & Seaborn — visualization

## 🧹 Data Cleaning and Preparation

- Standardized the Name column to consistent title case (source data mixed cases, e.g. "Bobby JacksOn" → "Bobby Jackson").
- Removed 534 duplicate rows, bringing the dataset from 55,500 to 54,966 records.
- Converted Date of Admission and Discharge Date from text to proper datetime types.
- Engineered a new Length of Stay column (days between admission and discharge).
- Flagged 106 records with negative Billing Amount values as data-entry errors (billing cannot be negative) and excluded them from all billing-related analysis, producing a separate     billing_df of 54,860 valid records used specifically for Q3, Q4, and Q10 — while the full 54,966-record dataset was retained for non-billing questions.

## 📈 Key Findings

### Age Distribution: 
Patients range from 13 to 89 years old, with a mean of 51.5 and median of 52  a fairly even spread across the age range rather than a skew toward any particular group.
### Most Common Conditions: 
Arthritis (9,218 patients), Diabetes (9,216), and Hypertension (9,151) lead, closely followed by Obesity (9,146), Cancer (9,140), and Asthma (9,095)  the six conditions are nearly evenly distributed.
### Average Billing by Condition: 
Obesity carries the highest average billing ($25,859), followed by Diabetes ($25,714) and Asthma ($25,685); Cancer has the lowest average billing ($25,206) but the full range across all six conditions spans only about $650, a narrow spread.
### Insurance Providers:
Blue Cross and Medicare are the most-used providers by patient volume; average billing amount is broadly similar across all providers, with no single insurer standing out on cost.
### Length of Stay by Admission Type: 
Emergency admissions average the longest stay (15.58 days), just ahead of Elective (15.51) and Urgent (15.40)  a real but modest difference of under two hours' equivalent across categories.
### Test Result Distribution:
Abnormal (18,437), Normal (18,331), and Inconclusive (18,198) are nearly perfectly split into thirds (33.6%, 33.4%, 33.1% respectively).
### Condition vs. Test Result:
No medical condition shows a meaningful skew toward Abnormal results — Abnormal rates range narrowly from 32.5% (Hypertension) to 34.2% (Arthritis) across all six conditions.
### Admissions Over Time: 
Admission volume holds fairly steady across the dataset's 2019–2024 span, with no major seasonal spikes visible in the monthly series.
Prescribed Medications: Aspirin, Ibuprofen, Paracetamol, Penicillin, and Lipitor are distributed fairly evenly, with no single medication dominating prescriptions.
### Age vs. Billing Correlation:
−0.00 no relationship. Older patients are not billed more than younger ones in this dataset.

## 📊 Business Insights

💵 Billing amount appears disconnected from clinical severity. Average billing spans only about $650 across all six medical conditions (from $25,206 for Cancer to $25,859 for Obesity), despite these conditions carrying very different real-world treatment costs. Combined with the near-zero age/billing correlation (Q10), this strongly suggests the Billing Amount field in this dataset does not reflect actual treatment cost or complexity — likely synthetic or randomly generated rather than clinically derived. Any downstream cost-modeling work built on this field should treat it with caution.

🩺 Test outcomes are statistically independent of diagnosis. Both the overall Normal/Abnormal/Inconclusive split (roughly a third each) and the condition-by-condition breakdown (Q7) show almost no variation — Abnormal rates cluster within a 1.7-point band across all six conditions. In a real clinical dataset, certain conditions would be expected to show test-result skew; the uniformity here is another signal pointing toward randomized or synthetic data generation rather than organic clinical outcomes.

🏥 Admission type has a real, if modest, effect on length of stay. Emergency admissions run about 0.18 days longer on average than Urgent admissions — small in absolute terms, but consistent with the intuition that emergency cases can involve more complex, harder-to-plan care. This is one of the few findings in the dataset that aligns with a real-world clinical expectation.

🧹 The negative-billing exclusion was a necessary and material cleaning step. 106 records (about 0.2% of the cleaned dataset) had impossible negative billing values. Excluding them rather than clipping or imputing them keeps the billing-based findings (Q3, Q4, Q10) honest, though it's a small enough share that it didn't materially change the overall billing patterns observed.

## 📉 Notable Trends

- The six medical conditions in the dataset are almost perfectly balanced in patient count (9,095–9,218), which is unusual for real-world clinical prevalence and reinforces that this dataset behaves more like a synthetic/benchmark dataset than an organic hospital extract.
- Admissions are evenly paced across 2019–2024 with no visible seasonality, again consistent with synthetic data generation rather than real seasonal healthcare demand patterns (e.g. flu season spikes).
- Every categorical breakdown examined — condition, insurance provider, admission type, test result, medication — shows values clustered tightly together rather than one category dominating, a consistent pattern across the whole analysis.

## 💡 Recommendations

- Prioritize preventive care for common conditions such as Arthritis, Diabetes, and Hypertension through regular screening, early detection, and ongoing patient monitoring.
- Strengthen chronic disease management by developing targeted follow-up programs for patients with recurring conditions, particularly Diabetes and Hypertension.
- Review billing data quality and investigate the 106 negative billing records to identify data-entry or system errors and prevent similar issues in future records.
- Improve billing analysis by incorporating additional factors such as treatment type, procedures, length of stay, and condition severity to better understand what drives healthcare costs.
- Monitor emergency admissions and investigate the factors contributing to differences in length of stay to identify opportunities for more efficient patient care.
- Use test-result patterns to support clinical monitoring, while investigating abnormal or inconclusive results to ensure appropriate follow-up and timely intervention.
- Maintain balanced medication monitoring by tracking prescription patterns, effectiveness, and patient outcomes rather than focusing only on prescription frequency.
- Analyze healthcare utilization over time to identify emerging trends in admissions, medical conditions, and treatments that may require additional resources.
- Develop more detailed patient segmentation using age, condition, admission type, and insurance provider to identify groups that may require different healthcare strategies.
