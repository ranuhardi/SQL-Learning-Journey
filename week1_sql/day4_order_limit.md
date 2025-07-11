# 📝 Day 4: Sorting Results with `ORDER BY`, `LIMIT`, and `OFFSET`

> **Objective:** Learn how to sort query results and limit the number of rows returned using `ORDER BY`, `LIMIT`, and `OFFSET`.

---

## 🔍 Overview

Sorting and limiting results are crucial in SQL when dealing with large datasets. These tools allow analysts to:
- Return only relevant rows
- Paginate through large datasets (e.g., for reports or dashboards)
- View top/bottom results (e.g., top customers by sales)

---

## 🧩 Sorting with `ORDER BY`

### Example: Sort by Last Name, Then First Name
```sql
SELECT last_name, first_name
FROM actor
ORDER BY last_name, first_name;
```

You can also use column positions instead of names:
```sql
SELECT last_name, first_name
FROM actor
ORDER BY 1, 2;
```

Use `DESC` for descending order:
```sql
SELECT last_name, first_name
FROM actor
ORDER BY last_name DESC, first_name DESC;
```

---

## 📄 Limiting Results with `LIMIT`

Used to restrict the number of rows returned.

### Example: Get Top 20 Actors Alphabetically
```sql
SELECT last_name, first_name
FROM actor
ORDER BY last_name, first_name
LIMIT 20;
```

This is useful for:
- Viewing sample data
- Performance optimization
- Generating summaries

---

## 📁 Paging Through Data with `OFFSET`

Used in combination with `LIMIT` to "paginate" through data.

### Example: Get Records 21–40
```sql
SELECT last_name, first_name
FROM actor
ORDER BY last_name, first_name
LIMIT 20 OFFSET 20;
```

This is often used in:
- Web applications with pagination
- Batch processing large datasets

---

## 🧠 Summary Notes

| Clause | Purpose |
|--------|---------|
| `ORDER BY` | Sorts result set by one or more columns |
| `LIMIT` | Limits the number of rows returned |
| `OFFSET` | Skips a specified number of rows before returning results |

---
