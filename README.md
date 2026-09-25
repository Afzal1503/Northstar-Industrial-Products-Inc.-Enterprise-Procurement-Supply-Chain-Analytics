# Northstar Industrial Products Inc. — Enterprise Procurement & Supply Chain Analytics

## Project Overview

Northstar Industrial Products Inc. is a synthetic Canadian manufacturing and distribution business used to develop an enterprise-style procurement, supply chain, inventory, sales, and operational analytics solution.

The project was designed around the types of analytical requirements a CFO, Procurement Director, Supply Chain Manager, Finance/AP Manager, and operational teams would use to monitor purchasing activity, supplier performance, inventory movement, shipment fulfillment, and sales operations.

The solution combines **Python-based data engineering and validation, dimensional data modeling, and Power BI analytics** into an end-to-end analytical workflow.

---

## Business Problem

Northstar manages large volumes of procurement, contract, inventory, sales, supplier, customer, and warehouse transactions.

The objective of the project was to create a structured analytical environment that could help management answer questions such as:

1. Where are the largest procurement cost opportunities?
2. Which suppliers provide the best overall value and performance?
3. Where are procurement cost leakages or purchasing-control issues?
4. Where are supply and inventory risks?
5. Where are invoice, transaction, or purchasing-control discrepancies?
6. What actions should management consider based on the analysis?

---

## Solution

The final solution consists of:

* Large-scale synthetic enterprise datasets
* A **13-table dimensional data model**
* Python-based data generation and validation
* Business-rule and data-quality certification
* Power BI analytical dashboards
* DAX-based measures and filter-context management
* Cross-table analysis using `TREATAS`
* A dedicated shipment-fulfillment analytical table for fulfillment analysis

The solution was developed iteratively, with data modeling, engineering, validation, and dashboard logic tested and corrected throughout the project.

---

## Dataset Scale

The final certified dataset contains:

| Area                   |    Volume |
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
| Date Records           |     1,461 |

The model contains **13 tables** covering master data, transactional data, contracts, purchasing, sales, inventory, and warehouse information.

---

## Data Model

The analytical model follows a dimensional/star-schema approach with separate fact and dimension tables.

### Dimension Tables

* `dim_date`
* `dim_category`
* `dim_product`
* `dim_supplier`
* `dim_customer`
* `dim_contract`
* `dim_warehouse`

### Fact Tables

* `fact_contract_line`
* `fact_purchase_order`
* `fact_purchase_order_line`
* `fact_sales_order`
* `fact_sales_order_line`
* `fact_inventory_transaction`

The warehouse dimension was incorporated to provide a dedicated structure for warehouse-level inventory analysis rather than embedding warehouse attributes directly within transactional data.

**See:** [`documentation/data-model.md`](documentation/data-model.md)

---

## Data Engineering

Python was used to generate and validate the synthetic enterprise datasets and to support the data engineering workflow.

The engineering process covered:

* Master-data generation
* Transaction generation
* Primary and foreign-key relationships
* Date dimensions
* Procurement and contract transactions
* Sales and customer transactions
* Inventory movements
* Warehouse activity
* Business-rule validation
* Data-quality testing

The final datasets were prepared for analytical consumption in Power BI.

**See:** [`documentation/data-engineering.md`](documentation/data-engineering.md)

---

## Data Quality & Certification

Data quality was treated as a core part of the project rather than a final cleanup step.

Validation included:

* Referential integrity
* Primary/foreign-key consistency
* Duplicate detection
* Null checks
* Date validation
* Transaction consistency
* Business-rule validation
* Inventory transaction validation
* Shipment and fulfillment validation

The project progressed through multiple validation checkpoints, including **Stage 4.5** and the final **Stage 4.7 certification**.

### Final Certification

**0 exceptions identified in the final certification.**

This certification reflects the final project state after corrections and validation.

**See:** [`documentation/data-quality.md`](documentation/data-quality.md)

---

## Power BI Solution

The final Power BI solution contains five analytical pages.

### 1. Executive Overview

Provides management-level visibility into:

* Sales
* Procurement
* Purchase orders
* Sales orders
* Contracts
* Inventory activity

### 2. Supplier & Procurement

Focuses on:

* Procurement spend
* Purchase quantities
* Purchase orders
* Supplier activity
* Unit cost
* Purchase lead time
* Supplier/category analysis

### 3. Inventory Management

Analyzes:

* Inventory movements
* Purchase receipts
* Sales shipments
* Customer and supplier returns
* Inventory adjustments
* Stock transfers
* Inventory exposure
* Inventory risk indicators

### 4. Shipment & Logistics

Analyzes:

* Order status
* Shipment fulfillment
* Order priority
* Shipping regions
* Quantity shortfalls
* Fulfillment trends

The final shipment analysis found that **98.32% of eligible orders were both on time and complete**, while **1.68% experienced quantity shortfalls**.

### 5. Customer & Sales

Provides analysis of:

* Sales performance
* Sales orders
* Sales quantities
* Average order value
* Sales pricing
* Discounts
* Customer and regional activity

**See:** [`documentation/power-bi-solution.md`](documentation/power-bi-solution.md)


---

## Shipment Fulfillment Analysis

A dedicated `Shipment Fulfillment` analytical table was created to evaluate order-level shipment performance without introducing ambiguous relationships into the core model.

The analysis evaluates:

* Required shipment date
* Final shipment date
* Ordered quantity
* Shipped quantity
* Timeliness
* Fulfillment completeness
* Order status
* Order priority
* Shipping region

The final validated population contained **281,882 eligible orders**.

Results:

* **277,152 — On Time & Complete**
* **4,730 — On Time & Short**
* **0 — Late & Complete**
* **0 — Late & Short**
* **50,601 total units short**

This showed that shipment timing was consistently on schedule in the validated dataset, while quantity shortfalls represented the primary fulfillment exception.

---

## Technical Highlights

### Dimensional Modeling

A 13-table dimensional model was designed to separate master data from transactional activity and support reusable analytical dimensions.

### DAX & Filter Context

DAX was used to create analytical KPIs and manage filter context across the model.

### TREATAS

`TREATAS` was used where analytical filtering needed to be transferred across tables without creating ambiguous bidirectional fact-to-fact relationships.

### Disconnected Analytical Table

The `Shipment Fulfillment` table was intentionally kept disconnected from the main model and filtered through DAX to avoid introducing ambiguous relationships while still supporting product, category, region, priority, and status analysis.

### Data Validation

Python-based validation was used throughout development to independently verify data quality and analytical results.

---

## Technology Stack

**Data Engineering & Analysis**

* Python
* Pandas
* SQL

**Business Intelligence**

* Microsoft Power BI
* DAX
* Power Query

**Data Modeling**

* Dimensional Modeling
* Star Schema
* Fact & Dimension Design

**Data Quality**

* Referential Integrity Testing
* Business Rule Validation
* Duplicate & Null Checks
* Independent Python Validation

---

## Key Project Outcomes

The completed solution provides:

* A scalable **13-table enterprise analytical model**
* **3M inventory transactions** and large-scale procurement/sales datasets
* Certified data quality with **0 final exceptions**
* Five Power BI analytical pages
* Cross-functional procurement, inventory, fulfillment, and sales analysis
* Validated shipment fulfillment metrics
* Reusable DAX-based analytical measures
* A documented approach to handling complex filter-context requirements

**See:** [`documentation/business-insights.md`](documentation/business-insights.md)


---


## Project Scope & Data Note

Northstar Industrial Products Inc. is a **synthetic business created for portfolio and analytical development purposes**.

The datasets are synthetic and do not represent actual company transactions, suppliers, customers, or financial results.

The project focuses on demonstrating the design and implementation of an enterprise-style analytics solution, including data engineering, modeling, validation, business intelligence, and analytical problem solving.
