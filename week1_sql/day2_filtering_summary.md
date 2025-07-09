## 🗓️ Day 2: Filtering Data with WHERE, IN, BETWEEN, and LIKE

### 🎯 Goals
- Learn how to filter query results using:
  - `WHERE`
  - `IN`
  - `BETWEEN`
  - `LIKE`
- Practice writing queries that extract specific subsets of data

---

### 🔍 What I Learned
Today I:
- Watched videos on filtering data in SQL
- Practiced using different operators to narrow down result sets
- Wrote several queries to filter rows based on conditions

---

### 💻 Sample SQL Queries from Today

#### Example 1: Filtering with `WHERE`
```sql
SELECT product_name, price
FROM products
WHERE price > 100;
```

Example 2: Using IN to Match Multiple Values

```sql
SELECT customer_id, name, country
FROM customers
WHERE country IN ('USA', 'Canada', 'UK');
```

Example 3: Using BETWEEN for Range Filtering

```sql
SELECT order_id, total_amount
FROM orders
WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';
```

Example 4: Pattern Matching with LIKE

```sql
SELECT employee_id, first_name
FROM employees
WHERE first_name LIKE 'A%';
```

📝 Notes

- `WHERE` is used to specify conditions for selecting rows
- `IN` allows matching against a list of values
- `BETWEEN` selects values within a range (inclusive)
- `LIKE` helps with pattern matching using wildcards (`%` for any number of characters, `_` for one character)

🚀 Next Steps

- Learn how to sort and limit query results using ORDER BY and LIMIT
- Understand how to remove duplicates using DISTINCT
- Keep practicing real-world filtering scenarios
