# Supply Chain Performance & Operational Analytics Dashboard

## Overview
This repository contains the technical documentation, data modeling architecture, and visualization framework for the Supply Chain Performance & Operational Analytics Power BI dashboard. The solution provides real-time tracking of order fulfillment, warehouse capacity utilization, supplier reliability, and interactive cross-report drill-through capabilities.

---

## Technical Visual Architecture & DAX Measures

### 1. Primary Metric Cards (KPI Deck)
The header deck provides immediate high-level visibility across fulfillment performance and network scope.

* **Total Orders**
  * Expression: `Total Orders = DISTINCTCOUNT(Fact_SupplyChain[order_id])`
  * Format: Standard Integer / Compact (`K`)

* **Active Categories**
  * Expression: `Active Categories = DISTINCTCOUNT(Fact_SupplyChain[product_category_id])`
  * Format: Whole Number

* **Fulfillment Rate (%)**
  * Expression: `Fulfillment Rate = DIVIDE([Total Orders] - [Canceled Orders], [Total Orders], 0)`
  * Format: Percentage (`0.00%`)

* **Active Warehouses**
  * Expression: `Active Warehouses = DISTINCTCOUNT(Dim_Warehouse[warehouse_id])`
  * Format: Whole Number

* **Supplier Count**
  * Expression: `Supplier Count = DISTINCTCOUNT(Dim_Supplier[supplier_id])`
  * Format: Whole Number

* **Dead Stock Volume**
  * Expression:
    ```dax
    Dead Stock Volume = 
    VAR DeadStockItems = 
        FILTER(
            VALUES(Fact_SupplyChain[product_id]),
            CALCULATE(MAX(Fact_SupplyChain[inventory_status])) = "Dead Stock"
        )
    RETURN
        SUMX(DeadStockItems, CALCULATE(AVERAGE(Fact_SupplyChain[stock_qty])))
    ```
  * Format: Compact Decimal (`M`)

---

### 2. Operational Gauge Visuals (Performance vs Targets)

* **Late Delivery Rate**
  * DAX Logic:
    ```dax
    Delayed Orders = 
    CALCULATE(
        DISTINCTCOUNT(Fact_SupplyChain[order_id]),
        Fact_SupplyChain[delivery_status] = "Late delivery"
    )

    Late Delivery % = DIVIDE([Delayed Orders], [Total Orders], 0)
    Late Delivery Target = 0.20
    ```
  * Gauge Setup: Minimum = `0.00`, Maximum = `1.00`, Target = `0.20`

* **Supplier Reliability Index**
  * Source Measure: `Supplier Reliability % = AVERAGE(Dim_Supplier[reliability_score])`
  * Gauge Setup: Minimum = `0`, Maximum = `100`

* **Network Utilization Index**
  * Source Measure: `Avg Utilization % = AVERAGE(Dim_Warehouse[utilization_pct])`
  * Target Measure: `Utilization Target = 80.00`
  * Gauge Setup: Minimum = `0`, Maximum = `100`, Target = `80`

* **Inventory Turnover Ratio**
  * DAX Logic:
    ```dax
    Avg Inventory Value = 
    SUMX(
        VALUES(Fact_SupplyChain[product_id]),
        CALCULATE(AVERAGE(Fact_SupplyChain[inventory_value]))
    )

    Inventory Turnover Ratio = DIVIDE([Total Revenue], [Avg Inventory Value], 0)
