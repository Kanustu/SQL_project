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

UPDATE all_sessions
SET city = 'N/A'
WHERE city = '(not set)'

UPDATE all_sessions
SET city = 'N/A'
WHERE city = 'not available in the demo dataset'

```

