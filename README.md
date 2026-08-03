# E-Commerce Customer & Sales Performance Dashboard

An interactive 3-page Power BI dashboard analyzing e-commerce sales, customer retention, and product performance for a simulated online retail business (2020-2026).

## Overview

This project uses a 4-table relational dataset (customers, orders, product_summary, monthly_revenue) to answer three core business questions:

1. How is the business performing overall?
2. How healthy is our customer base?
3. How are our products performing?

## Tools & Skills Used

- Power BI Desktop
- Power Query (data cleaning and transformation)
- DAX (12 custom measures)
- Data Modeling (relationship building across 4 tables)
- Dashboard Design & Data Storytelling

## Dataset

| Table | Rows | Description |
|---|---|---|
| customers | 8,000 | Customer-level data: ID, status (Active/Churned), spend |
| orders | 25,000 | Transaction-level detail (the central fact table) |
| product_summary | 140 | Per-product ratings, return rate, category |
| monthly_revenue | 75 | Pre-aggregated monthly summary |

## Data Preparation (Power Query)

- Removed redundant columns and added calculated columns (Customer_Types, Customer_Status, Month_Year, Month_Name)
- Verified and standardized percentage fields
- Used the transaction-level orders table as the source of truth over pre-aggregated summary tables, to avoid double-counting
- Built Many-to-One active relationships: orders -> customers, orders -> product_summary

## Key DAX Measures

- Total Revenue = SUM(orders[total_amount_usd])
- Total Orders = COUNTROWS(orders)
- AOV = DIVIDE([Total Revenue],[Total Orders])
- Return Rate %
- Total Customers = DISTINCTCOUNT(customers[customer_id])
- Churned Customers = CALCULATE(COUNTROWS(customers), Customer_Status = "Churned")
- Churn Rate % = DIVIDE([Churned Customers],[Total Customers])
- Avg Customer Spend = DIVIDE([Total Revenue],[Total Customers])
- Total Products = DISTINCTCOUNT(product_summary[product_name])
- Avg Rating = AVERAGE(product_summary[avg_rating])
- Avg Return Rate % = AVERAGE(product_summary[return_rate])

## Dashboard Pages

### Page 1: Overview
KPI cards (Total Revenue, Total Orders, AOV, Return Rate %, Active Customers), Yearly Revenue Trend, Total Revenue by Category, Payment Method Split, date and category slicers.

### Page 2: Customer Insight
KPI cards (Total Customers, Churn Rate %, Avg Customer Spend), Top 10 Customers by Spend, Active vs Churned Customers, date slicer.

### Page 3: Product Performance
KPI cards (Total Products, Avg Rating, Avg Return Rate %), Return Rate by Category, Top 10 Selling Products, date slicer.

## Key Insights

- Total revenue reached approximately $3.09M (30,91,634) across 24,635 orders.
- Electronics is the dominant revenue category, followed by Home & Kitchen.
- Credit Card is the most-used payment method at 38%, followed by Debit Card (22%) and PayPal (18%).
- Customer retention is strong: 91% active, 9% churned out of 8,000 total customers.
- The top individual customer contributed roughly 10x the average customer's spend (average spend ~$386), highlighting an opportunity for a loyalty/VIP program.
- Travel & Luggage has the highest product return rate (~10%), while Portable Charger 20000mAh is the best-selling individual product.
- Overall return rate across the business sits at a healthy 8%.

## Challenges & Resolutions

- **Locale-based currency formatting:** Power BI's currency format follows the report's regional locale, which caused a mismatch between the $ symbol and comma grouping. Resolved by removing the symbol and labeling fields "(USD)" explicitly instead, since the underlying fields are named total_amount_usd and revenue_usd.
- **Misleading incomplete-year comparison:** The dataset's final year (2026) only had partial data (Jan-Feb), which distorted the year-over-year trend. Resolved with a visual-level filter limiting the trend chart to complete years only (2020-2025).
- **Chart readability:** Category and customer-level charts were simplified using Top-N filters to stay scannable instead of showing all data points.
---

