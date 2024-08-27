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


