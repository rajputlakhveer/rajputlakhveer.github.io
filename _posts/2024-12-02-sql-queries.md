---
layout: home
title: "SQL Queries"
date: 2024-12-02
categories: "SQL"
tags: [SQL, Queries, MySQL, Tips, Hacks]
image: 'https://github.com/user-attachments/assets/ab882c2a-05b7-440a-9c25-ed34a70ed011'
---

# SQL Queries That Will Surprise You! 🚀💡

SQL is the backbone of data manipulation, but some queries are not your everyday `SELECT *`. 🤯 Whether you’re a beginner or a seasoned developer, these mind-bending SQL queries will leave you astonished. Let’s dive in! 🌊

![1700747773147](https://github.com/user-attachments/assets/ab882c2a-05b7-440a-9c25-ed34a70ed011)

---

## 1. **Finding the Second Highest Salary 🏆**

Have you ever struggled to find the runner-up salary in a table? Here’s how to do it without using `LIMIT` or `OFFSET`:

```sql
SELECT MAX(salary) AS second_highest_salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```
### Explanation:
- The inner query finds the highest salary.
- The outer query finds the maximum salary that is less than the highest.

---

## 2. **Detecting Missing Gaps in a Sequence 🔍**

To find missing IDs in a sequence:

```sql
SELECT t1.id + 1 AS missing_id
FROM table_name t1
LEFT JOIN table_name t2
ON t1.id + 1 = t2.id
WHERE t2.id IS NULL;
```
### Explanation:
- Join each row to the next expected row.
- Filter out rows where the next ID exists.

---

## 3. **Finding the Nth Highest Value 🥉**

If you need the nth highest value, here’s a general approach:

```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET n - 1;
```
### Explanation:
- Use `ORDER BY` to sort salaries.
- Skip `n-1` rows using `OFFSET`.

---

## 4. **Calculating a Running Total 🧮**

To compute a cumulative sum in SQL:

```sql
SELECT employee_id, salary,
       SUM(salary) OVER (ORDER BY employee_id) AS running_total
FROM employees;
```
### Explanation:
- The `SUM()` function with the `OVER` clause adds up salaries in order.
- `ORDER BY` defines the sequence for the running total.

---

## 5. **Pivoting Data Dynamically 🔄**

Transform rows into columns:

```sql
SELECT employee_id,
       MAX(CASE WHEN month = 'January' THEN sales END) AS january_sales,
       MAX(CASE WHEN month = 'February' THEN sales END) AS february_sales
FROM sales
GROUP BY employee_id;
```
### Explanation:
- Use `CASE` to pivot specific rows into columns.
- `MAX` ensures unique values per column.

---

## 6. **Finding Duplicates 👯**

To identify duplicate entries:

```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 1;
```
### Explanation:
- Group rows by the column you’re checking.
- Use `HAVING` to filter groups with multiple rows.

---

## 7. **Finding Overlapping Date Ranges 📅**

When you need to detect overlapping time periods:

```sql
SELECT a.*, b.*
FROM reservations a
JOIN reservations b
ON a.start_date < b.end_date
AND a.end_date > b.start_date
AND a.id <> b.id;
```
### Explanation:
- Check if the date ranges intersect.
- Exclude the same row from matching itself.

---

## 8. **Hierarchical Queries (Finding Manager Trees) 🌲**

To get the hierarchy of employees and their managers:

```sql
WITH RECURSIVE employee_tree AS (
    SELECT employee_id, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    SELECT e.employee_id, e.manager_id, et.level + 1
    FROM employees e
    JOIN employee_tree et
    ON e.manager_id = et.employee_id
)
SELECT * FROM employee_tree;
```
### Explanation:
- Use a recursive CTE to traverse the hierarchy.
- The base case includes top-level managers, and recursion builds levels.

---

## 9. **Generating Random Data 🎲**

To populate a table with random values:

```sql
INSERT INTO random_data (value)
SELECT FLOOR(1 + (RAND() * 100))
FROM numbers_table
LIMIT 10;
```
### Explanation:
- `RAND()` generates random numbers.
- Multiply and floor values to get integers in a range.

---

## 10. **Finding the Median 📈**

Calculating the median value in SQL can be tricky:

```sql
SELECT AVG(salary) AS median
FROM (
    SELECT salary
    FROM employees
    ORDER BY salary
    LIMIT 2 - (SELECT COUNT(*) FROM employees) % 2
    OFFSET (SELECT (COUNT(*) - 1) / 2 FROM employees)
) subquery;
```
### Explanation:
- Find the middle values using `LIMIT` and `OFFSET`.
- Average them if necessary to compute the median.

---

### Conclusion 🎉

SQL is full of hidden gems that can solve complex problems elegantly. 💎 With these queries in your arsenal, you’ll tackle any data challenge like a pro. Happy querying! 💻✨

Have a query that’s stumped you? Share it in the comments below! 👇

