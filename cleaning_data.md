What issues will you address by cleaning the data?
- proper datatypes of the columns of each table
- lack of referential integrity
- The lack of connections between the tables.
- Data uniformity
- Irrelevant data
- Duplicate values
- Null values



Queries:
Below, provide the SQL queries you used to clean your data.

- altered column datatypes after the initial import and decided upon more relevant types
```SQL
ALTER TABLE all_sessions
ALTER COLUMN full_visitor_id TYPE varchar(25)
  
```
```SQL
ALTER TABLE all_sessions
ALTER COLUMN total_transaction_revenue TYPE DECIMAL(13,2)
```
```SQL
ALTER TABLE analytics
ALTER COLUMN date TYPE varchar(10)
ALTER TABLE analytics
ALTER COLUMN date TYPE date
USING date::date
```

- created a users table to attempt to increase referrential integrity
```SQL
  CREATE TABLE users(user_id SERIAL,
				   full_visitor_id varchar(25),
				   city varchar(15),
				   country varchar(15));
```
- used full_visitor_id to update users table to populate user_id column
```SQL


```
- Increased uniformity between the invalid values within city and country
```SQL

UPDATE all_sessions
SET country = 'N/A'
WHERE country = '(not set)';
```
```SQL
UPDATE all_sessions
SET city = 'N/A'
WHERE city = '(not set)';
```
```SQL
UPDATE all_sessions
SET city = 'N/A'
WHERE city = 'not available in the demo dataset';

```
- CTE used in questions to alter the revenue total per the included hint
```SQL

WITH cte_revenue AS (SELECT
                     CASE
                         WHEN revenue IS NULL
	                     THEN 0
	                     ELSE CAST(revenue/1000000 AS numeric(13,2))
                     END revenue
                     FROM analytics);
```
- Where statement used within several queries to filter out unwanted values
```SQL
WHERE units_sold IS NOT NULL AND u.city != 'N/A';
```
- Checking for duplicates/nulls
```SQL

SELECT full_visitor_id, 'duplicate value' AS reason
FROM(
SELECT full_visitor_id, COUNT(*) AS counted
FROM all_sessions
GROUP BY full_visitor_id) AS counts
WHERE counted > 1

UNION ALL

SELECT full_visitor_id, 'null value' AS reason
FROM all_sessions
WHERE full_visitor_id IS NULL
GROUP BY full_visitor_id;

```
- checking for nulls, incorrect values
```SQL

SELECT channel_grouping, 'null value' AS reason
FROM all_sessions
WHERE channel_grouping IS NULL

UNION ALL

SELECT channel_grouping, 'incorrect value' AS reason
FROM all_sessions
WHERE channel_grouping NOT IN ('Organic Search', 'Display', 'Referral', 'Paid Search', 'Affiliates',  'Direct', '(Other)');

```
- checking for nulls
```SQL

SELECT city, 'null value' AS reason FROM all_sessions
WHERE city IS NULL

UNION ALL

SELECT country, 'null value' AS reason FROM all_sessions
WHERE country IS NULL;
```
- checking for duplicate values, format issues
```SQL

SELECT visit_id, 'duplicate value' AS reason
FROM (SELECT visit_id, COUNT(*) AS counted FROM all_Sessions
GROUP BY visit_id) AS qa_check
WHERE counted > 1

UNION ALL

SELECT visit_id, 'wrong format' AS reason
FROM all_Sessions
WHERE visit_id NOT LIKE '1%';

```
