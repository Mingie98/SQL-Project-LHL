## What are your risk areas? Identify and describe them.

- There are quite a few duplicates and using DISTINCT can clean them such as using it to find the unitque number of visitors
- Cleaning out NULL values or undefined data such as 'not available in demo dataset' that are not relevant to the analysis 


## QA Process Queries Example:
```
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
------------------------------------------------------------------------------------------------------------------
-- Cities and their respective country with the highest transaction revenue
SELECT country, city, SUM(totaltransactionrevenue) 
FROM all_sessionsv1 
WHERE city <> 'not available in demo dataset' -- filtering out cities that are not defined
GROUP BY country, city
ORDER BY SUM(totaltransactionrevenue) DESC; 
```
