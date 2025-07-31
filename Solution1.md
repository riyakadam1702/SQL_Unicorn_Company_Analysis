### 🧠 Question:
Identify companies whose valuation is more than **twice the average valuation** for their industry.

---

### 🛠️ Procedure:

- **CTE (`industry_avg`)**:  
  Calculates the **average valuation** of companies within each industry by joining the `industries` and `funding` tables. The results are grouped by `industry` to produce a benchmark valuation for comparison.

- **Main Query**:  
  Joins the `companies`, `industries`, and `funding` tables with the CTE using the `industry` column.

- **Filtering Condition**:  
  Returns only those companies whose valuation is **greater than 2× the industry average**.

- **Formatting & Sorting**:  
  The industry average is rounded to 2 decimal places for clarity. Results are sorted in descending order of valuation.

---

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
    ROUND(ia.avg_valuation, 2) AS industry_avg
FROM companies c 
JOIN industries i ON c.company_id = i.company_id
JOIN funding f ON c.company_id = f.company_id
JOIN industry_avg ia ON i.industry = ia.industry
WHERE CAST(f.valuation AS REAL) > 2 * ia.avg_valuation
ORDER BY f.valuation DESC;
```

---

### ✅ Sample Output

| Company    | Industry                           | Valuation        | Industry Avg     |
|------------|------------------------------------|------------------|------------------|
| Bytedance  | Artificial intelligence            | $180B            | ~$4.49B          |
| SHEIN      | E-commerce & direct-to-consumer    | $100B            | ~$3.84B          |
| SpaceX     | Other                              | $100B            | ~$4.34B          |
| Stripe     | Fintech                            | $95B             | ~$3.94B          |
| Klarna     | Fintech                            | $46B             | ~$3.94B          |

---

### 📊 Insight

These companies are **exceptional outliers** within their industries, with valuations significantly surpassing their peers. Bytedance and Stripe, for example, are valued at over **20×** the average in their sectors, indicating either massive market leadership, disruptive innovation, or unique investor confidence.

