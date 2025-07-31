
## SQL Query Library

###  Show the top 5 most valuable companies.
```sql
SELECT c.company, f.valuation
FROM companies c
JOIN funding f ON c.company_id = f.company_id
ORDER BY CAST(f.valuation AS REAL) DESC
LIMIT 5;
```

###  Find all unicorns in the 'Fintech' industry.
```sql
SELECT c.company, i.industry
FROM companies c
JOIN industries i ON c.company_id = i.company_id
WHERE i.industry = 'Fintech';
```

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

