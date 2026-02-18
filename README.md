#NYC Jobs Data Engineering Challenge
===================================================
1. ##Overview:

1.1 This solution implements a scalable PySpark-based data processing pipeline to analyze NYC job postings and derive the required KPIs.
1.2 The design focuses on:
- Clean modular preprocessing
- Data normalization
- Feature engineering
- Data quality validation
- Performance optimization
- Production-readiness
---------------------------------------------------
2. ##Assumptions:

2.1 Salary Normalization:
2.1.1 The dataset contains mixed salary frequency values (Annual, Hourly, Daily).
2.1.2 Salary conversion logic:
- Hourly salaries converted to annual using 2080 hours per year
- Daily salaries converted to annual using 260 working days per year
- Annual salaries kept as-is
2.1.3 Invalid salary ranges where salary_to < salary_from were removed.
2.1.4 A derived column mid_salary was created as the average of annual_salary_from and annual_salary_to.

2.2 Higher Degree Flag:
2.2.1 If “Minimum Qualifications” contains keywords such as:
- Master
- PhD
- Doctor
2.2.2 Then higher_degree_flag = 1, otherwise 0.

2.3 Skill Extraction:
2.3.1 From the “Preferred Skills” column, the following skills were identified using keyword matching:
- Python
- SQL
- Spark
2.3.2 Binary indicators were created:
- has_python
- has_sql
- has_spark
2.3.3 Note: Skill-based insights reflect the NYC dataset only.

2.4 Last 2 Years Logic:
2.4.1 The logic uses max(posting_year) from the dataset instead of system year.
2.4.2 This ensures reproducibility even if dataset is historical.
----------------------------------------------------
3. ##Data Processing:

3.1 The preprocessing function performs:
3.1.1 Salary normalization to annual scale
3.1.2 Removal of invalid salary rows
3.1.3 Mid salary calculation
3.1.4 Posting date parsing
3.1.5 Posting year extraction
3.1.6 Feature engineering (education and skill flags)
3.1.7 Column name sanitization (removal of spaces and special characters)
3.1.8 DataFrame caching for optimization

3.2 Processed data is written in Parquet format and partitioned by posting_year.
----------------------------------------------------
4. ##Feature Engineering Applied:

4.1 Salary normalization
4.2 Mid salary derivation
4.3 Higher degree binary feature
4.4 Skill indicator features
4.5 Posting year extraction
----------------------------------------------------
5. ##KPIs Implemented:

5.1 Top 10 job categories by number of postings
5.2 Salary distribution per job category
5.3 Correlation between higher degree and salary
5.4 Highest salary job per agency
5.5 Average salary per agency for the last 2 years in the dataset
5.6 Highest paid skill combinations
----------------------------------------------------
6. ##Data Quality Checks:

6.1 Record count validation
6.2 Salary null checks
6.3 Invalid salary filtering
6.4 Posting year validation
6.5 Basic aggregation unit test
6.6 Assertions to ensure salary consistency
----------------------------------------------------
7. ##Performance Optimizations:

7.1 DataFrame caching
7.2 Efficient window function usage
7.3 Repartition before write
7.4 Partitioned Parquet output
7.5 Restart and Run All validation to ensure deterministic execution
----------------------------------------------------
8. ##Challenges:

8.1 Column name sanitization required before Parquet write
8.2 Mixed salary frequency required careful normalization
8.3 Dataset limited to NYC and does not represent the full US job market
----------------------------------------------------
9. ##Proposed Production Deployment:

9.1 Orchestration using Apache Airflow
9.2 Raw data stored in Data Lake (S3 or ADLS)
9.3 Processed data stored as partitioned Parquet
9.4 Compute using EMR, Databricks, or Spark cluster
9.5 Monitoring using row count checks, schema drift detection, and anomaly alerts
9.6 CI/CD using GitHub with automated validation tests
----------------------------------------------------
10. ##Conclusion:

10.1 This solution provides a clean modular PySpark implementation with reproducible KPI generation, data validation mechanisms, performance optimizations, and production-oriented design.
10.2 The notebook executes successfully after full restart and all KPIs are reproducible.
====================================================