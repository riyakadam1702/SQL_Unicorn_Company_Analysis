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


