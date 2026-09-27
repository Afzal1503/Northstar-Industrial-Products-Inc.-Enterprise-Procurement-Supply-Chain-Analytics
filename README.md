# Northstar Industrial Products Inc. — Enterprise Procurement & Supply Chain Analytics

## Project Overview

Northstar Industrial Products Inc. is a fictional Canadian manufacturing and distribution company created for an enterprise-style analytics case study.

The project focuses on transforming large-scale procurement, supplier, inventory, shipment, customer, and sales data into a management-oriented Power BI analytics solution.

The project was built around six business questions:

1. Where are the largest procurement cost opportunities?
2. Which suppliers provide the strongest overall value and performance?
3. Where are procurement cost leakage and purchasing-control issues?
4. Where are the major inventory and supply risks?
5. Where are shipment fulfillment exceptions?
6. What actions should management investigate based on the analysis?

---

## Solution

The final solution combines:

**Business Requirements → Data Engineering → Dimensional Modeling → Data Quality Certification → Power BI → Business Insights**

The certified analytical model contains 13 tables and more than 7.3 million rows.

### Dataset Scale

| Area                   |     Scale |
| ---------------------- | --------: |
| Products               |    20,000 |
| Suppliers              |       500 |
| Customers              |     5,000 |
| Contracts              |    12,000 |
| Purchase Orders        |   500,000 |
| Purchase Order Lines   | 1,500,000 |
| Sales Orders           |   300,000 |
| Sales Order Lines      |   995,367 |
| Inventory Transactions | 3,000,000 |
| Warehouses             |        10 |
| Dates                  |     1,461 |

---

## Data Model

The final dimensional model contains:

### Dimensions

* Date
* Category
* Product
* Supplier
* Customer
* Contract
* Warehouse

### Fact Tables

* Contract Lines
* Purchase Orders
* Purchase Order Lines
* Sales Orders
* Sales Order Lines
* Inventory Transactions

The model was designed to support reusable filtering across procurement, inventory, shipment, and sales analysis while avoiding ambiguous fact-to-fact relationships.

---

## Power BI Solution

The final Power BI report contains five analytical pages:

### 1. Executive Overview

Provides management-level visibility into:

* Total Sales
* Total Purchase Value
* Sales Order Count
* Purchase Order Count
* Active Contract Count
* Inventory Transaction Count
* Contract discount analysis

### 2. Supplier & Procurement

Analyzes:

* Purchase Value
* Purchase Quantity
* Purchase Orders
* Average Unit Cost
* Average Purchase Lead Time
* Active Suppliers

### 3. Inventory Management

Analyzes:

* Inventory Transaction Value
* Net Inventory Movement
* Purchase Receipts
* Sales Shipments
* Inventory Adjustments
* Stock Transfers
* Returns
* Inventory exposure and risk

### 4. Shipment & Logistics

Analyzes:

* Order Status
* Order Priority
* Shipping Region
* Shipment Fulfillment
* Quantity Shortfalls
* Monthly Fulfillment Performance

### 5. Customer & Sales

Analyzes:

* Total Sales
* Gross Sales
* Sales Quantity
* Sales Orders
* Average Order Value
* Average Sales Price
* Sales Discounts
* Customer and product activity

---

## Key Validated Finding

Shipment fulfillment was independently validated against the underlying data.

Among **281,882 eligible orders**:

| Fulfillment Result |  Orders |
| ------------------ | ------: |
| On Time & Complete | 277,152 |
| On Time & Short    |   4,730 |
| Late & Complete    |       0 |
| Late & Short       |       0 |

### Result

* **100%** of eligible orders were on time
* **98.32%** were both on time and complete
* **1.68%** were on time but short
* **50,601 units** were short across affected orders

The analysis therefore identified **quantity completeness rather than shipment timing** as the primary fulfillment exception.

---

## Data Quality & Certification

Data quality was treated as a core part of the solution.

Validation included:

* Row-count validation
* Primary-key validation
* Referential integrity
* Duplicate checks
* Null checks
* Date validation
* Business-rule validation
* Inventory transaction validation
* Shipment fulfillment validation
* Power BI reconciliation

### Final Certification

**0 exceptions**

The certification refers to the defined data-quality and business-rule validation framework. It does not mean that the business dataset contains no operational exceptions.

---

## Technical Highlights

### Dimensional Modeling

Built a 13-table enterprise-style dimensional model supporting procurement, inventory, shipment, and sales analysis.

### DAX

Developed management KPIs and filter-aware analytical measures using DAX.

### TREATAS

Used `TREATAS` to transfer filter context into the disconnected shipment fulfillment analytical table without introducing ambiguous relationships.

### Independent Validation

Key dashboard results were independently validated against the underlying analytical data before final certification.

### Large-Scale Data

The solution was designed around millions of transactional records rather than a small demonstration dataset.

---

## Business Insights

The analysis provides a framework for investigating:

* Procurement cost and unit-cost exposure
* Supplier activity and lead time
* Contract discounts and purchasing controls
* Inventory movements and adjustments
* Warehouse-level inventory exposure
* Shipment fulfillment completeness
* Sales and customer activity

The project emphasizes the distinction between a **validated finding** and an area that requires further business investigation.

---

## Documentation

Detailed project documentation is available below:

* [Data Model](documentation/data-model.md) — Final 13-table dimensional model and relationship design
* [Data Engineering](documentation/data-engineering.md) — Data-generation approach, transaction engineering, and development corrections
* [Data Quality & Certification](documentation/data-quality.md) — Validation framework and final certification
* [Power BI Solution](documentation/power-bi-solution.md) — Dashboard pages, KPIs, DAX, filter context, and analytical design
* [Business Insights](documentation/business-insights.md) — Validated findings and management investigation areas

---


## Technology Stack

* **Power BI** — Dashboard development and business intelligence
* **DAX** — Measures and analytical logic
* **Python** — Data engineering and independent validation
* **Dimensional Modeling** — Enterprise analytical architecture
* **Parquet / Analytical Data Files** — Data preparation and Power BI consumption

---

## Project Scope

This is a fictional enterprise case study using synthetic data.

The project is intended to demonstrate the analytical workflow, modeling decisions, validation approach, Power BI development, and business interpretation rather than represent the actual operations or financial performance of a real organization.
