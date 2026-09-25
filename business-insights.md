# Business Insights

## Overview

The Northstar Industrial Products Inc. Power BI solution was designed not only to report operational activity, but also to identify business patterns, exceptions, and areas requiring management attention.

The analysis connects procurement, supplier, inventory, shipment, and sales activity across the certified enterprise dataset.

The key insights below are based on the final Power BI solution and independent validation of the underlying data.

---

# 1. Procurement and Purchasing Insights

## Procurement represents a major analytical area

The procurement model contains:

* 500,000 purchase orders
* 1,500,000 purchase order lines
* 500 suppliers
* 20,000 products
* 10 product categories
* 12,000 contracts
* 500,000 contract lines

This scale provides sufficient transaction detail to analyze purchasing activity beyond total spend alone.

The Supplier & Procurement dashboard therefore combines:

* Purchase Value
* Purchase Quantity
* Purchase Order Count
* Average Purchase Unit Cost
* Average Purchase Lead Time
* Active Supplier Count

This allows procurement activity to be investigated from both **cost and operational perspectives**.

## Unit cost should be evaluated alongside purchasing volume

Total purchase value alone does not explain procurement performance.

The combination of purchase value, purchase quantity, and average purchase unit cost allows users to investigate whether higher purchasing expenditure is driven by:

* Higher purchase volumes
* Higher unit costs
* Supplier mix
* Product/category mix
* Changes in purchasing activity

This provides a stronger basis for procurement analysis than relying on spend totals alone.

## Supplier analysis requires multiple performance dimensions

The supplier dashboard was designed around multiple measures rather than a single supplier ranking.

Supplier activity can be examined using:

* Purchase value
* Purchase quantity
* Order activity
* Unit cost
* Lead time

This is important because supplier value cannot be represented adequately by purchase price alone. A supplier with a lower unit cost may have a different purchasing volume or lead-time profile.

The dashboard therefore provides the analytical foundation for identifying suppliers or purchasing areas that require deeper review.

---

# 2. Contract and Purchasing-Control Insights

## Contract discounts represent a significant analytical measure

The contract validation produced the following results:

| Contract Metric         | Validated Result |
| ----------------------- | ---------------: |
| Contract Gross Value    |         ~$63.47B |
| Contract Discount Value |          ~$6.35B |
| Contract Net Value      |         ~$57.12B |
| Contract Discount Rate  |              10% |

The contract discount value is calculated from the contract data and represents the value of negotiated discounts within the contract dataset.

It should not be interpreted as total company savings or as a direct comparison with total sales.

## Contract analysis can support purchasing-control review

Because contract data is connected to procurement activity, the model provides a foundation for investigating whether purchasing activity is occurring within expected contractual structures.

Potential areas for further analysis include:

* Contract utilization
* Supplier purchasing concentration
* Contract pricing
* Discount application
* Purchasing activity by category
* Purchasing activity by supplier

These areas can be investigated further without assuming that every purchasing transaction represents a control exception.

---

# 3. Inventory and Supply-Risk Insights

## Inventory activity is driven by multiple transaction types

The final inventory dataset contains 3,000,000 transactions.

| Transaction Type     |  Transactions |
| -------------------- | ------------: |
| Purchase Receipt     |     1,500,000 |
| Sales Shipment       |       990,000 |
| Inventory Adjustment |       180,000 |
| Customer Return      |       150,000 |
| Supplier Return      |       120,000 |
| Stock Transfer       |        60,000 |
| **Total**            | **3,000,000** |

Purchase receipts and sales shipments account for the majority of inventory movement, while adjustments, returns, and transfers provide additional signals about inventory activity.

## Inventory adjustments are an important control signal

Inventory adjustments represent 180,000 transactions in the certified dataset.

These transactions should not automatically be interpreted as errors.

Instead, adjustment activity provides a useful control signal because unusually high adjustment activity can warrant investigation into:

* Inventory accuracy
* Operational processes
* Receiving or shipping discrepancies
* Data-entry activity
* Warehouse processes

The dashboard therefore presents inventory adjustments as a distinct movement category rather than combining them with normal inventory flows.

## Returns provide additional operational context

Customer and supplier returns are separated from normal receipts and shipments.

This distinction allows management to investigate return activity independently rather than treating returns as ordinary inventory movement.

Returns can potentially indicate areas requiring further investigation around:

* Product quality
* Order accuracy
* Supplier performance
* Customer activity
* Reverse logistics

The dashboard provides visibility into these movements without assuming a specific cause.

## Warehouse analysis adds location-level context

The final model includes 10 warehouses through the `dim_warehouse` dimension.

This allows inventory activity to be analyzed at warehouse level and provides a location-based perspective for investigating inventory exposure and movement.

Because the dataset does not contain a certified opening-stock or current-stock snapshot, the dashboard does not present an unsupported current inventory balance.

Instead, inventory risk is analyzed through transaction activity and exposure measures.

---

# 4. Shipment and Fulfillment Insights

## Shipment timing was not the primary exception

Shipment fulfillment was independently validated using the final shipment dataset.

Among eligible orders:

| Fulfillment Measure |  Result |
| ------------------- | ------: |
| Eligible Orders     | 281,882 |
| On Time & Complete  | 277,152 |
| On Time & Short     |   4,730 |
| Late & Complete     |       0 |
| Late & Short        |       0 |
| Overall On-Time     |    100% |
| On Time & Complete  |  98.32% |
| On Time & Short     |   1.68% |

The analysis therefore shows that the primary fulfillment exception was **quantity completeness rather than shipment timing**.

## Quantity shortfalls are the key fulfillment exception

A total of 4,730 eligible orders were classified as:

**On Time & Short**

These orders represented 50,601 units of total shortfall.

The average shortfall was approximately 10.70 units per affected order.

This distinction is important.

The business did not show a late-shipment problem within the eligible population, but a smaller group of orders did experience incomplete fulfillment.

The dashboard therefore focuses on fulfillment completeness rather than presenting on-time performance as the main operational problem.

## Fulfillment performance is relatively consistent across the reporting period

Monthly fulfillment analysis shows that the complete-fulfillment rate generally remained around the high-98% range.

The highest monthly exception rate identified during validation was approximately 2.03% in August 2024.

This indicates that the shortfall issue was present across the reporting period rather than being limited to a single isolated month.

## Shortfalls can be investigated by operational dimensions

The shipment analysis allows fulfillment exceptions to be examined by:

* Product
* Category
* Shipping Region
* Order Priority
* Order Status
* Required shipment month

This enables management to move from the overall exception rate to the specific segments associated with incomplete fulfillment.

---

# 5. Sales and Customer Insights

## Sales analysis provides a commercial perspective

The Customer & Sales page contains measures covering:

* Total Sales
* Gross Sales
* Sales Order Count
* Sales Order Line Count
* Total Sales Quantity
* Average Order Value
* Average Sales Price
* Total Sales Discount
* Sales Discount Rate

The sales model contains:

* 300,000 sales orders
* 995,367 sales order lines
* 5,000 customers
* 20,000 products

This provides a detailed transaction-level foundation for analyzing commercial activity.

## Revenue should be considered alongside transaction behavior

Total sales provides a high-level view of revenue activity, but the supporting measures allow users to investigate how that revenue is generated.

For example:

* Sales order count provides transaction volume.
* Total sales quantity provides product volume.
* Average order value provides order-level revenue context.
* Average sales price provides pricing context.
* Sales discount measures provide discounting context.

Together, these measures provide a more complete view of sales performance than revenue alone.

## Customer analysis can support deeper segmentation

The customer dimension contains 5,000 customers.

The model allows customer activity to be analyzed alongside product, category, date, and sales-order information.

This creates a foundation for further analysis such as:

* Customer sales concentration
* Order frequency
* Average order value
* Product purchasing patterns
* Regional or channel activity where applicable

The current project focuses on providing the analytical foundation rather than making unsupported assumptions about customer behavior.

---

# 6. Cross-Functional Insights

One of the main strengths of the Northstar solution is that procurement, inventory, shipment, and sales data are not analyzed as completely independent processes.

The model provides a connected analytical view of the broader business flow:

**Supplier → Procurement → Inventory → Sales Order → Shipment → Customer**

This creates several opportunities for cross-functional investigation.

## Procurement and inventory

Purchasing activity can be examined alongside inventory movement to investigate whether purchasing patterns are consistent with downstream operational activity.

## Inventory and shipment fulfillment

Inventory movement and shipment fulfillment can be analyzed together to investigate whether quantity shortfalls are associated with specific products, categories, warehouses, or operational segments.

## Sales and inventory

Sales quantities can be considered alongside inventory movements to understand demand and fulfillment activity.

## Supplier and procurement

Supplier activity can be evaluated using purchase value, quantity, unit cost, and lead time rather than relying on a single supplier metric.

---

# 7. Key Management Findings

The final analysis produces several important findings:

### 1. Procurement analysis should go beyond total spend

Northstar's procurement activity is large enough that total purchase value alone does not provide sufficient insight.

Unit cost, purchasing volume, supplier activity, and lead time should be considered together when investigating procurement performance.

### 2. Contract discounts provide measurable purchasing value

The certified contract dataset contains approximately $6.35B in contract discount value against approximately $63.47B in gross contract value, representing a validated 10% discount rate within the contract dataset.

### 3. Inventory activity contains multiple operational signals

The 3M inventory transactions include receipts, shipments, adjustments, returns, and transfers.

Separating these transaction types provides better visibility into normal inventory movement versus activities that may warrant operational review.

### 4. Shipment timing was not the primary fulfillment issue

Eligible orders were 100% on time according to the final shipment validation.

The primary identified fulfillment exception was quantity shortfall.

### 5. Fulfillment completeness was 98.32%

Among eligible orders, 98.32% were both on time and complete, while 1.68% were on time but short.

This provides a measurable starting point for investigating incomplete fulfillment.

### 6. The shortfall issue affected a defined subset of orders

4,730 eligible orders experienced quantity shortfalls, representing 50,601 units.

Because the dashboard supports product, category, region, priority, and status filtering, these exceptions can be investigated at a more granular level.

### 7. Sales analysis benefits from combining revenue and transaction measures

The Customer & Sales page combines revenue, quantity, order activity, pricing, and discount measures to provide a broader commercial view.

---

# 8. From Insights to Management Action

The analysis provides evidence for several areas that management can investigate further.

## Procurement

* Review purchasing categories with significant unit-cost or spend exposure.
* Investigate supplier performance using cost, volume, and lead-time measures together.
* Review contract utilization and discount application.

## Inventory

* Investigate unusually high inventory adjustment activity.
* Review return patterns by product, supplier, or warehouse.
* Examine warehouse-level inventory movement and exposure.

## Shipment Fulfillment

* Investigate the products and categories associated with quantity shortfalls.
* Examine whether shortfalls are concentrated in particular regions, priorities, or order statuses.
* Monitor monthly fulfillment completeness.

## Sales

* Examine customer and product concentration.
* Review average order value and pricing patterns.
* Analyze discount activity alongside sales performance.

These actions are analytical follow-ups rather than conclusions that every identified pattern represents a confirmed business problem.

---

# 9. Business Value of the Solution

The Northstar project demonstrates how an enterprise analytical model can move from raw transactional data to management-oriented insights.

The overall analytical process was:

**Business Questions**
→ **Data Model**
→ **Data Engineering**
→ **Data Quality Validation**
→ **Power BI Modeling**
→ **KPI Development**
→ **Exception Analysis**
→ **Business Insights**

The solution provides management with a structured way to investigate:

* Procurement cost
* Supplier performance
* Contract value
* Inventory movement
* Inventory risk
* Shipment fulfillment
* Sales activity
* Customer activity

The project therefore demonstrates both the technical construction of an analytics platform and the business interpretation of the resulting data.

---

# Final Insight Summary

The most important validated finding from the shipment analysis is that Northstar's primary fulfillment exception was **quantity completeness rather than shipment timing**.

At the broader enterprise level, the project demonstrates the value of combining procurement, inventory, shipment, and sales data within a single dimensional analytical environment.

The resulting Power BI solution gives management a consistent framework for identifying cost exposure, operational exceptions, and areas requiring deeper investigation while maintaining a clear separation between validated findings and assumptions requiring further analysis.
