# Power BI Solution

## Overview

The Northstar Industrial Products Inc. Power BI solution translates the enterprise procurement, supply chain, inventory, shipment, and sales data model into a management-focused analytical reporting environment.

The solution was designed around the six core business questions established during project planning:

1. Where are the largest procurement cost opportunities?
2. Which suppliers provide the strongest overall value and performance?
3. Where are procurement cost leakage and purchasing-control issues?
4. Where are the major inventory and supply risks?
5. Where are shipment fulfillment and purchasing-control exceptions?
6. What actions can management take based on the analysis?

The final Power BI solution contains five analytical pages:

1. Executive Overview
2. Supplier & Procurement
3. Inventory Management
4. Shipment & Logistics
5. Customer & Sales

The dashboards use the certified 13-table dimensional model and were validated against the underlying data before finalization.

---

## 1. Executive Overview

### Purpose

The Executive Overview provides a consolidated management view of Northstar's overall commercial, procurement, contract, and inventory activity.

The page is designed for senior stakeholders such as the CFO, Procurement Director, and Supply Chain Manager who need a high-level view before drilling into individual business areas.

### Key KPIs

The page includes KPI cards covering:

* Total Sales
* Total Purchase Value
* Sales Order Count
* Purchase Order Count
* Active Contract Count
* Inventory Transaction Count

These KPIs provide a starting point for understanding overall business activity and allow management to assess the scale of sales, purchasing, contractual activity, and inventory movement.

### Contract Analysis

Contract-related analysis is included to provide visibility into negotiated purchasing activity and discount value.

The contract discount metric is presented as **Contract Discount Value** rather than Total Contract Discount to avoid implying that the metric represents total contract value.

The underlying validation confirmed:

* Contract gross value: approximately $63.47B
* Contract discount value: approximately $6.35B
* Contract net value: approximately $57.12B
* Contract discount rate: 10%

The metric is therefore treated as a contract-level analytical measure rather than a direct comparison with total sales.

### Analytical Role

The Executive Overview acts as the entry point into the solution. Users can move from the overall business view into procurement, inventory, shipment, and customer/sales analysis on the remaining pages.

---

# 2. Supplier & Procurement

## Purpose

The Supplier & Procurement page focuses on purchasing activity, supplier exposure, purchasing cost, and procurement performance.

It supports the procurement-related questions around:

* Purchasing spend
* Supplier activity
* Purchase quantities
* Unit costs
* Purchase order volume
* Procurement lead time

### Key KPIs

The page includes:

* Total Purchase Value
* Total Purchase Quantity
* Purchase Order Count
* Average Purchase Unit Cost
* Average Purchase Lead Time
* Active Suppliers

These measures provide both volume and efficiency perspectives on procurement activity.

### Procurement Analysis

The page allows procurement activity to be analyzed across supplier and category dimensions.

The underlying model connects purchase order headers and purchase order lines through the purchasing model, allowing analysis at both order and line-item levels.

The final model contains:

* 500,000 purchase orders
* 1,500,000 purchase order lines
* 500 suppliers
* 20,000 products
* 10 product categories

### Analytical Role

The page helps users investigate purchasing concentration, supplier activity, purchasing cost, and lead-time patterns rather than relying only on total procurement spend.

The combination of purchase value, quantity, unit cost, supplier activity, and lead time provides a foundation for identifying areas requiring further procurement review.

---

# 3. Inventory Management

## Purpose

The Inventory Management page focuses on inventory movement, transaction activity, and potential inventory exposure and risk.

It supports the business question:

> Where are the major inventory and supply risks?

### Key KPIs

The final page uses KPI cards covering major inventory movement categories, including:

* Total Inventory Transaction Value
* Net Inventory Movement
* Purchase Receipt Quantity
* Sales Shipment Quantity
* Inventory Adjustment Quantity
* Stock Transfer Quantity

Additional inventory transaction measures are available for:

* Customer Returns
* Supplier Returns
* Inventory Transactions

### Inventory Transaction Model

The inventory analysis is based on the `fact_inventory_transaction` table containing 3,000,000 transaction records.

The final transaction distribution is:

| Transaction Type     |          Rows |
| -------------------- | ------------: |
| Purchase Receipt     |     1,500,000 |
| Sales Shipment       |       990,000 |
| Inventory Adjustment |       180,000 |
| Customer Return      |       150,000 |
| Supplier Return      |       120,000 |
| Stock Transfer       |        60,000 |
| **Total**            | **3,000,000** |

Stock transfers are represented as paired inventory movement events to support warehouse-level movement analysis.

### Inventory Risk Analysis

The page includes analytical views for inventory exposure and inventory health/risk.

Because the dataset does not contain a direct opening-stock or current-stock snapshot field, inventory risk is evaluated through transaction-based exposure and movement measures rather than presenting an unsupported current-stock balance.

The warehouse dimension was added to the final model to support warehouse-level inventory analysis.

The final model contains:

* 10 warehouses
* 3,000,000 inventory transactions

### Analytical Role

The page allows management to investigate where inventory is moving, which transaction types are driving activity, and where products or locations may require further investigation.

---

# 4. Shipment & Logistics

## Purpose

The Shipment & Logistics page focuses on sales-order status, order priorities, regional logistics, and shipment fulfillment performance.

It supports the business questions around:

* Shipment execution
* Fulfillment performance
* Quantity shortfalls
* Regional logistics
* Order status
* Order priority

### Order Status KPIs

The page contains KPI cards for the major sales-order statuses:

* Completed Orders
* Shipped Orders
* Processing Orders
* Cancelled Orders
* On Hold Orders

These measures provide an overview of the current distribution of sales-order activity.

### Shipment Fulfillment Analysis

A dedicated `Shipment Fulfillment` analytical table was created for shipment performance analysis.

This table is intentionally disconnected from the primary model to avoid introducing ambiguous relationships into the dimensional model.

Relevant attributes include:

* Sales Order
* Required Date
* Final Shipment Date
* Order Status
* Order Priority
* Shipping Region
* Ordered Quantity
* Shipped Quantity
* Timeliness
* Fulfillment Status
* Fulfillment Performance

### Fulfillment Performance

The final shipment validation produced the following results for eligible orders:

| Measure            |  Result |
| ------------------ | ------: |
| Eligible Orders    | 281,882 |
| On Time & Complete | 277,152 |
| On Time & Short    |   4,730 |
| Late & Complete    |       0 |
| Late & Short       |       0 |
| Overall On Time    |    100% |
| On Time & Complete |  98.32% |
| On Time & Short    |   1.68% |
| Total Units Short  |  50,601 |

The result shows that shipment timing was consistently within the required dates for eligible orders, while a smaller portion of orders experienced quantity shortfalls.

The dashboard therefore focuses on **fulfillment completeness and exceptions**, rather than presenting an on-time metric as the primary problem area.

### Monthly Fulfillment Analysis

The final dashboard includes a monthly shipment fulfillment percentage visual based on required shipment month.

The measure is filter-aware and responds to relevant dashboard selections, including:

* Product
* Category
* Shipping Region
* Order Priority
* Order Status

This allows users to move from the overall fulfillment result into specific product, regional, priority, or status segments.

### DAX and Filter Context

Because the `Shipment Fulfillment` table is disconnected from the primary model, `TREATAS` is used to transfer relevant filter context into the analytical table.

This approach avoids creating ambiguous fact-to-fact relationships while still allowing the shipment fulfillment analysis to respond to selections made elsewhere in the report.

### Analytical Role

The page provides management with visibility into order status and fulfillment exceptions while distinguishing between:

* On-time shipment performance
* Quantity completeness
* Order-level exceptions

This distinction prevents shipment timing and quantity fulfillment from being treated as the same analytical problem.

---

# 5. Customer & Sales

## Purpose

The Customer & Sales page focuses on sales activity, customer behavior, order volume, sales quantities, pricing, and discount patterns.

It supports the customer and commercial analysis component of the project.

### Core Sales Measures

The final Power BI solution includes measures for:

* Total Sales
* Gross Sales
* Sales Order Count
* Sales Order Line Count
* Total Sales Quantity
* Average Order Value
* Average Sales Price
* Total Sales Discount
* Sales Discount Rate

These measures allow sales activity to be analyzed from both revenue and transaction perspectives.

### Sales Order Model

The sales model contains:

* 300,000 sales orders
* 995,367 sales order lines
* 5,000 customers
* 20,000 products

Sales orders can be analyzed through dimensions such as product, category, customer, date, and other business attributes in the dimensional model.

### Sales Analysis

The page is designed to help identify patterns in:

* Sales volume
* Order activity
* Average order value
* Pricing
* Discounts
* Customer and product activity

The page intentionally does not repeat the order-status KPI cards already presented on the Shipment & Logistics page.

This keeps the analytical responsibilities of the two pages distinct:

* **Shipment & Logistics:** operational order status and fulfillment
* **Customer & Sales:** commercial activity, revenue, pricing, and customer analysis

---

# Power BI Modeling and Technical Implementation

## Dimensional Model

The Power BI solution uses the final 13-table dimensional model:

### Dimensions

* `dim_date`
* `dim_category`
* `dim_product`
* `dim_supplier`
* `dim_customer`
* `dim_contract`
* `dim_warehouse`

### Facts

* `fact_contract_line`
* `fact_purchase_order`
* `fact_purchase_order_line`
* `fact_sales_order`
* `fact_sales_order_line`
* `fact_inventory_transaction`

The model separates descriptive business dimensions from transactional fact tables to support reusable filtering and analytical measures.

---

## Filter Context and TREATAS

`TREATAS` was used where standard relationships were not appropriate, particularly for the disconnected shipment fulfillment analysis.

This allowed filters from the primary sales model to be transferred to the analytical fulfillment table without introducing ambiguous fact-to-fact relationships.

The approach was validated against independent Python calculations to ensure that dashboard results remained consistent with the underlying data.

---

## Disconnected Analytical Table

The `Shipment Fulfillment` table is a purpose-built analytical table rather than a replacement for the underlying sales model.

It was kept disconnected intentionally because directly connecting it to multiple fact tables could introduce ambiguous filter paths.

The table provides a controlled structure for:

* Shipment timing
* Shipment completeness
* Required dates
* Final shipment dates
* Order-level fulfillment classification

---

# KPI and Visual Design Approach

The Power BI report uses an enterprise-style dashboard design focused on analytical clarity rather than decorative visuals.

The report uses:

* KPI cards for high-level metrics
* Trend analysis for time-based performance
* Supplier and procurement analysis
* Inventory movement and risk analysis
* Shipment fulfillment analysis
* Customer and sales analysis
* Consistent filtering across relevant business dimensions

HTML-based KPI cards were also used in the report to provide a consistent management-dashboard presentation across pages.

The KPI design follows a consistent visual sequence across the report while keeping the underlying measures transparent and auditable.

---

# Validation of the Power BI Solution

Power BI results were validated against the certified dataset and independent validation outputs.

Validation included:

* KPI reconciliation
* Filter-context testing
* Product and category filtering
* Regional filtering
* Order priority filtering
* Order status filtering
* Shipment fulfillment reconciliation
* Date filtering
* Relationship testing

The shipment fulfillment results were independently validated before being incorporated into the final dashboard.

The final data-quality certification completed with **0 exceptions**.

This certification refers to the final data and business-rule validation process and should not be interpreted as meaning that the business dataset contains no operational exceptions. For example, shipment quantity shortfalls were intentionally identified as part of the analysis.

---

# Final Power BI Outcome

The completed Power BI solution converts the Northstar enterprise dataset into five connected analytical perspectives:

| Page                   | Primary Focus                                   |
| ---------------------- | ----------------------------------------------- |
| Executive Overview     | Management-level business activity              |
| Supplier & Procurement | Purchasing, suppliers, cost, and lead time      |
| Inventory Management   | Inventory movements and risk                    |
| Shipment & Logistics   | Order status and shipment fulfillment           |
| Customer & Sales       | Revenue, sales activity, pricing, and customers |

Together, the five pages provide a management-oriented analytical layer over the certified Northstar data model.

The solution demonstrates the progression from:

**Business requirements → dimensional data model → engineered transactional data → data-quality certification → DAX measures → filter-aware Power BI analysis → management reporting.**

The Power BI solution is therefore not treated as a standalone visualization exercise. It is the reporting layer of the broader Northstar analytics platform.
