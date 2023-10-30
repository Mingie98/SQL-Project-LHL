Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

-- Creating a temp table with totaltransactionrevenue cleaned of extra 0s
CREATE TEMP TABLE all_sessionsv1(
	country VARCHAR,
	city VARCHAR,
	totaltransactionrevenue NUMERIC 
);

INSERT INTO all_sessionsv1 (country, city, totaltransactionrevenue)
	SELECT 
		country,
		city,
		CAST(totaltransactionrevenue AS NUMERIC)/1000000 AS totaltransactionrevenue -- clean out the extra 0s
	FROM all_sessions
	WHERE CAST(totaltransactionrevenue AS NUMERIC) IS NOT NULL -- filter out all NULL values

-- Countries with the highest transaction revenue 
SELECT country, SUM(totaltransactionrevenue) 
FROM all_sessionsv1 
GROUP BY country 
ORDER BY SUM(totaltransactionrevenue) DESC

-- Cities and their respective country with the highest transaction revenue
SELECT country, city, SUM(totaltransactionrevenue) 
FROM all_sessionsv1 
GROUP BY country, city
ORDER BY SUM(totaltransactionrevenue) DESC


Answer:

The country with the highest level of transaction revenue is the United States with 13,154.17

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/34ec1e36-2236-4f20-ba3a-bff687ddf888)

The cities with the highest level of transaction revenue:

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/92b173bd-feae-4fee-abb9-c5cc899f4d13)

It is important to note however that the data is rather incomplete. After filtering out the NULL values from totaltransactionrevenue, we are left with only 81 rows. Drawing conclusion from such a small sample size, will likely be inacurate. Also, the highest transaction revenue city is 'not available in demo dataset' which should be considered as a NULL value and again, reflecting that the database is incomplete. 




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

-- Creating a temp table that joins columns from all_sessions and analytics tables (quantityordered = units_sold FROM analytics)
CREATE TEMP TABLE productsold(
	fullvisitorid VARCHAR,
	country VARCHAR,
	city VARCHAR,
	quantityordered INT,
);

INSERT INTO productsold (fullvisitorid, country, city, quantityordered)
	SELECT 
		als.fullvisitorid,
		als.country,
		als.city,
		CAST(a.units_sold AS INT) -- renamed to quantityordered in the temp table
	FROM all_sessions als
	JOIN analytics a ON als.fullvisitorid = a.fullvisitorid
	JOIN products
	WHERE CAST(a.units_sold AS INT) >= 1 -- filtering out NULL values and 0 values

-- returns average quantity ordered from visitors by country
WITH countrytotals AS ( 
    SELECT
        country,
        SUM(quantityordered) AS total_quantityordered, -- total quantity ordered from all visitors by country
        COUNT(DISTINCT fullvisitorid) AS total_unique_visitors -- total counts of unique visitors by country
    FROM
        productsold
    GROUP BY
        country
)
SELECT
    country,
    total_quantityordered / total_unique_visitors AS avg_quantityordered -- represents the average number of products ordered per unique visitors
FROM
    countrytotals
ORDER BY avg_quantityordered DESC;



Answer:

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/8d675123-2420-4ee2-962e-3992b3d85386)




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







