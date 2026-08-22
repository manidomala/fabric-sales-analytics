# Data

## Source Dataset

The project uses a sales transaction CSV file as the source dataset.

**File:** `sales_data.csv`

The dataset contains:

* **1,525 rows**
* **8 columns**

---

# Dataset Schema

| Column            | Description                           |
| ----------------- | ------------------------------------- |
| `OrderID`         | Unique identifier for the sales order |
| `OrderDate`       | Date of the sales order               |
| `CustomerName`    | Name of the customer                  |
| `Region`          | Sales region                          |
| `ProductCategory` | Product category                      |
| `Revenue`         | Revenue generated from the order      |
| `Quantity`        | Quantity sold                         |
| `Status`          | Status of the order                   |

---

# Data Types

| Column            | Data Type |
| ----------------- | --------- |
| `OrderID`         | Text      |
| `OrderDate`       | Date      |
| `CustomerName`    | Text      |
| `Region`          | Text      |
| `ProductCategory` | Text      |
| `Revenue`         | Decimal   |
| `Quantity`        | Integer   |
| `Status`          | Text      |

---

# Product Categories

The dataset contains:

* Electronics
* Furniture
* Office Supplies

---

# Regions

The dataset contains:

* North
* South
* East
* West

---

# Data Flow

```text
sales_data.csv
      │
      ▼
Fabric Data Factory
      │
      ▼
Bronze Layer
      │
      ▼
Silver Layer
      │
      ▼
Gold Layer
      │
      ▼
Fabric Warehouse
      │
      ▼
Power BI Semantic Model
```

---

# Data Processing

The raw dataset is processed through the following stages:

### Bronze

Raw ingested data.

### Silver

Cleaned and transformed data.

### Gold

Business-ready analytical data.

### Warehouse

Dimensional model consisting of:

* `fact_sales`
* `dim_customer`
* `dim_product`
* `dim_region`
* `dim_date`
