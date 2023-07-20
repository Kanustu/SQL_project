Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```SQL
--cleaned revenue column
WITH cte_revenue AS (SELECT
                     CASE
                         WHEN total_transaction_revenue IS NULL
	                     THEN 0
	                     ELSE CAST(total_transaction_revenue/1000000 AS numeric(13,2))
                     END AS total_transaction_revenue, city, country
                     FROM all_sessions)
                     
-- countries with highest revenue
SELECT country, SUM(total_transaction_revenue) AS total_country_revenue
    FROM cte_revenue 
GROUP BY country
    HAVING SUM(total_transaction_revenue) > 0
ORDER BY total_country_revenue DESC
```
```SQL
--cleaned revenue column
WITH cte_revenue AS (SELECT
                     CASE
                         WHEN total_transaction_revenue IS NULL
	                     THEN 0
	                     ELSE CAST(total_transaction_revenue/1000000 AS numeric(13,2))
                     END AS total_transaction_revenue, city, country
                     FROM all_sessions)
-- cities with highest revenue
SELECT city, SUM(total_transaction_revenue) AS total_city_revenue
    FROM cte_revenue 
GROUP BY city
    HAVING city != 'N/A' AND SUM(total_transaction_revenue) > 0
ORDER BY total_city_revenue DESC
```




Answer:

| **country**   | **total_country_revenue** |
|---------------|---------------------------|
| United States | 13154.17                  |
| Israel        | 602.00                    |
| Australia     | 358.00                    |
| Canada        | 150.15                    |
| Switzerland   | 16.99                     |


| **city**      | **total_city_revenue** |
|---------------|------------------------|
| San Francisco | 1564.32                |
| Sunnyvale     | 992.23                 |
| Atlanta       | 854.44                 |
| Palo Alto     | 608.00                 |
| Tel Aviv-Yafo | 602.00                 |
| New York      | 598.35                 |
| Mountain View | 483.36                 |
| Los Angeles   | 479.48                 |
| Chicago       | 449.52                 |
| Seattle       | 358.00                 |
| Sydney        | 358.00                 |
| San Jose      | 262.38                 |
| Austin        | 157.78                 |
| Nashville     | 157.00                 |
| San Bruno     | 103.77                 |
| Toronto       | 82.16                  |
| Houston       | 38.98                  |
| Columbus      | 21.99                  |
| Zurich        | 16.99                  |

**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
Calculating average order size per city
```SQL

SELECT u.city, CAST(AVG(units_sold) AS numeric(5,0)) AS city_avg 
    FROM users AS u
    JOIN analytics AS a USING(user_id)
WHERE units_sold IS NOT NULL AND city != 'N/A'
GROUP BY u.city
ORDER BY city_avg DESC

```
Calculating average order size per country
```SQL
SELECT u.country, CAST(AVG(units_sold) AS numeric(5,0)) AS country_avg 
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
| Jersey City         | 5        |
| Nashville           | 5        |
| Chicago             | 5        |
| Sunnyvale           | 4        |
| Austin              | 4        |
| Los Angeles         | 4        |
| Pittsburgh          | 4        |
| Paris               | 3        |
| Seattle             | 3        |
| Atlanta             | 3        |
| Cambridge           | 3        |
| Houston             | 3        |
| Toronto             | 2        |
| Hyderabad           | 2        |
| Denver              | 2        |
| Hamburg             | 2        |
| Kirkland            | 2        |
| Palo Alto           | 1        |
| Phoenix             | 1        |
| San Diego           | 1        |
| San Francisco       | 1        |
| Santa Clara         | 1        |
| Santa Monica        | 1        |
| Santiago            | 1        |
| Seoul               | 1        |
| Singapore           | 1        |
| South San Francisco | 1        |
| Stockholm           | 1        |
| Sydney              | 1        |
| Tel Aviv-Yafo       | 1        |
| Washington          | 1        |
| Yokohama            | 1        |
| Zhongli District    | 1        |
| Ahmedabad           | 1        |
| Zurich              | 1        |
| Ann Arbor           | 1        |
| Bangkok             | 1        |
| Berlin              | 1        |
| Bogota              | 1        |
| Courbevoie          | 1        |
| Cupertino           | 1        |
| Dallas              | 1        |
| Detroit             | 1        |
| Dublin              | 1        |
| Fremont             | 1        |
| Helsinki            | 1        |
| Hong Kong           | 1        |
| Irvine              | 1        |
| Jakarta             | 1        |
| Kitchener           | 1        |
| London              | 1        |
| Madrid              | 1        |
| Milpitas            | 1        |
| Minato              | 1        |
| Montevideo          | 1        |
| Munich              | 1        |
| Osaka               | 1        |

| country            | country_avg |
|--------------------|-------------|
| Czechia            | 15          |
| United States      | 12          |
| Bulgaria           | 2           |
| Canada             | 2           |
| Mexico             | 2           |
| Colombia           | 1           |
| Denmark            | 1           |
| Dominican Republic | 1           |
| Egypt              | 1           |
| Finland            | 1           |
| France             | 1           |
| Germany            | 1           |
| Guatemala          | 1           |
| Hong Kong          | 1           |
| India              | 1           |
| Indonesia          | 1           |
| Ireland            | 1           |
| Israel             | 1           |
| Japan              | 1           |
| Maldives           | 1           |
| Netherlands        | 1           |
| Norway             | 1           |
| Panama             | 1           |
| Portugal           | 1           |
| Romania            | 1           |
| Russia             | 1           |
| Singapore          | 1           |
| South Korea        | 1           |
| Spain              | 1           |
| Sri Lanka          | 1           |
| Sweden             | 1           |
| Switzerland        | 1           |
| Taiwan             | 1           |
| Thailand           | 1           |
| United Kingdom     | 1           |
| Uruguay            | 1           |
| Australia          | 1           |
| Vietnam            | 1           |
| Austria            | 1           |
| Belarus            | 1           |
| Belgium            | 1           |
| Chile              | 1           |



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL

SELECT DISTINCT v2_product_category, city
FROM all_sessions
WHERE v2_product_category NOT IN('(not set)', '${escCatTitle}') AND city != 'N/A'
AND transactions IS NOT NULL
ORDER BY city

```
```SQL

SELECT DISTINCT v2_product_category, country
FROM all_sessions
WHERE v2_product_category NOT IN('(not set)', '${escCatTitle}') AND country != 'N/A'
AND transactions IS NOT NULL
ORDER BY country

```

Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL

Highest selling product per city
WITH highest_per_city AS(SELECT 
			    u.city, 
			    v2_product_name AS product, 
			    SUM(units_sold) AS total_sold, 
			    RANK()OVER(PARTITION BY u.city ORDER BY SUM(units_sold) DESC) AS highest_sold
			 FROM all_sessions AS a
                            JOIN users AS u USING(user_id)
                            JOIN analytics AS an USING(user_id)
                         WHERE units_sold IS NOT NULL AND u.city != 'N/A'
                         GROUP BY u.city, v2_product_name)
			
SELECT * FROM highest_per_city
WHERE highest_sold = 1

```
Highest selling product per country
```SQL

--highest selling product per country
WITH highest_per_country AS(SELECT 
			    u.country, 
			    v2_product_name AS product, 
			    SUM(units_sold) AS total_sold, 
			    RANK()OVER(PARTITION BY u.country ORDER BY SUM(units_sold) DESC) AS highest_sold
			FROM all_sessions AS a
                JOIN users AS u USING(user_id)
                JOIN analytics AS an USING(user_id)
            WHERE units_sold IS NOT NULL AND u.country != 'N/A'
            GROUP BY u.country, v2_product_name)
			
SELECT country, product, total_sold FROM highest_per_country
WHERE highest_sold = 1

```



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







