#NYC Jobs Data Engineering Challenge

---------------------------------------------------
1. ##Overview:

- This solution implements a scalable PySpark-based data processing pipeline to analyze NYC job postings and derive the required KPIs.
- The design focuses on:
  - Clean modular preprocessing
  - Data normalization
  - Feature engineering
  - Data quality validation
  - Performance optimization
  - Production-readiness

---------------------------------------------------
2. ##Assumptions:

1) Salary Normalization:
- The dataset contains mixed salary frequency values (Annual, Hourly, Daily).
- Salary conversion logic:
  - Hourly salaries converted to annual using 2080 hours per year
  - Daily salaries converted to annual using 260 working days per year
  - Annual salaries kept as-is
- Invalid salary ranges where salary_to < salary_from were removed.
- A derived column mid_salary was created as the average of annual_salary_from and annual_salary_to.

2) Higher Degree Flag:
- If “Minimum Qualifications” contains keywords such as:
  - Master
  - PhD
  - Doctor
- Then higher_degree_flag = 1, otherwise 0.

3) Skill Extraction:
- From the “Preferred Skills” column, the following skills were identified using keyword matching:
  - Python
  - SQL
  - Spark
- Binary indicators were created:
  - has_python
  - has_sql
  - has_spark
- Note: Skill-based insights reflect the NYC dataset only.

4) Last 2 Years Logic:
  - The logic uses max(posting_year) from the dataset instead of system year.
  - This ensures reproducibility even if dataset is historical.

---------------------------------------------------
3. ##Data Processing:

- The preprocessing function performs:
  - Salary normalization to annual scale
  - Removal of invalid salary rows
  - Mid salary calculation
  - Posting date parsing
  - Posting year extraction
  - Feature engineering (education and skill flags)
  - Column name sanitization (removal of spaces and special characters)
  - DataFrame caching for optimization

- Processed data is written in Parquet format and partitioned by posting_year.

---------------------------------------------------
4. ##Feature Engineering Applied:

- Salary normalization
- Mid salary derivation
- Higher degree binary feature
- Skill indicator features
- Posting year extraction

---------------------------------------------------
5. ##KPIs Implemented:

- Top 10 job categories by number of postings
- Salary distribution per job category
- Correlation between higher degree and salary
- Highest salary job per agency
- Average salary per agency for the last 2 years in the dataset
- Highest paid skill combinations

---------------------------------------------------
6. ##Data Quality Checks:

- Record count validation
- Salary null checks
- Invalid salary filtering
- Posting year validation
- Basic aggregation unit test
- Assertions to ensure salary consistency

---------------------------------------------------
7. ##Performance Optimizations:

- DataFrame caching
- Efficient window function usage
- Repartition before write
- Partitioned Parquet output
- Restart and Run All validation to ensure deterministic execution

---------------------------------------------------
8. ##Challenges:

- Column name sanitization required before Parquet write
- Mixed salary frequency required careful normalization
- Dataset limited to NYC and does not represent the full US job market

---------------------------------------------------
9. ##Proposed Production Deployment:

- Orchestration using Apache Airflow
- Raw data stored in Data Lake (S3 or ADLS)
- Processed data stored as partitioned Parquet
- Compute using EMR, Databricks, or Spark cluster
- Monitoring using row count checks, schema drift detection, and anomaly alerts
- CI/CD using GitHub with automated validation tests

---------------------------------------------------
10. ##Conclusion:

- This solution provides a clean modular PySpark implementation with reproducible KPI generation, data validation mechanisms, performance optimizations, and production-oriented design.
- The notebook executes successfully after full restart and all KPIs are reproducible.

---------------------------------------------------
