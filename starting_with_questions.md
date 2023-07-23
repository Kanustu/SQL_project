Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
- Using the revenue table from analytics instead of the total transaction revenue column from all_sessions table
  as the analytics revenue table has over 15k values compared to the 81 in the all_sessions table
```SQL
-- cleaning up the revenue and unit price values
WITH cte_city AS(SELECT
                     DISTINCT visit_id,
                     revenue/1000000 AS revenue,
                     unit_price/1000000 AS unit_price,
                     units_sold,
                     city
                 FROM analytics
                     JOIN users AS u USING(user_id)
                 WHERE revenue IS NOT NULL AND city IS NOT NULL AND city != 'N/A')
-- 
SELECT
    city,
    CAST(SUM(revenue) AS numeric(8,2)) AS rev_total
FROM cte_city
GROUP BY city
ORDER BY rev_total DESC

```
```SQL
-- cleaning up the revenue and unitprice columns
WITH cte_country AS(SELECT
                        DISTINCT visit_id,
                        revenue/1000000 AS revenue,
                        unit_price/1000000 AS unit_price,
                        units_sold,
                        country
                    FROM analytics
                        JOIN users AS u USING(user_id)
                    WHERE revenue IS NOT NULL AND country IS NOT NULL AND country != 'N/A')

SELECT
    country,
    CAST(SUM(revenue) AS numeric(8,2)) AS rev_total
FROM cte_country
GROUP BY country
ORDER BY rev_total DESC

```




Answer:

| city          | rev_total |
|---------------|-----------|
| Mountain View | 10034.00  |
| San Bruno     | 4229.00   |
| New York      | 2999.00   |
| Sunnyvale     | 2991.00   |
| Chicago       | 1382.00   |

| country       | rev_total |
|---------------|-----------|
| United States | 55762     |
| Canada        | 445       |
| Germany       | 69        |
| Japan         | 29        |
| Switzerland   | 16        |


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
- Calculating average order size per city
```SQL

SELECT
    u.city,
    CAST(AVG(units_sold) AS numeric(5,0)) AS city_avg 
FROM users AS u
    JOIN analytics AS a USING(user_id)
WHERE units_sold IS NOT NULL AND city != 'N/A'
GROUP BY u.city
ORDER BY city_avg DESC

```
- Calculating average order size per country
```SQL
SELECT
    u.country,
    CAST(AVG(units_sold) AS numeric(5,0)) AS country_avg 
FROM users AS u
    JOIN analytics AS a USING(user_id)
WHERE units_sold IS NOT NULL AND country != 'N/A'
GROUP BY u.country
ORDER BY country_avg DESC
```



Answer:

| city                | city_avg |
|---------------------|----------|
| San Bruno           | 53       |
| Mountain View       | 18       |
| San Jose            | 9        |
| Salem               | 8        |
| New York            | 7        |
| Charlotte           | 5        |


| country            | country_avg |
|--------------------|-------------|
| Czechia            | 15          |
| United States      | 12          |
| Bulgaria           | 2           |
| Canada             | 2           |
| Mexico             | 2           |
| Colombia           | 1           |



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL

WITH city_category AS (SELECT
                           DISTINCT v2_product_category,
                           city
                       FROM all_sessions
                       WHERE v2_product_category NOT IN('(not set)', '${escCatTitle}') AND city != 'N/A'
                           AND transactions IS NOT NULL
                       ORDER BY city)

SELECT
    v2_product_category,
    COUNT(v2_product_category) AS prod_count FROM city_category
GROUP BY v2_product_category
ORDER BY prod_count DESC

```
```SQL

WITH country_category AS (SELECT
                              DISTINCT v2_product_category,
                              country
                          FROM all_sessions
                          WHERE v2_product_category NOT IN('(not set)', '${escCatTitle}') AND country != 'N/A'
                              AND transactions IS NOT NULL
                          ORDER BY country)

SELECT
    v2_product_category,
    COUNT(v2_product_category) AS prod_count FROM country_category
GROUP BY v2_product_category
ORDER BY prod_count DESC

```

Answer:
Nest USA products dominate as the most favored across all cities, with apparel items coming in a close second. The bulk of purchases originate within the USA, which also has the most extensive range of product categories.


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL

WITH rank_products AS (SELECT
                           city,
                           v2_product_name,
                           product_quantity,
                           DENSE_RANK()OVER(PARTITION BY city ORDER BY product_quantity DESC) AS rank 
                       FROM all_sessions 
                       WHERE product_quantity IS NOT NULL AND city != 'N/A'
                       ORDER BY RANK)

SELECT
    city,
    v2_product_name FROM rank_products
WHERE rank = 1


```
Highest selling product per country
```SQL

WITH rank_products AS (SELECT
                           country,
                           v2_product_name,
                           product_quantity,
                           DENSE_RANK()OVER(PARTITION BY country ORDER BY product_quantity DESC) AS rank
                       FROM all_sessions 
                       WHERE product_quantity IS NOT NULL 
                       ORDER BY RANK)

SELECT
    country,
    v2_product_name FROM rank_products
WHERE rank = 1

```



Answer:
The only discernible pattern is the overall popularity of Nest products, particularly in the city of Mountain View.



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

```SQL

SQL Queries:
--revenue by month by city
WITH month_rev as(SELECT city, TO_CHAR(date, 'Month') AS month, SUM(revenue/1000000) AS revenue FROM analytics
JOIN users AS u USING(user_id)
WHERE revenue IS NOT NULL AND city IS NOT NULL AND city != 'N/A'
GROUP BY city, date)

SELECT city, month, SUM(revenue), RANK()OVER(PARTITION BY city ORDER BY SUM(revenue) DESC) AS month_rev_rank FROM month_rev 
GROUP BY month, city
```
```SQL
--revenue by country by month
WITH month_rev as(SELECT country, TO_CHAR(date, 'Month') AS month, SUM(revenue/1000000) AS revenue FROM analytics
JOIN users AS u USING(user_id)
WHERE revenue IS NOT NULL AND country IS NOT NULL AND country != 'N/A'
GROUP BY country, date)


SELECT country, month, SUM(revenue), RANK()OVER(PARTITION BY country ORDER BY SUM(revenue) DESC) AS month_rev_rank FROM month_rev 
GROUP BY month, country
```
```SQL

--overall revenue by month
WITH month_rev as(SELECT city, TO_CHAR(date, 'Month') AS month, SUM(revenue/1000000) AS revenue FROM analytics
JOIN users AS u USING(user_id)
WHERE revenue IS NOT NULL AND city IS NOT NULL AND city != 'N/A'
GROUP BY city, date)

SELECT month, SUM(revenue) FROM month_rev GROUP BY month ORDER BY month DESC
```




Answer:
The revenue appears consistent on a monthly basis, with Mountainview emerging as the top revenue-generating location overall. Additionally, among all the countries, the United States stands out as the highest revenue contributor.






