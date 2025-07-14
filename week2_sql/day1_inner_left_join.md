# 📝 Day 1: Introduction to SQL JOINs – `INNER JOIN` & `LEFT JOIN`

> **Objective:** Understand the concept of JOINs in SQL, learn how to combine data from multiple tables, and practice writing queries using `INNER JOIN` and `LEFT JOIN`.

---

## 🔍 Introduction to JOINs

### Why JOINs Matter:
- Real-world databases are **normalized** to reduce redundancy.
- Data is often spread across **multiple tables**.
- JOINs allow you to **combine related data** from different tables.

### Key Concepts:
- **Primary Key** – Unique identifier for a record in a table
- **Foreign Key** – A field in one table that links to the primary key in another
- **Composite Key** – A key made up of **more than one column**
- **Referential Integrity** – Ensures relationships between tables remain consistent; avoids **orphaned records**

---

## 🧩 Types of JOINs (Focus: INNER & LEFT)

| Join Type | Description |
|-----------|-------------|
| `INNER JOIN` | Returns only matching rows from both tables |
| `LEFT JOIN` (or `LEFT OUTER JOIN`) | Returns all rows from the **left table**, and matched rows from the right table (or `NULL` if no match) |

---

## 📌 `INNER JOIN`: Get Matching Rows Only

### 🧩 Syntax:
```sql
SELECT columns
FROM table1
  INNER JOIN table2
    ON table1.column = table2.column;
```

### 📝 Example:
```sql
SELECT first_name
  , last_name
  , film.title
FROM actor
  INNER JOIN film_actor
    ON film_actor.actor_id = actor.actor_id
  JOIN film
    ON film.film_id = film_actor.film_id
WHERE film.title = 'Snowman Rollercoaster'
ORDER BY 3;
```

> ✅ This query returns only **actors who have appeared in "Snowman Rollercoaster"**.

---

## 📌 `LEFT JOIN`: Get All from Left, Matched or NULL from Right

### 🧩 Syntax:
```sql
SELECT columns
FROM table1
  LEFT JOIN table2
    ON table1.column = table2.column;
```

### 📝 Example:
```sql
SELECT first_name
  , last_name
  , film.title
FROM actor
  LEFT OUTER JOIN film_actor
    ON film_actor.actor_id = actor.actor_id
  LEFT OUTER JOIN film
    ON film.film_id = film_actor.film_id
WHERE first_name = 'Woody' AND last_name = 'Hoffman'
ORDER BY 3;
```

> ✅ This query returns **all films for actor "Woody Hoffman"**, including `NULL` if the actor has no film entries.

### Another Use Case:
```sql
SELECT first_name
  , last_name
  , film.title
FROM actor
  LEFT JOIN film_actor
    ON film_actor.actor_id = actor.actor_id
  LEFT JOIN film
    ON film.film_id = film_actor.film_id
WHERE film.title = 'Gilmore Boiled'
ORDER BY 3;
```

> ✅ Helps find actors who may or may not be linked to a specific film — useful for **data validation** or **auditing**.

---

## 🧠 Summary Notes

| Concept | Explanation |
|--------|-------------|
| `INNER JOIN` | Only returns rows where there is a match in both tables |
| `LEFT JOIN` | Returns all rows from the left table, even if no match exists |
| JOIN Order | Always define the `ON` clause immediately after each JOIN |
| Aliasing | Optional but recommended for readability (e.g., `a.actor_id = fa.actor_id`) |
| Avoiding Orphaned Records | Enforce referential integrity at the database level |

---

## ⚠️ Common Mistakes

| Mistake | Fix |
|--------|-----|
| Forgetting the `ON` clause | Always define how tables are related |
| Misusing `LEFT JOIN` when `INNER JOIN` is needed | Understand your data and what you're trying to retrieve |
| Filtering on the right table in `WHERE` with `LEFT JOIN` | Move such filters to the `ON` clause to avoid eliminating unmatched rows |

---
