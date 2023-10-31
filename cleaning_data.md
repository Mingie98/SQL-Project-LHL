## What issues will you address by cleaning the data?

- Cleaning the unit_price column where it needs to be divided by 1000000 for it to make sense and then rounded to 2 decimal point.
- Casting some columns from VARCHAR to INT in order to be able to use aggregate functions such as SUM.
- Filtering out NULL values or otherwise undefined data such as '(not set)'
- Simplified the too many categories of products into a few comprehensible ones


## Example of Queries:
```
	SELECT 
		als.fullvisitorid,
		als.country,
		als.city,
		als.productsku,
		als.v2productname,
		CASE 
			WHEN als.v2productcategory LIKE '%Apparel%' THEN 'Apparel'
			WHEN als.v2productcategory LIKE '%Bags%' THEN 'Bags'
			WHEN als.v2productcategory LIKE '%Bottles%' THEN 'Drinkware'
			WHEN als.v2productcategory LIKE '%Drinkware%' THEN 'Drinkware'
			WHEN als.v2productcategory LIKE '%Electronics%' THEN 'Electronics'
			WHEN als.v2productcategory LIKE '%Headgear%' THEN 'Apparel'
			WHEN als.v2productcategory LIKE '%Accessories%' THEN 'Accessories'
			WHEN als.v2productcategory LIKE '%Apparel%' THEN 'Apparel'
			WHEN als.v2productcategory LIKE '%Brand%' THEN 'Shop By Brand'
			WHEN als.v2productcategory LIKE '%Electronics%' THEN 'Electronics'
			WHEN als.v2productcategory LIKE '%Lifestyle%' THEN 'Lifestyle'
			WHEN als.v2productcategory LIKE '%Nest%' THEN 'Shop By Brand'
			WHEN als.v2productcategory LIKE '%Office%' THEN 'Office'
			WHEN als.v2productcategory LIKE '%Waze%' THEN 'Shop By Brand'
			WHEN als.v2productcategory LIKE '%Wearables%' THEN 'Apparel'
			WHEN als.v2productcategory LIKE '%Youtube%' THEN 'Shop By Brand'
			ELSE 'Other'
		END AS productcategory, 
		CAST(a.units_sold AS INT) 
	FROM all_sessions als
	JOIN analytics a ON als.fullvisitorid = a.fullvisitorid
	WHERE CAST(a.units_sold AS INT) >= 1; -- filtering out NULL values and 0 values
-------------------------------------------------------------------------------------
INSERT INTO productinfo(fullvisitorid, country, city, productsku, v2productname, units_sold, unit_price, total_price)
SELECT 
    als.fullvisitorid,
    als.country,
    als.city,
    als.productsku,
    als.v2productname,
    CAST(a.units_sold AS INT), 
    ROUND(CAST(a.unit_price AS NUMERIC) / 1000000, 2), -- Round to 2 decimal places
    CAST(a.units_sold AS INT) * ROUND(CAST(a.unit_price AS NUMERIC) / 1000000, 2) AS total_price -- Round to 2 decimal places  
FROM all_sessions als
JOIN analytics a ON als.fullvisitorid = a.fullvisitorid
WHERE CAST(a.units_sold AS INT) >= 1; -- filtering out NULL values and 0 values
```
