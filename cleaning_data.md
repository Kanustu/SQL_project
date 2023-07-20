What issues will you address by cleaning the data?

- The lack of connections between the tables.
- Data uniformity
- Irrelevant data
- Duplicate values
- Null values



Queries:
Below, provide the SQL queries you used to clean your data.
- Increased uniformity between the invalid values within city and country
```SQL

UPDATE all_sessions
SET country = 'N/A'
WHERE country = '(not set)'
```
```SQL
UPDATE all_sessions
SET city = 'N/A'
WHERE city = '(not set)'
```
```SQL
UPDATE all_sessions
SET city = 'N/A'
WHERE city = 'not available in the demo dataset'

```
- CTE used in questions to alter the revenue total per the included hint
```SQL

WITH cte_revenue AS (SELECT
                     CASE
                         WHEN revenue IS NULL
	                     THEN 0
	                     ELSE CAST(revenue/1000000 AS numeric(13,2))
                     END revenue
                     FROM analytics)
```
- Where statement used within several queries to filter out unwanted values
```SQL
WHERE units_sold IS NOT NULL AND u.city != 'N/A'
```
