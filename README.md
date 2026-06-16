# Olist-Ecommerce-PowerBI-Dashboard
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-yellow?logo=powerbi&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-Measures-blue)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-green)
![Python](https://img.shields.io/badge/Python-3.9-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-blueviolet?logo=pandas&logoColor=white)
![Star Schema](https://img.shields.io/badge/Star%20Schema-Data%20Modeling-red)
![Interactive Dashboard](https://img.shields.io/badge/Interactive-Dashboard-brightgreen)
![Geospatial](https://img.shields.io/badge/Geospatial-Map%20Visualization-lightblue)
![Status](https://img.shields.io/badge/Status-Completed-success)
![Dataset](https://img.shields.io/badge/Dataset-Olist%20Brazilian%20E--Commerce-9cf)
![Orders](https://img.shields.io/badge/Orders-100K+-informational)

## 📥 Download Power BI File
https://github.com/iHazelly/Olist-Ecommerce-PowerBI-Dashboard/releases/download/v1.0.0/BIA_Assignment1_Paradorn.Khanongsuwan.pbix


Power BI dashboard for Olist E-Commerce data. Features ETL pipeline (Python/Pandas), Star Schema modeling, DAX measures for revenue, delivery, and satisfaction KPIs, and interactive reports across 4 pages.

# Executive Summary
<img width="1611" height="698" alt="image" src="https://github.com/user-attachments/assets/0e124560-575e-4fbe-9ace-8bd74647c666" />

# Sales & Order Performance
<img width="1181" height="505" alt="image" src="https://github.com/user-attachments/assets/478ed569-b464-474f-bb21-b68424ce8a70" />

# Delivery & Seller Performance
<img width="1169" height="715" alt="image" src="https://github.com/user-attachments/assets/b672bdca-bed3-4121-88cd-a17438829495" />

# Customer Satisfaction & Geospatial Insights
<img width="1512" height="711" alt="image" src="https://github.com/user-attachments/assets/27ca4bba-49a3-4134-927b-23713aed7390" />



## Business Intelligence & Analytics – From Raw Data to Executive Dashboard

This project is a **complete Business Intelligence solution** that transforms 100,000+ rows of raw e-commerce transactional data into an **interactive Power BI dashboard** with actionable insights for Olist's senior leadership.

---

### Data Engineering Pipeline (ETL)

- **Extract** – Raw data was ingested from 9 relational CSV files (Olist Brazilian E-Commerce Public Dataset) covering orders, customers, products, sellers, payments, reviews, and geolocation.
- **Transform** – A Python-based ETL pipeline (Pandas) was executed to:
  - Enforce UTF-8 encoding to resolve Portuguese character corruption.
  - Pre-merge product categories with English translations using an inner-left join.
  - Deduplicate geolocation data by aggregating coordinates based on `zip_code_prefix` to prevent many-to-many cardinality.
  - Standardize timestamps across all date columns into `datetime64` objects.
- **Load** – Cleaned CSV files were exported and loaded into **Power BI** for dimensional modeling and visualization.

### Power Query Date Fix (Critical Step)

A critical date transformation was applied in **Power Query Editor** to resolve a severe data quality issue:
- Timestamp columns (`order_purchase_timestamp`, `order_approved_at`, `order_delivered_carrier_date`, `order_delivered_customer_date`) contained **Buddhist year values** (`2560`, `2559`, `2561` instead of `2017`, `2016`, `2018`), causing Power BI to incorrectly interpret dates as years `1474` and `1475`.
- **Solution:** Using Power Query’s `Replace Values` and `Split Column by Delimiter`, the Buddhist years were programmatically replaced with their Gregorian equivalents (`2560→2017`, `2559→2016`, `2561→2018`). The columns were then converted to proper `Date/Time` type using **Portuguese (Brazil)** locale to handle the `dd/MM/yyyy` format correctly.

### Data Modeling (Star Schema)

- **Fact Tables:** `orders_clean` (central fact) and `order_items_clean` (line-item details).
- **Dimension Tables:** `customers_clean`, `sellers_clean`, `products_clean`, `payments_clean`, `reviews_clean`, `geolocation_clean`, and `calendar_clean`.
- **Relationships:** Established one-to-many relationships via primary/foreign keys (`order_id`, `customer_id`, `product_id`, `seller_id`, `zip_code_prefix`). Cross-filter direction set to **Both** for all key relationships to enable seamless filtering across dimensions.

---

### Key Performance Indicators (DAX Measures)

| KPI | DAX Formula | Business Meaning |
|-----|-------------|------------------|
| **Total Revenue (Net)** | `SUM(order_items_clean[price])` | Net revenue from product sales (excluding freight) |
| **Total Orders** | `DISTINCTCOUNT(orders_clean[order_id])` | Total number of unique orders |
| **Average Order Value (AOV)** | `DIVIDE([Total Revenue], [Total Orders])` | Average revenue per order |
| **Avg Delivery Days** | `AVERAGE(orders_clean[delivery_days_actual])` | Average delivery time (days) |
| **Avg Review Score** | `AVERAGE(reviews_clean[review_score])` | Average customer satisfaction rating |
| **Late Rate** | `DIVIDE(CALCULATE([Total Orders], orders_clean[delivery_days_actual] > DATEDIFF(orders_clean[order_purchase_timestamp], orders_clean[order_estimated_delivery_date], DAY)), CALCULATE([Total Orders], orders_clean[order_status] = "delivered"))` | Percentage of orders delivered late |

---

### Dashboard Pages (Interactive Executive Report)

| Page | Name | Key Visuals |
|------|------|-------------|
| **0** | **Executive Summary** | KPI Cards (5), Line Chart (Revenue & Orders), Slicers (Year, Payment Type), Summary Text Box |
| **1** | **Sales & Order Performance** | Line Chart, Top 10 Product Categories Bar Chart, Payment Method Donut Chart, Monthly Revenue Matrix Table |
| **2** | **Delivery & Seller Performance** | Scatter Plot (Freight vs Delivery Days), Avg Delivery Days by Seller State Bar Chart, Late Rate by State Column Chart, Slowest Sellers Table |
| **3** | **Customer Satisfaction & Geospatial** | Filled Map (Review Score by State), Review Score by Delivery Delay Column Chart, Review Score by Product Category Treemap, Review Score by Payment Type Table |

---

### Key Insights & Business Recommendations

#### Key Insights
1. **Sales Concentration:** São Paulo (SP) dominates both customer base and seller locations, generating the highest order volume and revenue.
2. **Delivery Impact on Satisfaction:** There is a clear negative correlation between delivery time and review scores. Orders delivered late receive significantly lower ratings (mostly 1–2 stars).
3. **Payment Preferences:** Credit cards are the overwhelmingly preferred payment method (~75%), followed by boleto (~20%).
4. **Logistical Bottlenecks:** Orders shipped from South/Southeast sellers to North/Northeast customers face higher freight costs and extended delivery times.

#### Actionable Recommendations
- **Establish Regional Fulfillment Hubs:** Invest in distribution centers in the North/Northeast to reduce freight costs and delivery times.
- **Seller Performance Incentives:** Implement a tier-based reward system for sellers with fast delivery and high review scores.
- **Proactive Customer Communication:** Notify customers of potential delays before the estimated delivery date to reduce dissatisfaction.
- **Focus Marketing on SP Suburbs:** While SP is the strongest market, there is still room to penetrate suburban areas where logistics are already efficient.

---

### Why This Matters (Data Engineering + Business Intelligence)

> *“This project demonstrates how modern BI tools (Power BI) combined with robust data engineering practices (Python ETL, Power Query, Star Schema modeling) can transform raw, messy transactional data into an interactive executive dashboard – enabling data-driven decisions for expansion, delivery optimization, and customer satisfaction.”*

**Key integration points:**
- Python ETL provides **clean, reliable data** → Power BI consumes it for **accurate KPIs**.
- Power Query handles **critical date corrections** (Buddhist year fix) that would otherwise break time-series analysis.
- DAX enables **complex business logic** (Late Rate, AOV, delivery metrics) without altering source data.
- Star Schema modeling ensures **efficient filtering** and cross-table analysis.

---

### What You Can Learn From This Codebase

If you are a Business Intelligence analyst or data analyst, studying this repository will show you:

- How to build a **complete ETL pipeline** using Python (Pandas) to clean and prepare data for BI tools.
- How to design a **Star Schema data model** in Power BI with fact and dimension tables.
- How to write **DAX measures** for operational KPIs (Revenue, AOV, Delivery Performance, Satisfaction).
- How to resolve **real-world data quality issues** (date formatting, Buddhist year misinterpretation) using Power Query.
- How to design an **executive dashboard** with 4 pages tailored to different business functions.
- How to extract **actionable insights** and translate them into business recommendations.

---

## AI Use Declaration

During the development of this project, AI tools were used to assist in:

- **Code Generation & Debugging:** Python scripts for ETL (Pandas) and DAX expressions for KPI calculations (e.g., `Total Revenue`, `Avg Delivery Days`, `Late Rate`).
- **Data Cleaning Guidance:** Step-by-step recommendations to resolve date quality issues (Buddhist year fix) using Power Query’s `Replace Values` and `Split Column` features.
- **Dashboard Design & Visualization:** Suggestions for chart selection (e.g., Treemap, Scatter Plot, Filled Map) and layout composition across 4 dashboard pages.
- **Analytical Structuring:** Support in organizing findings, refining insights, and developing actionable recommendations based on visual analysis.

All final decisions regarding data interpretation, logic, conclusions, and recommendations were reviewed and verified by the author to ensure accuracy, relevance, and alignment with assignment requirements. The submitted work represents the author’s own analysis and understanding of the dataset and business context.

---

## 📄 Citation

If you use this code, structure, or findings from this project, please cite:

```bibtex
@misc{khanongsuwan_2026_olist_bi,
  title={Olist E-Commerce Business Intelligence Dashboard: A Power BI Case Study},
  author={Khanongsuwan, Paradorn},
  year={2026},
  howpublished={\url{https://github.com/iHazelly/Olist-Ecommerce-PowerBI-Dashboard}}
}
