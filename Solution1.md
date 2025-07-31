### Problem Statement:
Identify companies whose valuation is more than **twice the average valuation** for their industry.

---

### Procedure:

- **CTE (`industry_avg`)**:  
  Calculates the **average valuation** of companies within each industry by joining the `industries` and `funding` tables. The results are grouped by `industry` to produce a benchmark valuation for comparison.

- **Main Query**:  
  Joins the `companies`, `industries`, and `funding` tables with the CTE using the `industry` column.

- **Filtering Condition**:  
  Returns only those companies whose valuation is **greater than 2× the industry average**.

- **Formatting & Sorting**:  
  The industry average is rounded to 2 decimal places for clarity. Results are sorted in descending order of valuation.

---

### SQL Query
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
    ROUND(ia.avg_valuation, 2) AS industry_avg
FROM companies c 
JOIN industries i ON c.company_id = i.company_id
JOIN funding f ON c.company_id = f.company_id
JOIN industry_avg ia ON i.industry = ia.industry
WHERE CAST(f.valuation AS REAL) > 2 * ia.avg_valuation
ORDER BY f.valuation DESC;
```

---

### Top 5 Companies with Valuation > 2× Industry Average

| Company    | Industry                          | Valuation ($)      | Industry Avg ($)       |
|------------|-----------------------------------|---------------------|-------------------------|
| Bytedance  | Artificial intelligence           | 180,000,000,000     | 4,488,095,207.62        |
| SpaceX     | Other                             | 100,000,000,000     | 4,344,827,550.90        |
| SHEIN      | E-commerce & direct-to-consumer   | 100,000,000,000     | 3,837,878,719.39        |
| Stripe     | Fintech                           | 95,000,000,000      | 3,937,500,009.14        |
| Klarna     | Fintech                           | 46,000,000,000      | 3,937,500,009.14        |

---

### Insight

These companies are **exceptional outliers** within their industries, with valuations significantly surpassing their peers. Bytedance and SpaceX, for example, are valued at over **20×** the average in their sectors, indicating either massive market leadership, disruptive innovation, or unique investor confidence.

