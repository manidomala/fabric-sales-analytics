# Microsoft Fabric Sales Analytics

An end-to-end **sales data engineering and analytics project** built using **Microsoft Fabric**, implementing Medallion Architecture with PySpark and Delta Lake, loading curated data into a Fabric Warehouse, and delivering business insights through a Power BI Semantic Model and interactive dashboard.

---

## 📌 Project Overview

This project demonstrates a complete modern data analytics workflow, starting from raw sales CSV data and ending with an interactive Power BI dashboard.

The solution uses Microsoft Fabric to:

* Ingest raw sales data
* Store data in a Fabric Lakehouse
* Implement Bronze, Silver, and Gold layers
* Transform data using PySpark
* Store processed data using Delta Lake
* Load curated data into a Fabric Warehouse
* Implement a dimensional star schema
* Build a Power BI Semantic Model
* Create DAX measures
* Develop an interactive Sales Analytics dashboard

---

# 🔄 End-to-End Architecture

```text
                       Sales CSV
                           │
                           ▼
                ┌────────────────────┐
                │  Fabric Data       │
                │  Factory Pipeline   │
                └─────────┬──────────┘
                          │
                          ▼
                ┌────────────────────┐
                │     Lakehouse      │
                │                    │
                │ Bronze → Silver    │
                │        → Gold      │
                └─────────┬──────────┘
                          │
                          ▼
                ┌────────────────────┐
                │ Fabric Warehouse   │
                │                    │
                │ Fact + Dimensions  │
                └─────────┬──────────┘
                          │
                          ▼
                ┌────────────────────┐
                │ Power BI Semantic  │
                │ Model              │
                │                    │
                │ Star Schema + DAX  │
                └─────────┬──────────┘
                          │
                          ▼
                ┌────────────────────┐
                │ Power BI Dashboard │
                │ Sales Analytics    │
                └────────────────────┘
```

---

# 🛠️ Technologies Used

* Microsoft Fabric
* OneLake
* Fabric Data Factory
* Fabric Lakehouse
* Delta Lake
* PySpark
* Spark SQL
* Fabric Warehouse
* SQL
* Power BI
* Power BI Semantic Model
* DAX
* Medallion Architecture
* Star Schema

---

# 📂 Source Dataset

The project starts with a sales transaction CSV dataset.

**Source file:** `sales_data.csv`

The dataset contains:

* **1,525 records**
* **8 columns**

### Dataset Schema

| Column            | Description                      |
| ----------------- | -------------------------------- |
| `OrderID`         | Unique order identifier          |
| `OrderDate`       | Date of the order                |
| `CustomerName`    | Customer name                    |
| `Region`          | Sales region                     |
| `ProductCategory` | Product category                 |
| `Revenue`         | Revenue generated from the order |
| `Quantity`        | Quantity sold                    |
| `Status`          | Order status                     |

### Product Categories

* Electronics
* Furniture
* Office Supplies

### Regions

* North
* South
* East
* West

---

# 🏭 Data Engineering

## Microsoft Fabric Pipeline

The data ingestion and transformation process is orchestrated using a Microsoft Fabric Data Factory pipeline.

![Fabric Pipeline](screenshots/pipeline.png)

### Pipeline Name

`pl_ingest_sales`

### Pipeline Flow

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

The pipeline performs the following operations:

1. Ingests the source CSV
2. Loads the data into Delta format
3. Transforms Bronze data into Silver
4. Creates the Gold layer
5. Loads curated data into the Fabric Warehouse

Detailed pipeline documentation:

[`pipeline/pipeline-documentation.md`](pipeline/pipeline-documentation.md)

---

# 🏞️ Lakehouse

The project uses a Microsoft Fabric Lakehouse as the data engineering layer.

![Fabric Lakehouse](screenshots/lakehouse.png)

The Lakehouse follows the **Medallion Architecture**.

```text
Raw Sales Data
      │
      ▼
┌───────────────┐
│ Bronze Layer  │
│ Raw Data      │
└───────┬───────┘
        │
     PySpark
        │
        ▼
┌───────────────┐
│ Silver Layer  │
│ Cleaned Data  │
└───────┬───────┘
        │
     PySpark
        │
        ▼
┌───────────────┐
│  Gold Layer   │
│ Curated Data  │
└───────────────┘
```

## 🥉 Bronze Layer

The Bronze layer stores the raw ingested data.

### Purpose

* Preserve source data
* Maintain the original structure
* Provide a reliable starting point for transformations
* Support reprocessing

---

## 🥈 Silver Layer

The Silver layer contains cleaned and transformed data.

Typical transformations include:

* Data cleaning
* Data type standardization
* Date transformation
* Data validation
* Duplicate handling
* Column transformations

PySpark is used for the transformation process.

---

## 🥇 Gold Layer

The Gold layer contains curated and business-ready data.

The Gold layer prepares the data for downstream analytical consumption and warehouse loading.

---

# ⚡ PySpark

PySpark is used for transformations between the Lakehouse layers.

### Processing Flow

```text
Bronze
  │
  ▼
Read Data
  │
  ▼
Clean Data
  │
  ▼
Validate Data
  │
  ▼
Transform Columns
  │
  ▼
Silver
  │
  ▼
Gold
```

PySpark integrates with Microsoft Fabric, Apache Spark, and Delta Lake to support scalable data processing.

---

# 🏢 Fabric Warehouse

The curated data is loaded into a Microsoft Fabric Warehouse for analytical querying.

![Fabric Warehouse](screenshots/warehouse.png)

The Warehouse follows a dimensional modeling approach.

### Warehouse Tables

```text
analytics
│
├── fact_sales
├── dim_customer
├── dim_product
├── dim_region
└── dim_date
```

---

# ⭐ Star Schema

The Power BI Semantic Model uses a star schema.

![Star Schema](screenshots/star-schema.png)

```text
                    dim_customer
                         │
                         │
                         ▼
dim_product ─────── fact_sales ─────── dim_region
                         ▲
                         │
                         │
                     dim_date
```

The `fact_sales` table contains measurable business events, while dimension tables provide descriptive attributes used for filtering and analysis.

---

# 📊 Fact Table

## `fact_sales`

The fact table stores sales transaction data.

| Column           | Description                   |
| ---------------- | ----------------------------- |
| `order_id`       | Unique order identifier       |
| `order_date_key` | Foreign key to `dim_date`     |
| `customer_key`   | Foreign key to `dim_customer` |
| `product_key`    | Foreign key to `dim_product`  |
| `region_key`     | Foreign key to `dim_region`   |
| `quantity`       | Quantity sold                 |
| `revenue`        | Revenue in local currency     |
| `revenue_usd`    | Revenue converted to USD      |

---

# 📐 Dimension Tables

## `dim_customer`

Contains customer information.

| Column          | Description            |
| --------------- | ---------------------- |
| `customer_key`  | Surrogate customer key |
| `customer_name` | Customer name          |
| `region`        | Customer region        |

---

## `dim_product`

Contains product information.

| Column             | Description           |
| ------------------ | --------------------- |
| `product_key`      | Surrogate product key |
| `product_category` | Product category      |

---

## `dim_region`

Contains region information.

| Column       | Description          |
| ------------ | -------------------- |
| `region_key` | Surrogate region key |
| `region`     | Region name          |

---

## `dim_date`

Contains calendar attributes.

| Column           | Description        |
| ---------------- | ------------------ |
| `order_date_key` | Date surrogate key |
| `full_date`      | Full date          |
| `year`           | Year               |
| `quarter`        | Quarter            |
| `month_number`   | Month number       |
| `month_name`     | Month name         |

---

# 🔗 Relationships

The semantic model uses one-to-many relationships.

```text
dim_customer  1 ───────── * fact_sales

dim_product   1 ───────── * fact_sales

dim_region    1 ───────── * fact_sales

dim_date      1 ───────── * fact_sales
```

The dimension tables provide filtering context for the fact table.

---

# 📈 Power BI

The Power BI report is built using the Fabric Semantic Model.

![Power BI Dashboard](screenshots/dashboard.png)

The model contains:

* Fact table
* Dimension tables
* Relationships
* DAX measures
* Time-based attributes
* Business KPIs

Detailed Power BI documentation:

[`powerbi/powerbi-documentation.md`](powerbi/powerbi-documentation.md)

---

# 🧮 DAX Measures

## Total Customers

```DAX
Total Customers =
DISTINCTCOUNT(fact_sales[customer_key])
```

## Total Orders

```DAX
Total Orders =
DISTINCTCOUNT(fact_sales[order_id])
```

## Total Quantity

```DAX
Total Quantity =
SUM(fact_sales[quantity])
```

## Total Revenue

```DAX
Total Revenue =
SUM(fact_sales[revenue])
```

## Total Revenue USD

```DAX
Total Revenue USD =
SUM(fact_sales[revenue_usd])
```

## Additional Measures

* Avg Order Value
* Avg Quantity per Order
* Avg Revenue per Order
* Customer Rank

---

# 📊 Sales Analytics Dashboard

The final Power BI report provides an interactive overview of sales performance.

### KPI Cards

* Total Revenue
* Total Revenue (USD)
* Total Orders
* Total Quantity
* Average Revenue per Order
* Average Quantity per Order

### Slicers

* Date
* Month
* Quarter
* Year

### Visualizations

* Monthly Revenue & Orders Trend
* Orders by Product Category
* Revenue by Region
* Orders by Region
* Revenue by Product Category
* Quantity by Product Category
* Top 10 Customers by Revenue

---

# 📌 Dashboard KPIs

The current unfiltered dashboard displays approximately:

| KPI                    |    Value |
| ---------------------- | -------: |
| Total Revenue          |  ₹13.64M |
| Total Revenue (USD)    | $164.38K |
| Total Orders           |    1,250 |
| Total Quantity         |      10K |
| Avg Revenue per Order  |  ₹10.91K |
| Avg Quantity per Order |        8 |

---

# 🔍 Business Analysis

The dashboard enables analysis of:

### Revenue Performance

* Overall revenue
* Monthly revenue trends
* Revenue by region
* Revenue by product category

### Order Performance

* Total orders
* Monthly order trends
* Orders by region
* Orders by product category

### Product Performance

* Revenue by product category
* Orders by product category
* Quantity by product category

### Customer Performance

* Total customers
* Top customers by revenue
* Customer ranking

### Time Analysis

* Monthly analysis
* Quarterly analysis
* Yearly analysis

---

# 🧠 Data Modeling Approach

The project follows dimensional modeling principles.

### Fact

`fact_sales`

Stores measurable transactional data.

### Dimensions

* `dim_customer`
* `dim_product`
* `dim_region`
* `dim_date`

The star schema provides:

* Clear separation between facts and dimensions
* Simple filtering
* Efficient aggregation
* Better analytical usability
* Scalable Power BI modeling

---

# 🚀 Complete Project Flow

```text
                    SOURCE
                      │
                      ▼
                 Sales CSV
                      │
                      ▼
            Fabric Data Factory
                      │
                      ▼
                  LAKEHOUSE
                      │
           ┌──────────┼──────────┐
           ▼          ▼          ▼
        Bronze     Silver      Gold
           │          │          │
           └──────────┼──────────┘
                      │
                      ▼
             Fabric Warehouse
                      │
                      ▼
                 Star Schema
                      │
                      ▼
            Power BI Semantic Model
                      │
                      ▼
                    DAX
                      │
                      ▼
             Power BI Dashboard
```

---

# 📁 Repository Structure

```text
fabric-sales-analytics/
│
├── README.md
│
├── pipeline/
│   └── pipeline-documentation.md
│
├── powerbi/
│   └── powerbi-documentation.md
│
├── screenshots/
│   ├── pipeline.png
│   ├── lakehouse.png
│   ├── warehouse.png
│   ├── star-schema.png
│   └── dashboard.png
│
└── data/
    └── README.md
```

# 📚 Key Learning Outcomes

This project demonstrates hands-on experience with:

* Microsoft Fabric
* OneLake
* Fabric Data Factory
* Data ingestion
* Pipeline orchestration
* Lakehouse architecture
* Medallion Architecture
* Bronze/Silver/Gold layers
* Delta Lake
* PySpark
* Spark SQL
* Data transformation
* Data warehouse design
* Dimensional modeling
* Fact and dimension tables
* Star schema
* Surrogate keys
* Power BI Semantic Models
* DAX
* Data visualization
* Business intelligence

---

# 🔮 Future Improvements

Potential improvements include:

* Incremental data loading
* Pipeline parameterization
* Automated data quality checks
* Slowly Changing Dimensions
* Power BI Row-Level Security
* Pipeline monitoring and alerting
* Additional business KPIs
* Semantic model optimization
* CI/CD for Fabric artifacts
* Automated testing for transformations

---

# 👨‍💻 Conclusion

This project demonstrates an end-to-end modern data analytics solution using Microsoft Fabric.

Raw sales data is ingested through a Fabric Data Factory pipeline, processed through a Medallion Architecture using PySpark and Delta Lake, loaded into a Fabric Warehouse using dimensional modeling, and consumed through a Power BI Semantic Model and interactive dashboard.

The project combines:

**Data Engineering + Data Warehousing + Data Modeling + DAX + Business Intelligence**

into a single end-to-end sales analytics solution.

---

## 🧰 Tech Stack

```text
Microsoft Fabric
│
├── OneLake
├── Data Factory
├── Lakehouse
├── Delta Lake
├── PySpark
├── Spark SQL
└── Warehouse
        │
        ▼
    Power BI
        │
        ├── Semantic Model
        ├── Star Schema
        ├── DAX
        └── Dashboard
```

---

⭐ If you find this project useful, feel free to star the repository.
