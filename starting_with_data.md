Question 1: What are the top 10 and bottom 10 countries based on the percentage of visitors from each country?

SQL Queries:
```SQL

--getting total visitors by country
WITH country_totals AS (SELECT country, COUNT(*) As count
                        FROM users
                        WHERE country IS NOT NULL AND country != 'N/A'
                        GROUP BY country),
--getting percentage of users from each country
country_percentage AS (SELECT country, 
                       CAST((count)/(SUM(count)OVER()) AS numeric(6,4)) * 100 || '%' AS percentage 
                       FROM country_totals
                       ORDER BY percentage DESC),
--ranking the countries
country_ranked AS (SELECT country, percentage, 
                   ROW_NUMBER()OVER(ORDER BY percentage DESC) AS rank
                   FROM country_percentage),
--seperating the top 10 including rank from the total group
top_10 AS (SELECT country, percentage, 
           ROW_NUMBER()OVER(ORDER BY percentage DESC) AS rank
           FROM country_percentage
           ORDER BY rank LIMIT 10),
--seperating bottom 10 from total group
bottom_10 AS (SELECT country, percentage, 
              ROW_NUMBER()OVER(ORDER BY percentage DESC) AS rank
              FROM country_percentage
              ORDER BY rank DESC LIMIT 10),
--union function to put top and bottom in one table
union_top_bottom AS (SELECT country, percentage, rank 
                     FROM top_10

                     UNION

                     SELECT country, percentage, rank 
                     FROM bottom_10)
--ordering again by rank to maintain proper rank order					 
SELECT * 
FROM union_top_bottom
ORDER BY rank;

```

Answer: 

| country          | percentage | rank |
|------------------|------------|------|
| United States    | 57.1600%   | 1    |
| India            | 4.8800%    | 2    |
| United Kingdom   | 4.5100%    | 3    |
| Canada           | 4.2700%    | 4    |
| Germany          | 2.2600%    | 5    |
| Japan            | 1.6300%    | 6    |
| Australia        | 1.5400%    | 7    |
| France           | 1.4400%    | 8    |
| Taiwan           | 1.1300%    | 9    |
| Netherlands      | 1.0800%    | 10   |
| Palestine        | 0.0100%    | 126  |
| Papua New Guinea | 0.0100%    | 127  |
| Lebanon          | 0.0100%    | 128  |
| Uganda           | 0.0100%    | 129  |
| Jersey           | 0.0100%    | 130  |
| Gibraltar        | 0.0100%    | 131  |
| Haiti            | 0.0100%    | 132  |
| Montenegro       | 0.0100%    | 133  |
| Jamaica          | 0.0100%    | 134  |
| Kosovo           | 0.0100%    | 135  |



Question 2: What is the number of visits by year?

SQL Queries:
```SQL

--difference in date column accross two tables, decided to union them to get all values
WITH union_years AS (SELECT DISTINCT visit_id, date FROM all_sessions
                     UNION
                     SELECT DISTINCT visit_id, date FROM analytics),

--exttacting years from union table					 
year_selection AS(SELECT
                  CASE
                      WHEN EXTRACT(YEAR FROM date) = 2016
                      THEN 2016
	              WHEN EXTRACT(YEAR FROM date) = 2017
	              THEN 2017
	              WHEN EXTRACT(YEAR FROM date) = 2018
	              THEN 2018
	              ELSE 2099
		  END AS years
FROM union_years)

SELECT years, COUNT(years) AS visits
FROM year_selection
GROUP BY years

```

Answer:
| years | visits |
|-------|--------|
| 2017  | 153109 |
| 2016  | 6657   |


Question 3: What insights, if any can be gained by comparing page views and number of sales?

SQL Queries:
```SQL

WITH views_sold AS(SELECT page_views, SUM(units_sold) AS total_sold FROM analytics
WHERE units_sold IS NOT NULL AND page_views IS NOT NULL 
GROUP BY page_views
ORDER BY total_sold DESC)

SELECT page_views, total_sold, ROW_NUMBER()OVER(ORDER BY total_sold DESC)
FROM views_sold

```

Answer:
| page_views | total_sold | rank |
|------------|------------|------|
| 14         | 24329      | 1    |
| 23         | 18439      | 2    |
| 24         | 15654      | 3    |
| 15         | 14744      | 4    |
| 19         | 12731      | 5    |
| 30         | 12150      | 6    |
| 18         | 11688      | 7    |
| 29         | 11493      | 8    |
| 17         | 11385      | 9    |
| 21         | 11060      | 10   |

The top 10 amount of sales are for page views between 10 and 30
The lowest amount of sales are between 90 and 100 page views



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
