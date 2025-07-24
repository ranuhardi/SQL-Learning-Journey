# 📝 Day 3: Grouping & Aggregation – Analyze Customer Behavior

> **Objective:** Learn how to use `GROUP BY` with **aggregation functions** (`COUNT`, `SUM`, `AVG`, `MIN`, `MAX`) to answer real business questions about customers.

---

## 🧠 What is Aggregation?

Aggregation means **summarizing data** — not showing every row, but instead asking questions like:
- How many rentals did each customer make?
- How much money did they spend?
- What’s the average length of the films they watched?

We use **aggregation functions** and `GROUP BY` to get these answers.

---

## 📌 Key Aggregation Functions

| Function | What It Does |
|--------|--------------|
| `COUNT(*)` | Counts the number of rows |
| `SUM(column)` | Adds up all values in a column |
| `AVG(column)` | Calculates the average |
| `MIN(column)` | Finds the smallest value |
| `MAX(column)` | Finds the largest value |

> ✅ These are used with `GROUP BY` to summarize data by category (e.g., by customer).

---

## 🧩 The `GROUP BY` Rule

> 🔑 **Golden Rule:**  
Every column in the `SELECT` clause that is **not aggregated** must be in the `GROUP BY` clause.

### ✅ Correct:
```sql
SELECT customer_id, COUNT(*)
FROM rental
GROUP BY customer_id;
```

### ❌ Wrong:
```sql
SELECT customer_id, first_name, COUNT(*)
FROM rental
GROUP BY customer_id; -- ❌ first_name not in GROUP BY!
```

---

## 🧩 `GROUP BY` Order in SQL

The `GROUP BY` clause comes in this order:

```sql
SELECT ...
FROM ...
WHERE ...           -- Filter raw rows
GROUP BY ...        -- Group data
ORDER BY ...        -- Sort final results
```

> ⚠️ `WHERE` happens **before** grouping — so you filter data first, then group it.

---

## 🎯 Real-World Example: Customer Analysis

Let’s answer these questions:
1. How many rentals per customer?
2. How much money did they spend?
3. What’s the average film length they rented?
4. When was their first and last rental?

---

### 📝 Final Query

```sql
SELECT 
    customer.customer_id,
    customer.first_name,
    customer.last_name,
    COUNT(*) AS "Count of Rentals",
    SUM(payment.amount) AS "Total Money Spent",
    AVG(film.length) AS "Average Film Length",
    ROUND(AVG(film.length), 2) AS "Average Film Length (rounded)",
    TRUNC(AVG(film.length), 2) AS "Average Film Length (truncated)",
    ROUND(AVG(film.length)) AS "Average Film Length (rounded whole)",
    TRUNC(AVG(film.length)) AS "Average Film Length (truncated whole)",
    MIN(rental.rental_date) AS "Earliest Rental Date",
    MAX(rental.rental_date) AS "Latest Rental Date"
FROM film
    INNER JOIN inventory ON inventory.film_id = film.film_id
    INNER JOIN rental ON rental.inventory_id = inventory.inventory_id
    INNER JOIN customer ON customer.customer_id = rental.customer_id
    LEFT JOIN payment ON payment.rental_id = rental.rental_id 
                     AND payment.customer_id = customer.customer_id
GROUP BY 
    customer.customer_id,
    customer.first_name,
    customer.last_name
ORDER BY 
    "Count of Rentals" DESC;
```

---

## 🧠 Breakdown of the Query

| Part | Explanation |
|------|-------------|
| **Joins** | Connects `film` → `inventory` → `rental` → `customer` → `payment` |
| **LEFT JOIN payment** | Ensures we include rentals even if no payment was made |
| **GROUP BY** | Groups data by customer (so we get one row per customer) |
| **Aggregations** | Summarizes rental count, total spent, film length, etc. |
| **ORDER BY** | Sorts customers by number of rentals (highest first) |

---

## 🧮 Rounding & Truncating Numbers

When dealing with averages, you often get long decimals. Use these to clean them up:

| Function | Example | Result |
|--------|--------|--------|
| `ROUND(number, 2)` | `ROUND(123.456, 2)` | `123.46` |
| `TRUNC(number, 2)` | `TRUNC(123.456, 2)` | `123.45` |
| `ROUND(number)` | `ROUND(123.456)` | `123` |
| `TRUNC(number)` | `TRUNC(123.456)` | `123` |

> ✅ Use `ROUND` to round to nearest value  
> ✅ Use `TRUNC` to cut off decimals without rounding

---

## 📌 Summary Table: Aggregation in Practice

| Business Question | SQL Function |
|-------------------|--------------|
| How many rentals? | `COUNT(*)` |
| Total money spent? | `SUM(payment.amount)` |
| Average film length? | `AVG(film.length)` |
| First rental date? | `MIN(rental.rental_date)` |
| Last rental date? | `MAX(rental.rental_date)` |

---

## 🧩 Pro Tips

| Tip | Why It Matters |
|-----|----------------|
| Always qualify column names | Avoids ambiguity errors |
| Use aliases for long column names | Makes results easier to read |
| Use `LEFT JOIN` for optional data (like payments) | Prevents losing rentals with no payment |
| Round or truncate averages | Makes reports look professional |

---
