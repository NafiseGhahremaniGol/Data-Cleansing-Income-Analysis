# Data-Cleansing-Income-Analysis
Data Cleansing and Salary Analysis using Excel (Power Query)
# Data Cleansing & Salary Analysis — Excel + Power Query

**Project Summary**
This project demonstrates a full **data cleansing workflow** using **Microsoft Excel (Power Query)**.
The objective was to transform raw salary data into a reliable dataset to calculate **average minimum** and **average maximum salaries** based on:

* Role Type
* Company Size
* State

---

## Requirements

* Microsoft Excel (with Power Query support)
* Access to the raw dataset (`Uncleaned Data & State File`)
---

## Step-by-Step Data Cleansing (Power Query UI)

1. Import `income_raw.xlsx` → Data → Get Data → From Workbook.
2. Apply the following steps in **Power Query Editor** (rename each step for clarity):

   * **Remove Duplicates** → Home → Remove Rows → Remove Duplicates.
   * **Filter Invalid Company Size** → Filter negative and `Unknown` values.
   * **Split Salary** → Use delimiter `-` to create `Sal Min` and `Sal Max`.
   * **Convert & Scale Salary** → Convert to numbers and multiply by 1000:

     ```powerquery
     = Number.FromText(Text.Remove([Salary.Min.Text], {"$","k","K",","})) * 1000
     ```
   * **Split Location** → Extract State into a separate column, then Trim.
   * **Create Role Type (Custom Column)**:

     ```powerquery
     = if Text.Contains(Text.Lower([Job Title]), "data scientist") then "Data Scientist"
       else if Text.Contains(Text.Lower([Job Title]), "data analyst") then "Data Analyst"
       else if Text.Contains(Text.Lower([Job Title]), "data engineer") then "Data Engineer"
       else if Text.Contains(Text.Lower([Job Title]), "machine learning") then "Machine Learning Engineer"
       else "Other"
     ```
   * **Location Correction (Mapping Example)**:

     ```powerquery
     = if [Location] = "California" then "CA"
       else if [Location] = "New Jersey" then "NJ"
       else if [Location] = "Remote" then "Other"
       else if [Location] = "United States" then "Other"
       else if [Location] = "Texas" then "TX"
       else if [Location] = "Utah" then "UT"
       else [Location]
     ```
   * **MergeQueries** → On State Name of State File
   * **Aggregation** → Group By `Role Type`, `Size`, `State`:

     * `AvgMinSalary` = Average of Min Salary
     * `AvgMaxSalary` = Average of Max Salary
3. Close & Load → Save results into `data/cleaned/Cleaned Data.xlsx` or export as `reports/analysis_by_role_size_state.csv`.

---

## Key Data Cleansing Principles

* Always preserve the **raw dataset** for reproducibility.
* Use **clear step names** in Applied Steps for traceability.
* Document why rows were removed or values adjusted.
* Flag unusual values before deleting (avoid over-cleaning).
* Sanity check results (salary ranges should make sense).



If you have any questions or suggestions, feel free to open an issue or connect with me on LinkedIn.
