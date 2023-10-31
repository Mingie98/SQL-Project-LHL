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
	WHERE CAST(totaltransactionrevenue AS NUMERIC) IS NOT NULL; -- filter out all NULL values

-- Countries with the highest transaction revenue 
SELECT country, SUM(totaltransactionrevenue) 
FROM all_sessionsv1 
GROUP BY country 
ORDER BY SUM(totaltransactionrevenue) DESC;

-- Cities and their respective country with the highest transaction revenue
SELECT country, city, SUM(totaltransactionrevenue) 
FROM all_sessionsv1 
WHERE city <> 'not available in demo dataset' -- filtering out cities that are not defined
GROUP BY country, city
ORDER BY SUM(totaltransactionrevenue) DESC;


Answer:

The country with the highest level of transaction revenue is the United States with 13,154.17

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/34ec1e36-2236-4f20-ba3a-bff687ddf888)

The cities with the highest level of transaction revenue:

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/40e1eca5-dde1-44f0-9965-fb8b7bc992e9)

*It is important to note however that the data is rather incomplete. After filtering out the NULL values from totaltransactionrevenue, we are left with only 81 rows. Drawing conclusion from such a small sample size, will likely be inacurate. Equally, when we look at cities, further chunk of data is filtered out due to the fact that many rows for cities are 'not anailable in demo dataset'.  




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
	WHERE CAST(a.units_sold AS INT) >= 1 -- filtering out NULL values and 0 values;

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
    total_quantityordered / total_unique_visitors AS avg_quantityordered, -- represents the average number of products ordered per unique visitors
    total_unique_visitors
FROM
    countrytotals
ORDER BY avg_quantityordered DESC;

-- returns average quantity ordered from visitors by country and city
WITH citytotals AS ( 
    SELECT
        country,
		city,
        SUM(quantityordered) AS total_quantityordered, -- total quantity ordered from all visitors by country and city
        COUNT(DISTINCT fullvisitorid) AS total_unique_visitors -- total counts of unique visitors by country and city
    FROM
        productsold
    GROUP BY
        country, city
)
SELECT
    country,
	city,
    total_quantityordered / total_unique_visitors AS avg_quantityordered, -- represents the average number of products ordered per unique visitors
	total_unique_visitors
FROM
    citytotals
WHERE city <> 'not available in demo dataset' -- filtering out undefined data 
ORDER BY avg_quantityordered DESC;

Answer:

Average number of products ordered from visitors by country:

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/c4f679d5-be0c-4c57-8be0-5f9672042cd3)

Average number of products ordered from visitors by country and city:

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/c58f321c-841a-4593-8690-7be1ef880bc0)

*Some city/country have very few unique visitors which can skew the results of the average number of products ordered. If they have a high average number of orders it could possibly be a company buying in bulk like in the case of Charlotte, United States.


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







