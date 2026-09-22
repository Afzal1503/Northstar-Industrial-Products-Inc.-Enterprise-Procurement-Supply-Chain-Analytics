# Data Model

## Overview

The Northstar Industrial Products Inc. analytics solution uses a **13-table dimensional model** designed to support procurement, supplier, contract, inventory, sales, customer, warehouse, and shipment analysis.

The model separates descriptive business entities into dimension tables and transactional activity into fact tables. This structure supports reusable filtering, consistent KPI calculations, and analysis across multiple business processes.

The final model contains:

* **7 dimension tables**
* **6 fact tables**
* **20,000 products**
* **500 suppliers**
* **5,000 customers**
* **12,000 contracts**
* **10 warehouses**
* **500,000 purchase orders**
* **300,000 sales orders**
* **3,000,000 inventory transactions**

---

## Model Structure

### Dimension Tables

| Table           | Purpose                          | Approx. Rows |
| --------------- | -------------------------------- | -----------: |
| `dim_date`      | Calendar and time-based analysis |        1,461 |
| `dim_category`  | Product category classification  |           10 |
| `dim_product`   | Product master data              |       20,000 |
| `dim_supplier`  | Supplier master data             |          500 |
| `dim_customer`  | Customer master data             |        5,000 |
| `dim_contract`  | Procurement contract information |       12,000 |
| `dim_warehouse` | Warehouse/location information   |           10 |

### Fact Tables

| Table                        | Purpose                                     | Approx. Rows |
| ---------------------------- | ------------------------------------------- | -----------: |
| `fact_contract_line`         | Contract-level product and pricing activity |      500,000 |
| `fact_purchase_order`        | Purchase order header transactions          |      500,000 |
| `fact_purchase_order_line`   | Product-level purchase order activity       |    1,500,000 |
| `fact_sales_order`           | Sales order header transactions             |      300,000 |
| `fact_sales_order_line`      | Product-level sales activity                |      995,367 |
| `fact_inventory_transaction` | Inventory movement transactions             |    3,000,000 |

---

# Dimension Tables

## `dim_date`

The date dimension provides a consistent calendar structure for time-based analysis.

It supports:

* Year
* Month
* Date
* Date keys
* Time-based filtering
* Trend analysis

The table contains **1,461 dates**, covering the final project date range.

Date dimensions are used throughout the model instead of relying exclusively on transaction-date columns, allowing consistent time-based analysis across procurement, sales, contracts, and inventory processes.

---

## `dim_category`

Contains the product category hierarchy used to group products for procurement, inventory, and sales analysis.

The final dataset contains **10 categories**.

The category dimension provides a reusable filtering path between product information and transactional fact tables.

---

## `dim_product`

Contains the product master data used throughout the procurement, inventory, and sales processes.

The final dataset contains **20,000 products**.

Products connect analytical processes such as:

* Purchasing
* Procurement spend
* Inventory movement
* Sales
* Shipment fulfillment

Product-level analysis can also be rolled up through `dim_category`.

---

## `dim_supplier`

Contains supplier master information used for procurement and supplier-performance analysis.

The final dataset contains **500 suppliers**.

Supplier information supports analysis of:

* Purchase value
* Purchase quantity
* Unit cost
* Purchase order activity
* Lead time
* Supplier-level procurement patterns

---

## `dim_customer`

Contains customer master information used for customer and sales analysis.

The final dataset contains **5,000 customers**.

Customer information supports analysis of:

* Sales activity
* Order volume
* Sales value
* Customer-level purchasing patterns
* Regional/customer analysis

---

## `dim_contract`

Contains procurement contract information used to analyze contractual purchasing activity.

The final dataset contains **12,000 contracts**.

The contract dimension supports analysis of:

* Contract activity
* Contract pricing
* Contract lines
* Contract discounts
* Supplier/contract relationships

Contract-level information is connected to product-level contract activity through `fact_contract_line`.

---

## `dim_warehouse`

The warehouse dimension contains the warehouse/location structure used for inventory analysis.

The final dataset contains **10 warehouses**.

This dimension was added during model development to provide a dedicated analytical structure for warehouse-level inventory activity.

It allows inventory transactions to be analyzed by warehouse without embedding warehouse attributes directly into the inventory fact.

---

# Fact Tables

## `fact_contract_line`

Contains product-level contract activity.

The table contains **500,000 contract-line records**.

Its grain is a **contract-product line**.

The table supports analysis of:

* Contracted products
* Contract quantities
* Contract pricing
* Discounts
* Supplier/contract activity

This structure allows contract information to be analyzed at a more detailed level than the contract header.

---

## `fact_purchase_order`

Contains purchase order header-level transactions.

The final dataset contains **500,000 purchase orders**.

The table supports analysis of:

* Purchase order volume
* Order dates
* Supplier activity
* Purchase order status
* Procurement activity

The purchase order header connects to its detailed product-level transactions through `fact_purchase_order_line`.

---

## `fact_purchase_order_line`

Contains product-level purchase order transactions.

The final dataset contains **1,500,000 purchase order lines**.

Its grain is a **product line within a purchase order**.

The table supports detailed analysis of:

* Purchased products
* Quantities
* Unit costs
* Purchase values
* Supplier purchasing activity
* Category-level procurement

The `purchase_order_id` relationship connects purchase order lines to the corresponding purchase order header.

---

## `fact_sales_order`

Contains sales order header-level transactions.

The final dataset contains **300,000 sales orders**.

The table supports analysis of:

* Sales order volume
* Order status
* Order priority
* Shipping region
* Order dates
* Required dates

These fields also support shipment and fulfillment analysis.

---

## `fact_sales_order_line`

Contains product-level sales order transactions.

The final dataset contains **995,367 sales order lines**.

Its grain is a **product line within a sales order**.

The table supports analysis of:

* Sales quantity
* Sales value
* Unit price
* Discounts
* Product performance
* Customer purchasing activity

It connects detailed sales activity to the sales order header and relevant dimensions.

---

## `fact_inventory_transaction`

Contains inventory movement transactions.

The final dataset contains **3,000,000 inventory transactions**.

The transaction types include:

| Transaction Type     | Approx. Records |
| -------------------- | --------------: |
| Purchase Receipt     |       1,500,000 |
| Sales Shipment       |         990,000 |
| Inventory Adjustment |         180,000 |
| Customer Return      |         150,000 |
| Supplier Return      |         120,000 |
| Stock Transfer       |          60,000 |

The inventory fact supports analysis of:

* Inventory movement
* Purchase receipts
* Sales shipments
* Returns
* Adjustments
* Stock transfers
* Inventory transaction value
* Warehouse activity

All inventory transaction records in the final certified dataset have a **Posted** status.

---

# Relationship Design

The model follows a dimensional approach where reusable dimensions provide filtering paths into transactional fact tables.

The major analytical relationships include:

* Date → procurement transactions
* Date → sales transactions
* Date → inventory transactions
* Category → product
* Product → purchase order lines
* Product → sales order lines
* Product → inventory transactions
* Supplier → procurement activity
* Customer → sales activity
* Contract → contract lines
* Purchase order → purchase order lines
* Sales order → sales order lines
* Warehouse → inventory transactions

The model was designed to avoid unnecessary fact-to-fact relationships and ambiguous filter paths.

Where analytical filtering could not be achieved cleanly through physical relationships, DAX-based filtering techniques such as `TREATAS` were used.

---

# Fact Table Grain

Understanding the grain of each fact table was an important part of the model design.

| Fact Table                   | Grain                                    |
| ---------------------------- | ---------------------------------------- |
| `fact_contract_line`         | One product line within a contract       |
| `fact_purchase_order`        | One purchase order                       |
| `fact_purchase_order_line`   | One product line within a purchase order |
| `fact_sales_order`           | One sales order                          |
| `fact_sales_order_line`      | One product line within a sales order    |
| `fact_inventory_transaction` | One inventory movement transaction       |

Defining the grain explicitly helped prevent double counting and supported reliable aggregation in Power BI.

---

# Why a Dimensional Model?

The dimensional approach was selected because the project required analysis across several business processes while maintaining reusable dimensions.

For example, `dim_product` can be used to analyze the same product across:

* Procurement
* Inventory
* Sales
* Shipment activity

Similarly, `dim_date` provides consistent time-based analysis across multiple transactional processes.

This structure also makes the model easier to extend and maintain compared with a heavily normalized transactional design.

---

# Warehouse Modeling Decision

A dedicated `dim_warehouse` table was introduced during development after identifying the need for explicit warehouse-level analysis.

Instead of storing warehouse attributes directly within the inventory transaction structure, warehouse information was separated into its own dimension.

This provides a reusable filtering and grouping structure for:

* Inventory movement
* Warehouse activity
* Stock transfers
* Inventory exposure
* Warehouse-level risk analysis

The final certified model therefore contains **10 warehouses and 13 total tables**.

---

# Model Design Considerations

Several modeling decisions were made during development to maintain reliable analytical behavior.

### Avoiding Ambiguous Fact-to-Fact Relationships

Direct bidirectional relationships between transactional fact tables were avoided where they could introduce ambiguous filter paths.

The model instead relies primarily on shared dimensions.

### Purchase Order Header and Line Relationship

The purchase order header and line tables are connected through `purchase_order_id`, allowing order-level and product-line-level analysis without losing the relationship between the two grains.

### Sales Order Header and Line Relationship

Sales order headers and lines are similarly separated to preserve the distinction between order-level attributes and product-line-level measures.

### Inventory and Warehouse Analysis

Warehouse information is modeled through `dim_warehouse`, providing a dedicated analytical path into inventory transactions.

### Shipment Fulfillment Analysis

A separate analytical table was created for detailed shipment fulfillment analysis. This table was intentionally kept disconnected from the core model to avoid introducing ambiguous relationships.

DAX and `TREATAS` are used to transfer relevant filter context into the analytical table when required.

---

# Final Model Summary

The final Northstar model contains **13 tables** and supports analysis across procurement, contracts, suppliers, products, inventory, warehouses, sales, customers, and shipment fulfillment.

The model was designed around six core business questions:

1. Procurement cost opportunities
2. Supplier value and performance
3. Procurement leakage and controls
4. Supply and inventory risk
5. Purchasing and transaction discrepancies
6. Management actions based on analytical findings

The resulting dimensional structure provides the foundation for the Power BI dashboards and analytical measures developed in the project.
