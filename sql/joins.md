# SQL Joins — Cheat Sheet

## What is a JOIN?

A **JOIN** is used to combine data from **two or more tables** using a related column.

### Simple Definition

> **JOIN = Combine related data from multiple tables.**

---

# Example Tables

Let's use these two tables throughout the examples.

### `employees`

| id | name  | department_id |
| -: | ----- | ------------: |
|  1 | Alok  |            10 |
|  2 | Rahul |            20 |
|  3 | Priya |            30 |
|  4 | Aman  |          NULL |

### `departments`

| id | department_name |
| -: | --------------- |
| 10 | IT              |
| 20 | HR              |
| 40 | Finance         |

The common column is:

```text
employees.department_id
        ↕
departments.id
```

---

# 1. INNER JOIN

### Definition

**Returns only the rows that have a match in both tables.**

In simple words:

> **INNER JOIN = Give me only matching data.**

### Query

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
    ON e.department_id = d.id;
```

### Result

| name  | department_name |
| ----- | --------------- |
| Alok  | IT              |
| Rahul | HR              |

Why?

```text
Alok  → 10 → IT      ✅
Rahul → 20 → HR      ✅
Priya → 30 → No match ❌
Aman  → NULL         ❌
Finance → No employee ❌
```

### Remember

> **INNER JOIN = Matching rows only**

---

# 2. LEFT JOIN

### Definition

**Returns all rows from the left table and matching rows from the right table.**

If there is no match, the right-side columns become `NULL`.

In simple words:

> **LEFT JOIN = Give me everything from the left table.**

### Query

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d
    ON e.department_id = d.id;
```

### Result

| name  | department_name |
| ----- | --------------- |
| Alok  | IT              |
| Rahul | HR              |
| Priya | NULL            |
| Aman  | NULL            |

Notice:

```text
Employees → ALL included ✅
Departments → Only matching ones
```

### Remember

> **LEFT JOIN = All left + matching right**

---

# 3. RIGHT JOIN

### Definition

**Returns all rows from the right table and matching rows from the left table.**

In simple words:

> **RIGHT JOIN = Give me everything from the right table.**

### Query

```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d
    ON e.department_id = d.id;
```

### Result

| name  | department_name |
| ----- | --------------- |
| Alok  | IT              |
| Rahul | HR              |
| NULL  | Finance         |

Why?

Finance exists in `departments`, but no employee belongs to department `40`.

So:

```text
Departments → ALL included ✅
Employees   → Only matching ones
```

### Remember

> **RIGHT JOIN = All right + matching left**

---

# 4. FULL OUTER JOIN

### Definition

**Returns all rows from both tables.**

Matching rows are combined, and unmatched rows contain `NULL`.

In simple words:

> **FULL JOIN = Give me everything from both tables.**

### Query

```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d
    ON e.department_id = d.id;
```

### Result

| name  | department_name |
| ----- | --------------- |
| Alok  | IT              |
| Rahul | HR              |
| Priya | NULL            |
| Aman  | NULL            |
| NULL  | Finance         |

So we get:

```text
Employees      → ALL ✅
Departments    → ALL ✅
Matching rows  → Combined
Non-matching   → NULL
```

### Remember

> **FULL JOIN = Everything from both sides**

---

# 5. CROSS JOIN

### Definition

**Returns every possible combination of rows from both tables.**

In simple words:

> **CROSS JOIN = Every row with every row.**

If:

```text
Table A → 4 rows
Table B → 3 rows
```

Then:

```text
4 × 3 = 12 rows
```

### Query

```sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

### Example Result

| name  | department_name |
| ----- | --------------- |
| Alok  | IT              |
| Alok  | HR              |
| Alok  | Finance         |
| Rahul | IT              |
| Rahul | HR              |
| Rahul | Finance         |
| ...   | ...             |

There is **no `ON` condition**.

### Remember

> **CROSS JOIN = Cartesian product**

---

# 6. SELF JOIN

### Definition

**A SELF JOIN joins a table with itself.**

It is useful when rows in the same table are related to each other.

### Example

Suppose we have:

### `employees`

| id | name  | manager_id |
| -: | ----- | ---------: |
|  1 | Alok  |       NULL |
|  2 | Rahul |          1 |
|  3 | Priya |          1 |
|  4 | Aman  |          2 |

Here:

```text
Rahul → Manager = Alok
Priya → Manager = Alok
Aman  → Manager = Rahul
```

### Query

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.id;
```

### Result

| employee | manager |
| -------- | ------- |
| Alok     | NULL    |
| Rahul    | Alok    |
| Priya    | Alok    |
| Aman     | Rahul   |

The same table is used twice:

```text
employees e
    ↓
Employee

employees m
    ↓
Manager
```

### Remember

> **SELF JOIN = Join a table with itself**

---

# JOINs at a Glance

| JOIN                | What it returns                |
| ------------------- | ------------------------------ |
| **INNER JOIN**      | Matching rows from both tables |
| **LEFT JOIN**       | All left + matching right      |
| **RIGHT JOIN**      | All right + matching left      |
| **FULL OUTER JOIN** | All rows from both tables      |
| **CROSS JOIN**      | Every possible combination     |
| **SELF JOIN**       | A table joined with itself     |

---

# Visual Memory Trick

```text
INNER JOIN

     A ∩ B
      ↓
   Matches
```

```text
LEFT JOIN

     A ∪ matching B
      ↓
   Everything from A
```

```text
RIGHT JOIN

     matching A ∪ B
      ↓
   Everything from B
```

```text
FULL JOIN

     A ∪ B
      ↓
   Everything
```

```text
CROSS JOIN

     A × B
      ↓
 Every combination
```

---

# Most Important JOIN Pattern

Most SQL JOINs follow this structure:

```sql
SELECT columns
FROM table1
JOIN table2
    ON table1.common_column = table2.common_column;
```

For example:

```sql
SELECT
    e.name,
    d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.id;
```

Here:

```text
FROM
 ↓
employees

JOIN
 ↓
departments

ON
 ↓
How should they be connected?
```

---

# JOIN vs WHERE

Don't confuse these:

### `ON`

Defines **how two tables are related**.

```sql
ON e.department_id = d.id
```

### `WHERE`

Filters the **result**.

```sql
WHERE d.department_name = 'IT'
```

Example:

```sql
SELECT e.name, d.department_name
FROM employees e
JOIN departments d
    ON e.department_id = d.id
WHERE d.department_name = 'IT';
```

Meaning:

```text
JOIN → Connect employees with departments
WHERE → Keep only IT employees
```

---

# Interview Definition

If asked:

**"What is a JOIN in SQL?"**

Answer:

> **A JOIN is used to combine rows from two or more tables based on a related column between them. Common types include INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN, CROSS JOIN, and SELF JOIN.**

---

# Quick Revision

```text
INNER JOIN  → Matching rows only

LEFT JOIN   → All left + matching right

RIGHT JOIN  → All right + matching left

FULL JOIN   → Everything from both tables

CROSS JOIN  → Every possible combination

SELF JOIN   → Table joined with itself
```

### Golden Rule

> **INNER = Match**
>
> **LEFT = Keep left**
>
> **RIGHT = Keep right**
>
> **FULL = Keep both**
>
> **CROSS = Combine everything**
>
> **SELF = Same table**
