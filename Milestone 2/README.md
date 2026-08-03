# Supply Chain Analytics: Inventory & Logistics Performance Dashboard

A comprehensive Power BI dashboard developed to analyze inventory utilization and logistics performance. The project integrates inventory management metrics with delivery insights, enabling supply chain teams to monitor stock movement, optimize inventory levels, and evaluate shipping efficiency through interactive reports.

---

# Project Highlights

* **Inventory Performance:** Tracked inventory movement to identify slow-selling and inactive products while improving stock utilization.
* **Logistics Monitoring:** Evaluated scheduled versus actual shipment durations to measure delivery efficiency.
* **Interactive Analysis:** Built drill-down hierarchies for location, product categories, and time to support detailed business analysis.

---

# Data Model

The solution is built using a **Star Schema** where a centralized **Fact Table** stores transactional sales, inventory, and shipping information, connected to multiple dimension tables.

## Available Hierarchies

### Geography

`Region → Order Country → Order City`

### Product

`Category Name → Product Name`

### Time

`Order Date → Quarter → Year`

---

# Inventory Analytics

## Inventory Turnover Ratio

This KPI measures how effectively inventory is converted into sales over a selected period.

### Formula

[
\text{Inventory Turnover Ratio} =
\frac{\text{Total Sales}}
{\text{Average Inventory Value}}
]

### DAX

```DAX
Total Sales =
SUM(Fact_table[sales])

Avg Inventory Value =
AVERAGE(Fact_table[inventory_value])

Inventory Turnover Ratio =
DIVIDE([Total Sales], [Avg Inventory Value], 0)
```

### Why It Matters

* Measures inventory utilization.
* Detects slow-moving products.
* Supports better inventory planning and replenishment.

---

## Days Since Last Sale

Calculates the number of days since a product's most recent sale by comparing its latest transaction date with the latest available date in the dataset.

### DAX

```DAX
Days Since Last Sale (Col) =
VAR LastSale =
    CALCULATE(
        MAX(Fact_table[order_date_(dateorders)]),
        ALLEXCEPT(Fact_table, Fact_table[product_name])
    )

VAR MaxDate =
    CALCULATE(
        MAX(Fact_table[order_date_(dateorders)]),
        ALL(Fact_table)
    )

RETURN
    DATEDIFF(LastSale, MaxDate, DAY)
```

### Business Value

* Highlights products with low sales activity.
* Identifies potential dead stock.
* Assists in inventory optimization decisions.

---

# Delivery & Logistics Analytics

## On-Time Delivery Rate

Shows the percentage of customer orders delivered within the expected timeline.

### DAX

```DAX
On-Time Delivery Rate =
DIVIDE(
    CALCULATE(
        COUNTROWS(Fact_table),
        Fact_table[Late_delivery_risk] = 0
    ),
    COUNTROWS(Fact_table),
    0
)
```

### Benefits

* Tracks delivery reliability.
* Evaluates logistics performance.
* Supports service-level improvements.

---

## Average Actual Shipping Days

Calculates the average time taken for completed shipments.

### DAX

```DAX
Avg Shipping Days Real =
AVERAGE(Fact_table[Days for shipping (real)])
```

---

## Average Scheduled Shipping Days

Calculates the average planned shipment duration.

### DAX

```DAX
Avg Shipping Days Scheduled =
AVERAGE(Fact_table[Days for shipment (scheduled)])
```

---

## Average Delivery Delay

Measures the average deviation between planned and actual shipping durations.

### DAX

```DAX
Avg Delivery Delay Days =
[Avg Shipping Days Real] -
[Avg Shipping Days Scheduled]
```

### Business Value

* Identifies shipment delays.
* Measures logistics efficiency.
* Helps uncover fulfillment bottlenecks.

---

# Dashboard Capabilities

* Inventory Turnover Monitoring
* Slow-Moving & Dead Stock Analysis
* Days Since Last Sale Tracking
* On-Time Delivery KPI
* Shipping Delay Analysis
* Regional Performance Dashboard
* Product Category Insights
* Interactive Drill-Down Navigation
* Dynamic Filters and KPI Cards

---

# Business Outcomes

* Improved visibility into inventory health and stock movement.
* Supported identification of slow-moving inventory to reduce carrying costs.
* Enabled continuous monitoring of delivery performance using operational KPIs.
* Facilitated informed supply chain decisions through interactive reporting.
* Delivered detailed regional and product-level insights for operational improvement.
