**End-to-End Business Analytics Platform (PostgreSQL)**


 I built an end-to-end business analytics platform where I cleaned raw sales data, loaded it into PostgreSQL using client-side copy, and performed SQL-based KPI, category, and profitability analysis to derive actionable business insights.

---

## Project Overview

This project demonstrates a **complete business analytics workflow** starting from raw transactional sales data to **business-ready insights** using **PostgreSQL and SQL**.

The objective was to simulate a **real-world Data Analyst environment**, focusing on:

- Data preparation and standardization  
- Production-safe database loading  
- SQL-driven KPI and business analysis  
- Translating data into decision-making insights  

It reflects real challenges faced in analytics teams, especially around **data loading, permissions, and PostgreSQL operations on Windows systems**.

---

##  Tech Stack

<p align="left">
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/SQL-Analytics-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/pgAdmin-Database%20Tool-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Windows-OS-lightgrey?style=for-the-badge"/>
</p>

---

## 📂 Dataset Information

- **Source:** Kaggle  
- **Dataset:** Retail / Superstore Sales Dataset  
- **Link:** [https://www.kaggle.com/datasets ]
- (https://www.kaggle.com/datasets/anshika2301/e-commerce-dataset) 

### Dataset Columns
Order ID
Amount
Profit
Quantity
Category
Sub-Category
PaymentMode

The dataset represents transactional sales data commonly used in **retail and e-commerce analytics**.

---

## Project Architecture

Raw CSV Data (Kaggle)

↓

Data Cleaning & Standardization
 
 ↓

PostgreSQL Database (pgAdmin)

 ↓ 

SQL Queries for KPIs & Insights

 ↓

Business-Level Analysis

---

## Database Schema (PostgreSQL)

```sql
CREATE TABLE sales (
    order_id VARCHAR,
    amount NUMERIC,
    profit NUMERIC,
    quantity INTEGER,
    category VARCHAR,
    sub_category VARCHAR,
    paymentmode VARCHAR
);

**Data Loading Strategy (IMPORTANT)**

Why NOT COPY?
On Windows systems, PostgreSQL runs as a service and cannot access local user directories, which results in permission errors when using server-side COPY.

Solution Used: Client-Side \copy
Data was safely loaded using pgAdmin’s PSQL Tool with client-side copy.

sql(query in PSQL tool)

\copy sales(order_id,amount,profit,quantity,category,sub_category,paymentmode)
FROM 'C:/..../business-analytics-platform/data/cleaned_details.csv'
DELIMITER ','
CSV HEADER;
This approach reflects real-world production constraints and is considered a best practice on Windows environments.

Business KPIs & SQL Analysis
Overall Business Performance
sql:

SELECT 
    SUM(amount) AS total_revenue,
    SUM(profit) AS total_profit,
    SUM(quantity) AS total_quantity
FROM sales;

Sales & Profit by Category
sql:

SELECT 
    category,
    SUM(amount) AS revenue,
    SUM(profit) AS profit
FROM sales
GROUP BY category
ORDER BY revenue DESC;

Top Performing Sub-Categories
sql:

SELECT 
    sub_category,
    SUM(amount) AS revenue
FROM sales
GROUP BY sub_category
ORDER BY revenue DESC
LIMIT 10;

 Payment Mode Analysis
sql:

SELECT 
    paymentmode,
    COUNT(*) AS total_orders,
    SUM(amount) AS revenue
FROM sales
GROUP BY paymentmode
ORDER BY revenue DESC;

Profit Margin Analysis
sql:

SELECT 
    category,
    ROUND(SUM(profit)/NULLIF(SUM(amount),0)*100, 2) AS profit_margin_percentage
FROM sales
GROUP BY category
ORDER BY profit_margin_percentage DESC;

High Revenue but Low Profit Areas
sql:

SELECT 
    sub_category,
    SUM(amount) AS revenue,
    SUM(profit) AS profit
FROM sales
GROUP BY sub_category
HAVING SUM(amount) > 100000 
   AND SUM(profit) < 5000
ORDER BY revenue DESC;


Key Business Insights

Identified top revenue-driving categories
Highlighted loss-making or low-margin segments
Analyzed customer payment preferences
Detected high-revenue but low-profit sub-categories, enabling optimization decisions


What I Learned From This Project

Designing and managing a relational schema for analytics
Handling real-world PostgreSQL data loading challenges on Windows
Understanding the difference between server-side COPY vs client-side \copy
Writing business-focused SQL queries, not just technical ones
Translating raw data into actionable KPIs
Thinking like a business analyst, not just a coder


Why This Project Matters

This project demonstrates:
End-to-end project ownership
Production-aware database handling
Strong SQL and analytical fundamentals
Business-first analytical thinking


Future Enhancements

Power BI / Tableau dashboard connected to PostgreSQL
Time-series trend analysis
Customer segmentation
Automated ETL pipeline

Author
Aditya Patil

