# 📝 Day 5: Removing Duplicates with `DISTINCT` & Aggregating with `GROUP BY`

> **Objective:** Understand how to remove duplicate rows using `DISTINCT` and group data for aggregation using `GROUP BY`.

---

## 🔍 Overview

When working with real-world data, duplicates are common. Whether it's due to multiple entries or joins between tables, being able to eliminate redundancy is key.

There are two main ways to handle this:
- Use `DISTINCT` to remove duplicate rows
- Use `GROUP BY` to summarize and aggregate data

---

## 🧩 Using `DISTINCT` to Remove Duplicates

### Example: Get Unique Actor Names
```sql
SELECT DISTINCT last_name, first_name
FROM actor
ORDER BY last_name, first_name;
```

> ✅ `DISTINCT` applies to the entire row, not just one column.

### Common Mistake:
Using `DISTINCT` unnecessarily can impact performance on large datasets. Only apply it when needed.

---

## 📊 Grouping Data with `GROUP BY`

Use `GROUP BY` when you want to **aggregate** data across categories.

### Example: Count How Many Actors Share Each Last Name
```sql
SELECT last_name, COUNT(*) AS total
FROM actor
WHERE last_name > 'A'
GROUP BY last_name
ORDER BY total DESC;
```

### Example: Group by Multiple Columns
```sql
SELECT last_name, first_name, COUNT(*)
FROM actor
WHERE last_name > 'A'
GROUP BY last_name, first_name
ORDER BY 3 DESC;
```

> ✅ When using `GROUP BY`, every non-aggregated column in the `SELECT` clause must appear in the `GROUP BY` clause.

---

## ⚖️ `DISTINCT` vs `GROUP BY`

| Feature        | `DISTINCT`                     | `GROUP BY`                            |
|----------------|-------------------------------|----------------------------------------|
| Removes Duplicates | ✅ Yes                        | ✅ Yes (implicitly)                    |
| Used with Aggregation | ❌ No                      | ✅ Yes                                |
| Performance     | Often faster for simple deduplication | Better for summarizing grouped data |

---

## 🧠 Summary Notes

| Concept       | Use Case |
|---------------|----------|
| `DISTINCT`    | Eliminate duplicate rows from result sets |
| `GROUP BY`    | Group rows to perform aggregations like `COUNT`, `SUM`, `AVG` |
| `ORDER BY`    | Sort final output |
| `LIMIT` / `OFFSET` | Control how many rows to return and where to start |

---
