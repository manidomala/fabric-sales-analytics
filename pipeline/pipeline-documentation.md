# Microsoft Fabric Sales Analytics Pipeline

## Pipeline Name

`pl_ingest_sales`

## Overview

The Microsoft Fabric Data Factory pipeline orchestrates the movement and transformation of sales data from the source CSV through the Lakehouse and into the Fabric Warehouse.

## Pipeline Flow

```text
CopyRawSalesCSV
        │
        ▼
CopyDataToDelta
        │
        ▼
TransformBronzeToSilver
        │
        ▼
BuildGoldLayer
        │
        ▼
TransformSilverToWarehouse
```

## Pipeline Activities

### 1. CopyRawSalesCSV

Ingests the source `sales_data.csv` file into the Fabric environment.

### 2. CopyDataToDelta

Stores the ingested sales data in Delta format in the Lakehouse.

### 3. TransformBronzeToSilver

Uses PySpark to clean and standardize the Bronze data.

Typical processing includes:

- Data type standardization
- Date transformation
- Data validation
- Duplicate handling
- Column transformations

### 4. BuildGoldLayer

Creates curated, business-ready data for downstream analytical use.

### 5. TransformSilverToWarehouse

Loads the curated data into the Fabric Warehouse and prepares the dimensional model.

## Architecture

```text
Source CSV
    │
    ▼
Fabric Data Factory
    │
    ▼
Lakehouse
    │
    ├── Bronze
    ├── Silver
    └── Gold
    │
    ▼
Fabric Warehouse
    │
    ├── fact_sales
    ├── dim_customer
    ├── dim_product
    ├── dim_region
    └── dim_date
```

## Outcome

The pipeline provides an orchestrated path from raw source data to curated analytical data that can be consumed by the Fabric Semantic Model and Power BI Desktop.
