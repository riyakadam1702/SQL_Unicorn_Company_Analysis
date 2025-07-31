### 🧠 Question:
Which investor appears in the **largest number of unicorns**?

---

### 🛠️ Procedure:

- **CTE (`exploded_investors`)**:  
  The `select_investors` column contains comma-separated values. This Common Table Expression (CTE) uses `STRING_TO_ARRAY()` combined with `UNNEST()` to split the investor strings into individual rows, creating one row per investor per company.

- **Normalization**:  
  The `TRIM()` function ensures any leading/trailing whitespace is removed from investor names.

- **Aggregation**:  
  The main query groups by investor name and counts the number of times each investor appears (i.e., how many different unicorns they invested in).

- **Sorting and Limiting**:  
  The results are ordered by count in descending order and limited to the **top 10 investors**.

---

### 💻 SQL Query

```sql
WITH exploded_investors AS (
    SELECT
        TRIM(investor) AS investor
    FROM funding,
         UNNEST(STRING_TO_ARRAY(select_investors, ', ')) AS investor
    WHERE select_investors IS NOT NULL
)

SELECT investor, COUNT(*) AS unicorn_count
FROM exploded_investors
GROUP BY investor
ORDER BY unicorn_count DESC
LIMIT 10;
```

---

### 📊 Sample Output (Top 10 Investors)

| Investor                  | Unicorn Count |
|---------------------------|----------------|
| Sequoia Capital           | 45             |
| Andreessen Horowitz       | 38             |
| Tiger Global Management   | 36             |
| SoftBank Vision Fund      | 30             |
| Accel                     | 28             |
| Google Ventures           | 26             |
| DST Global                | 25             |
| Lightspeed Venture Partners | 24          |
| Coatue Management         | 22             |
| Bessemer Venture Partners | 21             |

---

### 💡 Insight

This analysis reveals the **most influential investors** in the unicorn ecosystem. VCs like *Sequoia Capital*, *Andreessen Horowitz*, and *Tiger Global* have backed a significant share of unicorns, showcasing their early and aggressive investment strategies in high-growth startups.
