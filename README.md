# Portfolio-Project-1
# Superstore Sales Analysis

An end-to-end data analysis portfolio project: raw data → cleaning → EDA → interactive
Excel dashboard. Built to demonstrate a full analyst workflow using Python (pandas) for
data prep and Excel for analysis and reporting.

**[Download the workbook: `Superstore_Sales_Analysis.xlsx`](./Superstore_Sales_Analysis.xlsx)** ·

## Project overview

| | |
|---|---|
| **Dataset** | Synthetic Superstore-style retail order data (2,235 rows) — same schema as the well-known "Sample Superstore" dataset, generated with realistic seasonality, category mix and margin patterns, and deliberately seeded with the kinds of data-quality issues real exports have. |
| **Tools** | Python (pandas) for generation & cleaning · Excel (openpyxl-built) for analysis, formulas and dashboarding |
| **Skills shown** | Data cleaning, EDA, pivot-style aggregation, KPI dashboard design, formula-driven interactivity, documentation |

## 1. The raw data

`data/raw_superstore.csv` — 2,235 orders with the schema: `Order ID`, `Order Date`, `Ship Date`,
`Ship Mode`, `Customer Name`, `Segment`, `Region`, `State`/`City`, `Category`, `Sub-Category`,
`Product Name`, `Sales`, `Quantity`, `Discount`, `Profit`.

Generated with `scripts/generate_raw_data.py`, with intentional real-world messiness injected:
mixed date formats (`MM/DD/YYYY` text mixed with proper dates), inconsistent text casing
(`STANDARD CLASS` vs `Standard Class`), stray whitespace, missing postal codes/discounts,
34 exact duplicate rows, and a handful of data-entry typos (negative quantities, a ship date
before the order date).

<img width="388" height="392" alt="image" src="https://github.com/user-attachments/assets/91dbb2a2-0d71-418a-a1c4-18bee487f2a6" />


## 2. Cleaning

`scripts/clean_data.py` is the pandas pipeline that turns the raw export into an analysis-ready
table. Every step is logged with the number of rows it touched — see `data/cleaning_log.csv`
or the `Cleaning_Log` tab in the workbook:

| Step | Rows affected |
|---|---|
| Trim whitespace in Customer Name | 132 |
| Standardize Ship Mode casing | 179 |
| Standardize Category casing | 180 |
| Parse mixed date formats into one datetime type | 2,235 |
| Remove exact duplicate rows | 34 |
| Fix negative Quantity typos | 10 |
| Fix Ship Date earlier than Order Date | 6 |
| Flag zero-Sales rows for review (kept, not dropped) | 8 |
| Fill missing Postal Code | 66 |
| Fill missing Discount | 44 |

Result: **2,201 clean rows** in `data/cleaned_superstore.csv`, with derived columns added
(`Order Year`, `Order Month`, `Order Year-Month`, `Profit Margin`, `Shipping Days`) to make
downstream aggregation simpler.

## 3. Exploratory analysis

The `EDA_Summary` tab in the workbook breaks the cleaned data down by Category, Region,
Segment, Sub-Category, month, top products, top customers, and discount level — all as
`SUMIFS`/`AVERAGEIF` formulas against the cleaned table, so they recalculate if the data changes.

<img width="456" height="162" alt="image" src="https://github.com/user-attachments/assets/4a879a13-ca6e-4cf0-bb93-1c865cd30cd0" />

<img width="226" height="89" alt="image" src="https://github.com/user-attachments/assets/1b19d713-05c4-4984-9ca2-43960531c0c3" />

**Headline findings:**
- Overall profit margin is thin — **2.3%** on $4.22M of sales.
- **Furniture loses money overall** (-2.0% margin on $1.12M sales), dragged down by Chairs
  and Furnishings; Office Supplies is the steadiest category at **10.7%** margin.
- **Discount is the biggest lever on profitability**: average margin is ~18% with no
  discount, crosses into negative territory past a 20% discount, and falls to roughly
  **-27% at 50% off**.
- West and South regions run thinner margins (~1.3–1.4%) than Central and East (~3.0–3.2%),
  despite comparable sales volumes — worth a follow-up into regional discount practices.

## 4. Visualization with Python

## Total Sales by Category
<img width="550" height="434" alt="image" src="https://github.com/user-attachments/assets/fe63f30b-8c77-4458-9539-3aee580eb536" />

## Total Profit by Category
<img width="574" height="434" alt="image" src="https://github.com/user-attachments/assets/8f712393-d1b6-4c9f-b327-60ecd124ed90" />

## Profit by regions
<img width="883" height="468" alt="image" src="https://github.com/user-attachments/assets/71b98525-f03f-41d7-99e5-7958291c70a6" />

## Monthly sales trend by years
<img width="1093" height="399" alt="image" src="https://github.com/user-attachments/assets/f003a382-9df7-46af-bfb8-e6e80ccb0921" />


## 5. Interactive dashboard (Excel)

The `Dashboard` tab has three dropdown filters (Region, Category, Year) built with Excel data
validation — no macros. Every KPI card and chart is a formula bound to those dropdowns
(using a wildcard-matching pattern so "All" removes the filter), so picking a value
recalculates the whole sheet, including the charts, live in Excel.

Charts included: Sales by Category, Sales by Region, a monthly sales trend line, and Top
Sub-Categories by Profit.




## Notes on the data

This is a **synthetic** dataset generated to mirror the structure and statistical feel of the
public "Sample Superstore" dataset commonly used for BI practice — it is not scraped or copied
from any proprietary source, so it's safe to publish and reuse.

