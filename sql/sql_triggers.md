# SQL Triggers — Cheat Sheet

## What is a Trigger?

A **trigger** is a database mechanism that automatically executes a predefined function or action when a specific event happens on a table or other database object.

### Simple Definition

> **Trigger = Automatically run some database logic when a specific event occurs.**

You don't normally call a trigger manually.

For example:

```text
INSERT employee
      ↓
Trigger automatically runs
      ↓
Create audit record
```

---

# Why Do We Use Triggers?

Triggers are useful when we want the database to automatically perform an action whenever data changes.

Common use cases:

```text
Audit logging
Automatic timestamps
Data validation
Maintaining related data
Tracking changes
Enforcing certain business rules
```

---

# Basic Trigger Flow

```text
User/Application
      ↓
INSERT / UPDATE / DELETE
      ↓
Trigger fires
      ↓
Trigger Function / Logic
      ↓
Database performs automatic action
```

---

# Trigger Events

The most common trigger events are:

```text
INSERT
UPDATE
DELETE
```

---

# 1. INSERT Trigger

An **INSERT trigger** runs when a new row is inserted.

Example:

```sql
INSERT INTO employees (name, salary)
VALUES ('Alok', 50000);
```

A trigger could automatically create an audit record:

```text
Employee Added
     ↓
Audit Log Created
```

### Simple Definition

> **INSERT Trigger = Automatically run when a new row is added.**

---

# 2. UPDATE Trigger

An **UPDATE trigger** runs when an existing row is modified.

Example:

```sql
UPDATE employees
SET salary = 60000
WHERE id = 1;
```

A trigger could record:

```text
Old Salary → 50000
New Salary → 60000
```

### Simple Definition

> **UPDATE Trigger = Automatically run when existing data changes.**

---

# 3. DELETE Trigger

A **DELETE trigger** runs when a row is deleted.

Example:

```sql
DELETE FROM employees
WHERE id = 1;
```

A trigger could save the deleted employee's information into an audit table.

### Simple Definition

> **DELETE Trigger = Automatically run when a row is deleted.**

---

# BEFORE vs AFTER Trigger

Triggers can also be categorized by **when** they execute.

```text
BEFORE
AFTER
```

---

# 4. BEFORE Trigger

### Definition

A **BEFORE trigger runs before the database operation is completed.**

Example:

```text
INSERT
  ↓
BEFORE Trigger
  ↓
Insert Row
```

### Use Cases

Common uses include:

```text
Validate data
Modify values before saving
Set default values
Perform checks
```

Example idea:

```text
Salary = -5000
      ↓
BEFORE INSERT Trigger
      ↓
Reject or modify invalid value
```

---

# 5. AFTER Trigger

### Definition

An **AFTER trigger runs after the database operation has occurred.**

Example:

```text
INSERT
  ↓
Row inserted
  ↓
AFTER Trigger
  ↓
Create audit log
```

### Use Cases

Common examples:

```text
Audit logging
Sending database-side notifications
Updating related information
Tracking changes
```

---

# BEFORE vs AFTER

| BEFORE                                   | AFTER                                |
| ---------------------------------------- | ------------------------------------ |
| Runs before the operation                | Runs after the operation             |
| Useful for validation/modification       | Useful for logging/follow-up actions |
| Can affect values before they are stored | Sees the operation after it occurs   |

The exact behavior and available trigger timing options depend on the database.

---

# Row-Level vs Statement-Level Triggers

Another important concept is **how many times the trigger executes**.

---

# 6. Row-Level Trigger

A **row-level trigger runs once for each affected row.**

Suppose:

```sql
UPDATE employees
SET salary = salary + 5000
WHERE department_id = 10;
```

Suppose 100 employees match.

A row-level trigger runs:

```text
100 affected rows
      ↓
100 trigger executions
```

### Simple Definition

> **Row-Level Trigger = Runs once for every affected row.**

---

# 7. Statement-Level Trigger

A **statement-level trigger runs once for the entire SQL statement**, regardless of how many rows are affected.

Suppose:

```sql
UPDATE employees
SET salary = salary + 5000
WHERE department_id = 10;
```

100 rows are affected.

A statement-level trigger runs:

```text
1 SQL statement
      ↓
1 trigger execution
```

### Simple Definition

> **Statement-Level Trigger = Runs once for the whole SQL statement.**

Not every database supports both styles in exactly the same way.

---

# Row-Level vs Statement-Level

| Row-Level                                    | Statement-Level                  |
| -------------------------------------------- | -------------------------------- |
| Executes for each affected row               | Executes once per SQL statement  |
| 100 rows → 100 executions                    | 100 rows → 1 execution           |
| Useful when logic depends on individual rows | Useful for statement-level logic |

---

# NEW and OLD Values

When working with row-level triggers, many database systems provide access to values related to the row being changed.

Common concepts:

```text
OLD → Value before the change
NEW → Value after / proposed change
```

### INSERT

Usually:

```text
NEW → Available
OLD → Not available
```

### UPDATE

Usually:

```text
OLD → Previous value
NEW → New value
```

### DELETE

Usually:

```text
OLD → Available
NEW → Not available
```

Easy memory:

```text
INSERT → NEW
UPDATE → OLD + NEW
DELETE → OLD
```

The exact syntax depends on the database.

---

# PostgreSQL Trigger Example

In PostgreSQL, a trigger usually works with a **trigger function**.

Suppose we have:

### employees

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    salary NUMERIC
);
```

And an audit table:

```sql
CREATE TABLE employee_audit (
    employee_id INT,
    old_salary NUMERIC,
    new_salary NUMERIC,
    changed_at TIMESTAMP
);
```

---

# Step 1: Create Trigger Function

```sql
CREATE OR REPLACE FUNCTION log_salary_change()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO employee_audit (
        employee_id,
        old_salary,
        new_salary,
        changed_at
    )
    VALUES (
        OLD.id,
        OLD.salary,
        NEW.salary,
        CURRENT_TIMESTAMP
    );

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

This function records the old and new salary.

---

# Step 2: Create Trigger

```sql
CREATE TRIGGER salary_change_trigger
AFTER UPDATE OF salary
ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

Now whenever salary changes:

```text
UPDATE employees
      ↓
Trigger fires
      ↓
log_salary_change()
      ↓
Audit record created
```

---

# Example

Run:

```sql
UPDATE employees
SET salary = 60000
WHERE id = 1;
```

Suppose old salary was:

```text
50000
```

The audit table may contain:

| employee_id | old_salary | new_salary |
| ----------: | ---------: | ---------: |
|           1 |      50000 |      60000 |

The application did not have to explicitly insert the audit record.

The trigger handled it automatically.

---

# Trigger Example with INSERT

Suppose we want to automatically record when a new employee is created.

Conceptually:

```text
INSERT employee
      ↓
AFTER INSERT trigger
      ↓
Create audit record
```

PostgreSQL trigger function:

```sql
CREATE OR REPLACE FUNCTION log_employee_insert()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO employee_audit (
        employee_id,
        changed_at
    )
    VALUES (
        NEW.id,
        CURRENT_TIMESTAMP
    );

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Trigger:

```sql
CREATE TRIGGER employee_insert_trigger
AFTER INSERT
ON employees
FOR EACH ROW
EXECUTE FUNCTION log_employee_insert();
```

---

# Dropping a Trigger

If the trigger is no longer required:

```sql
DROP TRIGGER salary_change_trigger
ON employees;
```

For PostgreSQL:

```sql
DROP TRIGGER IF EXISTS salary_change_trigger
ON employees;
```

---

# Trigger vs Stored Procedure

These are commonly confused.

## Trigger

Runs **automatically** when a specified event occurs.

```text
INSERT / UPDATE / DELETE
        ↓
Trigger automatically fires
```

## Stored Procedure

Usually runs when explicitly called by an application or SQL statement.

```text
CALL procedure_name();
```

### Easy Difference

> **Trigger = Automatic**

> **Procedure = Explicitly called**

---

# Trigger vs Function

A normal function:

```text
Function
   ↓
Called explicitly
```

A trigger function:

```text
Database Event
      ↓
Trigger
      ↓
Trigger Function
```

In PostgreSQL, the trigger itself is defined separately from the function that contains the trigger logic.

---

# Advantages of Triggers

## 1. Automatic Execution

No application code is required to explicitly perform the trigger action.

```text
INSERT
  ↓
Automatic action
```

---

## 2. Useful for Audit Logs

For example:

```text
Old value
New value
User/action
Timestamp
```

can be recorded automatically.

---

## 3. Centralized Database Logic

The rule exists in the database instead of being duplicated across multiple applications.

---

## 4. Useful for Automatic Data Maintenance

For example:

```text
Employee updated
      ↓
Update audit record
```

---

# Disadvantages of Triggers

## 1. Hidden Logic

A developer may run:

```sql
UPDATE employees
SET salary = 60000;
```

and not realize that several triggers execute additional operations.

This can make debugging harder.

---

## 2. Performance Overhead

Triggers execute automatically and may add extra database work.

For example:

```text
UPDATE 1,000,000 rows
        ↓
Row-level trigger
        ↓
1,000,000 trigger executions
```

This can be expensive.

---

## 3. Complex Debugging

The actual behavior may be spread across:

```text
Application Code
+
Triggers
+
Functions
+
Constraints
```

Making the overall flow harder to understand.

---

## 4. Cascading Effects

One trigger may modify another table, which may cause another trigger to execute.

```text
UPDATE A
   ↓
Trigger A
   ↓
UPDATE B
   ↓
Trigger B
   ↓
UPDATE C
```

This can become difficult to maintain if overused.

---

# When Should We Use Triggers?

Good use cases include:

```text
Audit logging
Change tracking
Automatic timestamps
Database-level validation
Maintaining derived or related data
```

But don't use triggers for every business rule.

Sometimes application/service-layer logic is easier to understand and maintain.

---

# Trigger Execution Flow

A useful mental model:

```text
SQL Operation
     ↓
BEFORE Trigger
     ↓
Database Operation
     ↓
AFTER Trigger
     ↓
Transaction Continues
     ↓
COMMIT / ROLLBACK
```

The exact execution order depends on the database and trigger definitions.

---

# Trigger and Transaction

Triggers normally execute as part of the transaction that caused them.

Example:

```sql
BEGIN;

UPDATE employees
SET salary = 60000
WHERE id = 1;

COMMIT;
```

If the trigger performs an audit insert:

```text
UPDATE
  ↓
Trigger
  ↓
Audit INSERT
  ↓
COMMIT
```

If the transaction is rolled back, the trigger's transactional changes are generally rolled back as well.

This makes triggers useful for maintaining transactional audit information.

---

# Real-Life Example

Consider an e-commerce application.

When an order is created:

```text
New Order
   ↓
INSERT INTO orders
   ↓
Trigger
   ↓
Create audit record
```

When the order status changes:

```text
Pending
   ↓
Shipped
   ↓
Trigger
   ↓
Store old status + new status
```

Audit table:

| order_id | old_status | new_status | changed_at |
| -------: | ---------- | ---------- | ---------- |
|      101 | Pending    | Shipped    | 2026-08-23 |

The application does not need to manually maintain every audit entry.

---

# Trigger Types Quick Revision

```text
EVENT
│
├── INSERT
├── UPDATE
└── DELETE
```

```text
TIMING
│
├── BEFORE
└── AFTER
```

```text
LEVEL
│
├── ROW-LEVEL
└── STATEMENT-LEVEL
```

---

# Most Important Combination

A trigger can be described using all three concepts:

```text
AFTER UPDATE
FOR EACH ROW
```

Meaning:

```text
AFTER
  ↓
Run after the update

UPDATE
  ↓
Trigger event

FOR EACH ROW
  ↓
Run once for every affected row
```

Example:

```sql
CREATE TRIGGER salary_change_trigger
AFTER UPDATE OF salary
ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

---

# Interview Questions

## What is a trigger?

> **A trigger is a database mechanism that automatically executes predefined logic when a specified database event occurs.**

## What events can fire a trigger?

> **Common events are INSERT, UPDATE, and DELETE.**

## What is a BEFORE trigger?

> **A BEFORE trigger executes before the database operation is completed and is commonly used for validation or modifying values.**

## What is an AFTER trigger?

> **An AFTER trigger executes after the database operation and is commonly used for audit logging or follow-up actions.**

## What is a row-level trigger?

> **A row-level trigger executes once for each affected row.**

## What is a statement-level trigger?

> **A statement-level trigger executes once for the entire SQL statement.**

## What are OLD and NEW?

> **OLD represents the previous row values, while NEW represents the new or proposed row values, where applicable.**

## What is a common use case of triggers?

> **Audit logging is one of the most common use cases because the database can automatically record changes whenever data is modified.**

---

# Quick Revision

```text
TRIGGER
   ↓
Automatic Database Logic
   ↓
INSERT / UPDATE / DELETE
```

```text
BEFORE
→ Before operation

AFTER
→ After operation
```

```text
ROW-LEVEL
→ Once per affected row

STATEMENT-LEVEL
→ Once per SQL statement
```

```text
OLD
→ Previous value

NEW
→ New / proposed value
```

```text
TRIGGER
→ Automatic

PROCEDURE
→ Explicitly called
```

# Golden Rule

> **A trigger is automatic database logic that fires when a specified event occurs. Use triggers carefully because they are powerful but can make database behavior harder to understand and debug.**
