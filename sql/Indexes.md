# SQL Indexes — Cheat Sheet

## What is an Index?

An **index** is a database data structure that helps the database **find rows faster** without scanning the entire table.

### Simple Definition

> **Index = A shortcut that helps the database find data faster.**

A simple real-life example is a book.

Without an index:

```text
Need topic: "Database"

Open page 1
↓
Check page 2
↓
Check page 3
↓
...
↓
Find the topic
```

With an index:

```text
Database → Page 120
```

You can directly go near the required page.

The same idea is used by database indexes.

---

# Why Do We Need Indexes?

Suppose we have a table containing:

```text
10,000,000 employees
```

Query:

```sql id="8gr1jw"
SELECT *
FROM employees
WHERE email = 'alok@example.com';
```

Without an index, the database may need to check many rows:

```text id="2j4c1q"
Row 1
Row 2
Row 3
Row 4
...
Row 10,000,000
```

This is called a **table scan**.

With an index on `email`:

```sql id="u9wh4h"
CREATE INDEX idx_employees_email
ON employees(email);
```

The database can use the index to locate the matching row much faster.

```text id="0k7z5n"
Query
  ↓
Index
  ↓
Find email
  ↓
Find row
```

---

# Creating an Index

Basic syntax:

```sql id="g6f7di"
CREATE INDEX index_name
ON table_name(column_name);
```

Example:

```sql id="yq0c5s"
CREATE INDEX idx_employee_email
ON employees(email);
```

Now the database has an index on:

```text id="wwj65b"
employees.email
```

---

# Checking an Index

The exact command depends on the database.

For PostgreSQL:

```sql id="7x4kz9"
\d employees
```

For MySQL:

```sql id="0j2a7w"
SHOW INDEX FROM employees;
```

---

# Dropping an Index

If an index is no longer needed:

```sql id="c9m3qk"
DROP INDEX idx_employee_email;
```

In PostgreSQL, you can also use:

```sql id="y6k7wt"
DROP INDEX IF EXISTS idx_employee_email;
```

---

# How Does an Index Work?

A database doesn't usually store an index as a simple list.

Common index structures include:

```text id="xqz8aa"
B-Tree / B+Tree
Hash
Bitmap
Full-Text
```

The exact structures available depend on the database system.

For many SQL databases, **B-tree indexes are the common default**.

---

# B-Tree Index

A B-tree keeps indexed values organized so the database can efficiently search them.

Imagine:

```text id="k2v8qj"
             50
           /    \
         25      75
        /  \    /  \
      10   40  60   90
```

If we search for:

```text id="9u4x8x"
60
```

the database does not need to check every value.

It can follow the appropriate path:

```text id="d0k5n4"
50
 ↓
75
 ↓
60
```

This makes searching much more efficient than checking every row.

---

# When Should You Create an Index?

Indexes are especially useful for columns frequently used in:

### 1. WHERE

```sql id="0ngq1s"
SELECT *
FROM employees
WHERE email = 'alok@example.com';
```

An index on `email` can help.

---

### 2. JOIN

```sql id="d1d4gb"
SELECT *
FROM employees e
JOIN departments d
    ON e.department_id = d.id;
```

Indexes on columns used frequently for joins can improve query performance.

---

### 3. ORDER BY

```sql id="u6d4x0"
SELECT *
FROM employees
ORDER BY salary;
```

An appropriate index can sometimes help with sorting.

---

### 4. GROUP BY

```sql id="q1w0af"
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id;
```

An appropriate index may help some query plans.

---

### 5. UNIQUE Values

For example:

```sql id="j7u1lo"
email
username
employee_id
```

These are commonly good candidates for indexes because they are frequently searched.

---

# Unique Index

A **unique index** ensures that duplicate values are not allowed in the indexed column or combination of columns.

Example:

```sql id="36p5b5"
CREATE UNIQUE INDEX idx_unique_email
ON employees(email);
```

Now:

```text id="x5n9xg"
alok@example.com   → Allowed
rahul@example.com  → Allowed
alok@example.com   → Not allowed
```

### Remember

> **UNIQUE INDEX = Fast lookup + No duplicate values**

A `UNIQUE` constraint also creates a unique index in many database systems, although the exact implementation is database-specific.

---

# Composite Index

A **composite index** is an index created on multiple columns.

Example:

```sql id="3c9r7j"
CREATE INDEX idx_employee_dept_salary
ON employees(department_id, salary);
```

This index contains:

```text
department_id
       +
salary
```

It can be useful for queries such as:

```sql id="5v9t6h"
SELECT *
FROM employees
WHERE department_id = 10
  AND salary > 50000;
```

---

# Column Order Matters in Composite Indexes

Suppose we create:

```sql id="27un08"
CREATE INDEX idx_dept_salary
ON employees(department_id, salary);
```

The order is:

```text
department_id → salary
```

This can efficiently support queries that use:

```text
department_id
```

and queries that use:

```text
department_id + salary
```

But a query using only:

```text
salary
```

may not be able to use this index as effectively.

### Remember

> **In a composite index, column order matters.**

---

# Indexes and NULL

Indexes can generally contain `NULL` values, but the exact behavior depends on the database system.

For example:

```sql id="3y1kni"
CREATE INDEX idx_employee_manager
ON employees(manager_id);
```

You can query:

```sql id="4uf6bd"
SELECT *
FROM employees
WHERE manager_id IS NULL;
```

Whether and how the index is used depends on the database and query plan.

---

# Advantages of Indexes

## 1. Faster SELECT Queries

Indexes can significantly improve searches on large tables.

```text id="xsp2iy"
Without Index
Table Scan
   ↓
Many rows checked

With Index
Index Lookup
   ↓
Relevant rows found faster
```

---

## 2. Faster Searching

Useful for frequently searched columns such as:

```text id="j71btr"
email
username
employee_id
order_id
```

---

## 3. Can Improve Sorting and Joins

Depending on the query and database, indexes can also help with:

```text id="hm1h29"
ORDER BY
JOIN
GROUP BY
```

---

# Disadvantages of Indexes

Indexes are not free.

## 1. Extra Storage

Indexes require additional disk space.

```text id="q3z8k4"
Table Data
+
Indexes
=
More Storage
```

---

## 2. Slower INSERT

When inserting a new row:

```sql id="k1x9r0"
INSERT INTO employees (...);
```

the database may also need to update the relevant indexes.

So:

```text id="x1b4k9"
INSERT
  ↓
Update Table
  +
Update Indexes
```

---

## 3. Slower UPDATE

If an indexed column changes:

```sql id="e9g3q6"
UPDATE employees
SET email = 'new@example.com'
WHERE id = 1;
```

the database may need to update the index as well.

---

## 4. Slower DELETE

When a row is deleted, corresponding index entries must also be removed.

---

# Index Trade-off

This is very important:

```text id="m7e4x2"
More Indexes
     ↓
Faster Reads
     ↓
But
     ↓
More Storage
     +
Slower INSERT / UPDATE / DELETE
```

Therefore:

> **Do not create indexes on every column.**

Create indexes based on actual query patterns and performance needs.

---

# When NOT to Use an Index?

Indexes may provide little benefit when:

### 1. Table is Very Small

If a table has only a few rows, scanning the table may already be very fast.

### 2. Column Has Very Few Distinct Values

For example:

```text
gender → Male / Female
status → Active / Inactive
```

An index may not always provide much benefit, depending on the query and database.

### 3. Column Changes Frequently

Every change may require index maintenance.

### 4. Too Many Indexes Already Exist

Too many indexes increase:

```text
Storage
+
Write Cost
+
Maintenance Cost
```

---

# Primary Key and Index

When you define:

```sql id="c8z3gf"
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

Most database systems automatically create an index or equivalent structure to enforce the primary key's uniqueness.

So you generally don't need to manually create another normal index on the same primary-key column.

---

# Foreign Key and Index

A foreign key does **not universally guarantee** that an index will automatically be created on the referencing column.

For example:

```sql id="x7j4ds"
CREATE TABLE employees (
    id INT PRIMARY KEY,
    department_id INT,
    FOREIGN KEY (department_id)
        REFERENCES departments(id)
);
```

You may choose to create:

```sql id="s0p7sa"
CREATE INDEX idx_employee_department
ON employees(department_id);
```

This can be useful for frequent joins and queries involving `department_id`.

---

# Index vs Primary Key

| Primary Key                                     | Index                                            |
| ----------------------------------------------- | ------------------------------------------------ |
| Identifies a row                                | Helps find rows faster                           |
| Must be unique                                  | Normal index does not have to be unique          |
| Cannot normally be NULL                         | Can generally contain NULL depending on DB/index |
| Usually creates an index automatically          | Created separately                               |
| A table normally has one primary key constraint | A table can have multiple indexes                |

---

# Index vs UNIQUE Index

| Normal Index                | Unique Index                    |
| --------------------------- | ------------------------------- |
| Improves lookup performance | Improves lookup performance     |
| Duplicate values allowed    | Duplicate values not allowed    |
| Used for faster searching   | Used for searching + uniqueness |

Example:

```sql id="r8s1vn"
CREATE INDEX idx_email
ON employees(email);
```

Duplicate emails can exist.

```sql id="m5g4bh"
CREATE UNIQUE INDEX idx_unique_email
ON employees(email);
```

Duplicate emails are not allowed.

---

# How to Know Whether an Index Is Being Used?

Use the database's query execution plan.

In PostgreSQL:

```sql id="l1b5m4"
EXPLAIN
SELECT *
FROM employees
WHERE email = 'alok@example.com';
```

For more detailed execution information:

```sql id="k8u9l2"
EXPLAIN ANALYZE
SELECT *
FROM employees
WHERE email = 'alok@example.com';
```

The execution plan helps you understand whether the database is using:

```text
Index Scan
Index Only Scan
Sequential Scan
```

and other operations.

---

# Important Concept: Index Scan vs Table Scan

### Table / Sequential Scan

```text id="z8x1q2"
Query
 ↓
Check many/all rows
 ↓
Find matching rows
```

### Index Scan

```text id="y4c7w9"
Query
 ↓
Index
 ↓
Locate matching entries
 ↓
Fetch required rows
```

For large tables and selective queries, an index can make a major difference.

But the database optimizer decides whether using the index is actually cheaper.

---

# Example: E-Commerce Database

Suppose we have:

```text id="p6j2md"
orders
----------------
order_id
customer_id
status
order_date
total_amount
```

Common queries:

```sql id="p0n8l5"
SELECT *
FROM orders
WHERE customer_id = 101;
```

Potential useful index:

```sql id="q8x3v1"
CREATE INDEX idx_orders_customer
ON orders(customer_id);
```

Another common query:

```sql id="v2w6r9"
SELECT *
FROM orders
WHERE status = 'SHIPPED'
ORDER BY order_date DESC;
```

A suitable composite index might be considered:

```sql id="h4k9s2"
CREATE INDEX idx_orders_status_date
ON orders(status, order_date);
```

The best index depends on the actual workload and database query planner.

---

# Interview Questions

## What is an index?

> **An index is a database data structure that helps the database locate rows faster without scanning the entire table.**

## Why do we use indexes?

> **We use indexes to improve the performance of read operations such as searching, joining, and sometimes sorting.**

## What is the disadvantage of indexes?

> **Indexes require extra storage and can slow down INSERT, UPDATE, and DELETE operations because indexes also need to be maintained.**

## Can we create multiple indexes on a table?

> **Yes. A table can have multiple indexes, but creating too many indexes can increase storage and write overhead.**

## What is a composite index?

> **A composite index is an index created using two or more columns.**

## Does index always make a query faster?

> **No. The database optimizer decides whether an index is beneficial. For small tables or low-selectivity queries, a sequential scan may be faster.**

---

# Quick Revision

```text
INDEX
  ↓
Shortcut for finding data
  ↓
Faster Reads
```

```text
Advantages:
    Faster SELECT
    Faster lookups
    Can help JOIN / ORDER BY / GROUP BY

Disadvantages:
    Extra storage
    Slower INSERT
    Slower UPDATE
    Slower DELETE
```

```text
CREATE INDEX
     ↓
CREATE UNIQUE INDEX
     ↓
COMPOSITE INDEX
     ↓
EXPLAIN / EXPLAIN ANALYZE
```

# Golden Rule

> **Indexes speed up reads but add cost to writes.**

The goal is not to create **as many indexes as possible**.

The goal is to create the **right indexes for the queries your application actually runs**.
