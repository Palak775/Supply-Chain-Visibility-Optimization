# Transportation & Supplier Performance Analytics Dashboard

## Overview

This project is an end-to-end supply chain analytics solution developed to monitor supplier performance, evaluate transportation efficiency, compare carrier effectiveness, and support operational decision-making. The dashboard provides actionable insights that help reduce logistics costs, improve delivery performance, optimize supplier selection, and minimize supply chain disruptions across multiple regions.

## Data Pipeline & Architecture

### Data Collection & Preparation

Transactional shipment records and supplier datasets are imported into Power Query, where they are cleaned and transformed by removing incomplete records, standardizing date formats, correcting inconsistencies, and mapping regional codes.

### Data Model

The dashboard follows a **Star Schema** model to improve query efficiency and optimize report performance.

**Fact Table**

* Fact_Shipments

  * Order ID
  * Ship Date
  * Freight Cost
  * Discount
  * Profit
  * Delivery Status

**Dimension Tables**

* Dim_Supplier
* Dim_ShippingMode
* Dim_Geography
* Dim_Calendar

### Analytics Layer

Power BI DAX measures are used to calculate dynamic KPIs, perform time-based analysis, generate supplier rankings, and evaluate transportation performance.

---

# Performance Evaluation Methodology

## 1. Supplier Performance Score

Supplier performance is assessed using a weighted scorecard based on three major operational indicators.

**Supplier Score**

Supplier Score = (W1 × On-Time Delivery) + (W2 × Quality Score) + (W3 × Cost Efficiency)

### Performance Components

**On-Time Delivery (Weight: 40%)**

OTD = (On-Time Deliveries / Total Completed Orders) × 100

Measures the percentage of shipments delivered within the expected timeline.

**Quality Performance (Weight: 35%)**

Quality Score = 100 − ((Returned or Damaged Units / Total Delivered Units) × 100)

Reflects supplier reliability by considering damaged or returned shipments.

**Cost Efficiency (Weight: 25%)**

Evaluates supplier pricing based on average discounts offered and freight cost competitiveness compared with market benchmarks.

---

## 2. Supplier Benchmarking Framework

Suppliers are classified into four performance categories using their overall weighted score.

| Performance Level | Score Range | Status             | Recommended Action                                                 |
| ----------------- | ----------- | ------------------ | ------------------------------------------------------------------ |
| Tier 1            | ≥ 90        | Preferred Supplier | Prioritize for long-term contracts and higher shipment allocation. |
| Tier 2            | 75–89       | Reliable           | Maintain partnership while pursuing continuous improvements.       |
| Tier 3            | 60–74       | Needs Improvement  | Monitor through performance reviews and corrective action plans.   |
| Tier 4            | < 60        | High Risk          | Reduce allocation or evaluate supplier replacement.                |

### Benchmarking Metrics

* Geographic peer comparison to minimize regional delivery bias.
* Profit-per-shipment analysis adjusted for shipment volume.
* Supplier comparison across market segments and transportation modes.

---

## 3. Transportation Cost Analysis

Transportation efficiency is measured by examining freight expenses, shipment volume, discounts, and profitability across shipping methods such as Standard, Second Class, First Class, and Same Day.

### Key Performance Indicators

**Average Profit per Order**

Average Profit = Total Profit ÷ Total Orders

**Average Discount Percentage**

Average Discount Rate = Total Discount Amount ÷ Total Gross Sales

**Cost-to-Volume Efficiency**

Analyzes whether higher shipment volumes generate lower transportation costs while maintaining healthy profit margins.

---

## 4. Route & Carrier Performance Analysis

Shipping routes and carriers are evaluated using two primary dimensions:

* Delivery Reliability (Late Delivery Percentage)
* Profit Contribution

This analysis helps identify the most dependable carriers, optimize transportation routes, and improve overall logistics performance through data-driven decision-making.

