<div align="center">

# 🏬 Store Sales Report Dashboard

### Power BI Project | Retail Store Performance, Sales Variance, Units, Margin & New Store Analysis

</div>

---

## ✨ Project Summary
This Power BI project analyzes **retail store sales performance** across stores, chains, categories, districts, and time periods.

The dashboard focuses on comparing **This Year vs Last Year** performance, tracking **new store expansion**, and evaluating operational KPIs such as:
- 💰 Total Sales
- 📦 Total Units
- 📉 Sales Variance
- 🧾 Gross Margin
- 📏 Sales Per Sq Ft
- 🏬 Store Count
- 🆕 New Store Count
- 💲 Average Unit Price

The report is designed to help business users understand store-level performance, identify growth opportunities, and monitor retail efficiency across locations.

---

## 🎯 Objectives
- Monitor core retail KPIs such as **Sales**, **Units**, **Gross Margin**, and **Store Count**
- Compare **This Year** and **Last Year** results using scenario-based measures
- Analyze **sales variance** by month, category, district, and store
- Track the performance and contribution of **new stores**
- Evaluate store productivity using **Sales Per Sq Ft** and **Average Selling Area Size**
- Identify top-performing chains, stores, and product categories

---

## ❓ Business Questions
This dashboard helps answer the following questions:

- What is the overall store sales performance this year?
- How do **This Year Sales** compare with **Last Year Sales**?
- What is the total sales variance in value and percentage?
- How are sales distributed across categories, chains, districts, and stores?
- Which stores generate the highest sales and unit volume?
- How many new stores were opened, and how are they performing?
- What is the average selling area size, and how efficiently is space being used?
- What are the trends in monthly sales variance and gross margin?

---

## 🗂️ Dataset Description
The report is built on a retail sales data model with the following main entities:

- **Store**
- **Sales**

### 🔑 Key Fields Used
- `Store[Store Type]`
- `Store[StoreNumberName]`
- `Store[OpenDate]`
- `Store[SellingAreaSize]`
- `Sales[ScenarioID]`
- `Sales[LocationID]`
- `Sales[MonthID]`
- Sales value and units fields such as:
  - `Sum_Regular_Sales_Dollars`
  - `Sum_Markdown_Sales_Dollars`
  - `Sum_GrossMarginAmount`
  - `Sum_Regular_Sales_Units`
  - `Sum_Markdown_Sales_Units`

---

## 🧩 Data Model
The dashboard appears to use:
- A **Store** table for store information, store type, open date, and selling area
- A **Sales** table for transaction/aggregate retail performance
- Scenario-based logic where:
  - `ScenarioID = 1` → **This Year**
  - `ScenarioID = 2` → **Last Year**

This structure supports analysis by:
- ⏳ Time (Fiscal Month)
- 🏬 Store / Chain / District
- 🛍️ Category
- 🆕 Store Type (New Store / Same Store)
- 📍 Geography / Postal Code

---

## 📐 Key DAX Measures

<details>
<summary><b>🏬 Store Measures</b></summary>

```DAX
Average Selling Area Size = AVERAGE([SellingAreaSize])

New Stores =
CALCULATE(
    COUNTA([Store Type]),
    FILTER(ALL(Store), [Store Type] = "New Store")
)

New Stores Target = 14

Total Stores = COUNTA([StoreNumberName])

Open Store Count = COUNTA([OpenDate])

Count of OpenDate = COUNTA('Store'[OpenDate])
```

</details>

<details>
<summary><b>💰 Sales & Margin Measures</b></summary>

```DAX
Regular_Sales_Dollars = SUM([Sum_Regular_Sales_Dollars])

Markdown_Sales_Dollars = SUM([Sum_Markdown_Sales_Dollars])

TotalSales = [Regular_Sales_Dollars] + [Markdown_Sales_Dollars]

TotalSalesTY = CALCULATE([TotalSales], Sales[ScenarioID] = 1)

TotalSalesLY = CALCULATE([TotalSales], Sales[ScenarioID] = 2)

This Year Sales = [TotalSalesTY]

Last Year Sales = [TotalSalesLY]

Total Sales Var = [TotalSalesTY] - [TotalSalesLY]

Total Sales Var % =
IF(
    [TotalSalesLY] <> 0,
    [Total Sales Var] / [TotalSalesLY],
    BLANK()
)

Total Sales Variance = [Total Sales Var]

Total Sales Variance % = [Total Sales Var %]

Gross Margin This Year =
CALCULATE(
    SUM([Sum_GrossMarginAmount]),
    Sales[ScenarioID] = 1
)

Gross Margin This Year % = [Gross Margin This Year] / [TotalSalesTY]

Gross Margin Last Year =
CALCULATE(
    SUM([Sum_GrossMarginAmount]),
    Sales[ScenarioID] = 2
)

Gross Margin Last Year % = [Gross Margin Last Year] / [TotalSalesLY]
```

</details>

<details>
<summary><b>📦 Unit Measures</b></summary>

```DAX
Regular_Sales_Units = SUM([Sum_Regular_Sales_Units])

Markdown_Sales_Units = SUM([Sum_Markdown_Sales_Units])

TotalUnits = [Regular_Sales_Units] + [Markdown_Sales_Units]

Total Units This Year = CALCULATE([TotalUnits], Sales[ScenarioID] = 1)

Total Units Last Year = CALCULATE([TotalUnits], Sales[ScenarioID] = 2)

Avg $/Unit TY =
IF(
    [Total Units This Year] <> 0,
    [TotalSalesTY] / [Total Units This Year],
    BLANK()
)

Avg $/Unit LY =
IF(
    [Total Units Last Year] <> 0,
    [TotalSalesLY] / [Total Units Last Year],
    BLANK()
)

Average Unit Price = [Avg $/Unit TY]

Average Unit Price Last Year = [Avg $/Unit LY]
```

</details>

<details>
<summary><b>📏 Productivity Measures</b></summary>

```DAX
Sales Per Sq Ft =
([TotalSalesTY] / (DISTINCTCOUNT([MonthID]) * SUM(Store[SellingAreaSize]))) * 12

Store Count = DISTINCTCOUNT([LocationID])
```

</details>

---

## 🖼️ Report Preview

> **Note:** The screenshots below use the current file names you uploaded. If you later move them into an `images/` folder, simply update the image paths in this README.

### 1️⃣ Store Sales Overview
This page provides an executive overview of store sales performance, including total sales, chain contribution, district-level trend, geographic store distribution, and district efficiency using Sales Per Sq Ft.

<p align="center">
  <img src="image%20%284%29.png" alt="Store Sales Overview" width="92%">
</p>

### 2️⃣ New Stores Dashboard
This page focuses on new store performance with a gauge for This Year Sales, comparison against Last Year Sales, monthly trend, and detailed drill-down by chain and store.

<p align="center">
  <img src="image%20%282%29.png" alt="New Stores Dashboard" width="92%">
</p>

### 3️⃣ New Stores Variance View
This view highlights Total Sales Variance % by Fiscal Month using a waterfall chart, helping users quickly spot improvement and decline periods.

<p align="center">
  <img src="image%20%281%29.png" alt="New Stores Variance View" width="92%">
</p>

### 4️⃣ District Monthly Sales Dashboard
This page compares This Year vs Last Year monthly sales, total variance %, average price per unit, and performance by category and chain/store.

<p align="center">
  <img src="image.png" alt="District Monthly Sales Dashboard" width="92%">
</p>

### 5️⃣ Detailed Info – Total Sales by Category
This detailed view ranks categories based on total sales, helping identify the strongest merchandising groups.

<p align="center">
  <img src="image.jpg" alt="Total Sales by Category" width="92%">
</p>

### 6️⃣ Detailed Info – Total Units by Store Name
This detailed view shows the highest-performing stores by total units sold.

<p align="center">
  <img src="image%20%283%29.png" alt="Total Units by Store Name" width="92%">
</p>

---

## 🔍 Key Insights
Based on the dashboard screenshots and DAX measures, several insights can be derived:

- 💰 The dashboard tracks **This Year Sales**, **Last Year Sales**, and their **variance** in both value and percentage
- 🆕 New stores are explicitly monitored, with a defined **New Stores Target = 14**
- 🏬 The overview page shows total sales of approximately **$22M** for the current year
- 🧭 Sales performance is analyzed by **chain**, **district**, **store**, **category**, and **postal code**
- 📦 Unit-based analysis helps identify stores with the strongest sales volume contribution
- 📉 Variance analysis reveals which months or stores are underperforming compared with last year
- 📏 Productivity metrics such as **Sales Per Sq Ft** and **Average Selling Area Size** support operational efficiency analysis
- 💲 Average unit price measures help evaluate pricing and product mix changes over time

---

## ✅ Recommendations
1. **Prioritize underperforming months and districts**
   - Investigate months with negative sales variance
   - Review district-level issues such as demand, staffing, promotion, or inventory

2. **Track new store ramp-up performance closely**
   - Compare new stores against target and expected maturity curve
   - Identify stores that require marketing or operational support after launch

3. **Improve store productivity**
   - Use **Sales Per Sq Ft** to benchmark stores and districts
   - Reallocate floor space toward high-performing categories where appropriate

4. **Refine merchandising strategy**
   - Focus on top-performing categories and chains
   - Review weak categories with declining variance or low unit productivity

5. **Monitor pricing and margin dynamics**
   - Compare average unit price and gross margin over time
   - Investigate whether growth is driven by volume, markdowns, or true value increase

---

## 🛠️ Tools & Technologies
- **Power BI Desktop**
- **DAX**
- **Data Modeling**
- **Retail Sales Dataset**
- **Store & Sales Scenario Analysis**

---

## 🧠 Skills Demonstrated
- Retail KPI dashboard development
- Scenario-based analysis (This Year vs Last Year)
- Sales variance analysis
- Gross margin analysis
- Store productivity analysis
- Geographic and district performance analysis
- Category and store ranking analysis
- Dashboard storytelling and business insight communication

---

## 🚀 How to Use
1. Open the `.pbix` file in **Power BI Desktop**
2. Refresh the dataset if needed
3. Navigate through report pages such as:
   - Store Sales Overview
   - New Stores
   - District Monthly Sales
   - Detailed Info
4. Use available filters/drill-downs to analyze by:
   - Fiscal Month
   - Chain
   - District
   - Store Name
   - Category
   - Store Type

---

## 👨‍💻 Author
**Vuong Minh Toan**

---

## 📝 Notes
- This project is intended for **portfolio** and **learning** purposes.
- The screenshots show multiple analytical views including overview, variance tracking, district analysis, and detailed ranking.
- If you rename or move image files, remember to update the paths in `README.md`.
