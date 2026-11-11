# CareSync – Azure Databricks | Healthcare Data Transformation

## Overview

This repository contains the Azure Databricks implementation of CareSync, an end-to-end healthcare data engineering project.

The solution implements a Medallion Architecture to transform healthcare data from raw landing files into refined and analytics-ready datasets.

Azure Databricks handles the transformation layer, while Azure Data Factory manages ingestion orchestration and pipeline execution.

The solution is designed to demonstrate scalable, reusable, and metadata-driven data engineering practices using the Azure ecosystem.

## Architecture

```text
Azure Data Factory
       |
       v
Azure Data Lake Storage Gen2
       |
       v
Landing Zone
       |
       v
BRONZE LAYER
Raw data ingestion
       |
       v
SILVER LAYER
Data refinement
FULL / APPEND / MERGE
       |
       v
GOLD LAYER
Business transformations
       |
       v
Healthcare Analytics
```

## Medallion Architecture

### 1. Bronze Layer – Raw Data

The Bronze layer stores ingested source data.

Key implementation features:
- Reads CSV and Parquet files from the landing zone.
- Uses parameterized Databricks notebooks.
- Supports dynamic table processing.
- Adds landing and ingestion timestamps.
- Appends ingested records into Bronze tables.

Notebook:
- `landing_to_bronze.ipynb`

### 2. Silver Layer – Refined Data

The Silver layer processes Bronze data and prepares it for downstream analytical use.

Key implementation features:
- Reads data for a specific landing timestamp.
- Adds insert and update timestamps.
- Supports multiple load strategies:
  - FULL – Complete table replacement.
  - APPEND – Adds incoming records.
  - MERGE – Updates matching records and inserts new records.
- Supports configurable merge keys.
- Uses Delta Lake MERGE operations.

Notebook:
- `bronze_to_silver.ipynb`

### 3. Gold Layer – Analytics-Ready Data

The Gold layer contains business-oriented healthcare analytical datasets.

#### Patient 360

Notebook: `NB_patient_360.ipynb`

Creates a consolidated patient-level analytical view.

Includes:
- Patient demographic information.
- Age calculation.
- Registered hospital details.
- Insurance provider information.
- Total laboratory tests.
- Abnormal and critical test counts.
- Last test date.

#### Hospital Daily Summary

Notebook: `NB_hospital_daily_summary.ipynb`

Creates a hospital-level daily analytical summary.

Includes:
- Total laboratory tests.
- Unique patients tested.
- Active doctor count.
- Abnormal and critical test counts.
- Abnormal and critical test percentages.
- Previous observed test-volume comparison.
- Test-volume percentage change.

#### Laboratory Test Trends

Notebook: `NB_lab_test_trends.ipynb`

Creates a monthly analytical dataset for laboratory test trends.

Includes:
- Test type.
- Year and month.
- Total tests.
- Abnormal and critical test counts.
- Abnormal and critical test percentages.
- Previous month's abnormal rate.
- Monthly abnormal-rate change.
- Quarter and month information.

## Repository Structure

```text
caresyncadbrepo/
│
├── landing_to_bronze.ipynb
│
├── bronze_to_silver.ipynb
│
├── calender_table.ipynb
│
├── gold/
│   ├── NB_patient_360.ipynb
│   ├── NB_hospital_daily_summary.ipynb
│   ├── NB_lab_test_trends.ipynb
│   └── sttm_silver_to_gold.xlsx
│
└── README.md
```

## Technologies Used

- Azure Databricks
- Apache Spark
- PySpark
- Spark SQL
- Delta Lake
- Python
- SQL
- Azure Data Lake Storage Gen2
- Azure Data Factory

## Integration with Azure Data Factory

This repository is integrated with the CareSync Azure Data Factory implementation.

ADF invokes the Databricks notebooks and passes parameters such as:
- Table name
- Landing timestamp
- Load type
- Merge keys
- Source system

This approach enables reusable, metadata-driven transformations rather than maintaining separate hardcoded pipelines for every table.

## Related Repository

**Azure Data Factory – Orchestration Layer**

https://github.com/Srinivas-49/caresync_adf_repo

Both repositories form one integrated Azure Data Engineering solution.

## Author

Srinivas

GitHub: https://github.com/Srinivas-49
