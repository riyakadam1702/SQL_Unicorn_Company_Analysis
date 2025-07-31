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

### 📊 Sample Output (Top Investors)

| Investor                    | Unicorn Count |
|----------------------------|----------------|
| Tiger Global Management    | 45             |
| Sequoia Capital            | 39             |
| Andreessen Horowitz        | 36             |
| Accel                      | 28             |
| SoftBank Group             | 25             |
| Insight Partners           | 23             |
| General Catalyst           | 22             |
| DST Global                 | 20             |
| Lightspeed Venture Partners| 19             |
| Coatue Management          | 18             |

---

### 💡 Insight

These investors have backed the **largest number of unicorn startups**, with Tiger Global, Sequoia, and Andreessen Horowitz leading the way. This indicates their strategic influence and investment strength across high-growth tech and innovation sectors.

