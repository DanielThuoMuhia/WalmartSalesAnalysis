# Walmart Sales Data Analysis

## About the Project
This project explores Walmart's sales data to identify the top-performing branches and products, analyze sales trends, and understand customer behavior. The goal is to provide insights that can improve and optimize Walmart's sales strategies. The dataset used in this analysis was obtained from the Kaggle Walmart Sales Forecasting Competition.

> **Dataset Overview**: This dataset includes historical sales data for 45 Walmart stores across different regions, each containing multiple departments. The challenge lies in forecasting sales for each department in each store, particularly considering the impact of selected holiday markdown events, which are known to affect sales.

## Purpose of the Project
The primary aim of this project is to gain insights into Walmart's sales data, focusing on the various factors that affect sales performance across different branches and departments.

## About the Data
- **Source**: Kaggle Walmart Sales Forecasting Competition
- **Content**: The dataset contains sales transactions from three Walmart branches located in Mandalay, Yangon, and Naypyitaw. It consists of 17 columns and 1000 rows.

### Columns Description:
| Column                  | Description                                     | Data Type           |
|-------------------------|-------------------------------------------------|---------------------|
| `invoice_id`            | Invoice ID of the sales transaction             | VARCHAR(30)         |
| `branch`                | Branch where the sales were made                | VARCHAR(5)          |
| `city`                  | City where the branch is located                | VARCHAR(30)         |
| `customer_type`         | Type of customer making the purchase            | VARCHAR(30)         |
| `gender`                | Gender of the customer                          | VARCHAR(10)         |
| `product_line`          | Product line of the sold item                   | VARCHAR(100)        |
| `unit_price`            | Price per unit of the product                   | DECIMAL(10, 2)      |
| `quantity`              | Quantity of the product sold                    | INT                 |
| `VAT`                   | Value Added Tax on the purchase                 | FLOAT(6, 4)         |
| `total`                 | Total cost of the purchase                      | DECIMAL(10, 2)      |
| `date`                  | Date of the transaction                         | DATE                |
| `time`                  | Time of the transaction                         | TIMESTAMP           |
| `payment_method`        | Payment method used by the customer             | DECIMAL(10, 2)      |
| `cogs`                  | Cost of Goods Sold                              | DECIMAL(10, 2)      |
| `gross_margin_percentage`| Gross margin percentage                        | FLOAT(11, 9)        |
| `gross_income`          | Gross income from the transaction               | DECIMAL(10, 2)      |
| `rating`                | Customer rating                                 | FLOAT(2, 1)         |

## Analysis Overview
1. **Product Analysis**
   - Understand the different product lines.
   - Identify the best-performing product lines and those needing improvement.

2. **Sales Analysis**
   - Analyze sales trends to measure the effectiveness of sales strategies.
   - Determine which sales strategies need modifications for improved performance.

3. **Customer Analysis**
   - Uncover different customer segments and their purchase behaviors.
   - Evaluate the profitability of each customer segment.

## Approach
1. **Data Wrangling**
   - Inspect and clean data, handle missing values, and prepare data for analysis.
   - Build a database and create tables with appropriate data types.
   - Add new features such as `time_of_day`, `day_name`, and `month_name` to enhance analysis.

2. **Feature Engineering**
   - Generate new columns from existing data to gain deeper insights:
     - **`time_of_day`**: Categorizes sales into Morning, Afternoon, and Evening.
     - **`day_name`**: Extracts the day of the week from the transaction date.
     - **`month_name`**: Extracts the month from the transaction date.

3. **Exploratory Data Analysis (EDA)**
   - Perform in-depth analysis to answer key business questions.

## Key Business Questions
- **Generic Questions:**
  - How many unique cities are in the data?
  - Which city does each branch belong to?

- **Product-Related Questions:**
  - How many unique product lines exist?
  - What is the most common payment method?
  - Which product line is the best seller?
  - What is the total revenue by month?
  - Which month had the highest COGS?
  - Which product line generated the highest revenue and VAT?
  - Which city had the highest revenue?
  - Which product line performed better than average?
  - Which branch sold more products than average?
  - What is the most common product line by gender?
  - What is the average rating for each product line?

- **Sales-Related Questions:**
  - How do sales vary by time of day and day of the week?
  - Which customer type generates the most revenue?
  - Which city has the highest VAT percentage?
  - Which customer type contributes the most VAT?

- **Customer-Related Questions:**
  - How many unique customer types and payment methods exist?
  - Which customer type is the most common and buys the most?
  - What is the gender distribution across branches?
  - Which time of day receives the most ratings?
  - Which day of the week has the highest average ratings?

## Code
```sql
-- Create database
CREATE DATABASE IF NOT EXISTS walmartSales;

-- Create table
CREATE TABLE IF NOT EXISTS sales(
    invoice_id VARCHAR(30) NOT NULL PRIMARY KEY,
    branch VARCHAR(5) NOT NULL,
    city VARCHAR(30) NOT NULL,
    customer_type VARCHAR(30) NOT NULL,
    gender VARCHAR(30) NOT NULL,
    product_line VARCHAR(100) NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    quantity INT NOT NULL,
    tax_pct FLOAT(6,4) NOT NULL,
    total DECIMAL(12, 4) NOT NULL,
    date DATETIME NOT NULL,
    time TIME NOT NULL,
    payment VARCHAR(15) NOT NULL,
    cogs DECIMAL(10,2) NOT NULL,
    gross_margin_pct FLOAT(11,9),
    gross_income DECIMAL(12, 4),
    rating FLOAT(2, 1)
);

-- Data cleaning
SELECT
    *
FROM sales;

-- Add the time_of_day column
SELECT
    time,
    (CASE
        WHEN `time` BETWEEN "00:00:00" AND "12:00:00" THEN "Morning"
        WHEN `time` BETWEEN "12:01:00" AND "16:00:00" THEN "Afternoon"
        ELSE "Evening"
    END) AS time_of_day
FROM sales;

ALTER TABLE sales ADD COLUMN time_of_day VARCHAR(20);

-- For this to work turn off safe mode for update
-- Edit > Preferences > SQL Edito > scroll down and toggle safe mode
-- Reconnect to MySQL: Query > Reconnect to server
UPDATE sales
SET time_of_day = (
    CASE
        WHEN `time` BETWEEN "00:00:00" AND "12:00:00" THEN "Morning"
        WHEN `time` BETWEEN "12:01:00" AND "16:00:00" THEN "Afternoon"
        ELSE "Evening"
    END
);

-- Add day_name column
SELECT
    date,
    DAYNAME(date)
FROM sales;

ALTER TABLE sales ADD COLUMN day_name VARCHAR(10);

UPDATE sales
SET day_name = DAYNAME(date);

-- Add month_name column
SELECT
    date,
    MONTHNAME(date)
FROM sales;

ALTER TABLE sales ADD COLUMN month_name VARCHAR(10);

UPDATE sales
SET month_name = MONTHNAME(date);

-- --------------------------------------------------------------------
-- ---------------------------- Generic ------------------------------
-- --------------------------------------------------------------------
-- How many unique cities does the data have?
SELECT 
    DISTINCT city
FROM sales;

-- In which city is each branch?
SELECT 
    DISTINCT city,
    branch
FROM sales;

-- --------------------------------------------------------------------
-- ---------------------------- Product -------------------------------
-- --------------------------------------------------------------------

-- How many unique product lines does the data have?
SELECT
    DISTINCT product_line
FROM sales;

-- What is the most selling product line
SELECT
    SUM(quantity) as qty,
    product_line
FROM sales
GROUP BY product_line
ORDER BY qty DESC;

-- What is the total revenue by month
SELECT
    month_name AS month,
    SUM(total) AS total_revenue
FROM sales
GROUP BY month_name 
ORDER BY total_revenue;

-- What month had the largest COGS?
SELECT
    month_name AS month,
    SUM(cogs) AS cogs
FROM sales
GROUP BY month_name 
ORDER BY cogs;

-- What product line had the largest revenue?
SELECT
    product_line,
    SUM(total) as total_revenue
FROM sales
GROUP BY product_line
ORDER BY total_revenue DESC;

-- What is the city with the largest revenue?
SELECT
    branch,
    city,
    SUM(total) AS total_revenue
FROM sales
GROUP BY city, branch 
ORDER BY total_revenue;

-- What product line had the largest VAT?
SELECT
    product_line,
    AVG(tax_pct) as avg_tax
FROM sales
GROUP BY product_line
ORDER BY avg_tax DESC;

-- Fetch each product line and add a column to those product 
-- line showing "Good", "Bad". Good if its greater than average sales
SELECT 
    AVG(quantity) AS avg_qnty
FROM sales;

SELECT
    product_line,
    CASE
        WHEN AVG(quantity) > 6 THEN "Good"
        ELSE "Bad"
    END AS remark
FROM sales
GROUP BY product_line;

-- Which branch sold more products than average product sold?
SELECT 
    branch, 
    SUM(quantity) AS qnty
FROM sales
GROUP BY branch
HAVING SUM(quantity) > (SELECT AVG(quantity) FROM sales);

-- What is the most common product line by gender
SELECT
    gender,
    product_line,
    COUNT(gender) AS total_cnt
FROM sales
GROUP BY gender, product_line
ORDER BY total_cnt DESC;

-- What is the average rating of each product line
SELECT
    ROUND(AVG(rating), 2) as avg_rating,
    product_line
FROM sales
GROUP BY product_line
ORDER BY avg_rating DESC;

-- --------------------------------------------------------------------
-- --------------------------------------------------------------------

-- --------------------------------------------------------------------
-- -------------------------- Customers -------------------------------
-- --------------------------------------------------------------------

-- How many unique customer types does the data have?
SELECT
    DISTINCT customer_type
FROM sales;

-- How many unique payment methods does the data have?
SELECT
    DISTINCT payment
FROM sales;

-- What is the most common customer type?
SELECT
    customer_type,
    count(*) as count
FROM sales
GROUP BY customer_type
ORDER BY count DESC;

-- Which customer type buys the most?
SELECT
    customer_type,
    COUNT(*)
FROM sales
GROUP BY customer_type;

-- What is the gender of most of the customers?
SELECT
    gender,
    COUNT(*) as gender_cnt
FROM sales
GROUP BY gender
ORDER BY gender_cnt DESC;

-- What is the gender distribution per branch?
SELECT
    gender,
    COUNT(*) as gender_cnt
FROM sales
WHERE branch = "C"
GROUP BY gender
ORDER BY gender_cnt DESC;
-- Gender per branch is more or less the same hence, I don't think has
-- an effect of the sales per branch and other factors.

-- Which time of the day do customers give most ratings?
SELECT
    time_of_day,
    AVG(rating) AS avg_rating
FROM sales
GROUP BY time_of_day
ORDER BY avg_rating DESC;
-- Looks like time of the day does not really affect the rating, its
-- more or less the same rating each time of the day.

-- Which time of the day do customers give most ratings per branch?
SELECT
    time_of_day,
    AVG(rating) AS avg_rating
FROM sales
WHERE branch = "A"
GROUP BY time_of_day
ORDER BY avg_rating DESC;
-- Branch A and C are doing well in ratings, branch B needs to do a 
-- little more to get better ratings.

-- Which day of the week has the best avg ratings?
SELECT
    day_name,
    AVG(rating) AS avg_rating
FROM sales
GROUP BY day_name 
ORDER BY avg_rating DESC;
-- Mon, Tue and Friday are the top best days for good ratings
-- why is that the case, how many sales are made on these days?

-- Which day of the week has the best average ratings per branch?
SELECT 
    day_name,
    COUNT(day_name) total_sales
FROM sales
WHERE branch = "C"
GROUP BY day_name
ORDER BY total_sales DESC;

-- --------------------------------------------------------------------
-- --------------------------------------------------------------------

-- --------------------------------------------------------------------
-- ---------------------------- Sales ---------------------------------
-- --------------------------------------------------------------------

-- Number of sales made in each time of the day per weekday 
SELECT
    time_of_day,
    COUNT(*) AS total_sales
FROM sales
WHERE day_name = "Sunday"
GROUP BY time_of_day 
ORDER BY total_sales DESC;
-- Evenings experience most sales, the stores are 
-- filled during the evening hours

-- Which of the customer types brings the most revenue?
SELECT
    customer_type,
    SUM(total) AS total_revenue
FROM sales
GROUP BY customer_type
ORDER BY total_revenue;

-- Which city has the largest tax/VAT percent?
SELECT
    city,
    ROUND(AVG(tax_pct), 2) AS avg_tax_pct
FROM sales
GROUP BY city 
ORDER BY avg_tax_pct DESC;

-- Which customer type pays the most in VAT?
SELECT
    customer_type,
    AVG(tax_pct) AS total_tax
FROM sales
GROUP BY customer_type
ORDER BY total_tax;

