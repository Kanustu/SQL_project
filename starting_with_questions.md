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

|country	|    total_country_revenue |       
|---------------|   ------------------------ |
"United States"	|    13154.17
"Israel"	|    602.00
"Australia"	|     358.00
"Canada"	|     150.15
"Switzerland"	|     16.99


|city           |total_city_revenue|
|---------------|------------------|
"San Francisco"	| 1564.32
"Sunnyvale"	|     992.23
"Atlanta"	|     854.44
"Palo Alto"	|     608.00
"Tel Aviv-Yafo"	| 602.00
"New York"	|     598.35
"Mountain View"	| 483.36
"Los Angeles"	| 479.48
"Chicago"	|     449.52
"Seattle"	|     358.00
"Sydney"	|     358.00
"San Jose"	|     262.38
"Austin"	|     157.78
"Nashville"	|     157.00
"San Bruno"	|     103.77
"Toronto"	|     82.16
"Houston"	|     38.98
"Columbus"	|     21.99
"Zurich"	|     16.99


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```SQL

--calculating order avg by city
SELECT u.city, CAST(AVG(total_ordered) AS numeric(5,0)) AS city_avg 
FROM users AS u
JOIN all_sessions AS a_s USING(user_id)
JOIN products AS p USING(product_id)
JOIN sales_report AS sr USING(product_id)
GROUP BY u.city
ORDER BY city_avg DESC

--calculating order avg by country
SELECT u.country, CAST(AVG(total_ordered) AS numeric (5,0)) AS country_avg 
FROM users AS u
JOIN all_sessions AS a_s USING(user_id)
JOIN products AS p USING(product_id)
JOIN sales_report AS sr USING(product_id)
GROUP BY u.country
ORDER BY country_avg DESC

```



Answer:

|"country"	|"country_avg"|
----------------|-------------|
"Saudi Arabia"	|96
"Kuwait"	|86
"Oman"	        |85
"Ethiopia"	|85
"Laos"	        |85
"Papua New Guinea"|	58
"Honduras"	|56
"Croatia"	|52
"United Arab Emirates"|	47
"Trinidad & Tobago"	|47
"Kazakhstan"	|43
"Greece"	|37
"Hong Kong"	|36
"South Korea"	|36
"Guatemala"	|33
"Réunion"	|32
"Czechia"	|32
"Nepal"	|31
"Mauritius"|	31
"Moldova"|	30
"Mali"	|30
"Montenegro"	|30
"Morocco"	|30
"Spain"	|29
"Italy"	|29
"Japan"	|28
"Denmark"|	28
"Germany"|	28
"Taiwan"|	28
"Ukraine"|	27
"Romania"|	27
"Brazil"|	26
"Mexico"|	26
"Thailand"|	25
"San Marino"|	25
"Israel"|	24
"Bulgaria"|	24
"Myanmar (Burma)"|	24
"Ireland"	|24
"Malaysia"	|24
"Pakistan"	|23
"Georgia"	|23
"Slovenia"	|23
"Russia"	|23
"Portugal"	|23
"Singapore"	|22
"Argentina"	|21
"United Kingdom"	|21
"Colombia"	|21
"United States"	|21
"Canada"	|20
"Venezuela"	|20
"Australia"	|19
"Netherlands"	|19
"N/A"	|18
"Vietnam"|	18
"Macedonia (FYROM)|"	18
"India"	|17
"Albania"|	17
"Armenia"|	16
"Costa Rica"|	16
"Indonesia"|	16
"Switzerland"|	16
"Côte d’Ivoire"|	16
"Kyrgyzstan"	|16
"Belgium"	|16
"France"	|15
"Tunisia"	|15
"Panama"	|15
"Philippines"	|15
"Peru"	|15
"Slovakia"|	15
"Turkey"|	14
"Sweden"|	13
"Uganda"|	13
"Poland"|	13
"Paraguay"|	13
"Bahrain"|	12
"Puerto Rico"|	12
"Hungary"|	12
"Finland"|	12
"Dominican Republic"|	12
"New Zealand"	|12
"Zimbabwe"	|12
"Lithuania"	|11
"Jersey"	|11
"Nigeria"	|11
"Serbia"	|10
"South Africa"	|10
"Chile"	|10
"Uruguay"	|9
"Egypt"	|9
"Tanzania"|	9
"Algeria"|	9
"Ghana"	|8
"El Salvador"|	7
"Norway"	|7
"Bangladesh"	|6
"Austria"	|6
"Sri Lanka"	|5
"Botswana"	|5
"Jordan"	|5
"Nicaragua"	|5
"China"	|4
"Bolivia"|	4
"Palestine"|	3
"Latvia"|	3
"Jamaica"|	3
"Cyprus"|	3
"Cambodia"|	3
"Bahamas"|	3
"Sint Maarten"|	2
"Martinique"	|2
"Brunei"	|2
"Bosnia & Herzegovina"|	2
"Belarus"	|2
"Macau"	|2
"Estonia"|	2
"Gibraltar"|	2
"Ecuador"|	2
"Kosovo"|	1
"Maldives"|	1
"Kenya"	|1
"Iraq"	|1
"Qatar"	|1
"Malta"	|0
"Iceland"|	0

|"country"|	"country_avg"|
----------|------------------|
"Saudi Arabia"|	96
"Kuwait"	|86
"Oman"	|85
"Ethiopia"|	85
"Laos"	85
"Papua New Guinea"	58
"Honduras"	56
"Croatia"	52
"United Arab Emirates"	47
"Trinidad & Tobago"	47
"Kazakhstan"	43
"Greece"	37
"Hong Kong"	36
"South Korea"	36
"Guatemala"	33
"Réunion"	32
"Czechia"	32
"Nepal"	31
"Mauritius"	31
"Moldova"	30
"Mali"	30
"Montenegro"	30
"Morocco"	30
"Spain"	29
"Italy"	29
"Japan"	28
"Denmark"	28
"Germany"	28
"Taiwan"	28
"Ukraine"	27
"Romania"	27
"Brazil"	26
"Mexico"	26
"Thailand"	25
"San Marino"	25
"Israel"	24
"Bulgaria"	24
"Myanmar (Burma)"	24
"Ireland"	24
"Malaysia"	24
"Pakistan"	23
"Georgia"	23
"Slovenia"	23
"Russia"	23
"Portugal"	23
"Singapore"	22
"Argentina"	21
"United Kingdom"	21
"Colombia"	21
"United States"	21
"Canada"	20
"Venezuela"	20
"Australia"	19
"Netherlands"	19
"N/A"	18
"Vietnam"	18
"Macedonia (FYROM)"	18
"India"	17
"Albania"	17
"Armenia"	16
"Costa Rica"	16
"Indonesia"	16
"Switzerland"	16
"Côte d’Ivoire"	16
"Kyrgyzstan"	16
"Belgium"	16
"France"	15
"Tunisia"	15
"Panama"	15
"Philippines"	15
"Peru"	15
"Slovakia"	15
"Turkey"	14
"Sweden"	13
"Uganda"	13
"Poland"	13
"Paraguay"	13
"Bahrain"	12
"Puerto Rico"	12
"Hungary"	12
"Finland"	12
"Dominican Republic"	12
"New Zealand"	12
"Zimbabwe"	12
"Lithuania"	11
"Jersey"	11
"Nigeria"	11
"Serbia"	10
"South Africa"	10
"Chile"	10
"Uruguay"	9
"Egypt"	9
"Tanzania"	9
"Algeria"	9
"Ghana"	8
"El Salvador"	7
"Norway"	7
"Bangladesh"	6
"Austria"	6
"Sri Lanka"	5
"Botswana"	5
"Jordan"	5
"Nicaragua"	5
"China"	4
"Bolivia"	4
"Palestine"	3
"Latvia"	3
"Jamaica"	3
"Cyprus"	3
"Cambodia"	3
"Bahamas"	3
"Sint Maarten"	2
"Martinique"	2
"Brunei"	2
"Bosnia & Herzegovina"	2
"Belarus"	2
"Macau"	2
"Estonia"	2
"Gibraltar"	2
"Ecuador"	2
"Kosovo"	1
"Maldives"	1
"Kenya"	1
"Iraq"	1
"Qatar"	1
"Malta"	0
"Iceland"	0





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







