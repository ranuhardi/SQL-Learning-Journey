# 📝 Day 4: Subqueries & Derived Tables – Think Like a Data Analyst

> **Objective:** Learn how to use **subqueries** (inner queries) and **derived tables** to answer complex business questions that can't be solved with simple joins.

---

## 🧠 What is a Subquery?

A **subquery** is a query **inside another query** — like a question inside a question.

### 🔍 Why Use It?
- To filter data based on dynamic values (e.g., "above average rental rate")
- To find data that exists (or doesn’t exist) in another table
- To avoid duplicates when joining

> ✅ Always **indent** your subqueries to make them readable!

---

## 🧩 1. Basic Subquery – Filter by a Dynamic Value

### Example: Films with rental rate above average

```sql
SELECT film.title, film.rental_rate
FROM film
WHERE film.rental_rate > (
    SELECT AVG(film.rental_rate)
    FROM film
);
```

### 🔍 How It Works:
1. Inner query: `SELECT AVG(film.rental_rate) FROM film` → returns a number (e.g., 2.98)
2. Outer query: shows films with rental rate **greater than that number**

> ✅ This is better than hardcoding a number — it updates automatically!

---

## 🧩 2. Subquery with `IN` – Find Customers Who Rented on Specific Dates

### Task: List customers who rented between May 25–26, 2005

```sql
SELECT customer.customer_id, customer.first_name, customer.last_name
FROM customer
WHERE customer.customer_id IN (
    SELECT rental.customer_id
    FROM rental
    WHERE rental.rental_date BETWEEN '2005-05-25' AND '2005-05-26'
);
```

### 🔍 Why Use Subquery Instead of JOIN?
- A `JOIN` would return **one row per rental**, so the same customer might appear multiple times.
- This `IN` subquery returns **each customer only once**.

---

## 🧩 3. Correlated Subquery – Match Data Row-by-Row

### Task: Show the **most recent film** each customer rented

```sql
SELECT DISTINCT
    customer.customer_id,
    customer.first_name,
    customer.last_name,
    MAX(rental.rental_date) AS "Latest Rental Date",
    film.title
FROM customer
    INNER JOIN rental ON rental.customer_id = customer.customer_id
    INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
    INNER JOIN film ON film.film_id = inventory.film_id
WHERE rental.rental_date = (
    SELECT MAX(R1.rental_date)
    FROM rental R1
    WHERE R1.customer_id = rental.customer_id
)
GROUP BY customer.customer_id, customer.first_name, customer.last_name, film.title
ORDER BY "Latest Rental Date" DESC;
```

### 🔍 What’s a Correlated Subquery?
- The inner query (`R1`) refers to the outer query (`rental.customer_id`)
- It runs **once for each row** in the outer query
- It finds the **latest rental date** for each customer

> ⚠️ This returns multiple rows per customer if they rented multiple films on their latest visit.

---

## 🧩 4. Fix Duplicates with `STRING_AGG` – Combine Multiple Rows

### Problem: One customer rented multiple films on the same day → multiple rows

### Solution: Use `STRING_AGG` to combine titles into one cell

```sql
SELECT 
    customer.customer_id,
    customer.first_name,
    customer.last_name,
    MAX(rental.rental_date) AS "Latest Rental Date",
    STRING_AGG(film.title, '; ' ORDER BY film.title) AS "Films Rented"
FROM customer
    INNER JOIN rental ON rental.customer_id = customer.customer_id
    INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
    INNER JOIN film ON film.film_id = inventory.film_id
WHERE rental.rental_date = (
    SELECT MAX(R1.rental_date)
    FROM rental R1
    WHERE R1.customer_id = rental.customer_id
)
GROUP BY customer.customer_id, customer.first_name, customer.last_name
ORDER BY "Latest Rental Date" DESC;
```

### 🔍 What is `STRING_AGG`?
- Aggregates multiple strings into one
- Syntax: `STRING_AGG(column, separator ORDER BY ...)`
- Example: `"Film A; Film B; Film C"`

> ✅ Perfect for reports where you want to show **all items in one cell**

---

## 🧩 5. Subquery in `SELECT` Clause – Add Summary Data

### Task: Show each customer with their **first and last rental date**

```sql
SELECT 
    customer.customer_id,
    customer.first_name,
    customer.last_name,
    (
        SELECT MIN(rental.rental_date)
        FROM rental
        WHERE rental.customer_id = customer.customer_id
    ) AS "Earliest Rental Date",
    (
        SELECT MAX(rental.rental_date)
        FROM rental
        WHERE rental.customer_id = customer.customer_id
    ) AS "Latest Rental Date"
FROM customer
ORDER BY customer.last_name, customer.first_name;
```

> ✅ Each subquery runs per customer → returns one value per row

---

## 🧩 6. Derived Table (Subquery in `FROM`) – Create a Temporary Table

### Task: Find top 5 customers who spent > $30 between Feb 15–21, 2007

#### Step 1: Write the inner query
```sql
SELECT customer_id, SUM(amount) AS TotalAmount
FROM payment
WHERE payment_date BETWEEN '2007-02-15' AND '2007-02-21'
GROUP BY customer_id
HAVING SUM(amount) > 30
ORDER BY TotalAmount DESC
LIMIT 5;
```

#### Step 2: Use it as a derived table and join with `customer`
```sql
SELECT 
    c.first_name,
    c.last_name,
    c.email,
    top_five.TotalAmount
FROM (
    SELECT customer_id, SUM(amount) AS TotalAmount
    FROM payment
    WHERE payment_date BETWEEN '2007-02-15' AND '2007-02-21'
    GROUP BY customer_id
    HAVING SUM(amount) > 30
    ORDER BY TotalAmount DESC
    LIMIT 5
) AS top_five
INNER JOIN customer c ON c.customer_id = top_five.customer_id;
```

> ✅ This is powerful: you’re treating a query **like a table**  
> 🧠 Think of it as a **temporary table** created on the fly

---

## 🧩 7. `EXISTS` and `NOT EXISTS` – Check for Presence

### When to Use:
- You care **only if data exists**, not the data itself
- Better performance than `IN` for large datasets

### Example: Customers who made at least one payment
```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE EXISTS (
    SELECT 1
    FROM payment p
    WHERE p.customer_id = c.customer_id
);
```

### Example: Customers who never paid (ANTI JOIN with `NOT EXISTS`)
```sql
SELECT c.first_name, c.last_name
FROM customer c
WHERE NOT EXISTS (
    SELECT 1
    FROM payment p
    WHERE p.customer_id = c.customer_id
);
```

> ✅ `EXISTS` stops as soon as it finds a match → fast and efficient

---

## 📌 Summary Table

| Technique | Use Case |
|---------|----------|
| **Subquery in `WHERE`** | Filter based on dynamic values or existence |
| **Correlated Subquery** | Match data row-by-row (e.g., "latest rental") |
| **`STRING_AGG`** | Combine multiple rows into one string |
| **Subquery in `SELECT`** | Add calculated values per row |
| **Derived Table (`FROM`)** | Treat a query as a temporary table |
| **`EXISTS` / `NOT EXISTS`** | Check if data exists (faster than `IN`) |

---

## 🧠 Pro Tips

| Tip | Why It Matters |
|-----|----------------|
| **Indent subqueries** | Makes code readable and maintainable |
| **Use aliases** | Helps distinguish tables and derived tables |
| **Use `DISTINCT` or aggregation** | Avoid duplicate rows from joins |
| **Prefer `EXISTS` over `IN`** | When checking for existence, especially with `NULL` values |
| **Test subqueries alone** | Run inner queries first to verify logic |

---
