# 📝 Day 5: Combining Results – `UNION`, `UNION ALL`, `INTERSECT`, `EXCEPT`

> **Objective:** Learn how to combine results from multiple queries using `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT` — essential tools for reporting and data analysis.

---

## 🧠 What Are Set Operations?

Set operations let you **combine the results of two or more queries** into a single result set.

They’re like **Venn diagrams** for data:
- `UNION` → All data from both circles
- `INTERSECT` → Only what’s in the middle (common data)
- `EXCEPT` → What’s in one circle but not the other

---

## 🧩 1. `UNION` vs `UNION ALL` – Combine Lists

### Task: Show all people named **Susan** — whether they are customers or actresses

```sql
SELECT customer.last_name, customer.first_name
FROM customer
WHERE customer.first_name = 'Susan'

UNION ALL

SELECT actor.last_name, actor.first_name
FROM actor
WHERE actor.first_name = 'Susan';
```

### 🔍 What’s the Difference?

| Operation | Behavior |
|---------|----------|
| `UNION` | Combines results and **removes duplicates** |
| `UNION ALL` | Combines results and **keeps duplicates** |

> ✅ Use `UNION ALL` when you want speed and don’t care about duplicates  
> ✅ Use `UNION` when you want clean, unique results

---

### Example Output:

| last_name | first_name |
|-----------|------------|
| Wilson    | Susan      |
| Davis     | Susan      |
| Monroe    | Susan      |
| Wilson    | Susan      | ← duplicate (only removed if using `UNION`)

---

## 🧩 2. Rules for Using `UNION` / `UNION ALL`

To use `UNION`, your queries must follow these rules:

| Rule | Explanation |
|------|-------------|
| ✅ Same number of columns | Both `SELECT` statements must return the same number of columns |
| ✅ Similar data types | Columns should be compatible (e.g., text with text, date with date) |
| ✅ Column names come from the first query | The final result uses the column names from the **first** `SELECT` |
| ✅ `ORDER BY` applies to the final result | Only one `ORDER BY` at the end |

---

### Example: Adding a "Type" Label

```sql
SELECT 'Customer' AS Type, last_name, first_name
FROM customer
WHERE first_name = 'Susan'

UNION

SELECT 'Actor', last_name, first_name
FROM actor
WHERE first_name = 'Susan'

ORDER BY last_name DESC;
```

> ✅ This helps distinguish where each person comes from  
> ✅ `ORDER BY` works on the final combined list

---

## 🧠 Logical Processing Order Reminder

Even though we write `SELECT` first, SQL processes clauses in this order:

```
F → FROM
W → WHERE
G → GROUP BY
H → HAVING
S → SELECT
D → (UNION / INTERSECT / EXCEPT)
O → ORDER BY
L → LIMIT
```

> 🔑 `UNION` happens **before** `ORDER BY` — so sorting applies to the **final combined result**

---

## 🧩 3. `INTERSECT` – Show Only What’s in Both Queries

### What It Does:
Returns only rows that appear in **both** result sets.

### Task: Find films that were acted in by **both**:
- **Penelope Monroe**
- **Christian Bale**

```sql
WITH CTE AS (
    SELECT a.first_name, a.last_name, f.title
    FROM actor a
        INNER JOIN film_actor fa ON fa.actor_id = a.actor_id
        INNER JOIN film f ON f.film_id = fa.film_id
)
SELECT title
FROM CTE
WHERE last_name = 'Bale' AND first_name = 'Christian'

INTERSECT

SELECT title
FROM CTE
WHERE last_name = 'Monroe' AND first_name = 'Penelope';
```

> ✅ Returns only films that **both actors appeared in**

---

### Example Output:

| title |
|-------|
| Running Duck |
| Harpoon Hurricane |
| Dogma Lost |

---

## 🧩 4. `EXCEPT` – Show What’s in One List But Not the Other

### What It Does:
Returns rows from the **first query** that are **not in the second query**.

### Example: Films Christian Bale acted in, but Penelope Monroe did not

```sql
SELECT title
FROM CTE
WHERE last_name = 'Bale' AND first_name = 'Christian'

EXCEPT

SELECT title
FROM CTE
WHERE last_name = 'Monroe' AND first_name = 'Penelope';
```

> ✅ Shows films where Bale appears, but Monroe does **not**

---

## 📌 Summary Table: Set Operations

| Operation | What It Does | Use Case |
|----------|---------------|----------|
| `UNION` | Combine two queries, remove duplicates | List all Susans (customers + actors) |
| `UNION ALL` | Combine two queries, keep duplicates | Fast reporting when duplicates are acceptable |
| `INTERSECT` | Return only rows in **both** queries | Find common films between two actors |
| `EXCEPT` | Return rows in first query but **not** in second | Find films one actor did but another didn’t |

---

## 🧠 Pro Tips

| Tip | Why It Matters |
|-----|----------------|
| Always match column count and types | Prevents errors |
| Use `UNION ALL` for better performance | No duplicate-checking overhead |
| Use `ORDER BY` only once — at the end | Applies to the full combined result |
| Use CTEs (`WITH`) for clarity | Makes complex queries easier to read |
| Alias columns in the **first** `SELECT` | Ensures proper naming in final output |

---
