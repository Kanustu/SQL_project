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
```SQL

--calculating order avg by city
SELECT u.city, CAST(AVG(total_ordered) AS numeric(5,0)) AS city_avg 
    FROM users AS u
    JOIN all_sessions AS a_s USING(user_id)
    JOIN products AS p USING(product_id)
    JOIN sales_report AS sr USING(product_id)
GROUP BY u.city
ORDER BY city_avg DESC

```
```SQL
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

| **country**          | **country_avg** |
|----------------------|-----------------|
| Saudi Arabia         | 96              |
| Kuwait               | 86              |
| Oman                 | 85              |
| Ethiopia             | 85              |
| Laos                 | 85              |
| Papua New Guinea     | 58              |
| Honduras             | 56              |
| Croatia              | 52              |
| United Arab Emirates | 47              |
| Trinidad & Tobago    | 47              |
| Kazakhstan           | 43              |
| Greece               | 37              |
| Hong Kong            | 36              |
| South Korea          | 36              |
| Guatemala            | 33              |
| Réunion              | 32              |
| Czechia              | 32              |
| Nepal                | 31              |
| Mauritius            | 31              |
| Moldova              | 30              |
| Mali                 | 30              |
| Montenegro           | 30              |
| Morocco              | 30              |
| Spain                | 29              |
| Italy                | 29              |
| Japan                | 28              |
| Denmark              | 28              |
| Germany              | 28              |
| Taiwan               | 28              |
| Ukraine              | 27              |
| Romania              | 27              |
| Brazil               | 26              |
| Mexico               | 26              |
| Thailand             | 25              |
| San Marino           | 25              |
| Israel               | 24              |
| Bulgaria             | 24              |
| Myanmar (Burma)      | 24              |
| Ireland              | 24              |
| Malaysia             | 24              |
| Pakistan             | 23              |
| Georgia              | 23              |
| Slovenia             | 23              |
| Russia               | 23              |
| Portugal             | 23              |
| Singapore            | 22              |
| Argentina            | 21              |
| United Kingdom       | 21              |
| Colombia             | 21              |
| United States        | 21              |
| Canada               | 20              |
| Venezuela            | 20              |
| Australia            | 19              |
| Netherlands          | 19              |
| N/A                  | 18              |
| Vietnam              | 18              |
| Macedonia (FYROM)    | 18              |
| India                | 17              |
| Albania              | 17              |
| Armenia              | 16              |
| Costa Rica           | 16              |
| Indonesia            | 16              |
| Switzerland          | 16              |
| Côte d’Ivoire        | 16              |
| Kyrgyzstan           | 16              |
| Belgium              | 16              |
| France               | 15              |
| Tunisia              | 15              |
| Panama               | 15              |
| Philippines          | 15              |
| Peru                 | 15              |
| Slovakia             | 15              |
| Turkey               | 14              |
| Sweden               | 13              |
| Uganda               | 13              |
| Poland               | 13              |
| Paraguay             | 13              |
| Bahrain              | 12              |
| Puerto Rico          | 12              |
| Hungary              | 12              |
| Finland              | 12              |
| Dominican Republic   | 12              |
| New Zealand          | 12              |
| Zimbabwe             | 12              |
| Lithuania            | 11              |
| Jersey               | 11              |
| Nigeria              | 11              |
| Serbia               | 10              |
| South Africa         | 10              |
| Chile                | 10              |
| Uruguay              | 9               |
| Egypt                | 9               |
| Tanzania             | 9               |
| Algeria              | 9               |
| Ghana                | 8               |
| El Salvador          | 7               |
| Norway               | 7               |
| Bangladesh           | 6               |
| Austria              | 6               |
| Sri Lanka            | 5               |
| Botswana             | 5               |
| Jordan               | 5               |
| Nicaragua            | 5               |
| China                | 4               |
| Bolivia              | 4               |
| Palestine            | 3               |
| Latvia               | 3               |
| Jamaica              | 3               |
| Cyprus               | 3               |
| Cambodia             | 3               |
| Bahamas              | 3               |
| Sint Maarten         | 2               |
| Martinique           | 2               |
| Brunei               | 2               |
| Bosnia & Herzegovina | 2               |
| Belarus              | 2               |
| Macau                | 2               |
| Estonia              | 2               |
| Gibraltar            | 2               |
| Ecuador              | 2               |
| Kosovo               | 1               |
| Maldives             | 1               |
| Kenya                | 1               |
| Iraq                 | 1               |
| Qatar                | 1               |
| Malta                | 0               |
| Iceland              | 0               |


| **city**            | **city_avg** |
|---------------------|--------------|
| Riyadh              | 319          |
| Brno                | 319          |
| Rexburg             | 251          |
| Lisbon              | 189          |
| Sacramento          | 189          |
| Kalamazoo           | 105          |
| Saint Petersburg    | 101          |
| Avon                | 100          |
| Rome                | 98           |
| Sherbrooke          | 97           |
| Longtan District    | 97           |
| Nashville           | 94           |
| Santa Monica        | 90           |
| Dubai               | 89           |
| Shinjuku            | 73           |
| Pune                | 71           |
| Westville           | 70           |
| Rio de Janeiro      | 65           |
| Vladivostok         | 62           |
| Bellingham          | 62           |
| Santa Fe            | 60           |
| San Antonio         | 58           |
| Seoul               | 55           |
| Menlo Park          | 52           |
| Athens              | 51           |
| Palo Alto           | 48           |
| Zhongli District    | 46           |
| Salem               | 45           |
| Indore              | 44           |
| Belo Horizonte      | 43           |
| Boston              | 40           |
| Bangkok             | 38           |
| Hanoi               | 38           |
| Hong Kong           | 36           |
| Santa Clara         | 35           |
| Hamburg             | 34           |
| Kitchener           | 34           |
| Pozuelo de Alarcon  | 34           |
| Munich              | 33           |
| San Bruno           | 32           |
| Phoenix             | 32           |
| Dublin              | 31           |
| Bellflower          | 30           |
| Cork                | 30           |
| Toronto             | 26           |
| Tel Aviv-Yafo       | 26           |
| Seattle             | 26           |
| Mumbai              | 26           |
| Milan               | 26           |
| Los Angeles         | 25           |
| Mississauga         | 25           |
| Irvine              | 24           |
| Mountain View       | 23           |
| Montreuil           | 23           |
| Madrid              | 23           |
| Pittsburgh          | 22           |
| Buenos Aires        | 22           |
| Charlotte           | 22           |
| Chicago             | 22           |
| San Francisco       | 22           |
| Melbourne           | 22           |
| Ann Arbor           | 22           |
| Amsterdam           | 22           |
| Santiago            | 21           |
| Vancouver           | 21           |
| Austin              | 21           |
| N/A                 | 20           |
| Calgary             | 20           |
| Sydney              | 20           |
| Bogota              | 20           |
| Philadelphia        | 20           |
| Kolkata             | 19           |
| San Jose            | 19           |
| Sao Paulo           | 19           |
| Oakland             | 19           |
| Berlin              | 18           |
| Ahmedabad           | 18           |
| Sunnyvale           | 18           |
| Ho Chi Minh City    | 18           |
| London              | 18           |
| New York            | 18           |
| Cupertino           | 18           |
| Redwood City        | 18           |
| Hyderabad           | 17           |
| Barcelona           | 17           |
| Chennai             | 17           |
| Copenhagen          | 16           |
| Warsaw              | 16           |
| Ghent               | 16           |
| Houston             | 16           |
| Kuala Lumpur        | 16           |
| Redmond             | 15           |
| Bucharest           | 15           |
| Prague              | 15           |
| Perth               | 14           |
| Budapest            | 14           |
| Quebec City         | 14           |
| Singapore           | 14           |
| Paris               | 13           |
| Akron               | 13           |
| Montreal            | 13           |
| La Victoria         | 13           |
| LaFayette           | 13           |
| Lake Oswego         | 13           |
| Chandigarh          | 13           |
| Cambridge           | 13           |
| The Dalles          | 13           |
| Asuncion            | 13           |
| Wrexham             | 13           |
| Mexico City         | 12           |
| Washington          | 12           |
| Dallas              | 12           |
| Minato              | 12           |
| Osaka               | 12           |
| Detroit             | 12           |
| Gurgaon             | 12           |
| Zurich              | 11           |
| Ipoh                | 11           |
| Istanbul            | 11           |
| Jaipur              | 11           |
| Kiev                | 11           |
| Manila              | 11           |
| New Delhi           | 11           |
| Bengaluru           | 10           |
| San Diego           | 10           |
| Jakarta             | 10           |
| Stockholm           | 10           |
| Burnaby             | 10           |
| Taguig              | 10           |
| Atlanta             | 9            |
| South San Francisco | 9            |
| Kirkland            | 9            |
| Courbevoie          | 8            |
| Moscow              | 8            |
| Montevideo          | 8            |
| Thessaloniki        | 7            |
| East Lansing        | 7            |
| Oviedo              | 7            |
| Fremont             | 7            |
| Frankfurt           | 7            |
| Medellin            | 7            |
| St. Louis           | 7            |
| Helsinki            | 6            |
| San Mateo           | 6            |
| Bandung             | 6            |
| Nanded              | 6            |
| Tempe               | 6            |
| Kharagpur           | 6            |
| Yokohama            | 6            |
| Orlando             | 5            |
| Quezon City         | 5            |
| Denver              | 5            |
| Columbia            | 5            |
| Vienna              | 5            |
| Timisoara           | 5            |
| Jacksonville        | 5            |
| Boulder             | 5            |
| University Park     | 5            |
| Richardson          | 4            |
| Culiacan            | 4            |
| Kharkiv             | 4            |
| Brussels            | 4            |
| Eau Claire          | 4            |
| Doha                | 4            |
| Ashburn             | 4            |
| Marseille           | 4            |
| Portland            | 4            |
| Poznan              | 4            |
| Izmir               | 4            |
| Cluj-Napoca         | 4            |
| Colombo             | 4            |
| Milpitas            | 3            |
| Madison             | 3            |
| Adelaide            | 3            |
| Norfolk             | 3            |
| Petaling Jaya       | 3            |
| Jersey City         | 3            |
| Druid Hills         | 3            |
| Stanford            | 3            |
| Wellesley           | 3            |
| Zagreb              | 3            |
| Las Vegas           | 2            |
| Chico               | 2            |
| Iasi                | 2            |
| Brisbane            | 2            |
| Antwerp             | 2            |
| Council Bluffs      | 2            |
| Maracaibo           | 2            |
| Salford             | 2            |
| Greer               | 1            |
| Lahore              | 1            |
| Piscataway Township | 1            |
| Rosario             | 1            |
| Kansas City         | 1            |
| Oslo                | 1            |
| Villeneuve-d'Ascq   | 1            |
| Vilnius             | 1            |
| Patna               | 1            |
| Edmonton            | 1            |
| Nagoya              | 0            |
| Lucknow             | 0            |
| Beijing             | 0            |
| Makati              | 0            |
| Auckland            | 0            |
| Waterloo            | 0            |
| Westlake Village    | 0            |
| Manchester          | 0            |
| Marlboro            | 0            |
| Fortaleza           | 0            |
| Forest Park         | 0            |
| San Salvador        | 0            |
| Goose Creek         | 0            |
| Coventry            | 0            |
| Hayward             | 0            |
| Indianapolis        | 0            |
| Shibuya             | 0            |
| Chuo                | 0            |
| South El Monte      | 0            |
| Pleasanton          | 0            |
| St. John's          | 0            |
| Minneapolis         | 0            |
| Karachi             | 0            |
| Panama City         | 0            |
| Tampa               | 0            |
| Bratislava          | 0            |
| London(U.S.)        | 0            |
| Nairobi             | 0            |




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL

--products sold per category in each city
SELECT u.city, a.v2_product_category
FROM all_sessions AS a
    JOIN users as u USING(user_id)
    JOIN analytics AS an USING(user_id)
--filter out category and city that are not valid
WHERE v2_product_category NOT IN('(not set)', '${escCatTitle}') AND a.city != 'N/A'
GROUP BY u.city, a.v2_product_category
    HAVING COUNT(units_sold) > 0


-- seems that a majority of the orders are coming from California
-- at least from the citites we have values for

```

1. Mountainview has a large variety of categories
2. seems that a majority of the orders are coming from California, at least from the cities we have values for

```SQL

--products sold per category in each country
SELECT u.country, a.v2_product_category
FROM all_sessions AS a
    JOIN users as u USING(user_id)
    JOIN analytics AS an USING(user_id)
--filter out category and city that are not valid
WHERE v2_product_category NOT IN('(not set)', '${escCatTitle}') AND u.country != 'N/A'
--AND v2_product_category LIKE '%Women%'
GROUP BY u.country, a.v2_product_category
    HAVING COUNT(units_sold) > 0

-- United States has the most amount/variety of categories
-- Men's clothing makes up 18% of the categories accross all countries
-- Women's clothing makes up 4.5% of the categories accross all countries



```
1. United States has the most amount/variety of categories
2. Men's clothing makes up 18% of the categories accross all countries
3. Women's clothing makes up 4.5% of the categories accross all countries


Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL

--highest selling product per city
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







