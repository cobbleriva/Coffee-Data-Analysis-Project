I created a data analysis project based on this dataset from Kaggle - https://www.kaggle.com/datasets/ahmedabbas757/coffee-sales. I used Claude AI to create me 12 business questions around this dataset. Using Bigquery, I first answered them and then created tables that displayed the results. I also made visualisations of these business questions in Tableau. All the relevant files can be found below.

Here are the 12 business questions with my queries to answer them.

-- Question 1: What is the total revenue generated across all store locations?
SELECT 
  ROUND(SUM(unit_price * transaction_qty), 2) AS total_revenue
FROM 
  coffee.salesdata;

-- Question 2: Which product category generates the most revenue?
SELECT 
  product_category,
  ROUND(SUM(unit_price * transaction_qty), 2) AS total_revenue
FROM 
  coffee.salesdata
GROUP BY 
  product_category
ORDER BY 
  total_revenue DESC;

-- Question 3: What are the top 5 best selling products by number of units sold?
SELECT 
  product_detail,
  SUM(transaction_qty) AS total_units_sold
FROM 
  coffee.salesdata
GROUP BY 
  product_detail
ORDER BY 
  total_units_sold DESC
LIMIT 5;

-- Question 4: Which month had the highest total sales revenue?
SELECT 
  FORMAT_DATE('%B %Y', transaction_date) AS month,
  ROUND(SUM(unit_price * transaction_qty), 2) AS total_revenue
FROM 
  coffee.salesdata
GROUP BY 
  month, EXTRACT(MONTH FROM transaction_date)
ORDER BY 
  EXTRACT(MONTH FROM transaction_date) ASC;

-- Question 5: What is the busiest hour of the day based on number of transactions?
SELECT 
  EXTRACT(HOUR FROM transaction_time) AS hour_of_day,
  COUNT(transaction_id) AS total_transactions
FROM 
  coffee.salesdata
GROUP BY 
  hour_of_day
ORDER BY 
  total_transactions DESC;

-- Question 6: How does revenue differ between weekdays and weekends?
SELECT 
  CASE 
    WHEN EXTRACT(DAYOFWEEK FROM transaction_date) IN (1, 7) THEN 'Weekend'
    ELSE 'Weekday'
  END AS day_type,
  ROUND(SUM(unit_price * transaction_qty), 2) AS total_revenue
FROM 
  coffee.salesdata
GROUP BY 
  day_type
ORDER BY 
  total_revenue DESC;

-- Question 7: Which store location has the highest total revenue?
SELECT 
  store_location,
  ROUND(SUM(unit_price * transaction_qty), 2) AS total_revenue
FROM 
  coffee.salesdata
GROUP BY 
  store_location
ORDER BY 
  total_revenue DESC;

-- Question 8: How many transactions did each store location process in total?
SELECT 
  store_location,
  COUNT(transaction_id) AS total_transactions
FROM 
  coffee.salesdata
GROUP BY 
  store_location
ORDER BY 
  total_transactions DESC;

-- Question 9: What is the average transaction value per product category?
SELECT 
  product_category,
  ROUND(AVG(unit_price * transaction_qty), 2) AS avg_transaction_value
FROM 
  coffee.salesdata
GROUP BY 
  product_category
ORDER BY 
  avg_transaction_value DESC;

-- Question 10: Which product type has the highest average unit price?
SELECT 
  product_detail,
  ROUND(AVG(unit_price), 2) AS avg_unit_price
FROM 
  coffee.salesdata
GROUP BY 
  product_detail
ORDER BY 
  avg_unit_price DESC
LIMIT 1;

-- Question 11: What is the total number of transactions per month?
SELECT 
  FORMAT_DATE('%B %Y', transaction_date) AS month,
  COUNT(transaction_id) AS total_transactions
FROM 
  coffee.salesdata
GROUP BY 
  month, EXTRACT(MONTH FROM transaction_date)
ORDER BY 
  EXTRACT(MONTH FROM transaction_date) ASC;

-- Question 12: Which store location sells the most units per transaction on average?
SELECT 
  store_location,
  ROUND(AVG(transaction_qty), 2) AS avg_units_per_transaction
FROM 
  coffee.salesdata
GROUP BY 
  store_location
ORDER BY 
  avg_units_per_transaction DESC;

 Thanks for viewing.
