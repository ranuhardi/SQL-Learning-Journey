# 🧑‍💻 My Data Analyst + Cloud Security Journey

> I'm on a mission to become a **Security Data Analyst** by learning SQL, data analysis, and cloud security fundamentals — starting with SQL basics.

---

## 🗓️ Day 1: Introduction to SQL & Basic Queries

### 🎯 Goals
- Understand what SQL is
- Write my first SQL query
- Learn how to sort results using `ORDER BY`

---

### 🔍 What I Learned
Today I:
- Watched the *Introduction to SQL* and *History of SQL* videos from the DavidsonX course in EDX.org
- Installed PostgreSQL
- Wrote my first SQL query to retrieve actor data and sort it alphabetically

---

### 💻 My First SQL Query
```sql
SELECT actor_ID, 
       first_name, 
       last_name
FROM actor
ORDER BY first_name,
         last_name;

-- Alternative: ORDER BY 2, 3;
```
This query retrieves all actors and sorts them alphabetically by first and last name.

### 📝 Notes
- SELECT pulls data from a table
- FROM specifies which table to pull from
- ORDER BY controls how the data is sorted
- You can also sort by column position instead of name (ORDER BY 2, 3)

### 🚀 Next Steps
- Practice filtering data using WHERE, IN, BETWEEN, and LIKE
- Learn how to remove duplicates using DISTINCT
- Keep building my SQL foundation!

 📌 Tip: All queries and notes will be saved here so I can show progress to future employers. 
