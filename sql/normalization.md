# Database Normalization — Cheat Sheet

## What is Normalization?

**Normalization is the process of organizing data in a database to reduce duplicate data and prevent data-related problems.**

### Simple Definition

> **Normalization = Organize tables properly so data is not unnecessarily repeated.**

The main goals are:

```text
Reduce Data Duplication
        ↓
Avoid Data Anomalies
        ↓
Keep Data Consistent
```

---

# Why Do We Need Normalization?

Suppose we have one table:

### `student_course`

| student_id | student_name | course | teacher | teacher_phone |
| ---------: | ------------ | ------ | ------- | ------------- |
|          1 | Alok         | SQL    | Rahul   | 9876543210    |
|          1 | Alok         | Python | Priya   | 9876543220    |
|          2 | Aman         | SQL    | Rahul   | 9876543210    |

Notice that Rahul's information is repeated.

```text
Rahul
9876543210
```

appears multiple times.

This creates problems when data changes.

---

# Problems Without Normalization

These problems are called **data anomalies**.

## 1. Update Anomaly

Suppose Rahul changes his phone number.

If his phone number is stored in 100 rows, we need to update it in 100 places.

If we update only 99 rows:

```text
Some rows → Old number
Some rows → New number
```

The database becomes inconsistent.

### Remember

> **Update Anomaly = Same data must be updated in multiple places.**

---

# 2. Insert Anomaly

Suppose we want to add a new teacher:

```text
Teacher = Amit
Phone = 9999999999
```

But the table requires a student/course to exist.

We cannot properly add the teacher without adding unrelated information.

### Remember

> **Insert Anomaly = Cannot add data without unrelated data.**

---

# 3. Delete Anomaly

Suppose Aman is the only student taking SQL.

If we delete Aman:

```text
Aman → SQL
```

we might accidentally lose information about:

```text
SQL → Rahul → 9876543210
```

The teacher/course information is lost along with the student record.

### Remember

> **Delete Anomaly = Deleting one thing accidentally removes useful information.**

---

# Normal Forms

Normalization is usually explained using:

```text
1NF
 ↓
2NF
 ↓
3NF
 ↓
BCNF
```

For most SQL interviews, **1NF, 2NF, and 3NF** are the most important.

---

# 1NF — First Normal Form

## Definition

A table is in **1NF** when:

1. Each column contains atomic values.
2. There are no repeating groups.
3. Each row can be uniquely identified.

### What does Atomic Mean?

Atomic means:

> **One cell should contain one value.**

### Not in 1NF

| student_id | name | phone_numbers |
| ---------: | ---- | ------------- |
|          1 | Alok | 9876, 8765    |

One cell contains multiple phone numbers.

### In 1NF

| student_id | name | phone |
| ---------: | ---- | ----- |
|          1 | Alok | 9876  |
|          1 | Alok | 8765  |

Each cell now contains one value.

### Remember

> **1NF = One cell, one value.**

---

# 2NF — Second Normal Form

## Definition

A table is in **2NF** when:

1. It is already in 1NF.
2. There is no **partial dependency**.

### What is Partial Dependency?

Partial dependency occurs when a non-key column depends on **only part of a composite primary key**.

Consider:

### `student_course`

| student_id | course_id | student_name | course_name |
| ---------: | --------: | ------------ | ----------- |
|          1 |       101 | Alok         | SQL         |
|          1 |       102 | Alok         | Python      |
|          2 |       101 | Aman         | SQL         |

Suppose the primary key is:

```text id="n6wxet"
(student_id, course_id)
```

Now:

```text id="i9j1mm"
student_id → student_name
course_id  → course_name
```

But the complete primary key is:

```text id="u2i8bm"
(student_id, course_id)
```

So:

```text id="h3cg7v"
student_name depends only on student_id
course_name depends only on course_id
```

They do not depend on the complete composite key.

This is **partial dependency**.

---

## Fixing 2NF

Split the table.

### `students`

| student_id | student_name |
| ---------: | ------------ |
|          1 | Alok         |
|          2 | Aman         |

### `courses`

| course_id | course_name |
| --------: | ----------- |
|       101 | SQL         |
|       102 | Python      |

### `student_courses`

| student_id | course_id |
| ---------: | --------: |
|          1 |       101 |
|          1 |       102 |
|          2 |       101 |

Now the dependencies are properly separated.

### Remember

> **2NF = 1NF + No Partial Dependency**

---

# 3NF — Third Normal Form

## Definition

A table is in **3NF** when:

1. It is already in 2NF.
2. There is no **transitive dependency**.

### What is Transitive Dependency?

A non-key column should not depend on another non-key column.

Consider:

### `employees`

| employee_id | employee_name | department_id | department_name |
| ----------: | ------------- | ------------: | --------------- |
|           1 | Alok          |            10 | IT              |
|           2 | Rahul         |            20 | HR              |
|           3 | Priya         |            10 | IT              |

Dependencies:

```text id="4b2pdb"
employee_id → department_id

department_id → department_name
```

Therefore:

```text id="l0zzh5"
employee_id
     ↓
department_id
     ↓
department_name
```

`department_name` indirectly depends on `employee_id`.

This is a **transitive dependency**.

---

# Fixing 3NF

Split the table.

### `employees`

| employee_id | employee_name | department_id |
| ----------: | ------------- | ------------: |
|           1 | Alok          |            10 |
|           2 | Rahul         |            20 |
|           3 | Priya         |            10 |

### `departments`

| department_id | department_name |
| ------------: | --------------- |
|            10 | IT              |
|            20 | HR              |

Now:

```text id="13x5g4"
employees
    |
    | department_id
    ↓
departments
```

### Remember

> **3NF = 2NF + No Transitive Dependency**

---

# 1NF vs 2NF vs 3NF

| Normal Form | Main Rule                         |
| ----------- | --------------------------------- |
| **1NF**     | One cell should contain one value |
| **2NF**     | No partial dependency             |
| **3NF**     | No transitive dependency          |

Easy memory:

```text id="6x3q1d"
1NF → Atomic values

2NF → No partial dependency

3NF → No transitive dependency
```

---

# BCNF — Boyce-Codd Normal Form

BCNF is a stronger version of 3NF.

### Simple Definition

> **Every determinant must be a candidate key.**

In simple terms:

> **If column A determines column B, then A should be a candidate key.**

For most beginner/intermediate SQL interviews, understanding **1NF, 2NF, and 3NF** is usually more important.

---

# Normalization Example From Start to Finish

Suppose we start with:

### Unnormalized Table

| student_id | student_name | courses     | teacher      |
| ---------: | ------------ | ----------- | ------------ |
|          1 | Alok         | SQL, Python | Rahul, Priya |

Problems:

```text
Multiple values in one cell
Data repeated
Difficult to update
Difficult to query
```

---

## Step 1 — 1NF

Make values atomic:

### `student_course`

| student_id | student_name | course | teacher |
| ---------: | ------------ | ------ | ------- |
|          1 | Alok         | SQL    | Rahul   |
|          1 | Alok         | Python | Priya   |

Now each cell contains a single value.

---

## Step 2 — 2NF

Separate data that depends on different parts of the key.

### `students`

| student_id | student_name |
| ---------: | ------------ |
|          1 | Alok         |

### `courses`

| course_id | course_name | teacher_id |
| --------: | ----------- | ---------: |
|       101 | SQL         |          1 |
|       102 | Python      |          2 |

### `student_courses`

| student_id | course_id |
| ---------: | --------: |
|          1 |       101 |
|          1 |       102 |

---

## Step 3 — 3NF

Separate teacher information.

### `teachers`

| teacher_id | teacher_name |
| ---------: | ------------ |
|          1 | Rahul        |
|          2 | Priya        |

Now the data is much more organized.

---

# Normalization vs Denormalization

## Normalization

Data is split into multiple related tables.

```text id="8t0qmc"
Students
   |
   +---- Student_Courses
              |
              +---- Courses
```

### Advantages

* Less duplicate data
* Better consistency
* Easier updates
* Better data integrity

### Disadvantage

More tables can mean more `JOIN`s.

---

# Denormalization

**Denormalization intentionally adds some duplicate data to improve read performance or simplify queries.**

Example:

Instead of joining:

```text id="87d5hx"
orders
   +
customers
```

we may store:

```text id="7epn0w"
order_id
customer_id
customer_name
customer_phone
```

inside the order-related data.

### Advantages

* Fewer JOINs
* Faster reads in some workloads
* Useful for reporting and analytics

### Disadvantages

* More duplicate data
* More storage
* Updates can become harder
* Greater risk of inconsistent data

---

# Normalization vs Denormalization

| Normalization                   | Denormalization                        |
| ------------------------------- | -------------------------------------- |
| Reduces duplication             | Allows some duplication                |
| More tables                     | Fewer tables in some designs           |
| More JOINs may be needed        | Fewer JOINs may be needed              |
| Better data consistency         | Can improve read performance           |
| Common in transactional systems | Common in analytics/read-heavy systems |

---

# Interview Questions

## What is normalization?

> **Normalization is the process of organizing database tables to reduce data redundancy and prevent insert, update, and delete anomalies.**

## What is 1NF?

> **1NF requires atomic values and no repeating groups.**

## What is 2NF?

> **2NF means the table is in 1NF and every non-key attribute depends on the entire primary key, not just part of it.**

## What is 3NF?

> **3NF means the table is in 2NF and non-key attributes do not depend on other non-key attributes.**

## What is partial dependency?

> **Partial dependency occurs when a non-key attribute depends on only part of a composite primary key.**

## What is transitive dependency?

> **Transitive dependency occurs when a non-key attribute depends on another non-key attribute.**

## Why do we normalize databases?

> **To reduce data duplication, improve consistency, and prevent insert, update, and delete anomalies.**

---

# Quick Revision

```text
Normalization
      |
      ↓
Reduce Duplicate Data
      |
      ↓
Prevent Anomalies
      |
      ↓
Better Data Organization
```

```text
1NF
↓
Atomic values
One value per cell

2NF
↓
1NF + No Partial Dependency

3NF
↓
2NF + No Transitive Dependency

BCNF
↓
Every determinant is a Candidate Key
```

# Golden Rule

> **1NF → Atomic values**
>
> **2NF → Depend on the whole key**
>
> **3NF → Depend only on the key**
>
> **BCNF → Every determinant is a candidate key**
