# 📝 Day 3: Filtering with `WHERE`, `IN`, `BETWEEN`, and `LIKE`

> **Objective:** Understand how to filter data using comparison operators, logical operators, and pattern matching with `LIKE`.

---

## 🔍 Overview

Filtering allows us to extract only the relevant rows from our dataset. This is done using the `WHERE` clause along with comparison and logical operators such as:

- Comparison Operators: `=`, `<>`, `!=`, `<`, `>`, `<=`, `>=`
- Logical Operators: `AND`, `OR`, `NOT`, `IN`, `BETWEEN`, `LIKE`

---

## 🧩 Basic Filtering with WHERE

### Example: Filtering by Exact Match
```sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE first_name = 'Tom';
```

### Example: Combining Conditions
```sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE first_name = 'Tom'
   OR (first_name = 'Ben' AND last_name = 'Willis');
```

> ✅ **Explanation**: This query retrieves actors named "Tom" or "Ben Willis".

---

## 📌 Logical Operators

### `IN`: Filter Based on Multiple Values
```sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE first_name NOT IN ('Tom', 'Ben', 'Jim', 'Angela')
ORDER BY first_name, last_name;
```

> ✅ **Explanation**: Returns all actors whose first names are not Tom, Ben, Jim, or Angela.

---

### `LIKE`: Pattern Matching

The `%` symbol is used as a wildcard to match any number of characters.

#### Example: Names Starting With "H" and Ending With "s"
```sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name LIKE 'H%' AND last_name LIKE '%s'
ORDER BY first_name, last_name;
```

> ✅ **Explanation**: Retrieves actors whose last names start with H and end with s.

#### Alternative: Using a Single `LIKE` Expression
```sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name LIKE 'H%s%'
ORDER BY first_name, last_name;
```

> 💡 **Tip**: Use `NOT LIKE` to exclude patterns.

---

## 🔁 Comparison Operators

Use these to compare values directly.

### Examples:
```sql
-- Get actors with last name alphabetically after 'W'
SELECT first_name, last_name
FROM actor
WHERE last_name > 'W'
ORDER BY last_name, first_name;

-- Get actors with last name starting between A and Bo
SELECT first_name, last_name
FROM actor
WHERE last_name BETWEEN 'A' AND 'Bo'
ORDER BY last_name;
```

| Operator | Description         |
|----------|---------------------|
| `>`      | Greater than        |
| `<`      | Less than           |
| `>=`     | Greater than or equal |
| `<=`     | Less than or equal  |
| `<>` / `!=` | Not equal       |

> ✅ `BETWEEN` is inclusive — meaning it includes both endpoints.

---

## 🧠 Summary Notes

| Concept | Use Case |
|--------|----------|
| `WHERE` | Filters rows based on conditions |
| `IN` | Matches any value in a list |
| `LIKE` | Performs pattern matching with wildcards (`%`) |
| `NOT` | Reverses logic (e.g., `NOT IN`, `NOT LIKE`) |
| `BETWEEN` | Selects values within a range (inclusive) |
| Comparison Operators | Used to compare values (`>`, `<`, `=`, etc.) |

---
