### 🧠 Problem Statement:
Which company has the **largest difference** between its current valuation and the **average valuation** of its industry?

---

### 🛠️ Procedure:

- **CTE 1 (`industry_avg`)**:  
  Calculates the average valuation for each industry by joining the `industries` and `funding` tables and grouping by industry.  
  This forms a benchmark to compare individual company valuations against.

- **CTE 2 (`company_with_diff`)**:  
  Joins `companies`, `industries`, and `funding` with `industry_avg`.  
  Computes:
  - Each company’s actual valuation
  - Its industry average
  - The absolute difference (`valuation_diff`) between the two

- **Main Query**:  
  Retrieves all records from `company_with_diff` and orders them in descending order of `valuation_diff` to highlight the most extreme deviation.

---

### 💻 SQL Query

```sql
-- Which company has the largest difference between its valuation and the industry average?
WITH industry_avg AS (
    SELECT 
        i.industry, 
        AVG(CAST(f.valuation AS NUMERIC)) AS avg_valuation
    FROM industries i
    JOIN funding f ON i.company_id = f.company_id
    GROUP BY i.industry
),
company_with_diff AS (
    SELECT 
        c.company, 
        i.industry, 
        CAST(f.valuation AS NUMERIC) AS valuation, 
        ia.avg_valuation,
        ABS(CAST(f.valuation AS NUMERIC) - ia.avg_valuation) AS valuation_diff
    FROM companies c
    JOIN industries i ON c.company_id = i.company_id
    JOIN funding f ON c.company_id = f.company_id
    JOIN industry_avg ia ON i.industry = ia.industry
)
SELECT *
FROM company_with_diff
ORDER BY valuation_diff DESC;
```

---

### 📊 Output (Top Entry)

| Company    | Industry                | Valuation         | Industry Avg       | Valuation Diff     |
|------------|--------------------------|--------------------|---------------------|---------------------|
| Bytedance  | Artificial intelligence | 180,000,000,000    | 4,488,095,208       | 175,511,904,792     |

---

### 💡 Insight

This query highlights companies like **Bytedance** whose market valuation is an extreme outlier relative to peers in their industry. Such companies may be **industry disruptors**, benefit from unique technology, massive user bases, or exceptional growth models.

