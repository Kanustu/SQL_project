What are your risk areas? Identify and describe them.

The primary concern regarding this dataset is its data quality, characterized by missing values, unrelated tables, irrelevant data, and redundant data, all of which can lead to compromised and lower-quality outcomes.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

My QA process will focus on the following:
- Expected Output: Check if query results fall within the expected range.
- Data Validation: Cross-reference results with specific queries designed to verify their accuracy.
- Handling Null Values: Ensure proper handling of null and missing values in the data.
- Documentation: Maintain documentation of the SQL queries, including their purpose and intended use.

```SQL
-- used on question 1 to crosscheck for QA purposes
SELECT
    country,
    SUM(revenue)
FROM users AS u
    JOIN analytics AS a USING(user_id)
WHERE country IS NOT NULL AND revenue IS NOT NULL
GROUP BY country;
```
```SQL
-- used as QA on question 1 to crosscheck for QA purposes
SELECT
    city,
    SUM(revenue)
FROM users AS u
    JOIN analytics AS a USING(user_id)
WHERE city IS NOT NULL AND revenue IS NOT NULL AND city != 'N/A'
GROUP BY city;
```
```SQL
-- QA for question 2
SELECT
    city
FROM all_sessions AS a
    FULL OUTER JOIN analytics AS b USING(user_id)
WHERE units_sold IS NOT NULL AND city != 'N/A'
GROUP BY city;
```
```SQL
-- QA for question 2
SELECT
    city
FROM all_sessions AS a
    FULL OUTER JOIN analytics AS b USING(user_id)
WHERE units_sold IS NOT NULL AND city != 'N/A'
GROUP BY city;
```
```SQL
-- used as QA on question 4
SELECT city FROM all_sessions WHERE product_quantity IS NOT NULL AND city != 'N/A';
```
