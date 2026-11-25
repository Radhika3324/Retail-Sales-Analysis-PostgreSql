# Retail Sales Analysis Using PostgreSQL

## 1. Project Overview

**Project Name:** Retail Sales Analysis  
**Domain:** Retail / Data Analytics  
**Database:** PostgreSQL  
**Language:** SQL  
**Project Type:** Data Analysis Project

Retail Sales Analysis is a SQL-based data analytics project designed to analyze retail business performance, customer purchasing behaviour, and product category trends.

The project uses PostgreSQL to explore retail transaction data and generate meaningful business insights. By writing SQL queries, the analysis identifies high-performing product categories, valuable customers, monthly sales trends, gender-based purchasing patterns, and customer buying behaviour.

The findings can help retail businesses understand sales performance, identify growth opportunities, and make data-driven business decisions.

---

## 2. Business Statement

A retail business wants to understand its sales performance, customer behaviour, and product category trends to identify opportunities for growth.

The objective is to analyze the available retail sales data using SQL to identify high-performing product categories, valuable customers, monthly sales trends, and purchasing patterns.

These findings will help the business understand its performance, improve business strategies, and make data-driven decisions.

---

## 3. Project Objectives

The main objectives of this project are:

- Analyze overall retail sales performance.
- Identify high-performing product categories.
- Understand customer purchasing behaviour.
- Identify the top five customers based on total spending.
- Analyze monthly and yearly sales trends.
- Compare purchasing patterns across genders.
- Calculate the average age of customers purchasing Beauty products.
- Identify high-value sales transactions.
- Analyze customer participation across product categories.
- Understand purchasing patterns across different time shifts.

---

## 4. Tools & Technologies

| Technology | Purpose |
|---|---|
| PostgreSQL | Database management |
| SQL | Data extraction and analysis |
| pgAdmin | Database management and query execution |
| GitHub | Project documentation and version control |

---

## 5. Dataset Description

The project uses a retail sales transaction dataset containing information about customers, products, sales transactions, and purchasing details.

### Dataset Columns

| Column Name | Description |
|---|---|
| transactions_id | Unique transaction identifier |
| sale_date | Date of transaction |
| sale_time | Time of transaction |
| customer_id | Unique customer identifier |
| gender | Customer gender |
| age | Customer age |
| category | Product category |
| quantity | Number of products purchased |
| price_per_unit | Price per product unit |
| cogs | Cost of goods sold |
| total_sale | Total sales amount |

**Table Name:** `retail_sales`

---

## 6. Data Analysis & SQL Queries

The project addresses the following 10 business questions using PostgreSQL.

### Question 1: Sales on a Specific Date

**Business Requirement:** Retrieve all transactions made on November 5, 2022.

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Business Purpose:** Identify sales transactions that occurred on a particular date and understand daily sales activity.

### Question 2: Clothing Sales Analysis

**Business Requirement:** Identify Clothing transactions with quantities greater than 2 during November 2022.

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
AND quantity > 2
AND sale_date >= '2022-11-01'
AND sale_date < '2022-12-01';
```

**Business Purpose:** Analyze clothing purchases involving multiple units and understand customer buying patterns.

### Question 3: Sales Performance by Category

**Business Requirement:** Calculate total sales revenue and transaction counts for each product category.

```sql
SELECT
    category,
    SUM(total_sale) AS total_revenue,
    COUNT(DISTINCT transactions_id) AS total_transactions
FROM retail_sales
GROUP BY category
ORDER BY total_revenue DESC;
```

**Business Purpose:** Identify the highest-performing product categories based on revenue and transaction volume.

### Question 4: Customer Demographics

**Business Requirement:** Calculate the average age of customers purchasing Beauty products.

```sql
SELECT
    ROUND(AVG(age), 2) AS average_customer_age
FROM retail_sales
WHERE category = 'Beauty';
```

**Business Purpose:** Understand the age profile of customers purchasing Beauty products and support customer segmentation.

### Question 5: High-Value Transactions

**Business Requirement:** Identify transactions with sales exceeding 1,000.

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000
ORDER BY total_sale DESC;
```

**Business Purpose:** Identify high-value purchases and understand transactions contributing significantly to sales revenue.

### Question 6: Gender-Based Sales Analysis

**Business Requirement:** Compare transaction counts by gender and product category.

```sql
SELECT
    category,
    gender,
    COUNT(DISTINCT transactions_id) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category, total_transactions DESC;
```

**Business Purpose:** Understand purchasing patterns across genders and identify category-level customer preferences.

### Question 7: Monthly Sales Trends

**Business Requirement:** Identify the best-selling month in each year based on total revenue.

```sql
WITH monthly_sales AS (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS sales_year,
        EXTRACT(MONTH FROM sale_date) AS sales_month,
        SUM(total_sale) AS total_revenue
    FROM retail_sales
    GROUP BY
        EXTRACT(YEAR FROM sale_date),
        EXTRACT(MONTH FROM sale_date)
),
ranked_months AS (
    SELECT
        sales_year,
        sales_month,
        total_revenue,
        DENSE_RANK() OVER (
            PARTITION BY sales_year
            ORDER BY total_revenue DESC
        ) AS sales_rank
    FROM monthly_sales
)
SELECT
    sales_year,
    sales_month,
    total_revenue
FROM ranked_months
WHERE sales_rank = 1
ORDER BY sales_year;
```

**Business Purpose:** Identify the months generating the highest revenue in each year and understand seasonal sales trends.

### Question 8: Top Five Customers

**Business Requirement:** Identify the five customers with the highest total spending.

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_spending
FROM retail_sales
GROUP BY customer_id
ORDER BY total_spending DESC
LIMIT 5;
```

**Business Purpose:** Identify valuable customers who contribute significantly to business revenue and support customer retention strategies.

### Question 9: Customer Analysis by Category

**Business Requirement:** Calculate the number of unique customers purchasing from each product category.

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category
ORDER BY unique_customers DESC;
```

**Business Purpose:** Understand customer distribution across product categories and identify categories attracting the highest number of customers.

### Question 10: Sales by Time Shift

**Business Requirement:** Analyze transaction counts across Morning, Afternoon, and Evening shifts.

```sql
WITH hourly_sales AS (
    SELECT
        transactions_id,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12
                THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17
                THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT
    shift,
    COUNT(DISTINCT transactions_id) AS total_transactions
FROM hourly_sales
GROUP BY shift
ORDER BY
    CASE shift
        WHEN 'Morning' THEN 1
        WHEN 'Afternoon' THEN 2
        WHEN 'Evening' THEN 3
    END;
```

**Business Purpose:** Understand customer shopping activity during different times of the day and identify peak business hours.

---

## 7. SQL Concepts Used

The following SQL concepts were applied throughout the project:

| SQL Concept | Application |
|---|---|
| SELECT | Retrieve required data |
| WHERE | Filter records based on conditions |
| GROUP BY | Group sales data by categories |
| ORDER BY | Sort query results |
| Aggregate Functions | Calculate SUM, AVG, and COUNT |
| DISTINCT | Identify unique customers and transactions |
| CASE WHEN | Categorize sales into time shifts |
| CTE | Organize complex SQL queries |
| Window Functions | Rank monthly sales performance |
| EXTRACT | Extract year and month from dates |
| LIMIT | Retrieve the top five customers |

---

## 8. Business Insights & Findings

The SQL analysis is designed to generate insights in the following areas:

1. **Category Performance:** Identify product categories contributing the highest sales revenue and transaction volume.

2. **Customer Behaviour:** Understand purchasing patterns across different customer groups and product categories.

3. **Customer Value:** Identify high-spending customers who contribute significantly to total revenue.

4. **Sales Trends:** Identify the best-performing months in each year to understand seasonal demand.

5. **Gender-Based Analysis:** Compare purchasing activity across genders and product categories.

6. **Time-Based Analysis:** Identify peak shopping periods across morning, afternoon, and evening shifts.

7. **High-Value Transactions:** Identify large purchases that contribute significantly to business sales.

*Note: Actual numerical findings should be added after executing the queries against the dataset.*

---

## 9. Conclusion

The Retail Sales Analysis project demonstrates how PostgreSQL can be used to transform raw retail transaction data into meaningful business insights.

By applying SQL queries, aggregate functions, CTEs, CASE statements, and window functions, the project analyzes sales performance, customer behaviour, product category trends, and purchasing patterns.

The analysis helps identify valuable customers, high-performing categories, seasonal sales trends, and peak shopping hours.

Overall, this project demonstrates practical SQL skills and the ability to solve real-world business problems through data analysis.

---

## 10. Future Scope

The project can be enhanced further by:

- Developing interactive dashboards using Power BI.
- Performing customer segmentation using RFM analysis.
- Analyzing profit margins and product profitability.
- Identifying monthly and yearly revenue growth.
- Performing customer retention and repeat-purchase analysis.
- Building sales forecasting models.

---

## 11. GitHub Repository Structure

Recommended folder structure:

```text
Retail-Sales-Analysis-PostgreSQL/
│
├── README.md
│
├── Dataset/
│   └── retail_sales.csv
│
├── SQL/
│   └── retail_sales_analysis.sql
│
└── Screenshots/
    ├── sales_by_category.png
    ├── top_customers.png
    ├── monthly_sales_trends.png
    └── sales_by_time_shift.png
```

---

## 12. Project Information

**Project:** Retail Sales Analysis

**Domain:** Retail Analytics

**Database:** PostgreSQL

**Language:** SQL

**Author:**  Radhika Lagad

**Platform:** GitHub

**Key Skills Demonstrated:** SQL Querying, Data Analysis, Data Aggregation, Customer Analysis, Sales Analysis, CTEs, and Window Functions.

---

*This project is developed for learning and portfolio purposes to demonstrate practical SQL and data analytics skills.*
