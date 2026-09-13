# Global FMCG Commercial & Supply Chain Intelligence

![Power BI](https://img.shields.io/badge/Power%20BI-Analytics-yellow)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-blue)
![DAX](https://img.shields.io/badge/DAX-Data%20Modeling-orange)
![Dataset](https://img.shields.io/badge/Dataset-1.1M%20Records-green)

An end-to-end **Power BI commercial and supply-chain intelligence solution** built using 1.1 million FMCG transaction records across 3 years, 7 countries, 13 stores, 102 products, and 60 suppliers.

The project combines **Power Query, dimensional data modeling, DAX, time-series analysis, inventory analytics, supplier performance analysis, geographic analysis, forecasting, and interactive what-if scenario modeling** into a single executive-style business intelligence solution.

---

## Dashboard Preview

### Executive Overview

![Executive Overview](screenshots/ExecutiveOverview.png)

The executive dashboard provides a high-level view of commercial performance, profitability, inventory risk, and supply-chain indicators.

**Key questions answered:**

* How much revenue are we generating?
* Which countries and categories are performing best?
* Where are the largest inventory and stockout risks?
* How are contribution margins changing over time?
* Which areas require management attention?

---

## Business Problem

FMCG organizations operate across large numbers of products, stores, suppliers, channels, and geographic markets.

With millions of transaction records, it can be difficult for decision-makers to quickly identify:

* Revenue and sales trends
* High- and low-performing products
* Category and channel performance
* Inventory risk
* Stockout patterns
* Supplier performance differences
* Geographic opportunities
* Demand patterns
* Potential commercial scenarios

This project transforms raw transaction-level data into an interactive business intelligence solution designed to support **commercial, inventory, supplier, and operational decision-making**.

---

# Project Objectives

The dashboard was designed to:

1. Monitor overall commercial performance
2. Analyze sales by product, category, channel, country, and store
3. Evaluate estimated product contribution
4. Identify inventory and stockout risks
5. Compare supplier performance
6. Analyze geographic and store-level performance
7. Investigate demand patterns and seasonality
8. Generate short-term demand forecasts
9. Test hypothetical commercial and supply-chain scenarios

---

# Dataset

The dataset contains approximately **1.1 million transaction records** covering:

* **2021–2023**
* **7 countries**
* **9 cities**
* **13 stores**
* **4 sales channels**
* **102 SKUs**
* **17 subcategories**
* **6 brands**
* **60 suppliers**

### Dataset characteristics

| Metric            |     Value |
| ----------------- | --------: |
| Records           | 1,100,000 |
| Columns           |        33 |
| Time Period       | 2021–2023 |
| Countries         |         7 |
| Cities            |         9 |
| Stores            |        13 |
| SKUs              |       102 |
| Suppliers         |        60 |
| Sales Channels    |         4 |
| Missing Values    |         0 |
| Duplicate Rows    |         0 |
| Promotion Records |    88,257 |
| Stockout Records  |    33,114 |

The dataset is used for portfolio and analytical demonstration purposes.

---

# Data Preparation

The raw CSV was imported into **Power BI Power Query** and prepared for analytical modeling.

### Data preparation steps

* Imported the transaction dataset
* Validated column data types
* Checked for missing values
* Checked for duplicate records
* Validated sales calculations
* Created reusable dimension tables
* Standardized categorical fields
* Prepared a dedicated date dimension
* Separated descriptive attributes from transactional measures

The resulting model uses a **star schema** rather than placing the entire dataset into one flat table.

---

# Data Model

The Power BI model contains a central fact table supported by multiple dimension tables.

### Fact table

**FactSales**

Contains transaction-level measures including:

* Units sold
* List price
* Discount
* Gross sales
* Net sales
* Stock on hand
* Purchase cost
* Lead time
* Stockout flag
* Promotion flag
* Weather variables

### Dimension tables

**DimDate**

* Date
* Year
* Month
* Quarter
* Week
* Weekday

**DimProduct**

* SKU
* Product name
* Category
* Subcategory
* Brand

**DimStore**

* Store
* Country
* City
* Channel
* Latitude
* Longitude

**DimSupplier**

* Supplier ID

All dimension-to-fact relationships use a **single-direction one-to-many relationship**.

---

# Key DAX Measures

The model uses DAX measures to create reusable business metrics.

### Net Sales

```DAX
Net Sales =
SUM(FactSales[net_sales])
```

### Total Units

```DAX
Total Units =
SUM(FactSales[units_sold])
```

### Estimated Product Cost

```DAX
Estimated Product Cost =
SUMX(
    FactSales,
    FactSales[units_sold] * FactSales[purchase_cost]
)
```

### Estimated Contribution

```DAX
Estimated Contribution =
[Net Sales] - [Estimated Product Cost]
```

### Estimated Contribution Margin

```DAX
Estimated Contribution Margin % =
DIVIDE(
    [Estimated Contribution],
    [Net Sales]
)
```

### Stockout Rate

```DAX
Stockout Rate =
DIVIDE(
    [Stockout Records],
    COUNTROWS(FactSales)
)
```

### Year-over-Year Growth

```DAX
Net Sales YoY % =
DIVIDE(
    [Net Sales] - [Net Sales LY],
    [Net Sales LY]
)
```

### Rolling 30-Day Sales

```DAX
Rolling 30 Day Sales =
CALCULATE(
    [Net Sales],
    DATESINPERIOD(
        DimDate[Date],
        MAX(DimDate[Date]),
        -30,
        DAY
    )
)
```

---

# Dashboard Pages

## 01 — Executive Overview

Provides an executive summary of commercial and operational performance.

### Key metrics

* Net Sales
* Estimated Contribution
* Contribution Margin
* Total Units
* Stockout Rate
* Average Lead Time

### Analysis

* Sales trend
* Country performance
* Category performance
* Inventory/stockout risk
* Commercial performance by category

---

## 02 — Sales & Commercial

Focuses on commercial performance and pricing.

### Analysis

* Sales by category
* Sales by channel
* Top products by revenue
* Top products by contribution
* Promotional vs non-promotional performance
* Pricing vs volume relationships

### Key questions

* Which products generate the most revenue?
* Which products generate the most contribution?
* Which channels perform best?
* How does promotional activity relate to sales volume?

---

## 03 — Inventory & Stockouts

Focuses on inventory efficiency and availability risk.

### Analysis

* Inventory trends
* Stockout rate by category
* Stockout rate by store
* Inventory vs demand
* SKU-level inventory risk
* Stockout patterns

The dashboard allows users to identify products or locations where inventory levels and demand may require further investigation.

---

## 04 — Supplier Performance

Analyzes supplier-level commercial and operational metrics.

### Metrics

* Supplier sales
* Units
* Average purchase cost
* Average lead time
* Stockout rate
* Estimated contribution

Supplier performance is analyzed at the transaction level because suppliers are not modeled as permanently assigned to individual SKUs in the source data.

---

## 05 — Store & Geographic Performance

Analyzes performance across the organization's geographic footprint.

### Analysis

* Country performance
* City performance
* Store performance
* Sales channels
* Geographic sales distribution
* Store contribution margin
* Store stockout rates

Users can drill from:

**Country → City → Store**

---

## 06 — Demand & Forecasting

Focuses on demand behavior and short-term forecasting.

### Analysis

* Daily demand
* Rolling 30-day sales
* Monthly seasonality
* Weekday behavior
* Holiday vs non-holiday performance
* Weather associations
* Short-term demand forecast

Power BI's forecasting functionality is used as an analytical aid rather than as a standalone production forecasting system.

---

## 07 — Scenario Simulator

An interactive decision-support page allowing users to test hypothetical business assumptions.

### Scenario parameters

* Additional discount
* Volume uplift
* Lead-time improvement
* Inventory increase

Users can adjust the parameters and compare:

**Current → Scenario**

for:

* Revenue
* Contribution
* Lead Time
* Inventory

### Important methodology note

The scenario simulator is **illustrative rather than predictive**.

The volume uplift assumption is user-defined and is not presented as a causal estimate derived from historical data.

---

# Drill-Through Analysis

The dashboard also includes supporting drill-through pages.

### Product Detail

Users can investigate an individual SKU across:

* Sales
* Units
* Contribution
* Inventory
* Stockouts
* Countries
* Channels
* Time

### Store Detail

Users can investigate:

* Store sales
* Units
* Contribution
* Stockouts
* Inventory
* Category performance
* Product performance

### Supplier Detail

Users can investigate:

* Supplier sales
* Units
* Purchase cost
* Lead time
* Stockout rate
* Contribution
* Product/category activity

---

# Business Intelligence Techniques Demonstrated

This project demonstrates:

### Data Preparation

* Power Query
* Data type validation
* Data quality checks
* Query transformations
* Dimension extraction

### Data Modeling

* Star schema
* Fact and dimension tables
* Relationships
* Date dimension
* Dimensional filtering

### DAX

* Aggregations
* `SUMX`
* `CALCULATE`
* `DIVIDE`
* `RANKX`
* Time intelligence
* Year-over-year analysis
* Rolling periods
* Dynamic scenario measures

### Visualization

* KPI cards
* Line charts
* Bar charts
* Scatter plots
* Tables
* Matrix visuals
* Maps
* Tooltips
* Drill-through
* Interactive slicers

### Analytics

* Trend analysis
* Contribution analysis
* Inventory analysis
* Stockout analysis
* Supplier benchmarking
* Geographic analysis
* Seasonality
* Forecasting
* What-if scenario analysis

---

# Data Quality & Modeling Considerations

Several validation checks were performed before building the dashboard.

### Data quality

* 0 missing values
* 0 duplicate rows
* 1.1 million transaction records
* Stable SKU/product mappings
* Stable store/location mappings
* Validated gross sales calculations
* Validated net sales calculations

### Important modeling consideration

The source data does not contain every possible **Store × SKU × Date** combination.

Therefore, missing combinations are **not automatically interpreted as zero demand**.

This prevents the analysis from incorrectly treating an absent transaction as a confirmed zero-sales day.

### Stockout methodology

Stockout analysis uses the provided `stock_out_flag` rather than assuming that zero units sold automatically means a stockout.

---

# Contribution Methodology

The dataset does not provide complete operating expenses.

Therefore, the dashboard does **not** claim to calculate full business profit.

Instead, it calculates an estimated product-level contribution:

```text
Estimated Contribution
=
Net Sales
-
(Unit Volume × Purchase Cost)
```

This provides a useful product-level commercial contribution indicator while avoiding the claim that the metric represents complete net profit.

---

# Scenario Modeling Methodology

The scenario simulator is based on user-selected assumptions.

For example:

```text
Scenario Units
=
Current Units × (1 + Volume Uplift)
```

and:

```text
Scenario Revenue
=
Scenario Units × Scenario Average Selling Price
```

The model is intended for **interactive decision support and sensitivity analysis**, not causal inference or production forecasting.

---

# Limitations

This project is designed as a portfolio/business intelligence demonstration and has several limitations.

### Synthetic dataset

The dataset is not a representation of an actual company's commercial operations.

### No operating expenses

Contribution analysis does not include:

* Labor
* Rent
* Logistics overhead
* Marketing expenses
* Taxes
* Other operating expenses

### Forecasting

Forecasting is intended for exploratory analysis and should not be treated as a production demand-planning model.

### Scenario analysis

Scenario outputs depend on user-defined assumptions and should not be interpreted as statistically validated causal predictions.

### Supplier relationships

Supplier IDs are treated as transaction-level dimensions because the source data does not establish a permanent supplier-to-SKU relationship.

---

# Key Takeaways

This project demonstrates an end-to-end approach to transforming a large transactional dataset into a business intelligence solution.

The main workflow was:

**Raw Data → Data Preparation → Star Schema → DAX → Analytics → Interactive Dashboard → Business Decision Support**

Rather than focusing only on visualization, the project emphasizes the complete BI workflow:

**Data → Model → Measures → Analysis → Decision**

