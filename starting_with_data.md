## Question 1: Find how many unique visitors are in the database? 

### SQL Queries:
```
SELECT DISTINCT fullvisitorid
FROM all_sessions

SELECT DISTINCT fullvisitorid
FROM analytics
```
### Answer: 

Unique number of visitors from the all_sessions table (14,223)

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/e7f31dbb-8082-4140-be50-a7de347f1ee0)

Unique number of visitors from the analytics table (120,018)

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/f9c83e2f-db4e-4f48-977c-dd91b2e22706)

The database seems to be extremely incomplete as there is a large discrepensies in the number of unique visitors between the all_sessions and analytics tables.

## Question 2: What is the most common channelgrouping from the all_sessions table?

### SQL Queries:
```
SELECT channelgrouping, COUNT(channelgrouping) 
FROM all_sessions
GROUP BY channelgrouping
ORDER BY COUNT(channelgrouping) DESC
```
### Answer:

Organic search is the most popular

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/fcb9a964-0d1d-4fb2-8c0c-f926f1e49381)



## Question 3: Is there a preference in channelgrouping by country?

### SQL Queries:
```
SELECT country, channelgrouping, COUNT(channelgrouping) 
FROM all_sessions
GROUP BY country, channelgrouping
ORDER BY country, COUNT(channelgrouping) DESC
```
### Answer:

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/c72490b8-08f7-4a19-accc-29849b1576c0)

![image](https://github.com/Mingie98/SQL-Project-LHL/assets/138625460/b66e2f1c-7755-442d-ad10-1c46968864f9)

Organic search is the most popular channelgrouping amongst the majority of the countries



