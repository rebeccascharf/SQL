# Customer and Order Analysis with SQL

This project demonstrates SQL analysis across multiple sales and customer tables to answer common business questions about orders, products, revenue, customer accounts, and purchasing behavior.

Using SQL joins, aggregation, filtering, and subqueries, I explored monthly sales activity across January and February and analyzed product-level and customer-level patterns.

---

## Objective

The primary goals of this analysis were to:

- Count unique orders placed in January
- Identify how many January orders included iPhones
- Retrieve customer account numbers tied to February orders
- Find the cheapest product sold in January
- Calculate total revenue by product for January
- Analyze product sales and revenue for a specific February store location
- Measure customer purchasing behavior for larger February orders

---

## Business Questions

1. How many unique orders were placed in January?
2. How many of those January orders were for an iPhone?
3. Which customer account numbers were associated with orders placed in February?
4. Which product was the cheapest one sold in January, and what was its price?
5. What was the total revenue for each product sold in January?
6. Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?
7. How many customers ordered more than 2 products at a time in February, and what was the average amount spent for those customers?

---

## Dataset Overview

This project uses three tables from the BIT_DB database:

### 1. JanSales
Contains January sales transactions, including:
- orderid
- product
- quantity
- price

### 2. FebSales
Contains February sales transactions, including:
- orderid
- product
- quantity
- price
- location

### 3. customers
Contains customer account and order mapping data, including:
- acctnum
- order_id

---

## Tools & Skills Used

- SQL
- SELECT
- DISTINCT
- WHERE
- INNER JOIN
- LEFT JOIN
- GROUP BY
- ORDER BY
- COUNT
- SUM
- AVG
- Subqueries
- Filtering invalid header rows
- Revenue calculations

---

## Methodology

- Filtered out invalid rows where `orderid` contained header text instead of actual order data
- Used `COUNT(DISTINCT orderid)` to measure true order counts
- Joined `customers` with `FebSales` to connect customer accounts to February orders
- Used a subquery to identify the minimum product price in January
- Aggregated revenue by multiplying quantity and price, then grouping by product
- Analyzed sales performance at a specific February location
- Measured high-quantity purchasing behavior using customer joins and average spend calculations

---

## SQL Analysis

-- 1. How many unique orders were placed in January?
SELECT COUNT(DISTINCT orderid) AS january_order_count
FROM BIT_DB.JanSales
WHERE LENGTH(orderid) = 6
  AND orderid <> 'Order ID';

-- 2. How many of those January orders were for an iPhone?
SELECT COUNT(DISTINCT orderid) AS january_iphone_orders
FROM BIT_DB.JanSales
WHERE product = 'iPhone'
  AND LENGTH(orderid) = 6
  AND orderid <> 'Order ID';

-- 3. Select the customer account numbers for all the orders that were placed in February.
SELECT DISTINCT cust.acctnum
FROM BIT_DB.customers cust
INNER JOIN BIT_DB.FebSales feb
  ON cust.order_id = feb.orderid
WHERE LENGTH(feb.orderid) = 6
  AND feb.orderid <> 'Order ID';

-- 4. Which product was the cheapest one sold in January, and what was the price?
SELECT DISTINCT product, price
FROM BIT_DB.JanSales
WHERE price IN (
    SELECT MIN(price)
    FROM BIT_DB.JanSales
);

-- Alternative version
SELECT DISTINCT product, price
FROM BIT_DB.JanSales
ORDER BY price ASC
LIMIT 1;

-- 5. What is the total revenue for each product sold in January?
SELECT product,
       SUM(quantity) * price AS revenue
FROM BIT_DB.JanSales
GROUP BY product, price
ORDER BY revenue DESC;

-- 6. Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?
SELECT product,
       SUM(quantity) AS units_sold,
       SUM(quantity) * price AS revenue
FROM BIT_DB.FebSales
WHERE location = '548 Lincoln St, Seattle, WA 98101'
GROUP BY product, price
ORDER BY revenue DESC;

-- 7. How many customers ordered more than 2 products at a time in February,
-- and what was the average amount spent for those customers?
SELECT COUNT(DISTINCT cust.acctnum) AS customer_count,
       AVG(feb.quantity * feb.price) AS avg_amount_spent
FROM BIT_DB.FebSales feb
LEFT JOIN BIT_DB.customers cust
  ON feb.orderid = cust.order_id
WHERE feb.quantity > 2
  AND LENGTH(feb.orderid) = 6
  AND feb.orderid <> 'Order ID';

## Key Skills Demonstrated

-Cleaning messy transactional data
-Joining customer and sales tables
-Counting distinct business events instead of raw rows
-Using aggregation to calculate revenue
-Applying filtering to improve data quality
-Translating business questions into SQL queries
-Structuring query results into actionable analysis

## What I Learned

-How to validate order data before counting it
-How to connect customer information to sales records using joins
-How to calculate revenue at the product and location level
-How subqueries can help answer pricing questions
-How SQL can be used to answer practical business questions around sales and customer behavior

## Next Steps

To expand this project, I could:

-Compare January vs. February product performance
-Analyze top-performing products across both months
-Calculate average order value by month
-Segment customer purchasing behavior further
-Visualize revenue and product trends in Tableau or Power BI

## Note

This project uses a sample dataset for educational and portfolio-building purposes. It is designed to demonstrate practical SQL skills used in sales, customer, and revenue analysis.

