# 📝 Day 2: Advanced JOINs and Table Expressions

> **Objective:** Learn how to use **Common Table Expressions (CTEs)**, **CROSS JOINs**, **ANTI JOINs**, and **FULL OUTER JOINs** to analyze and combine data from multiple tables.

---

## 🧩 1. Common Table Expressions (CTEs)

### What is a CTE?

A **Common Table Expression** is like a **temporary table** that only exists during your query. It helps make complex queries easier to read and write.

### 🧠 Why Use It?
- Improves readability
- Reusable within the same query
- Helps break down complex logic

### 📝 Example:
```sql
WITH A AS (
    SELECT GENERATE_SERIES(5,9) AS number
),
B AS (
    SELECT CHR(GENERATE_SERIES(90,99)) AS letter
)
SELECT number, letter
FROM A
CROSS JOIN B;
```

> ✅ This creates two temporary tables (`A` and `B`) and combines them using a `CROSS JOIN`.

---

## 🧩 2. CROSS JOIN – All Possible Combinations

### What is a CROSS JOIN?

It combines **every row from one table** with **every row from another** — also called a **Cartesian Product**.

### 🧠 Why Use It?
- To generate all possible combinations
- Useful for tasks like assigning work to locations or creating test data

### 📝 Example:
```sql
-- Imagine you have 2 stores and 3 tasks
-- This gives you all possible combinations
SELECT store_id, task_name
FROM stores
CROSS JOIN tasks;
```

> ✅ You’ll get `store 1 - task A`, `store 1 - task B`, `store 2 - task A`, etc.

---

## 🧩 3. ANTI JOIN – Find Missing Matches

### What is an ANTI JOIN?

An **ANTI JOIN** helps you find rows in one table that **do not have a match** in another.

### 🧠 Why Use It?
- Find customers who never paid
- Find rentals with no payment
- Find products that never sold

### 📝 Example:
```sql
SELECT c.first_name, c.last_name
FROM customer c
LEFT JOIN payment p ON p.customer_id = c.customer_id
WHERE p.customer_id IS NULL;
```

> ✅ This finds **customers who have never made a payment**

### 🧩 How It Works:
- `LEFT JOIN` brings in payment data
- `WHERE p.customer_id IS NULL` filters for **no match**

---

## 🧩 4. Qualifying Columns – Avoiding Ambiguity

### What is Qualifying?

**Qualifying** means **prefixing column names with the table name** to avoid confusion.

### 🧠 Why Do It?
- Avoids ambiguous column errors
- Makes your code easier to read
- Helps when tables have columns with the same name

### 📝 Example:
```sql
SELECT c.first_name, c.last_name, f.title
FROM customer c
JOIN rental r ON r.customer_id = c.customer_id
JOIN inventory i ON i.inventory_id = r.inventory_id
JOIN film f ON f.film_id = i.film_id;
```

> ✅ Each column is prefixed with its table alias (`c`, `r`, `i`, `f`)

### ❌ Without Qualifying:
```sql
SELECT first_name, last_name, title
FROM customer
JOIN rental ON customer_id = customer_id -- ❌ AMBIGUOUS!
```

> 🚫 PostgreSQL will throw an error:  
> `column reference "customer_id" is ambiguous`

---

## 🧩 5. FULL OUTER JOIN – Show All from Both Tables

### What is a FULL OUTER JOIN?

A **FULL OUTER JOIN** shows **all records from both tables**, and matches them where possible.

### 🧠 Why Use It?
- You want to see **all data**, whether or not there’s a match
- Great for **data validation** or **auditing**

### 📝 Example:
```sql
SELECT c.first_name, c.last_name, r.return_date
FROM customer c
FULL OUTER JOIN rental r ON r.customer_id = c.customer_id
WHERE c.first_name IS NULL OR r.return_date IS NULL;
```

> ✅ Shows **all customers and rentals**, even if they don’t match

---

## 📌 Summary Table

| Concept | Purpose | Example |
|--------|---------|---------|
| **CTE** | Temporary table for complex queries | `WITH A AS (...) SELECT * FROM A` |
| **CROSS JOIN** | Combine every row with every other row | `SELECT * FROM A CROSS JOIN B` |
| **ANTI JOIN** | Find rows without a match | `LEFT JOIN ... WHERE ... IS NULL` |
| **Qualifying** | Avoid ambiguous column names | Use table aliases: `customer.first_name` |
| **FULL OUTER JOIN** | Show all from both tables | `FULL OUTER JOIN table ON condition` |

---

## 🧠 Final Notes

| Concept | Takeaway |
|--------|----------|
| **CTEs** | Use them to simplify complex queries |
| **CROSS JOIN** | Use for combinations (like assigning tasks to people) |
| **ANTI JOIN** | Use to find missing data (e.g., unpaid customers) |
| **Qualifying** | Always qualify your column names to avoid confusion |
| **FULL OUTER JOIN** | Use when you want to see everything from both tables |

---
