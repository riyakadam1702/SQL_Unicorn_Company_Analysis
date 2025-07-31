### 🧠 Question:
Identify companies whose valuation is more than **twice the average valuation** for their industry.

---

### 🛠️ Procedure:

- **CTE (`industry_avg`)**:  
  Calculates the **average valuation** of companies within each industry by joining the `industries` and `funding` tables. The results are grouped by `industry` to produce a benchmark valuation for comparison.

- **Main Query**:  
  Joins the `companies`, `industries`, and `funding` tables with the previously defined CTE (`industry_avg`) using the `industry` column as the key.

- **Filtering Condition**:  
  Applies a `WHERE` clause to return only those companies whose individual `valuation` is **greater than twice the average valuation** (`2 × industry_avg`) of their respective industry.

- **Formatting & Sorting**:  
  The industry average is rounded to **2 decimal places** for clarity using `ROUND()` with an explicit cast to `NUMERIC`. The final result is sorted in **descending order** based on company valuation to highlight the largest outliers.

### 💻 SQL Query
```sql
WITH industry_avg AS (
  SELECT 
     i.industry,
     AVG(CAST(f.valuation AS REAL)) AS avg_valuation
  FROM industries i 
  JOIN funding f ON i.company_id = f.company_id
  GROUP BY i.industry
)

SELECT 
    c.company,
    i.industry,
    f.valuation,
    ROUND(ia.avg_valuation::numeric, 2) AS industry_avg
FROM companies c 
JOIN industries i ON c.company_id = i.company_id
JOIN funding f ON c.company_id = f.company_id
JOIN industry_avg ia ON i.industry = ia.industry
WHERE CAST(f.valuation AS REAL) > 2 * ia.avg_valuation
ORDER BY f.valuation DESC;
