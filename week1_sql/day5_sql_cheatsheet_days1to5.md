# 📄 SQL Cheat Sheet – Days 1 to 5  
*For Data Analysts | Based on edX DavidsonX Course*

---

## 🔹 Day 1: Introduction & Basic Queries

### 🧩 Syntax Template
```sql
SELECT column1, column2
FROM table_name;
```

### 📝 Example:
```sql
SELECT first_name, last_name
FROM actor;
```

### ✅ Tips:
- Use `*` to select all columns.
- Keep queries readable with line breaks and indentation.

---

## 🔹 Day 2: Filtering with `WHERE`, `IN`, `BETWEEN`, `LIKE`

### 🧩 Syntax
```sql
SELECT columns
FROM table
WHERE conditions;
```

### ✅ Operators:

| Operator      | Use Case |
|---------------|----------|
| `=`           | Exact match |
| `<>` / `!=`   | Not equal |
| `<`, `>`, `<=`, `>=` | Comparison |
| `AND`         | Combine multiple conditions |
| `OR`          | Match one of multiple conditions |
| `NOT`         | Reverse logic |
| `IN`          | Match values in a list |
| `BETWEEN`     | Range (inclusive) |
| `LIKE`        | Pattern matching (`%` = wildcard) |

### 📝 Examples:
```sql
-- Multiple conditions
SELECT * FROM actor
WHERE first_name = 'Tom' OR (first_name = 'Ben' AND last_name = 'Willis');

-- IN Clause
SELECT * FROM actor
WHERE first_name NOT IN ('Tom', 'Ben', 'Jim', 'Angela');

-- LIKE Clause
SELECT * FROM actor
WHERE last_name LIKE 'H%s%';

-- BETWEEN
SELECT * FROM actor
WHERE last_name BETWEEN 'A' AND 'Bo';
```

---

## 🔹 Day 3: Sorting Results with `ORDER BY`, `LIMIT`, and `OFFSET`

### 🧩 Syntax
```sql
SELECT columns
FROM table
ORDER BY column(s)
LIMIT number
OFFSET number;
```

### ✅ Notes:
- Use `ASC` (default) or `DESC` for sort direction.
- `LIMIT` restricts the number of rows returned.
- `OFFSET` skips a number of rows — useful for pagination.

### 📝 Examples:
```sql
-- Sort by name
SELECT first_name, last_name
FROM actor
ORDER BY last_name, first_name;

-- Top 20 results
SELECT first_name, last_name
FROM actor
ORDER BY last_name
LIMIT 20;

-- Page 2 of 20 records
SELECT first_name, last_name
FROM actor
ORDER BY last_name
LIMIT 20 OFFSET 20;
```

---

## 🔹 Day 4: Removing Duplicates with `DISTINCT`

### 🧩 Syntax
```sql
SELECT DISTINCT column(s)
FROM table;
```

### ✅ Notes:
- Applies to entire row, not just single column.
- Avoid overusing — impacts performance on large datasets.

### 📝 Examples:
```sql
-- Get unique names
SELECT DISTINCT first_name, last_name
FROM actor
ORDER BY last_name;

-- Count distinct names
SELECT COUNT(DISTINCT last_name)
FROM actor;
```

---

## 🔹 Day 5: Grouping Data with `GROUP BY`

### 🧩 Syntax
```sql
SELECT column, aggregate_function(column)
FROM table
WHERE condition
GROUP BY column
ORDER BY column;
```

### ✅ Notes:
- Every non-aggregated column in `SELECT` must be in `GROUP BY`.
- Use with `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.

### 📝 Examples:
```sql
-- Count actors per last name
SELECT last_name, COUNT(*) AS total
FROM actor
WHERE last_name > 'A'
GROUP BY last_name
ORDER BY total DESC;

-- Group by multiple columns
SELECT last_name, first_name, COUNT(*)
FROM actor
WHERE last_name > 'A'
GROUP BY last_name, first_name
ORDER BY 3 DESC;
```

---

## 🧠 Summary Table: Core SQL Clauses

| Clause       | Purpose |
|--------------|---------|
| `SELECT`     | Choose columns |
| `FROM`       | Specify source table |
| `WHERE`      | Filter rows |
| `ORDER BY`   | Sort results |
| `LIMIT`      | Limit number of rows |
| `OFFSET`     | Skip rows before returning results |
| `DISTINCT`   | Remove duplicate rows |
| `GROUP BY`   | Group data for aggregation |

---
