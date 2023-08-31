# Big Sales Data analysis Project
Second Portfolio
data analyst portfolio (Big Sales Data 2019)

-- Kaggle datasets on Sales of electronic devices and Tech Products
-- Contain the sales data for each product for every month Jan 2019 - Dec 2019
-- cleaning the data in excel and done some data format and data wrangling
-- import the data to postgresql to view a better insight since this is a large data (excel is bit slow managing this data)

-- checking and evaluating the datasets

SELECT * FROM january;
SELECT * FROM february;
SELECT * FROM march;
SELECT * FROM april;
SELECT * FROM may;
SELECT * FROM june;
SELECT * FROM july;
SELECT * FROM august;
SELECT * FROM september;
SELECT * FROM october;
SELECT * FROM november;
SELECT * FROM december;

-- planning on combining all the data so that we can get an insight on annual sales
-- detecting that in the december month, there are few data that were recorded for January 2020

SELECT * 
FROM december
WHERE date > '31-12-2019';

CREATE TABLE dec AS SELECT * 
FROM december
WHERE date <= '31-12-2019';

SELECT * FROM dec;

CREATE TABLE annual_sales AS
SELECT * FROM january
UNION
SELECT * FROM february
UNION
SELECT * FROM march
UNION
SELECT * FROM april
UNION
SELECT * FROM may
UNION
SELECT * FROM june
UNION
SELECT * FROM july
UNION
SELECT * FROM august
UNION
SELECT * FROM september
UNION
SELECT * FROM october
UNION
SELECT * FROM november
UNION
SELECT * FROM dec;

-- create a new column where we group month by date 

ALTER TABLE annual_sales
ADD month VARCHAR(50);

UPDATE annual_sales
SET month =
	CASE 
		WHEN date >= '2019-01-01' AND date < '2019-02-01' THEN 'Jan'
		WHEN date >= '2019-02-01' AND date < '2019-03-01' THEN 'Feb'
		WHEN date >= '2019-03-01' AND date < '2019-04-01' THEN 'Mar'
		WHEN date >= '2019-04-01' AND date < '2019-05-01' THEN 'Apr'
		WHEN date >= '2019-05-01' AND date < '2019-06-01' THEN 'May'
		WHEN date >= '2019-06-01' AND date < '2019-07-01' THEN 'June'
		WHEN date >= '2019-07-01' AND date < '2019-08-01' THEN 'July'
		WHEN date >= '2019-08-01' AND date < '2019-09-01' THEN 'Aug'
		WHEN date >= '2019-09-01' AND date < '2019-10-01' THEN 'Sep'
		WHEN date >= '2019-10-01' AND date < '2019-11-01' THEN 'Oct'
		WHEN date >= '2019-11-01' AND date < '2019-12-01' THEN 'Nov'
		WHEN date >= '2019-12-01' THEN 'Dec'
		ELSE '2020'
	END;

-- now i will calculate and analyze the new annual_sales table 

SELECT SUM(price)
FROM annual_sales;

-- count the total sold for each product for a year

SELECT product, COUNT(product)
FROM annual_sales
GROUP BY product;


/*
<img width="615" alt="Count of product sold" src="https://github.com/Ayyad96/Ayyads_portfolio_2/assets/140683898/927244e4-3d20-4259-a765-9e9889ecb176">
*/


-- Find the total sales by month for a year

SELECT month, SUM(sales)
FROM annual_sales
GROUP BY month;


/*
<img width="615" alt="Sales by month" src="https://github.com/Ayyad96/Ayyads_portfolio_2/assets/140683898/b0c325ef-ca5e-4b33-99c5-44b62e20fc39">
*/


-- find where the highest sales by city

SELECT city, SUM(sales)
FROM annual_sales
GROUP BY city
ORDER BY SUM(sales) DESC;


/*
<img width="614" alt="Sales by City" src="https://github.com/Ayyad96/Ayyads_portfolio_2/assets/140683898/bbd0b1fa-42e4-49c6-8627-27d3ff9363da">
*/


-- Find which product has the highest sales 

SELECT product, SUM(sales)
FROM annual_sales
GROUP BY product 
ORDER BY SUM(sales) DESC
LIMIT 5;


/*
<img width="614" alt="Sales by Product top 5" src="https://github.com/Ayyad96/Ayyads_portfolio_2/assets/140683898/31d59db5-5d66-4435-b4a5-7af1f71e4f9e">
*/



-- the result shows that the highest sales recorded coming from an apple product and the sales are rising drastically during the month of december
-- the data shows that San Francisco with 8.25M has the highest sales of all time compared to other city followed by Los Angeles with 5.45M
-- Besides that, the data also shows that customer tends to look for battery product where data recording that the highest count for product sold was battery
-- Further, we can see that the sales growth increase in a year eventhough if we take the measure of the sales growth from month tend to fluctuate 


/*
<img width="616" alt="Full report" src="https://github.com/Ayyad96/Ayyads_portfolio_2/assets/140683898/7efd0c2c-76b3-4ef1-bdd7-e031983ce926">
*/
