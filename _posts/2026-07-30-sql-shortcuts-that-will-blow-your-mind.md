---
layout: home
title: "SQL Shortcuts That Will Blow Your Mind"
date: 2026-07-30
categories: "Programming"
tags: [SQL, Database, PostgreSQL, MySQL, Backend Development, Software Engineering, Data Engineering, Programming]
image: 'https://github.com/user-attachments/assets/ff86a86d-ff2b-47c8-9e66-7937d00d8065'
---

# 🚀 SQL Shortcuts That Will Blow Your Mind 🤯

## 25 SQL Tricks Every Developer Should Know to Write Faster, Cleaner & Smarter Queries

> **"Good SQL gets the job done. Great SQL gets it done in milliseconds."** ⚡

SQL (Structured Query Language) is one of the few technologies that has remained relevant for decades. Whether you're a Backend Developer, Data Analyst, Data Engineer, AI Engineer, or DevOps Engineer, writing efficient SQL can save hours of debugging and dramatically improve application performance.

Most developers know the basics:

```sql
SELECT * FROM users;
```

But very few know the hidden shortcuts that make SQL elegant, readable, and incredibly powerful.

<img width="1024" height="1536" alt="ChatGPT Image Jul 30, 2026, 08_34_46 PM" src="https://github.com/user-attachments/assets/ff86a86d-ff2b-47c8-9e66-7937d00d8065" />

Let's explore some mind-blowing SQL shortcuts! 🚀

---

# 1. SELECT DISTINCT Instead of GROUP BY

Many developers use GROUP BY just to remove duplicates.

❌ Instead of

```sql
SELECT department
FROM employees
GROUP BY department;
```

✅ Use

```sql
SELECT DISTINCT department
FROM employees;
```

### Why?

* Cleaner
* Faster
* Easier to understand

Output

```
IT
HR
Finance
Marketing
```

---

# 2. LIMIT with OFFSET

Need pagination?

Instead of fetching everything:

```sql
SELECT *
FROM products;
```

Use

```sql
SELECT *
FROM products
LIMIT 10 OFFSET 20;
```

This returns rows **21–30**.

Perfect for:

* APIs
* Dashboards
* Infinite scrolling

---

# 3. ORDER BY Multiple Columns

Instead of sorting once,

```sql
ORDER BY salary;
```

Do

```sql
ORDER BY department,
         salary DESC;
```

Now SQL first sorts by department, then highest salary.

---

# 4. COALESCE() — Replace NULL Instantly

Imagine

```
Name    Bonus
John    NULL
Sam     5000
```

Instead of

```sql
NULL
```

Show

```sql
SELECT
name,
COALESCE(bonus,0)
FROM employees;
```

Output

```
John   0
Sam    5000
```

Very useful for reports.

---

# 5. CASE WHEN Instead of Multiple Queries

Instead of writing many filters,

```sql
SELECT
name,
CASE
WHEN salary > 100000 THEN 'High'
WHEN salary > 50000 THEN 'Medium'
ELSE 'Low'
END AS Salary_Level
FROM employees;
```

Output

```
John High
Sam Medium
Raj Low
```

---

# 6. EXISTS Instead of COUNT()

Many developers write

```sql
SELECT COUNT(*)
FROM users
WHERE email='abc@gmail.com';
```

Better

```sql
SELECT EXISTS(
SELECT 1
FROM users
WHERE email='abc@gmail.com'
);
```

Why?

✅ Stops after first match.

Much faster.

---

# 7. IN Instead of OR

Instead of

```sql
WHERE department='IT'
OR department='HR'
OR department='Finance'
```

Use

```sql
WHERE department IN
('IT','HR','Finance');
```

Cleaner.

Readable.

Optimized.

---

# 8. BETWEEN

Instead of

```sql
salary>=50000
AND salary<=100000
```

Write

```sql
salary BETWEEN 50000 AND 100000;
```

Perfect for:

* Dates
* Numbers
* IDs

---

# 9. Alias Everything

Bad

```sql
SELECT employees.first_name
```

Good

```sql
SELECT e.first_name
FROM employees e;
```

Especially helpful in joins.

---

# 10. USING Instead of ON

If both tables have same column name.

Instead of

```sql
SELECT *
FROM employees
JOIN departments
ON employees.department_id =
departments.department_id;
```

Use

```sql
SELECT *
FROM employees
JOIN departments
USING(department_id);
```

Much cleaner.

---

# 11. RETURN Only Needed Columns

Never do

```sql
SELECT *
```

Instead

```sql
SELECT
id,
name,
salary
```

Benefits

✅ Less memory

✅ Less network traffic

✅ Better cache

---

# 12. HAVING Instead of WHERE on Aggregates

Wrong

```sql
WHERE COUNT(*) > 5
```

Correct

```sql
GROUP BY department
HAVING COUNT(*) > 5;
```

---

# 13. COUNT(column) vs COUNT(*)

```sql
COUNT(*)
```

Counts everything.

```sql
COUNT(phone)
```

Counts only non-null values.

---

# 14. UPDATE Multiple Columns Together

Instead of

```sql
UPDATE users
SET first_name='John';

UPDATE users
SET last_name='Doe';
```

Do

```sql
UPDATE users
SET
first_name='John',
last_name='Doe';
```

One query.

One transaction.

---

# 15. INSERT Multiple Rows

Instead of

```sql
INSERT INTO products VALUES(...);

INSERT INTO products VALUES(...);

INSERT INTO products VALUES(...);
```

Use

```sql
INSERT INTO products(name,price)
VALUES
('Mouse',500),
('Keyboard',800),
('Monitor',15000);
```

Much faster.

---

# 16. DELETE with LIMIT (MySQL)

```sql
DELETE
FROM logs
LIMIT 100;
```

Useful for cleaning huge tables gradually.

---

# 17. LIKE Wildcards

Starts with

```sql
LIKE 'A%'
```

Ends with

```sql
LIKE '%A'
```

Contains

```sql
LIKE '%SQL%'
```

Single character

```sql
LIKE '_A%'
```

---

# 18. CONCAT()

Instead of application code

```sql
SELECT
CONCAT(first_name,' ',last_name)
FROM users;
```

Output

```
John Doe
```

---

# 19. CURRENT_DATE

Instead of hardcoding

```sql
'2026-07-30'
```

Use

```sql
CURRENT_DATE
```

Or

```sql
NOW()
```

---

# 20. LIMIT 1

Need only one row?

Instead of

```sql
SELECT *
FROM users
WHERE email='abc@gmail.com';
```

Use

```sql
LIMIT 1;
```

Huge optimization.

---

# 21. Window Functions Without Losing Rows 🪟

Need rankings without collapsing data?

```sql
SELECT
name,
salary,
RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

Unlike `GROUP BY`, window functions keep every row while calculating rankings, running totals, moving averages, and much more.

Other useful functions include:

* `ROW_NUMBER()`
* `DENSE_RANK()`
* `LAG()`
* `LEAD()`
* `SUM() OVER()`

---

# 22. Common Table Expressions (CTEs) for Readability 📖

Instead of deeply nested subqueries:

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 100000
)
SELECT *
FROM high_salary
WHERE department = 'IT';
```

Benefits:

* Easier to debug
* Easier to maintain
* Reusable query blocks

---

# 23. Conditional Aggregation in One Query 🎯

Instead of running multiple queries:

```sql
SELECT
COUNT(*) AS total,
SUM(CASE WHEN status='active' THEN 1 ELSE 0 END) AS active_users,
SUM(CASE WHEN status='inactive' THEN 1 ELSE 0 END) AS inactive_users
FROM users;
```

One scan of the table gives multiple metrics.

---

# 24. UPSERT Instead of Separate INSERT/UPDATE 🔄

Many databases support an "upsert" pattern.

**PostgreSQL**

```sql
INSERT INTO products(id, name, price)
VALUES (1, 'Keyboard', 800)
ON CONFLICT (id)
DO UPDATE
SET
name = EXCLUDED.name,
price = EXCLUDED.price;
```

This avoids checking if the row already exists.

---

# 25. EXPLAIN Before Optimizing 🔍

Want to know why your query is slow?

```sql
EXPLAIN
SELECT *
FROM orders
WHERE customer_id = 42;
```

Or, in PostgreSQL:

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE customer_id = 42;
```

This shows:

* Whether indexes are used
* Estimated cost
* Execution strategy
* Actual execution time (with `ANALYZE`)

Never optimize blindly—measure first.

---

# ⚡ SQL Performance Hacks

## 🚀 Index the Right Columns

Create indexes on:

* Primary keys
* Foreign keys
* Frequently searched columns
* Frequently joined columns

Example

```sql
CREATE INDEX idx_email
ON users(email);
```

---

## 🚀 Avoid SELECT *

Only fetch the columns you actually need.

---

## 🚀 Filter Early

Reduce rows before joins whenever possible.

```sql
SELECT *
FROM (
    SELECT *
    FROM orders
    WHERE order_date >= CURRENT_DATE - INTERVAL '30 days'
) recent_orders
JOIN customers
ON recent_orders.customer_id = customers.id;
```

---

## 🚀 Prefer EXISTS for Existence Checks

Checking whether a record exists?

Use `EXISTS` instead of `COUNT(*)`.

---

## 🚀 Keep Transactions Short

Long-running transactions:

* Lock rows longer
* Increase contention
* Slow down concurrent users

Commit as soon as practical.

---

## 🚀 Batch Inserts

Thousands of single inserts:

```sql
INSERT ...
```

are slower than one bulk insert.

---

## 🚀 Beware Functions on Indexed Columns

This may prevent index usage:

```sql
WHERE LOWER(email) = 'john@example.com'
```

Instead, consider storing normalized values or using functional indexes if your database supports them.

---

## 🚀 Use Appropriate Data Types

Don't store integers as text.

Smaller data types reduce:

* Storage
* Memory
* Index size
* I/O

---

## 🚀 Monitor Slow Queries

Most production databases provide slow-query logs.

Review them regularly to identify expensive queries before users notice.

---

# 💡 Pro Tips

✨ Learn execution plans (`EXPLAIN` and `EXPLAIN ANALYZE`).

✨ Design indexes based on actual query patterns, not assumptions.

✨ Avoid `SELECT DISTINCT` when duplicate rows indicate a data modeling issue.

✨ Write readable SQL first, then optimize based on evidence.

✨ Use parameterized queries to improve security and allow execution plan reuse.

✨ Understand your database engine—optimization techniques vary between PostgreSQL, MySQL, SQL Server, Oracle, and SQLite.

---

# 🎯 Final Thoughts

SQL is far more than a language for retrieving data—it's a powerful toolkit for solving complex problems efficiently. Mastering shortcuts like `COALESCE`, `CASE`, `EXISTS`, window functions, CTEs, and `EXPLAIN` helps you write queries that are not only shorter but also faster, more maintainable, and easier for teammates to understand.

The best SQL developers don't just make queries work—they make them **scalable**, **readable**, and **efficient**. Start incorporating these shortcuts into your daily workflow, and you'll quickly notice improvements in both development speed and application performance.

> **"The fastest query isn't always the shortest one—it's the one that reads the least data and does the least work."** 🚀
