
## SQL Query Library

### How many unicorn companies are in the dataset?
```sql
SELECT COUNT(*) AS total_unicorns FROM companies;
```
### Output
| total\_unicorns |
| --------------- |
| 1074            |

### List the names of all unicorns from India.
```sql
SELECT company FROM companies WHERE country = 'India';
```
### Output top 10
| company      |
| ------------ |
| Darwinbox    |
| CoinDCX      |
| Mamaearth    |
| Hasura       |
| LEAD School  |
| Pristyn Care |
| GlobalBees   |
| apna         |
| UpGrad       |
| Cars24       |

### Which continents have unicorns?
```sql
SELECT DISTINCT continent FROM companies;
```
### Output 
| continent     |
| ------------- |
| Africa        |
| Asia          |
| South America |
| North America |
| Europe        |
| Oceania       |


###  Show the top 5 most valuable companies.
```sql
SELECT c.company, f.valuation
FROM companies c
JOIN funding f ON c.company_id = f.company_id
ORDER BY CAST(f.valuation AS REAL) DESC
LIMIT 5;
```
### Output 

| company   | valuation    |
| --------- | ------------ |
| ByteDance | 180000000000 |
| SpaceX    | 100000000000 |
| SHEIN     | 100000000000 |
| Stripe    | 95000000000  |
| Klarna    | 46000000000  |


###  Find all unicorns in the 'Fintech' industry.
```sql
SELECT c.company, i.industry
FROM companies c
JOIN industries i ON c.company_id = i.company_id
WHERE i.industry = 'Fintech';
```
### Output 

| company     | industry |
| ----------- | -------- |
| Matrixport  | Fintech  |
| candy.com   | Fintech  |
| CoinTracker | Fintech  |
| 4 Numbers   | Fintech  |
| MobileCoin  | Fintech  |
| Clara       | Fintech  |
| CoinDCX     | Fintech  |
| Masterworks | Fintech  |
| AgentSync   | Fintech  |
| Marshmallow | Fintech  |


###  What is the average valuation of all unicorns?
```sql
SELECT ROUND(AVG(CAST(valuation AS REAL)), 2) AS avg_valuation
FROM funding;
```

###  Which companies were founded before the year 2010?
```sql
SELECT c.company, d.year_founded
FROM companies c
JOIN dates d ON c.company_id = d.company_id
WHERE d.year_founded < 2010;
```

###  What is the minimum and maximum valuation among all companies and which companies have those valuations?
```sql
SELECT company, valuation
FROM companies c
JOIN funding f ON c.company_id = f.company_id
WHERE CAST(f.valuation AS REAL) = (
    SELECT MIN(CAST(valuation AS REAL)) FROM funding
)
   OR CAST(f.valuation AS REAL) = (
    SELECT MAX(CAST(valuation AS REAL)) FROM funding
);
```

###  Which unicorns were added in the year 2022?
```sql
SELECT c.company, d.date_joined
FROM companies c
JOIN dates d ON c.company_id = d.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) = 2022;
```

###  How many companies belong to each country?
```sql
SELECT country, COUNT(*) AS company_count
FROM companies
GROUP BY country
ORDER BY company_count DESC;
```

###  What is the average year of founding for unicorn companies?
```sql
SELECT ROUND(AVG(year_founded), 0) AS avg_year_founded
FROM dates
WHERE year_founded IS NOT NULL;
```

### Which 5 countries have the most unicorn companies?
```sql
SELECT country, COUNT(*) AS unicorn_count
FROM companies
GROUP BY country
ORDER BY unicorn_count DESC
LIMIT 5;
```

###  What are the top 3 industries by number of unicorns?
```sql
SELECT industry, COUNT(*) AS unicorn_count
FROM industries
GROUP BY industry
ORDER BY unicorn_count DESC
LIMIT 3;
```

###  Calculate the average valuation per industry.
```sql
SELECT i.industry, ROUND(AVG(CAST(f.valuation AS REAL)), 2) AS avg_valuation
FROM industries i
JOIN funding f ON i.company_id = f.company_id
GROUP BY i.industry
ORDER BY avg_valuation DESC;
```

###  List all companies that have a valuation above the industry average.
```sql
WITH industry_avg AS (
    SELECT industry, AVG(CAST(valuation AS REAL)) AS avg_valuation
    FROM industries i
    JOIN funding f ON i.company_id = f.company_id
    GROUP BY industry
)
SELECT c.company, i.industry, f.valuation
FROM companies c
JOIN industries i ON c.company_id = i.company_id
JOIN funding f ON c.company_id = f.company_id
JOIN industry_avg ia ON i.industry = ia.industry
WHERE CAST(f.valuation AS REAL) > ia.avg_valuation
ORDER BY i.industry, f.valuation DESC;
```

### What is the average valuation per country (limit to top 10 by count)?
```sql
SELECT c.country, AVG(ROUND(f.valuation)) as Avg_valuation
FROM companies c
JOIN funding f ON c.company_id = f.company_id
GROUP BY country 
ORDER BY Avg_valuation
DESC LIMIT 10;
```

### List companies that are younger than 5 years when they became unicorns.
```sql
SELECT company, year_founded, EXTRACT(YEAR FROM date_joined) AS year_joined
FROM companies
JOIN dates USING (company_id)
WHERE EXTRACT(YEAR FROM date_joined) - year_founded < 5
ORDER BY year_founded;
```

### Group companies by year of joining and count the unicorns per year.
```sql
SELECT EXTRACT(YEAR FROM date_joined) AS year_joined, Count(*) AS Unicorn_Count
FROM dates 
GROUP BY year_joined
ORDER BY Unicorn_Count DESC;
```

### Show companies that have more than 3 investors (based on the number of commas).
```sql
SELECT company, select_investors
FROM companies
JOIN funding USING (company_id)
WHERE select_investors IS NOT NULL
  AND LENGTH(select_investors) - LENGTH(REPLACE(select_investors, ',', '')) > 3;
```

### Find the 5 most recent unicorns to join the list.  
```sql
SELECT company, date_joined 
FROM companies 
JOIN dates USING (company_id)
ORDER BY date_joined DESC
LIMIT 5;
```

### What is the cumulative number of unicorns over time (per year)?
```sql
SELECT EXTRACT(YEAR FROM date_joined) as year_joined,
COUNT(*) as Unicorn_this_year,
SUM(COUNT(*)) OVER (ORDER BY EXTRACT(YEAR FROM date_joined))  AS Cumulative_unicorns
FROM dates 
GROUP BY EXTRACT(YEAR FROM date_joined)
ORDER BY year_joined;
```
 
### Find the average number of years it took unicorns to become unicorns after founding (exclude nulls).
```sql
SELECT 
  ROUND(AVG(EXTRACT(YEAR FROM date_joined) - year_founded),2) AS Avg_Years_to_unicorn
FROM dates 
WHERE date_joined IS NOT NULL AND year_founded IS NOT NULL;
```

