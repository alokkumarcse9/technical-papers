# SQL Aggregations and Filters — Cheat Sheet

## 1. What are Aggregations?

An **aggregate function** performs a calculation on multiple rows and returns a single result.

### Simple Definition

> **Aggregation = Take many rows and calculate one result.**

For example:

```text
100
200
300
400
```

An aggregate function can calculate:

```text
SUM   → 1000
AVG   → 250
MIN   → 100
MAX   → 400
COUNT → 4
```

---

# Common Aggregate Functions

| Function  | Meaning              |
| --------- | -------------------- |
| `COUNT()` | Counts rows          |
| `SUM()`   | Adds values          |
| `AVG()`   | Calculates average   |
| `MIN()`   | Finds smallest value |
| `MAX()`   | Finds largest value  |

---

# 2. COUNT()

### Definition

**`COUNT()` counts rows or non-NULL values.**

### Example

```sql
SELECT COUNT(*)
FROM employees;
```

If there are 5 employees:

```text
COUNT(*) → 5
```

### Count a specific column

```sql
SELECT COUNT(department_id)
FROM employees;
```

`COUNT(column)` does not count `NULL` values.

### Remember

> **COUNT = How many?**

---

# 3. SUM()

### Definition

**`SUM()` adds all numeric values together.**

Suppose:

| employee | salary |
| -------- | -----: |
| Alok     |  50000 |
| Rahul    |  60000 |
| Priya    |  70000 |

```sql
SELECT SUM(salary)
FROM employees;
```

Result:

```text
180000
```

### Remember

> **SUM = Total**

---

# 4. AVG()

### Definition

**`AVG()` calculates the average of numeric values.**

```sql
SELECT AVG(salary)
FROM employees;
```

For:

```text
50000
60000
70000
```

Result:

```text
60000
```

### Remember

> **AVG = Average**

---

# 5. MIN()

### Definition

**`MIN()` returns the smallest value.**

```sql
SELECT MIN(salary)
FROM employees;
```

Result:

```text
50000
```

### Remember

> **MIN = Smallest**

---

# 6. MAX()

### Definition

**`MAX()` returns the largest value.**

```sql
SELECT MAX(salary)
FROM employees;
```

Result:

```text
70000
```

### Remember

> **MAX = Largest**

---

# 7. GROUP BY

## Why do we need GROUP BY?

Aggregate functions can give one result for the entire table.

But sometimes we want a result **for each group**.

### Example

Suppose:

| name  | department | salary |
| ----- | ---------- | -----: |
| Alok  | IT         |  50000 |
| Rahul | IT         |  60000 |
| Priya | HR         |  70000 |
| Aman  | HR         |  80000 |

We want:

```text
IT → Total salary
HR → Total salary
```

Use `GROUP BY`.

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

Result:

| department | total_salary |
| ---------- | -----------: |
| IT         |       110000 |
| HR         |       150000 |

### Simple Definition

> **GROUP BY = Make groups so aggregate functions can calculate each group separately.**

---

# 8. GROUP BY with Multiple Aggregations

You can use multiple aggregate functions together.

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary
FROM employees
GROUP BY department;
```

Result:

| department | employee_count | total_salary | average_salary | minimum_salary | maximum_salary |
| ---------- | -------------: | -----------: | -------------: | -------------: | -------------: |
| IT         |              2 |       110000 |          55000 |          50000 |          60000 |
| HR         |              2 |       150000 |          75000 |          70000 |          80000 |

---

# 9. Filters in SQL Queries

A **filter** is used to select only the rows that satisfy a condition.

The main filtering clauses are:

```text
WHERE
HAVING
```

---

# 10. WHERE

### Definition

**`WHERE` filters individual rows before grouping or aggregation.**

### Example

Get employees whose salary is greater than ₹60,000:

```sql
SELECT *
FROM employees
WHERE salary > 60000;
```

Result:

| name  | salary |
| ----- | -----: |
| Priya |  70000 |
| Aman  |  80000 |

### Remember

> **WHERE = Filter rows**

---

# 11. Common WHERE Operators

### Comparison Operators

```sql
=
!=
<>
>
<
>=
<=
```

Examples:

```sql
WHERE salary = 50000

WHERE salary > 50000

WHERE salary >= 50000

WHERE salary < 50000

WHERE salary != 50000
```

---

# 12. AND

### Definition

**`AND` requires all conditions to be true.**

```sql
SELECT *
FROM employees
WHERE department = 'IT'
  AND salary > 50000;
```

Meaning:

```text
Department must be IT
AND
Salary must be greater than 50000
```

---

# 13. OR

### Definition

**`OR` requires at least one condition to be true.**

```sql
SELECT *
FROM employees
WHERE department = 'IT'
   OR department = 'HR';
```

Meaning:

```text
Department can be IT
OR
Department can be HR
```

---

# 14. NOT

### Definition

**`NOT` reverses a condition.**

```sql
SELECT *
FROM employees
WHERE NOT department = 'IT';
```

Meaning:

```text
Give employees who are NOT in IT.
```

---

# 15. BETWEEN

### Definition

**`BETWEEN` checks whether a value is within a range.**

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 50000 AND 70000;
```

This includes both boundaries:

```text
50000 <= salary <= 70000
```

### Remember

> **BETWEEN = Within a range**

---

# 16. IN

### Definition

**`IN` checks whether a value matches any value from a list.**

Instead of:

```sql
WHERE department = 'IT'
   OR department = 'HR'
   OR department = 'Finance'
```

You can write:

```sql
WHERE department IN ('IT', 'HR', 'Finance');
```

### Remember

> **IN = Match one of these values**

---

# 17. LIKE

### Definition

**`LIKE` is used for pattern matching in text.**

### Starts with A

```sql
WHERE name LIKE 'A%';
```

Meaning:

```text
Names starting with A
```

Example:

```text
Alok
Aman
Ankit
```

### Ends with n

```sql
WHERE name LIKE '%n';
```

### Contains "lo"

```sql
WHERE name LIKE '%lo%';
```

### Important Wildcards

```text
% → Zero or more characters

_ → Exactly one character
```

Example:

```sql
WHERE name LIKE 'A___';
```

Matches a four-character name starting with `A`.

---

# 18. IS NULL

You should not use:

```sql
WHERE department = NULL;
```

Use:

```sql
WHERE department IS NULL;
```

### Example

```sql
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

### NOT NULL

```sql
WHERE manager_id IS NOT NULL;
```

### Remember

> **NULL is checked using IS NULL / IS NOT NULL**

---

# 19. HAVING

### Definition

**`HAVING` filters groups after `GROUP BY` and aggregation.**

This is the most important difference:

```text
WHERE  → Filters rows
HAVING → Filters groups
```

### Example

Suppose we want departments whose total salary is greater than ₹100,000.

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
HAVING SUM(salary) > 100000;
```

Result:

| department | total_salary |
| ---------- | -----------: |
| IT         |       110000 |
| HR         |       150000 |

### Remember

> **HAVING = Filter aggregated groups**

---

# 20. WHERE vs HAVING

This is a very common interview question.

| WHERE                                        | HAVING                                 |
| -------------------------------------------- | -------------------------------------- |
| Filters rows                                 | Filters groups                         |
| Used before `GROUP BY`                       | Used after `GROUP BY`                  |
| Works with individual rows                   | Works with grouped/aggregated results  |
| Usually cannot use aggregate result directly | Commonly used with aggregate functions |

### Example

Filter employees first:

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
WHERE salary > 40000
GROUP BY department;
```

Then filter groups:

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

---

# 21. WHERE + GROUP BY + HAVING

You can use them together.

### Question

Find departments where:

1. Only employees with salary greater than ₹40,000 are considered.
2. Employees are grouped by department.
3. The department must have at least 2 employees.

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
WHERE salary > 40000
GROUP BY department
HAVING COUNT(*) >= 2;
```

### How SQL processes it

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
```

Conceptually, this means:

```text
1. Get employees
2. Remove employees with salary <= 40000
3. Group remaining employees by department
4. Remove groups having fewer than 2 employees
5. Display the result
```

---

# 22. ORDER BY with Aggregations

You can sort aggregated results.

### Example

Show departments from highest total salary to lowest:

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC;
```

Result:

| department | total_salary |
| ---------- | -----------: |
| HR         |       150000 |
| IT         |       110000 |

### Remember

```text
ASC  → Small to large
DESC → Large to small
```

---

# 23. LIMIT

### Definition

**`LIMIT` restricts the number of rows returned.**

Example:

```sql
SELECT *
FROM employees
LIMIT 5;
```

Returns only 5 rows.

### Top 3 highest salaries

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

---

# 24. Complete Example

### Question

Find the top 3 departments by average salary, considering only employees earning more than ₹40,000, and only include departments whose average salary is greater than ₹60,000.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
WHERE salary > 40000
GROUP BY department
HAVING AVG(salary) > 60000
ORDER BY average_salary DESC
LIMIT 3;
```

### Flow

```text
FROM
  ↓
WHERE
  ↓
GROUP BY
  ↓
HAVING
  ↓
SELECT
  ↓
ORDER BY
  ↓
LIMIT
```

---

# Aggregate Functions Quick Revision

```text
COUNT() → How many?
SUM()   → What is the total?
AVG()   → What is the average?
MIN()   → What is the smallest?
MAX()   → What is the largest?
```

# Filter Quick Revision

```text
WHERE       → Filter rows

HAVING      → Filter groups

AND         → All conditions must be true

OR          → At least one condition must be true

NOT         → Reverse a condition

BETWEEN     → Check a range

IN          → Match from a list

LIKE        → Match a text pattern

IS NULL     → Check for NULL

IS NOT NULL → Check for non-NULL
```

# Interview Cheat Sheet

### What is an aggregate function?

> An aggregate function performs a calculation on multiple rows and returns a single result.

### What is GROUP BY?

> `GROUP BY` divides rows into groups so aggregate functions can calculate results for each group.

### WHERE vs HAVING?

> `WHERE` filters individual rows before grouping, while `HAVING` filters groups after aggregation.

### Can WHERE use aggregate functions?

Generally, **no**. Use `HAVING` when you need to filter based on an aggregate result.

```sql
-- Correct
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

### Most Important Pattern

```sql
SELECT
    column,
    AGGREGATE_FUNCTION(column)
FROM table
WHERE row_condition
GROUP BY column
HAVING aggregate_condition
ORDER BY column
LIMIT number;
```

## Final Memory Rule

```text
WHERE  → Which ROWS do I want?

GROUP BY → How should I GROUP those rows?

HAVING → Which GROUPS do I want?

ORDER BY → How should I SORT the result?

LIMIT → How many rows do I need?
```
