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
SELECT country, SUM(total_transaction_revenue) AS total_country_revenue FROM cte_revenue 
GROUP BY country
HAVING SUM(total_transaction_revenue) > 0
ORDER BY total_country_revenue DESC

-- cities with highest revenue
SELECT city, SUM(total_transaction_revenue) AS total_city_revenue FROM cte_revenue 
GROUP BY city
HAVING city != 'N/A' AND SUM(total_transaction_revenue) > 0
ORDER BY total_city_revenue DESC

```




Answer:
"country"	    "total_country_revenue"  
"United States"	 13154.17
"Israel"	     602.00
"Australia"	     358.00
"Canada"	     150.15
"Switzerland"	 16.99

"city"	        "total_city_revenue"
"San Francisco"	 1564.32
"Sunnyvale"	     992.23
"Atlanta"	     854.44
"Palo Alto"	     608.00
"Tel Aviv-Yafo"	 602.00
"New York"	     598.35
"Mountain View"	 483.36
"Los Angeles"	 479.48
"Chicago"	     449.52
"Seattle"	     358.00
"Sydney"	     358.00
"San Jose"	     262.38
"Austin"	     157.78
"Nashville"	     157.00
"San Bruno"	     103.77
"Toronto"	     82.16
"Houston"	     38.98
"Columbus"	     21.99
"Zurich"	     16.99


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







