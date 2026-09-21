# 📊 Sales Performance Dashboard | Power BI

An end-to-end Power BI project analyzing revenue, profit, growth %, regional performance, and top-selling products for a simulated retail sales dataset — built from raw, intentionally messy data all the way through to a fully interactive, multi-page dashboard.

## 🎯 Project Overview

This project demonstrates the complete analytics workflow a data analyst follows in a real job: starting from dirty, inconsistent raw data and ending with a polished, decision-ready dashboard. The dataset (~25,800 rows) was deliberately generated with real-world data quality issues — inconsistent date formats, currency symbols, typos, duplicates, and outliers — to practice and showcase data cleaning skills alongside dashboard design.

## 🛠️ Tools & Skills Used

- **Power BI Desktop** — data modeling, DAX, report design
- **Power Query (M)** — data cleaning and transformation
- **Python (Pandas)** — alternate data cleaning pipeline
- **DAX** — KPI measures, time intelligence, ranking
- **Data Modeling** — star schema with a dedicated Date table

## 🧹 Data Cleaning

Cleaned the same raw dataset using **two different approaches** to compare methods and outcomes:

**Python (Pandas)**
- Standardized 6+ mixed date formats
- Removed currency symbols (₹, "Rs.", commas) and converted to numeric
- Fixed text casing and typos (Region, State, Payment Method)
- Removed duplicate and corrupted rows
- Handled outliers in Profit using the IQR method
- Missing values: dropped rows missing critical fields (Sales, OrderDate); filled secondary fields (Region, State, Customer Name) with "Unknown"
- Final dataset: **22,835 clean rows**

**Power Query**
- Repeated the full cleaning process natively in Power BI's UI
- Used Column Quality/Distribution profiling, Replace Values, Number/Text filters, and Remove Duplicates
- Final dataset: **15,864 clean rows** (a stricter drop-based approach)

📌 *Comparing both approaches surfaced an important real-world lesson: aggressive dropping of incomplete rows across many columns compounds data loss quickly, whereas filling non-critical fields preserves more usable business data. The Python-cleaned dataset was used as the final source for the dashboard.*

## 🗂️ Data Model

- Star schema with a dedicated **Date table** (built via `CALENDAR()`, marked as an official Date table)
- One-to-many relationship between the Date table and the Sales fact table
- Enables clean time intelligence (YoY/MoM growth, YTD running totals)

## 📐 Key DAX Measures

```
Total Revenue = SUM(Sales)
Total Profit = SUM(Profit)
Total Orders = DISTINCTCOUNT(OrderID)
Profit Margin % = DIVIDE([Total Profit], [Total Revenue], 0)
MoM Growth % = DIVIDE([Total Revenue] - [Previous Month Revenue], [Previous Month Revenue], 0)
YoY Growth % = DIVIDE([Total Revenue] - [Previous Year Revenue], [Previous Year Revenue], 0)
Average Order Value = DIVIDE([Total Revenue], [Total Orders], 0)
Running Total Revenue = TOTALYTD([Total Revenue], DateTable[Date])
Product Rank = RANKX(ALL(Product), [Total Revenue])
```

## 📑 Dashboard Pages

1. **Dashboard** — KPI cards (Revenue, Profit, Orders, Margin %), regional performance bar chart, top 10 products chart, revenue trend line, and Region/Category/Date slicers
2. **Product Details** — Drill-through page showing order-level detail for any selected product
3. **Insights** — Region × Category matrix and Payment Method distribution
4. **Data Notes** — Transparency page documenting the cleaning decisions and data limitations

## 🔍 Key Insights

- Electronics is the top-performing category across every region
- West region leads in total revenue among the five regions
- Clear month-to-month revenue fluctuation, useful for spotting seasonal patterns

## 📈 Skills Demonstrated

Data cleaning (Python & Power Query) · Data modeling · DAX (time intelligence, ranking) · Interactive report design · Drill-through analysis · Data quality documentation
