# Data Quality & Certification

## Overview

Data quality was treated as a core component of the Northstar Industrial Products Inc. analytics solution.

Because the Power BI dashboards depend on large interconnected transactional datasets, the project required validation at multiple levels:

* Structural validation
* Referential integrity
* Data completeness
* Duplicate detection
* Date consistency
* Business-rule validation
* Transaction validation
* Independent analytical validation

The objective was to ensure that the final Power BI results were supported by a consistent and validated underlying dataset rather than relying only on visual or measure-level checks.

---

# Data Quality Objectives

The validation framework was designed to answer five key questions:

1. Are the generated datasets structurally valid?
2. Are relationships between dimensions and facts consistent?
3. Do transactional records follow the intended business rules?
4. Can Power BI measures be independently validated?
5. Is the final dataset suitable for the project's six business questions?

The validation process was performed throughout development rather than only at the end.

---

# Validation Framework

The overall validation framework covered the following areas:

| Validation Area        | Purpose                                                 |
| ---------------------- | ------------------------------------------------------- |
| Row counts             | Confirm expected dataset volumes                        |
| Key validation         | Confirm identifiers are usable and consistent           |
| Referential integrity  | Verify fact records connect to valid dimensions/parents |
| Null checks            | Identify unexpected missing values                      |
| Duplicate checks       | Identify unintended duplicate records                   |
| Date validation        | Confirm transactional dates map correctly to `dim_date` |
| Business rules         | Confirm records follow defined project logic            |
| Transaction validation | Confirm inventory movement types and statuses           |
| Shipment validation    | Independently validate fulfillment calculations         |
| Power BI validation    | Compare dashboard calculations against expected results |

---

# Structural Validation

Structural checks were used to confirm that the final datasets contained the expected tables and volumes.

The final certified model contains:

* 13 tables
* 1,461 date records
* 10 categories
* 20,000 products
* 500 suppliers
* 5,000 customers
* 12,000 contracts
* 10 warehouses
* 500,000 purchase orders
* 1,500,000 purchase order lines
* 300,000 sales orders
* 995,367 sales order lines
* 3,000,000 inventory transactions

Row-count validation was important because the project went through multiple development iterations and dataset configurations.

The final documentation therefore uses the **final certified dataset volumes**, not earlier development configurations.

---

# Referential Integrity

Referential-integrity checks were used to ensure that transactional records referenced valid parent or dimension records.

Important relationships validated included:

* Product → purchase order lines
* Product → sales order lines
* Product → inventory transactions
* Supplier → purchase activity
* Customer → sales activity
* Contract → contract lines
* Purchase order → purchase order lines
* Sales order → sales order lines
* Warehouse → inventory transactions
* Transaction dates → `dim_date`

Referential integrity is particularly important in this project because missing dimension references can cause Power BI filters and aggregations to produce incomplete or misleading results.

---

# Key Validation

Keys were reviewed across the model to ensure that identifiers required for relationships were present and usable.

Examples include:

* Product identifiers
* Supplier identifiers
* Customer identifiers
* Contract identifiers
* Purchase order identifiers
* Sales order identifiers
* Warehouse identifiers
* Date identifiers

Header-to-line relationships were also specifically checked because the purchase order and sales order models use separate header and line grains.

---

# Null and Completeness Checks

Null checks were used to identify unexpected missing values in fields required for:

* Relationships
* Dates
* Transaction classification
* Quantities
* Pricing
* Order attributes
* Shipment analysis

Not every field in an enterprise dataset must necessarily be populated, so the validation process focused on identifying missing values that could affect the intended analytical logic.

This distinction is important because a technically valid null value is different from a missing value that breaks a business rule or relationship.

---

# Duplicate Validation

Duplicate checks were used to identify unintended duplication within the datasets.

The objective was not simply to require every table to have unique rows.

Instead, validation considered the intended **grain** of each table.

For example:

* `fact_purchase_order` → one record represents a purchase order
* `fact_purchase_order_line` → one record represents a product line within a purchase order
* `fact_sales_order` → one record represents a sales order
* `fact_sales_order_line` → one record represents a product line within a sales order
* `fact_inventory_transaction` → one record represents an inventory movement

This grain-based approach helps distinguish legitimate repeated business entities from accidental duplicate records.

---

# Date Validation

Date integrity was important because time-based filtering is used throughout the Power BI solution.

The project validated the relationship between transactional date identifiers and the shared `dim_date` table.

Date validation covered:

* Purchase dates
* Sales order dates
* Required shipment dates
* Inventory transaction dates
* Contract-related dates where applicable

For shipment fulfillment specifically, Python validation confirmed:

* Missing shipment dates: **0**
* Missing required dates: **0**
* Unmatched shipment dates in `dim_date`: **0**
* Unmatched required dates in `dim_date`: **0**

This ensured that the shipment analysis could be independently evaluated against the date dimension.

---

# Business-Rule Validation

Business-rule validation was used to confirm that generated records behaved according to the intended project logic.

Examples include:

### Inventory Transactions

The final inventory transaction population was validated against the intended transaction types:

* Purchase Receipt
* Sales Shipment
* Inventory Adjustment
* Customer Return
* Supplier Return
* Stock Transfer

All final inventory transactions have a **Posted** status.

### Stock Transfers

Stock transfers were designed as paired warehouse movement events representing inventory leaving one warehouse and entering another.

### Purchase and Sales Structures

Purchase orders and sales orders were maintained at header level, with separate product-level line tables.

This preserves the intended business grain and prevents order-level information from being duplicated across product lines.

---

# Inventory Transaction Validation

The final inventory transaction table contains **3,000,000 records**.

The validated transaction distribution is:

| Transaction Type     |       Records |
| -------------------- | ------------: |
| Purchase Receipt     |     1,500,000 |
| Sales Shipment       |       990,000 |
| Inventory Adjustment |       180,000 |
| Customer Return      |       150,000 |
| Supplier Return      |       120,000 |
| Stock Transfer       |        60,000 |
| **Total**            | **3,000,000** |

The transaction population was reviewed to ensure that the intended inventory processes were represented in the final dataset.

This validation directly supports the Inventory Management dashboard, where inventory movement is analyzed by transaction type.

---

# Shipment Fulfillment Validation

Shipment fulfillment received additional independent validation because the analysis combines sales-order information with shipment transactions and quantity comparisons.

The validation process evaluated:

* Sales shipment records
* Sales order quantities
* Required shipment dates
* Shipment dates
* Order status
* Order priority
* Shipping region

### Shipment Date Validation

Python validation established:

* Sales shipment rows: **990,000**
* Missing shipment dates: **0**
* Missing required dates: **0**
* Unmatched shipment dates: **0**
* Unmatched required dates: **0**

### Shipment Order Population

The validation identified:

* Unique sales orders with shipments: **299,684**
* Orders without a shipment date: **316**
* Orders with multiple shipment records: **239,709**

Because the project evaluates fulfillment at the order level, multiple shipment records for an order were consolidated to determine the final shipment position.

---

# Fulfillment Eligibility

Cancelled orders were excluded from the final eligible fulfillment population.

The resulting eligible population was:

**281,882 orders**

The validated results were:

| Fulfillment Performance |      Orders |
| ----------------------- | ----------: |
| On Time & Complete      |     277,152 |
| On Time & Short         |       4,730 |
| Late & Complete         |           0 |
| Late & Short            |           0 |
| **Eligible Orders**     | **281,882** |

This produces:

* **98.32% On Time & Complete**
* **1.68% On Time & Short**
* **0% Late & Complete**
* **0% Late & Short**

The analysis also identified:

* **50,601 total units short**
* **10.70 average units short per affected order**

The result was independently validated before being used in the Power BI dashboard.

---

# Shipment Timing Validation

The shipment analysis was specifically checked for actual lateness rather than assuming that a shipment KPI should contain both late and on-time records.

The validated timing gap was:

* Minimum: **-30 days**
* Maximum: **0 days**
* Mean: **-15.38 days**
* Late orders: **0**

Therefore, the final dashboard did not present an artificial "late shipment" problem.

Instead, the analysis was redesigned around the actual validated business pattern:

> Northstar's synthetic dataset shows consistently on-time shipment timing, while the primary fulfillment exception is quantity shortfall.

This led to the final **Shipment Fulfillment Performance** analysis rather than an unsupported generic "On-Time Performance" visualization.

---

# Power BI Validation

Data quality validation continued after the data was loaded into Power BI.

The Power BI model and measures were checked against expected results.

This included verifying:

* KPI totals
* Filter behavior
* Date filtering
* Product filtering
* Category filtering
* Supplier filtering
* Region filtering
* Order priority filtering
* Order status filtering
* Shipment fulfillment results

This was particularly important for measures using `TREATAS` and the disconnected `Shipment Fulfillment` table.

---

# Model and Filter Validation

Several issues were identified during development and corrected before final certification.

### Purchase Order Line Relationship

The purchase order line dataset initially lost the required `purchase_order_id` relationship during the Power BI loading workflow.

The source data was checked, the field was restored, and the corrected dataset was reloaded.

The final model contains the intended purchase-order-to-purchase-order-line relationship.

### Inventory Reference Identifier

A mixed data-type issue was identified in `fact_inventory_transaction[reference_id]`.

The field contained values represented using inconsistent data types.

The issue was corrected before the final model was used for certification.

### Ambiguous Relationships

Potentially ambiguous fact-to-fact and bidirectional relationships were avoided.

The final solution relies on shared dimensions where appropriate and uses `TREATAS` for specific analytical filtering requirements.

---

# Stage 4.5 Validation

Stage 4.5 represented the project's formal pre-certification validation checkpoint.

The objective was to confirm that:

* Structural issues had been corrected
* Relationships were functioning
* Business rules were satisfied
* Transaction populations were consistent
* Dashboard calculations could be validated
* No known blocking data-quality issues remained

The project progressed from this checkpoint into final certification.

---

# Stage 4.7 Final Certification

Stage 4.7 represented the final data-quality certification of the project.

The final certified state was reviewed across the project's structural, relational, business-rule, and analytical validation requirements.

### Final Result

**0 exceptions**

The certification result means that no unresolved validation exceptions remained in the final project state under the defined validation framework.

This certified dataset became the source of truth for the completed Power BI solution.

---

# Data Quality and Dashboard Reliability

The validation process was directly connected to the final analytical pages.

### Executive Overview

Validated common measures for:

* Sales
* Procurement
* Purchase orders
* Sales orders
* Contracts
* Inventory activity

### Supplier & Procurement

Validated:

* Purchase value
* Purchase quantity
* Purchase order count
* Unit cost
* Supplier activity
* Lead-time analysis

### Inventory Management

Validated:

* Inventory transaction counts
* Transaction values
* Purchase receipts
* Sales shipments
* Returns
* Adjustments
* Stock transfers
* Net inventory movement

### Shipment & Logistics

Validated:

* Order status
* Shipment quantities
* Required dates
* Shipment dates
* Fulfillment completeness
* Fulfillment timing
* Regional and priority filtering

### Customer & Sales

Validated:

* Sales values
* Sales quantities
* Sales orders
* Order lines
* Pricing
* Discounts
* Customer-level activity

---

# Why Independent Validation Was Important

The project deliberately separated **data validation** from **dashboard presentation**.

Power BI was used to analyze and present the data, while Python was used as an independent validation layer.

This reduced the risk of accepting an incorrect result simply because a Power BI measure produced a plausible number.

The approach was particularly valuable for:

* Large transaction volumes
* Multi-table calculations
* Shipment fulfillment
* Quantity comparisons
* Date-based analysis
* Filter-context behavior

---

# Final Data Quality Outcome

The final Northstar dataset passed the project's defined validation and certification process with:

**0 unresolved exceptions**

The certified dataset then became the foundation for the completed five-page Power BI solution.

The data-quality process therefore served two purposes:

1. **Protect the integrity of the underlying analytical dataset**
2. **Provide evidence that the resulting dashboard metrics could be independently validated**

This validation-first approach was an important part of the overall solution rather than a separate cleanup activity performed after dashboard development.
