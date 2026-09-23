# Data Engineering

## Overview

The Northstar Industrial Products Inc. project uses Python-based data engineering to create a large-scale synthetic enterprise dataset that supports procurement, supplier, contract, inventory, sales, customer, warehouse, and shipment analysis.

The objective was not simply to generate large volumes of data. The data was designed around the project's **six business questions** and the analytical requirements of the final Power BI solution.

The engineering workflow therefore focused on:

* Creating realistic business entities and relationships
* Generating transactional data at different business grains
* Maintaining referential integrity across related tables
* Supporting procurement and supplier analysis
* Supporting inventory and warehouse analysis
* Supporting sales and shipment analysis
* Supporting contract and purchasing-control analysis
* Creating enough transactional volume to demonstrate enterprise-scale analytical handling
* Independently validating the resulting datasets before Power BI analysis

---

# Engineering Objectives

The data engineering process was designed to support the following analytical requirements.

### 1. Procurement Cost Analysis

The model needed sufficient purchase-order and purchase-order-line activity to analyze:

* Procurement spend
* Purchase quantities
* Unit costs
* Supplier activity
* Product/category purchasing
* Purchase order volume

This led to the creation of:

* `fact_purchase_order`
* `fact_purchase_order_line`
* `dim_supplier`
* `dim_product`
* `dim_category`
* `dim_date`

---

### 2. Supplier Performance Analysis

Supplier analysis required a consistent supplier master and transactional purchasing history.

The dataset therefore includes:

* 500 suppliers
* 500,000 purchase orders
* 1,500,000 purchase order lines

This supports the Supplier & Procurement Power BI page, including supplier-level analysis of purchasing activity, cost, quantity, and lead time.

---

### 3. Contract and Purchasing-Control Analysis

The project also required the ability to analyze procurement contracts and their associated product-level activity.

The final dataset contains:

* 12,000 contracts
* 500,000 contract lines

This supports analysis of contractual purchasing activity, contract pricing, discounts, and procurement relationships.

---

### 4. Inventory and Supply Risk Analysis

Inventory analysis required transaction-level movement rather than a single static inventory balance.

The final inventory dataset contains **3,000,000 inventory transactions** across multiple transaction types:

* Purchase receipts
* Sales shipments
* Inventory adjustments
* Customer returns
* Supplier returns
* Stock transfers

Warehouse-level analysis is supported through the 10-record `dim_warehouse` table.

This structure supports the Inventory Management dashboard and allows inventory activity to be analyzed by product, category, warehouse, and transaction type.

---

### 5. Shipment and Fulfillment Analysis

The shipment dataset needed to support order-level fulfillment analysis rather than simply showing shipment counts.

Sales orders therefore include:

* Order status
* Order priority
* Shipping region
* Order date
* Required date

Sales order lines provide ordered quantities, while inventory transactions provide sales shipment activity.

These structures were later used to create the dedicated `Shipment Fulfillment` analytical table in Power BI.

The final shipment analysis evaluates:

* Whether an order was shipped on time
* Whether the order was completely fulfilled
* Quantity shortfalls
* Order status
* Order priority
* Shipping region
* Required date trends

---

### 6. Customer and Sales Analysis

The Customer & Sales Power BI page required both order-level and line-level sales data.

The final dataset contains:

* 5,000 customers
* 300,000 sales orders
* 995,367 sales order lines

Separating sales-order headers from sales-order lines allows order-level attributes such as status, priority, and shipping region to be analyzed independently from product-level quantities, prices, and discounts.

---

# Synthetic Data Generation

The Northstar dataset is synthetic and was created specifically for portfolio and analytical development.

The generation process creates interconnected master and transactional data rather than independent random tables.

The main entity groups are:

### Master Data

* Dates
* Categories
* Products
* Suppliers
* Customers
* Contracts
* Warehouses

### Transactional Data

* Contract lines
* Purchase orders
* Purchase order lines
* Sales orders
* Sales order lines
* Inventory transactions

The relationships between these entities were maintained through shared identifiers and foreign-key-style references.

---

# Final Dataset Scale

The final certified dataset contains the following volumes:

| Dataset                      |      Rows |
| ---------------------------- | --------: |
| `dim_date`                   |     1,461 |
| `dim_category`               |        10 |
| `dim_product`                |    20,000 |
| `dim_supplier`               |       500 |
| `dim_customer`               |     5,000 |
| `dim_contract`               |    12,000 |
| `dim_warehouse`              |        10 |
| `fact_contract_line`         |   500,000 |
| `fact_purchase_order`        |   500,000 |
| `fact_purchase_order_line`   | 1,500,000 |
| `fact_sales_order`           |   300,000 |
| `fact_sales_order_line`      |   995,367 |
| `fact_inventory_transaction` | 3,000,000 |

The final date dimension contains **1,461 dates**, representing the four-year project period from **January 1, 2022 through December 31, 2025**.

---

# Purchase Order Engineering

Purchase order data was generated at two levels.

### Purchase Order Header

`fact_purchase_order` contains **500,000 purchase orders**.

The header-level structure supports analysis of purchase order activity and order-level attributes.

### Purchase Order Lines

`fact_purchase_order_line` contains **1,500,000 lines**.

The line-level structure provides detailed product purchasing activity, including quantities and costs.

The separation between header and line tables preserves the different grains required for accurate analysis.

A purchase order can therefore contain multiple product lines without duplicating order-level information.

---

# Sales Order Engineering

Sales activity was similarly separated into order headers and product lines.

### Sales Order Header

`fact_sales_order` contains **300,000 sales orders**.

The final dataset includes attributes used directly in the Shipment & Logistics and Customer & Sales dashboards, including:

* `order_status`
* `order_priority`
* `shipping_region`
* Order date
* Required date

### Sales Order Lines

`fact_sales_order_line` contains **995,367 product-level lines**.

This structure supports analysis of:

* Sales quantities
* Sales prices
* Discounts
* Product activity
* Customer purchasing activity

The separation of header and line data also helps avoid double-counting order-level metrics when analyzing product-level transactions.

---

# Inventory Transaction Engineering

The inventory model contains **3,000,000 transactions**.

The transaction population was structured around the actual inventory processes required by the dashboard:

| Transaction Type     |       Records |
| -------------------- | ------------: |
| Purchase Receipt     |     1,500,000 |
| Sales Shipment       |       990,000 |
| Inventory Adjustment |       180,000 |
| Customer Return      |       150,000 |
| Supplier Return      |       120,000 |
| Stock Transfer       |        60,000 |
| **Total**            | **3,000,000** |

All final inventory transaction records have a **Posted** status.

### Stock Transfers

Stock transfers represent movement between warehouse locations.

The final dataset contains **60,000 stock-transfer transaction records**, representing paired movement events across warehouses.

This allows warehouse activity to be analyzed separately from purchasing receipts, sales shipments, returns, and adjustments.

---

# Inventory Movement Design

Rather than attempting to represent inventory as a single static balance, the project uses transaction-level movements.

This allows the Inventory Management dashboard to analyze:

* Purchase receipt quantity
* Sales shipment quantity
* Customer return quantity
* Supplier return quantity
* Inventory adjustment quantity
* Stock transfer quantity
* Net inventory movement
* Total inventory transaction value

This transaction-based approach also provides a foundation for identifying unusual movement patterns and inventory exposure.

---

# Date Engineering

A dedicated `dim_date` table was created with **1,461 dates** covering January 1, 2022 through December 31, 2025.

The date structure supports consistent time-based analysis across:

* Procurement
* Purchase orders
* Contracts
* Sales orders
* Required shipment dates
* Inventory transactions
* Shipment fulfillment

Using a shared date dimension also allows Power BI pages to use consistent time filtering rather than relying on independently generated date attributes in every fact table.

---

# Key and Relationship Engineering

Shared identifiers were used to connect transactional records to their corresponding dimensions and parent transactions.

Important relationships include:

* Product identifiers connecting products to purchasing, sales, and inventory activity
* Supplier identifiers connecting suppliers to procurement activity
* Customer identifiers connecting customers to sales activity
* Contract identifiers connecting contracts to contract lines
* Purchase order identifiers connecting purchase orders to purchase order lines
* Sales order identifiers connecting sales orders to sales order lines
* Warehouse identifiers connecting warehouses to inventory activity
* Date identifiers connecting transactional activity to the date dimension

Maintaining these relationships was important because the Power BI dashboards depend on dimensions being able to filter the appropriate transactional populations.

---

# Data Preparation for Power BI

After generation and validation, the engineered datasets were prepared for analytical consumption in Power BI.

The workflow involved:

1. Generating the synthetic business entities
2. Generating transactional records
3. Maintaining key relationships
4. Running independent validation checks
5. Correcting identified data/model issues
6. Preparing the final datasets for Power BI
7. Loading the final data model
8. Building and validating Power BI measures and dashboards

The final Power BI model was based on the certified project state rather than an earlier development dataset.

---

# Large-Volume Data Considerations

The project intentionally used large transactional volumes to demonstrate analytical handling beyond small demonstration datasets.

The largest tables include:

* 3,000,000 inventory transactions
* 1,500,000 purchase order lines
* 995,367 sales order lines
* 500,000 purchase orders
* 500,000 contract lines
* 300,000 sales orders

The engineering process therefore treated table grain, keys, relationship design, and validation as important components of the solution rather than simply generating records for visualization.

---

# Independent Validation

Python was also used as an independent validation layer.

This was important because Power BI calculations should not be treated as the only source of truth when validating large analytical datasets.

Validation was used to verify:

* Row counts
* Key consistency
* Referential integrity
* Duplicate conditions
* Null conditions
* Date relationships
* Transaction distributions
* Business rules
* Shipment quantities
* Shipment timing
* Final analytical results

This independent validation was particularly important for the shipment fulfillment analysis, where Python was used to validate the final order-level results independently of the Power BI calculations.

---

# Engineering Corrections During Development

The project was developed iteratively rather than treating the first generated dataset as final.

Several issues identified during development were corrected before final certification.

Examples included:

### Purchase Order Line Relationship

The purchase order line dataset was corrected to retain the required `purchase_order_id` relationship and later the required date identifier used in the analytical workflow.

The corrected dataset was then reloaded into Power BI.

### Inventory Reference Identifier

A mixed-type issue was identified in the inventory transaction reference identifier, where values from different source processes had inconsistent data types.

The field was corrected so that the final model could handle the reference values consistently.

### Model Ambiguity

Fact-to-fact and bidirectional relationship designs were reviewed to avoid ambiguous filter paths.

Where direct relationships were not appropriate, shared dimensions and DAX-based filtering were used instead.

These corrections were completed before the final data-quality certification.

---

# Engineering Decisions Driven by the Dashboard

The dataset structure was intentionally shaped by the final analytical requirements.

### Procurement & Supplier Dashboard

Required:

* Suppliers
* Products
* Categories
* Purchase orders
* Purchase order lines
* Purchase costs
* Purchase quantities
* Lead-time information

### Inventory Dashboard

Required:

* Products
* Categories
* Warehouses
* Inventory transactions
* Transaction types
* Transaction values
* Inventory movement

### Shipment & Logistics Dashboard

Required:

* Sales orders
* Sales order lines
* Required dates
* Shipment dates
* Ordered quantities
* Shipped quantities
* Status
* Priority
* Shipping region

### Customer & Sales Dashboard

Required:

* Customers
* Sales orders
* Sales order lines
* Products
* Sales quantities
* Sales prices
* Discounts

### Executive Overview

Required reusable measures across the model for:

* Sales
* Procurement
* Purchase orders
* Sales orders
* Contracts
* Inventory activity

This ensured that the engineering work directly supported the final Power BI solution rather than generating unrelated datasets.

---

# Final Engineering Outcome

The completed engineering workflow produced a connected synthetic enterprise dataset capable of supporting the project's six business questions and five Power BI analytical pages.

The final state contains:

* **13 interconnected model tables**
* **20,000 products**
* **500 suppliers**
* **5,000 customers**
* **12,000 contracts**
* **500,000 purchase orders**
* **1.5M purchase order lines**
* **300,000 sales orders**
* **995,367 sales order lines**
* **3M inventory transactions**
* **10 warehouses**
* **1,461 date records**

The datasets were subsequently subjected to the project's formal data-quality and business-rule validation process, culminating in the final **Stage 4.7 certification with 0 exceptions**.

The engineering work therefore served as the foundation for the project's dimensional model, Power BI dashboards, DAX measures, and validated business analysis.
