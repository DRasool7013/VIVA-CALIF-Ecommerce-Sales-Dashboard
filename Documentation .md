# Documentation — VIVA CALIF Ecommerce Sales Dashboard

## 1. Project Aim / Goal

VIVA CALIF, a fictional apparel ecommerce brand operating in California, needed a single dashboard to help stakeholders quickly answer core sales and operations questions — order volume, revenue, how customers shop, which products sell, delivery speed, and customer satisfaction — without digging through raw transaction data.

The goal of this project was to design and build a **fully interactive, single-page Excel dashboard** that:
- Surfaces the 5 headline KPIs at a glance
- Answers 7 specific business questions with the right chart type for each
- Lets any user filter the entire dashboard by **Order Mode** and **Gender Value** using slicers

---

## 2. Dataset Description

**File:** **[`ecommerce-blank_dataset.xlsx`](https://github.com/DRasool7013/VIVA-CALIF-Ecommerce-Sales-Dashboard/blob/main/ecommerce-blank%20dataset.xlsx)** (sheet: `Data`)
**Size:** 2,400 rows × 15 columns

| Column | Description |
|---|---|
| `TX ID` | Unique transaction identifier |
| `Product` | Product name (20 unique products — T-Shirts, Jeans, Sneakers, Dresses, etc.) |
| `Quantity` | Units purchased in the order |
| `Unit Price` | Price per unit ($) |
| `Amount` | Total order value (Quantity × Unit Price) |
| `Order Date` | Date the order was placed (Jan 1 – Mar 29, 2025) |
| `Ship Date` | Date the order shipped |
| `Customer Gender` | Raw gender code from source system (`F`, `M`, `O`, or blank) |
| `Order Mode` | Channel used to order — App, Instagram, Partner App, Target.com, Website |
| `Rating C` | Customer satisfaction rating, 1–5 |
| `State` | Customer state (California for all records) |
| `County` | Customer county (58 unique CA counties) |
| `Days to Deliver` | Ship Date − Order Date, in days (range: 0–14) |
| `Weeknum` | Week number of the order within the 13-week window |
| `Gender Value` | Cleaned/labeled version of `Customer Gender` |

---

## 3. Data Cleaning Process

1. **Gender standardization** — `Customer Gender` came in as raw single-letter/blank codes. A `Gender Value` column was added to map them into readable labels:
   - `F` → Female (1,231 orders)
   - `M` → Male (959 orders)
   - `O` → Other (75 orders)
   - *blank* → Unknown (135 orders)

2. **Delivery time calculation** — `Days to Deliver` was derived as `Ship Date − Order Date` for every row, giving a 0–14 day range used to build the shipping-speed chart.

3. **Week bucketing** — `Weeknum` was derived from `Order Date` to bucket each transaction into its week (1–13) of the reporting period, powering the 13-week trend chart.

4. **Quantity bucketing (chart-level)** — for the "How many they buy?" chart, `Quantity` was grouped into buckets (1, 2, 3, 4, 5, 6–10, More than 10) inside the PivotTable's row grouping.

5. **Geography validation** — `State`/`County` fields were checked against Excel's Geography data type so the Filled Map chart could resolve every county correctly.

No rows were removed — all 2,400 transactions are used; "Unknown" and "Other" gender values are retained as their own categories rather than dropped, so the dashboard stays representative of the full dataset.

---

## 4. Dashboard Build Process

### 4.1 Workbook structure
The workbook (`ecommerce-blank_excel_project.xlsx`) has 4 sheets:
- **Data** — the cleaned source table
- **Questions & KPIs** — planning sheet listing the KPIs and business questions the dashboard needed to answer
- **Sheet1** — helper sheet holding supporting pivot/chart data (e.g., the week-trend series)
- **Dashboard** — the final published, single-page dashboard

### 4.2 KPI cards
Five KPI cards (Orders, Quantity, Amount, Avg. Rating, Days to Deliver) were built from PivotTable summary values referenced into styled cells/shapes with icons (🛒 💰 📅 ⭐ 👕), so they recalculate live with the slicers.

### 4.3 Charts (each built from its own PivotTable)

| Chart | Type | Fields used |
|---|---|---|
| Last 13-Week Trends | Line chart, 2 series | `Weeknum` (axis) vs. Sum of `Quantity` & Sum of `Amount` |
| How they like to buy | Concentric ring/donut chart | `Order Mode` (rings) segmented by `Gender Value` |
| How many they buy? | Column chart | Quantity buckets (1–5, 6–10, 10+) vs. count of `TX ID` |
| Which products are popular? | Horizontal bar + mini donut | `Product` vs. Sum of `Quantity`, broken down by `Gender Value` |
| Where do our customers live? | Filled Map (native Excel Region Map) + inset overview map | `County`/`State` vs. Sum of `Quantity` |
| How long we take to ship | Column chart | `Days to Deliver` (0–14) vs. count of `TX ID` |
| How satisfied are our customers? | Stacked column | Month vs. count of `Rating C` (1–5, color-coded) |

### 4.4 Slicers
- **Order Mode** and **Gender Value** slicers were inserted from the PivotTable Analyze tab and connected to every PivotTable on the dashboard via **Report Connections**, so one click filters all 7 charts and all 5 KPI cards simultaneously (see step-by-step in the README).

### 4.5 Layout & styling
- Left-hand panel: KPI cards stacked above the two slicers, on a dark gold/brown theme matching the "VIVA CALIF" branding.
- Main canvas: a 2×3+map grid of charts, bordered and titled to read like a stakeholder-ready report.

---

## 5. Final Result

**Unfiltered dataset totals** (all 2,400 orders):

| KPI | Value |
|---|---|
| Orders | 2,400 |
| Quantity | 11,997 units |
| Amount | $649,019.80 |
| Avg. Rating | 3.96 ≈ 4.0 |
| Avg. Days to Deliver | 2.34 ≈ 2.3 |

**Example filtered view** (as shown in the dashboard screenshot, with slicers applied):

| KPI | Value |
|---|---|
| Orders | 2,400 |
| Quantity | 447 |
| Amount | $31.4k |
| Avg. Rating | 4.0 |
| Days to Deliver | 2.1 |

This demonstrates the dashboard's core interactivity — the same KPI cards and charts recompute instantly for any Order Mode / Gender Value combination selected.

---

## 6. Outcomes & Insights

- **Product mix:** T-Shirts (325 orders), Jeans (286), and Sneakers (247) are the top 3 sellers; Jewelry (17) and Tote Bags (33) are the slowest movers.
- **Channel mix:** App (868 orders, ~36%) and Website (569, ~24%) together drive the majority of volume; Instagram is the smallest channel (248 orders).
- **Delivery performance:** Most orders ship in a **0–3 day** window, with a long tail out to 14 days — a candidate area for operational improvement.
- **Customer satisfaction:** Ratings skew positive, with **4s and 5s making up ~73%** of all ratings (1,757 of 2,400); only 21 orders received a 1-star rating.
- **Demand seasonality:** The 13-week trend line shows a clear demand spike around weeks 10–11, useful for future inventory/staffing planning.
- **Customer geography:** Orders span all 58 California counties, with visible concentration in major metro counties — useful for regional marketing targeting.

---

## 7. Repository File Guide

| File | Purpose |
|---|---|
| **[`ecommerce-blank_dataset.xlsx`](https://github.com/DRasool7013/VIVA-CALIF-Ecommerce-Sales-Dashboard/blob/main/ecommerce-blank%20dataset.xlsx)**| Standalone cleaned dataset (source of truth for the `Data` sheet) |
| **[`ecommerce-blank_excel_project.xlsx`](https://github.com/DRasool7013/VIVA-CALIF-Ecommerce-Sales-Dashboard/blob/main/ecommerce-blank%20excel%20project.xlsx)**| Full working Excel file — Data, Questions & KPIs, Dashboard, and helper sheet, with all   PivotTables/PivotCharts/Slicers intact |
| **[`ecommerse-blank_dashboard.png`](https://github.com/DRasool7013/VIVA-CALIF-Ecommerce-Sales-Dashboard/blob/main/ecommerse-blank_dashboard.png)**| Final rendered dashboard screenshot |
| **[Questions___KPIs.png](Documentation.md)**| Original planning notes — KPIs and business questions scoped before building |
| `README.md` **[README.md](https://github.com/DRasool7013/VIVA-CALIF-Ecommerce-Sales-Dashboard/blob/main/README.md)**| 
| **[Documentation.md](Documentation.md)** | This file — full build and analysis documentation |
