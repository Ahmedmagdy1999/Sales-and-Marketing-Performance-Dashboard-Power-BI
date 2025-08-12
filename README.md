# Executive Sales & Marketing Dashboard (Power BI)  
**Internship Project — Real Business Data**

---
<img width="679" height="368" alt="1" src="https://github.com/user-attachments/assets/062960c4-3e1c-49dd-819c-471a6b33c972" />
<img width="666" height="375" alt="3" src="https://github.com/user-attachments/assets/e4043914-7f97-4cf5-a5d1-f119c6bf3a17" />
<img width="675" height="372" alt="2" src="https://github.com/user-attachments/assets/c5e22842-e626-4c6c-8975-7b9b1020af19" />

## 🧾 Overview
This project was developed during my **internship** using **Power BI** and is based on **real business data** from a retail environment.  
The goal was to transform raw data into an interactive dashboard that enables decision-makers to monitor performance, evaluate marketing campaigns, and identify key sales trends.

---

## 🗂️ Project Contents
- `Executive_Dashboard.pbix` — Final Power BI report file  
- `data/` — (Optional) Source CSV/Excel files (fact_sales_normalized.csv, products.csv, stores.csv, customers.csv, campaign.csv, dates.csv, salesperson.csv)  
- `README.md` — Project documentation (this file)  
- `screenshots/` — Dashboard screenshots

---

## 🧩 Datasets
The project integrates 7 main tables cleaned and modeled in a **Star Schema**:
- **fact_sales_normalized** (Fact) — Sales transactions (OrderID, SalesDate, ProductID, CustomerID, StoreID, SalesAmount, Quantity, UnitPrice, UnitCost, DiscountPercent, CampaignID, SalespersonID, …)  
- **products** 
- **stores** 
- **customers** 
- **campaign** 
- **dates**  
- **salesperson**

---

## 🛠 Tools & Technologies
- **Power BI Desktop** — Data modeling, visualization, dashboard building  
- **Power Query** — Data cleaning and transformation (ETL)  
- **DAX** — Calculated measures and KPIs  
- **Data Modeling** — Star schema with one-to-many relationships  
- Visuals: Line chart, Bar chart, Donut chart, Matrix, KPI Cards, Slicers, Drillthrough pages

---

## 🎯 Analysis Goals
1. Sales growth tracking 
2. Top and bottom performing stores  
3. Best and worst-selling products  
4. Marketing campaign performance and ROI  
5. Category and brand profitability  
6. Sales performance by location and salesperson  

---

## 📊 Report Pages
- **Overview** — KPIs, sales trends, category/brand performance, top & bottom products, top stores  
- **Marketing** — Campaign analysis (budget vs. sales), ROI %, campaign sales %, campaign timeline  
- **Sales** — Salesperson performance, sales by location, product rankings, top/bottom stores and products (dynamic)

---

## 📐 Data Model
- Fact table: `fact_sales_normalized` connected to dimension tables:
  - `CustomerID` → `customers[CustomerID]`  
  - `ProductID` → `products[ProductID]`  
  - `StoreID` → `stores[StoreID]`  
  - `CampaignID` → `campaign[CampaignID]`  
  - `SalespersonID` → `salesperson[SalespersonID]`  
  - `SalesDate` → `dates[FullDate]`  

Most relationships are **Single direction** (1-to-many), with **Both** direction enabled where necessary.

---

## 🧮 Key Measures (DAX)

```dax
-- Core KPIs
Total Sales = SUM('fact_sales_normalized'[SalesAmount])
Total Quantity = SUM('fact_sales_normalized'[Quantity])
Total Orders = DISTINCTCOUNT('fact_sales_normalized'[OrderID])
Total Customers = DISTINCTCOUNT('fact_sales_normalized'[CustomerID])
Average Order Value = DIVIDE([Total Sales], [Total Orders], 0)

-- Profitability
Gross Profit = SUMX('fact_sales_normalized', ('fact_sales_normalized'[UnitPrice] - 'fact_sales_normalized'[UnitCost]) * 'fact_sales_normalized'[Quantity])
Profit Margin % = DIVIDE([Gross Profit], [Total Sales], 0)



-- Campaign Analysis
Total Campaign Budget = SUM('campaign'[Budget])
Total Sales from Campaigns = CALCULATE([Total Sales], FILTER('fact_sales_normalized', NOT(ISBLANK('fact_sales_normalized'[CampaignID]))))
Campaign Sales % = DIVIDE([Total Sales from Campaigns], [Total Sales], 0)
Campaign ROI % = DIVIDE([Total Sales from Campaigns] - [Total Campaign Budget], [Total Campaign Budget], 0)

-- Dynamic Ranking Example (Products)
Product Sales Rank = RANKX(ALLSELECTED('products'[ProductName]), [Total Sales], , DESC)
