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



Question 2: 

SQL Queries:

Answer:



Question 3: 

SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
