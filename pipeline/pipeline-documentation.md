# Fabric Sales Ingestion Pipeline

## Overview

The `pl_ingest_sales` pipeline orchestrates the ingestion and transformation of sales data from the source CSV into the analytical warehouse.

---

## Pipeline

![Fabric Pipeline](../screenshots/pipeline.png)

### Pipeline Name

`pl_ingest_sales`

---

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

---

# Pipeline Activities

## 1. CopyRawSalesCSV

Copies the source sales CSV into the Microsoft Fabric environment.

This is the initial data ingestion step.

---

## 2. CopyDataToDelta

Loads the ingested data into Delta format in the Fabric Lakehouse.

Delta Lake provides reliable and transactional storage for the data engineering workflow.

---

## 3. TransformBronzeToSilver

A PySpark notebook processes the Bronze data and creates the Silver layer.

### Transformations

* Data cleaning
* Data type standardization
* Data validation
* Column transformations
* Duplicate handling
* Date transformations

---

## 4. BuildGoldLayer

Creates curated and business-ready data in the Gold layer.

The Gold layer is prepared for downstream analytical consumption.

---

## 5. TransformSilverToWarehouse

Loads/transforms the curated data into the Fabric Warehouse.

The Warehouse contains the dimensional model used by the Power BI Semantic Model.

---

# Pipeline Architecture

```text
Source CSV
    │
    ▼
Fabric Data Factory
    │
    ▼
Lakehouse Bronze
    │
    ▼
PySpark Transformation
    │
    ▼
Lakehouse Silver
    │
    ▼
PySpark Transformation
    │
    ▼
Lakehouse Gold
    │
    ▼
Fabric Warehouse
    │
    ▼
Power BI
```

---

# Technologies

* Microsoft Fabric Data Factory
* Fabric Lakehouse
* Delta Lake
* PySpark
* Fabric Warehouse
* Power BI

---

# Purpose

The pipeline provides a repeatable workflow for moving data from the source system through the data engineering layers and into the analytical warehouse.

This separates:

* Data ingestion
* Data transformation
* Data curation
* Analytical storage
* Reporting
