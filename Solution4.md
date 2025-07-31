### Problem Statement:
Identify unicorn companies that are backed by **more than one top-tier investor**, such as **Tiger Global**, **Sequoia**, and **SoftBank**.

---

### Procedure:

1. **Investor Analysis**:
   - Check if each company's `select_investors` string contains any of the three investors using `POSITION(...)`.
2. **Scoring**:
   - Assign a score of 1 for each match using `CASE WHEN ... THEN 1 ELSE 0 END`.
3. **Summation**:
   - Add all three scores to get a `top_tier_count`.
4. **Filter**:
   - Only include companies where `top_tier_count > 1`.

---

### SQL Query

```sql
SELECT
  c.company,
  f.select_investors,
  (
    CASE WHEN POSITION('Tiger Global' IN f.select_investors) > 0 THEN 1 ELSE 0 END +
    CASE WHEN POSITION('Sequoia' IN f.select_investors) > 0 THEN 1 ELSE 0 END +
    CASE WHEN POSITION('SoftBank' IN f.select_investors) > 0 THEN 1 ELSE 0 END
  ) AS top_tier_count
FROM funding f
JOIN companies c ON c.company_id = f.company_id
WHERE f.select_investors IS NOT NULL
  AND (
    CASE WHEN POSITION('Tiger Global' IN f.select_investors) > 0 THEN 1 ELSE 0 END +
    CASE WHEN POSITION('Sequoia' IN f.select_investors) > 0 THEN 1 ELSE 0 END +
    CASE WHEN POSITION('SoftBank' IN f.select_investors) > 0 THEN 1 ELSE 0 END
  ) > 1;
```

---

### Output

| Company                | Select Investors                                                                                   | Top-Tier Count |
|------------------------|----------------------------------------------------------------------------------------------------|----------------|
| SHEIN                  | Tiger Global Management, Sequoia Capital China, Shunwei Capital Partners                          | 2              |
| CoinSwitch Kuber       | Tiger Global Management, Sequoia Capital India, Ribbit Capital                                     | 2              |
| Groww                  | Tiger Global Management, Sequoia Capital India, Ribbit Capital                                     | 2              |
| OYO Rooms              | SoftBank Group, Sequoia Capital India, Lightspeed India Partners                                   | 2              |
| Five Star Business Fin | Sequoia Capital India, Tiger Global Management, Tencent                                            | 2              |
| Ola Cabs               | Accel Partners, SoftBank Group, Sequoia Capital                                                    | 2              |
| Cohesity               | SoftBank Group, Sequoia Capital, Wing Venture Capital                                              | 2              |
| Thumbtack              | Tiger Global, Sequoia Capital, Google Capital                                                      | 2              |
| Razorpay               | Sequoia Capital India, Tiger Global Management, Matrix Partners India                              | 2              |
| Ola Electric Mobility  | SoftBank Group, Tiger Global Management, Matrix Partners India                                     | 2              |
| CRED                   | Tiger Global Management, DST Global, Sequoia Capital India                                         | 2              |

---

### Insight:

Among unicorn startups, **12 companies** stand out by being backed by **multiple elite venture capital firms**. These companies are not just valuable—they are **highly trusted by major global investors**, indicating strong strategic positions, promising growth potential, and investor confidence.  
This kind of analysis helps **VC firms, analysts, and stakeholders** identify firms that are likely to be future industry leaders.


