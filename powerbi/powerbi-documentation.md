# Power BI Documentation

## Overview

The Power BI report is built on top of the curated data stored in the Fabric Warehouse.

The Power BI Semantic Model follows a **star schema** consisting of one fact table and four dimension tables.

---

# Semantic Model

## Fact Table

* `fact_sales`

## Dimension Tables

* `dim_customer`
* `dim_product`
* `dim_region`
* `dim_date`

---

# Star Schema

![Star Schema](../screenshots/star-schema.png)

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

---

# Relationships

```text
dim_customer  1 ───── * fact_sales

dim_product   1 ───── * fact_sales

dim_region    1 ───── * fact_sales

dim_date      1 ───── * fact_sales
```

The dimension tables are on the `1` side and `fact_sales` is on the `*` side.

---

# DAX Measures

## Total Customers

```DAX
Total Customers =
DISTINCTCOUNT(fact_sales[customer_key])
```

Counts the unique customers.

---

## Total Orders

```DAX
Total Orders =
DISTINCTCOUNT(fact_sales[order_id])
```

Counts the unique orders.

---

## Total Quantity

```DAX
Total Quantity =
SUM(fact_sales[quantity])
```

Calculates total quantity sold.

---

## Total Revenue

```DAX
Total Revenue =
SUM(fact_sales[revenue])
```

Calculates total revenue.

---

## Total Revenue USD

```DAX
Total Revenue USD =
SUM(fact_sales[revenue_usd])
```

Calculates total revenue converted to USD.

---

# Additional Measures

The report also contains:

* Avg Order Value
* Avg Quantity per Order
* Avg Revenue per Order
* Customer Rank

---

# Dashboard

![Power BI Dashboard](../screenshots/dashboard.png)

The Power BI report provides an interactive Sales Analytics Overview.

---

# KPI Cards

The dashboard contains:

* Total Revenue
* Total Revenue USD
* Total Orders
* Total Quantity
* Avg Revenue per Order
* Avg Quantity per Order

---

# Slicers

The report contains slicers for:

* Date
* Month
* Quarter
* Year

These slicers allow users to perform time-based analysis.

---

# Visualizations

## Monthly Revenue & Orders

A combination chart showing revenue and order trends over time.

---

## Orders by Product Category

Shows the distribution of orders across product categories.

---

## Revenue by Region

Compares revenue across different regions.

---

## Orders by Region

Compares order volume across different regions.

---

## Revenue by Product Category

Shows revenue contribution by product category.

---

## Quantity by Product Category

Shows quantity distribution across product categories.

---

## Top 10 Customers by Revenue

Ranks customers based on revenue contribution.

---

# Business Questions Answered

The dashboard helps answer:

* What is the total revenue?
* How many orders were placed?
* How many customers made purchases?
* Which product categories generate the most revenue?
* Which regions generate the most revenue?
* How do revenue and orders change over time?
* Who are the top customers by revenue?
* What is the average revenue per order?
* What is the average quantity per order?

---

# Modeling Principles

The semantic model follows these principles:

* Star schema
* Fact and dimension separation
* One-to-many relationships
* Surrogate keys
* Centralized DAX measures
* Dedicated date dimension
* Business-friendly analytical model
