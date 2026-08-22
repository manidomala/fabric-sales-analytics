# Power BI Documentation

## Overview

The analytical model for this project is created in **Microsoft Fabric** using a Fabric Semantic Model.

The report itself is developed in **Power BI Desktop** by connecting to the Fabric Semantic Model.

## Reporting Architecture

```text
Fabric Warehouse
       │
       ▼
Fabric Semantic Model
       │
       │ Live Connection
       ▼
Power BI Desktop
       │
       ▼
Sales Analytics Report
       │
       ▼
Microsoft Fabric
       │
       ▼
Sales Report App
```

## Semantic Model

The semantic model follows a star schema.

### Fact Table

- `fact_sales`

### Dimension Tables

- `dim_customer`
- `dim_product`
- `dim_region`
- `dim_date`

### Relationships

```text
dim_customer  1 ───────── * fact_sales

dim_product   1 ───────── * fact_sales

dim_region    1 ───────── * fact_sales

dim_date      1 ───────── * fact_sales
```

## DAX Measures

### Total Customers

```DAX
Total Customers =
DISTINCTCOUNT(fact_sales[customer_key])
```

### Total Orders

```DAX
Total Orders =
DISTINCTCOUNT(fact_sales[order_id])
```

### Total Quantity

```DAX
Total Quantity =
SUM(fact_sales[quantity])
```

### Total Revenue

```DAX
Total Revenue =
SUM(fact_sales[revenue])
```

### Total Revenue USD

```DAX
Total Revenue USD =
SUM(fact_sales[revenue_usd])
```

## Additional Measures

- Avg Order Value
- Avg Quantity per Order
- Avg Revenue per Order
- Customer Rank

## Report Development

The report is developed in Power BI Desktop using the Fabric Semantic Model.

The report includes:

- KPI cards
- Date slicers
- Month slicers
- Quarter slicers
- Year slicers
- Monthly Revenue & Orders Trend
- Revenue by Region
- Orders by Region
- Revenue by Product Category
- Quantity by Product Category
- Top 10 Customers by Revenue

## Publishing

After development in Power BI Desktop, the completed report is published to Microsoft Fabric.

The published report is then made available through the **Sales Report** Microsoft Fabric App.

> Access to the Fabric App requires appropriate Microsoft Fabric / Power BI permissions.
