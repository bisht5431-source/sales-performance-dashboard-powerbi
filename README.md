<div align="center">

<img src="Banner.svg" alt="Sales Performance Dashboard Banner" width="90%">

# 📊 Sales Performance Dashboard — Power BI

**An end-to-end sales analytics dashboard built to track revenue, profit, and performance across regions, categories, and salespeople.**

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)](https://learn.microsoft.com/en-us/dax/)
[![Power Query](https://img.shields.io/badge/Power%20Query-742774?style=for-the-badge&logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/power-query/)
[![Status](https://img.shields.io/badge/Status-Complete-4CAF7D?style=for-the-badge)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-F2A541?style=for-the-badge)](LICENSE)

[View Report](#-dashboard-pages) • [DAX Measures](#-dax-measures) • [Key Insights](#-key-insights) • [Contact](#-author--contact)

</div>

---

## 📌 Overview

This project analyzes **300 sales transactions** across 4 regions, 4 product categories, and 8 salespeople to help sales leadership answer four questions at a glance:

- Where is revenue and profit coming from — which regions and categories are driving the business?
- Who are the top and bottom performing salespeople?
- How is profit margin trending over time?
- Where should resources be reallocated next quarter?

The report is built as a **4-page interactive Power BI dashboard**, fully slicer-driven, with every visual responding live to Region, Category, Salesperson, and Order Year filters.

---

## 🧮 Dataset

| Field | Description |
|---|---|
| `OrderID`, `OrderDate` | Unique order identifier and transaction date |
| `Region`, `Category`, `Product` | Where and what was sold |
| `Salesperson` | Who closed the order |
| `Quantity`, `UnitPrice`, `Discount` | Transaction-level pricing detail |
| `Sales`, `Cost`, `Profit` | Core financial metrics |

**Coverage:** 300 orders · Jan 2024 – Aug 2026 · 4 Regions · 4 Categories · 8 Salespeople

---

## 🖥️ Dashboard Pages

| Page | What it shows |
|---|---|
| **1. Comparison** | KPI summary cards + Sales, Profit and Cost compared side-by-side across Region and Category |
| **2. Profit Visuals** | Profit margin trend over time, monthly profit trend, profit breakdown by price tier and category |
| **3. Top and Bottom** | Best and weakest performing products and salespeople, ranked by profit and sales |
| **4. Result by Regions** | Region-level deep dive — Sales vs Profit, order volume, quantity sold, and average order value by region |

> 📷 *Add your dashboard screenshots here — export each page as PNG from Power BI (File → Export → PDF, then convert pages to PNG) and drop them in an `/assets/screenshots` folder, then reference them like:*
> `![Comparison Page](assets/screenshots/comparison.png)`

---

## 🔑 Key Insights

- **Total Sales: ₹71.2L** generated across 300 orders, with an overall **profit margin of 33.4%**
- **South** is the top-performing region (₹22.8L in sales), while **West** lags behind at ₹11.1L — a clear resource-reallocation signal
- **Furniture** dominates category-wise sales (₹42.6L, ~60% of total revenue), far ahead of Electronics, Clothing, and Office Supplies
- **Priya Nair** is the top-performing salesperson by revenue (₹13.5L in sales)
- Average Order Value stands at **₹23,750**, giving a benchmark to track upsell/cross-sell performance against

---

## 🧠 DAX Measures

All calculations are split into three deliberate layers — a structure built to keep the model fast and every card/chart fully dynamic under slicers.

<details>
<summary><b>1. Calculated Columns</b> (row-level, used for slicing/grouping)</summary>

```DAX
Profit Margin % (Row) = DIVIDE([Profit], [Sales], 0)

Discount Amount = [Quantity] * [UnitPrice] * [Discount]

Order Year = YEAR([OrderDate])

Order Month = FORMAT([OrderDate], "MMM YYYY")

Price Tier = IF([UnitPrice] > 10000, "Premium", IF([UnitPrice] > 1000, "Mid", "Budget"))
```
</details>

<details>
<summary><b>2. Core Measures</b> (reusable building blocks)</summary>

```DAX
Total Sales = SUM('Sales'[Sales])
Total Cost = SUM('Sales'[Cost])
Total Profit = SUM('Sales'[Profit])
Total Quantity = SUM('Sales'[Quantity])
Total Orders = DISTINCTCOUNT('Sales'[OrderID])

Average Order Value = DIVIDE([Total Sales], [Total Orders], 0)
Overall Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)
```
</details>

<details>
<summary><b>3. KPI / Time-Intelligence Measures</b> (trend & comparison-ready)</summary>

```DAX
Sales YTD = TOTALYTD([Total Sales], 'Date'[Date])

Sales MoM Growth % =
DIVIDE([Total Sales] - CALCULATE([Total Sales], PREVIOUSMONTH('Date'[Date])),
       CALCULATE([Total Sales], PREVIOUSMONTH('Date'[Date])), 0)

Category Contribution % =
DIVIDE([Total Sales], CALCULATE([Total Sales], ALL('Sales'[Category])))

Top Salesperson =
CALCULATE(SELECTEDVALUE('Sales'[Salesperson]),
          TOPN(1, ALL('Sales'[Salesperson]), [Total Sales], DESC))
```
</details>

Full measure list with explanations: [`DAX_Measures.md`](DAX_Measures.md)

---

## 🛠️ Tech Stack

![Power BI](https://img.shields.io/badge/-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/-DAX-217346?style=flat-square)
![Power Query](https://img.shields.io/badge/-Power%20Query%20(M)-742774?style=flat-square)
![Excel](https://img.shields.io/badge/-Excel-217346?style=flat-square&logo=microsoftexcel&logoColor=white)

---

## 🚀 How to Use

1. Clone this repository or download the `.pbix` file
2. Open `Sales_Performance_Dashboard.pbix` in **Power BI Desktop** (free download from Microsoft)
3. Use the slicers (Region, Category, Salesperson, Order Year) on any page to explore the data interactively
4. Navigate between the 4 report pages using the tabs at the top

---

## 📂 Repository Structure

```
sales-performance-dashboard-powerbi/
│
├── Sales_Performance_Dashboard.pbix   # Main Power BI report file
├── PowerBI_Sales_Data.csv             # Source dataset
├── DAX_Measures.md                    # Full documented list of DAX measures
├── README.md                          # Project documentation (this file)
├── banner.svg                         # Repository banner
├── LICENSE                            # MIT License
└── assets/
    └── screenshots/                   # Dashboard page exports (add your own)
```

---

## 👤 Author & Contact

**Manish**
Data Analyst | Power BI • SQL • DAX • Python (pandas) | Alpha Insights

📧 Email: [alphainsights123@gmail.com](mailto:alphainsights123@gmail.com)
🔗 LinkedIn: [linkedin.com/in/dataanalyst-manish](https://linkedin.com/in/dataanalyst-manish)
💻 GitHub: [github.com/bisht5431-source](https://github.com/bisht5431-source)
🌐 Portfolio: [manishbisht.in](https://manishbisht.in)

> If you found this project useful or interesting, consider ⭐ starring the repository!

---

<div align="center">
<sub>© 2026 Manish — Alpha Insights. Licensed under the MIT License.</sub>
</div>
