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
WHERE date > '31-12-2019'

CREATE TABLE dec AS SELECT * 
FROM december
WHERE date <= '31-12-2019'

SELECT * FROM dec

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
SELECT * FROM dec

-- create a new column where we group month by date 

ALTER TABLE annual_sales
ADD month VARCHAR(50)

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
FROM annual_sales

