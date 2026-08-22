# SQL Transactions — Cheat Sheet

## What is a Transaction?

A **transaction** is a group of one or more SQL operations that are treated as **one logical unit of work**.

### Simple Definition

> **Transaction = A group of database operations that either completes successfully or is undone.**

### Real-Life Example

Suppose Alok transfers ₹1,000 from Account A to Account B.

Two operations are required:

```text
1. Remove ₹1,000 from Account A
2. Add ₹1,000 to Account B
```

Both operations should succeed together.

```text
Transaction
    |
    +-- Debit ₹1,000 from A
    |
    +-- Credit ₹1,000 to B
    |
    +-- COMMIT
```

If something goes wrong:

```text
Transaction
    |
    +-- Debit ₹1,000 from A
    |
    +-- Credit fails
    |
    +-- ROLLBACK
```

The database returns to the previous state.

---

# Why Do We Need Transactions?

Without transactions, imagine:

```text
Account A = ₹5000
Account B = ₹3000
```

We transfer ₹1000.

First:

```text
A = ₹4000
```

Then the server crashes before updating B.

Now:

```text
A = ₹4000
B = ₹3000
```

₹1000 has disappeared.

A transaction prevents this kind of incomplete operation.

With a transaction:

```text
Start
  ↓
Debit A
  ↓
Credit B
  ↓
COMMIT
```

If any important operation fails:

```text
ROLLBACK
```

---

# Transaction Commands

The most important transaction commands are:

```text
BEGIN / START TRANSACTION
COMMIT
ROLLBACK
SAVEPOINT
```

---

# 1. BEGIN

### Definition

**`BEGIN` starts a transaction.**

PostgreSQL:

```sql
BEGIN;
```

MySQL:

```sql
START TRANSACTION;
```

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;
```

At this point, the transaction has not been permanently committed yet.

---

# 2. COMMIT

### Definition

**`COMMIT` permanently saves the changes made by the transaction.**

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

After:

```text
COMMIT
   ↓
Changes are saved
```

### Remember

> **COMMIT = Save the transaction**

---

# 3. ROLLBACK

### Definition

**`ROLLBACK` cancels the changes made during the current transaction that have not been committed.**

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

ROLLBACK;
```

Result:

```text
All changes made after BEGIN
        ↓
     Undone
```

### Remember

> **ROLLBACK = Undo the transaction**

---

# COMMIT vs ROLLBACK

| Command    | Purpose                  |
| ---------- | ------------------------ |
| `COMMIT`   | Save changes permanently |
| `ROLLBACK` | Undo uncommitted changes |

Easy memory:

```text
COMMIT   → Keep changes
ROLLBACK → Cancel changes
```

---

# 4. SAVEPOINT

### Definition

A **SAVEPOINT** creates a point inside a transaction to which you can roll back without cancelling the entire transaction.

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

SAVEPOINT after_debit;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

ROLLBACK TO after_debit;

COMMIT;
```

Here:

```text
BEGIN
  ↓
Debit Account A
  ↓
SAVEPOINT
  ↓
Credit Account B
  ↓
ROLLBACK TO SAVEPOINT
  ↓
Go back to SAVEPOINT
  ↓
COMMIT
```

The entire transaction is not cancelled.

### Remember

> **SAVEPOINT = Create a checkpoint inside a transaction**

---

# Transaction Flow

The basic transaction flow is:

```text
BEGIN
  ↓
SQL Operations
  ↓
Everything successful?
  ↓
 ┌───────────────┐
 │               │
YES             NO
 │               │
 ↓               ↓
COMMIT        ROLLBACK
 │               │
 ↓               ↓
Save           Undo
```

---

# Example: Bank Transfer

Suppose:

```text
Account A = ₹5000
Account B = ₹3000
```

We want to transfer ₹1000.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 'A';

UPDATE accounts
SET balance = balance + 1000
WHERE id = 'B';

COMMIT;
```

Final result:

```text
A = ₹4000
B = ₹4000
```

Both operations are part of the same transaction.

---

# What Happens If Something Fails?

Suppose the second operation fails:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 'A';

UPDATE accounts
SET balance = balance + 1000
WHERE id = 'INVALID';

ROLLBACK;
```

The first update is also undone.

Final state:

```text
A = ₹5000
B = ₹3000
```

The transaction follows:

```text
All operations succeed
        ↓
     COMMIT

Something fails
        ↓
    ROLLBACK
```

---

# Transactions and ACID

Transactions are closely related to the **ACID properties**.

```text
A → Atomicity
C → Consistency
I → Isolation
D → Durability
```

## Atomicity

> **All operations happen or none happen.**

Example:

```text
Debit + Credit
     ↓
Both succeed
OR
Both are undone
```

## Consistency

> **The transaction keeps the database in a valid state.**

Example:

```text
Balance should not violate database rules.
```

## Isolation

> **Concurrent transactions should not incorrectly interfere with each other.**

Example:

```text
Transaction A
      +
Transaction B
      ↓
Database controls their interaction
```

## Durability

> **After COMMIT, the changes remain saved even if the system crashes.**

---

# Transaction Example with Orders

Suppose a customer places an order.

We need to:

```text
1. Create order
2. Reduce product stock
3. Create payment record
```

These operations should be treated as one transaction.

```sql
BEGIN;

INSERT INTO orders (...);

UPDATE products
SET stock = stock - 1
WHERE id = 101;

INSERT INTO payments (...);

COMMIT;
```

If payment creation fails:

```sql
ROLLBACK;
```

Then the previous changes are undone as well.

This prevents situations like:

```text
Order created
Stock reduced
Payment failed
```

without proper recovery.

---

# Transaction Isolation Levels

When multiple transactions run at the same time, databases need rules about what one transaction can see from another.

Common SQL isolation levels are:

```text
READ UNCOMMITTED
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```

The exact behavior and implementation can vary by database system.

---

# 1. READ UNCOMMITTED

### Simple Definition

A transaction may be allowed to read changes that another transaction has not committed yet.

This can lead to a:

> **Dirty Read**

Example:

```text
Transaction A:
Updates salary to ₹80,000
        ↓
Not committed yet

Transaction B:
Reads ₹80,000
        ↓
Transaction A rolls back
```

Transaction B read data that never actually became permanent.

---

# 2. READ COMMITTED

### Simple Definition

A transaction only reads data that has been committed.

This prevents dirty reads.

Example:

```text
Transaction A → Update
Transaction A → Not committed

Transaction B → Cannot see that uncommitted change
```

This is a common default isolation level in systems such as PostgreSQL.

---

# 3. REPEATABLE READ

### Simple Definition

If a transaction reads a row, repeated reads within the same transaction are designed to give a consistent result under the database's isolation semantics.

This provides stronger consistency than `READ COMMITTED`.

---

# 4. SERIALIZABLE

### Simple Definition

**The strongest standard isolation level.**

It makes concurrent transactions behave as if they were executed one after another, according to the database's serializable execution rules.

This provides strong consistency but can reduce concurrency and may cause transactions to be retried or aborted.

---

# Isolation Levels Quick Comparison

| Isolation Level      | General Idea                                              |
| -------------------- | --------------------------------------------------------- |
| **READ UNCOMMITTED** | May allow uncommitted reads                               |
| **READ COMMITTED**   | Reads committed data                                      |
| **REPEATABLE READ**  | Provides a stable view for repeated reads under its rules |
| **SERIALIZABLE**     | Strongest; behaves like serial execution                  |

---

# Common Transaction Problems

When multiple transactions run simultaneously, some problems can occur depending on the isolation level.

## 1. Dirty Read

Reading uncommitted data from another transaction.

```text
A writes
 ↓
Not committed
 ↓
B reads it
 ↓
A rolls back
```

B read data that never became permanent.

---

## 2. Non-Repeatable Read

A transaction reads the same row twice and gets different values because another committed transaction changed it between the reads.

```text
Transaction A:
Read salary → ₹50,000

Transaction B:
Update salary → ₹60,000
COMMIT

Transaction A:
Read salary again → ₹60,000
```

---

## 3. Phantom Read

A transaction executes the same query twice and gets a different set of rows because another transaction inserted or deleted matching rows.

Example:

```text
First query:
WHERE salary > 50000
→ 5 employees

Another transaction inserts a matching employee.

Second query:
WHERE salary > 50000
→ 6 employees
```

The extra row is a "phantom".

---

# Transaction vs Query

Do not confuse a query with a transaction.

### Query

A single SQL operation.

```sql
SELECT *
FROM employees;
```

### Transaction

A logical unit that can contain multiple SQL operations.

```sql
BEGIN;

UPDATE accounts ...;

UPDATE accounts ...;

COMMIT;
```

Simple difference:

```text
Query       → One database operation

Transaction → One logical unit containing
              one or more operations
```

---

# Transaction vs ACID

| Transaction                        | ACID                                          |
| ---------------------------------- | --------------------------------------------- |
| A group of database operations     | Properties that make transactions reliable    |
| Uses `BEGIN`, `COMMIT`, `ROLLBACK` | Atomicity, Consistency, Isolation, Durability |
| Defines a unit of work             | Defines guarantees for that work              |

Easy way to remember:

```text
Transaction
    ↓
Unit of Work

ACID
    ↓
Rules that make the Unit of Work reliable
```

---

# Important SQL Syntax

### Start

```sql
BEGIN;
```

### Save

```sql
COMMIT;
```

### Undo

```sql
ROLLBACK;
```

### Create checkpoint

```sql
SAVEPOINT point1;
```

### Roll back to checkpoint

```sql
ROLLBACK TO SAVEPOINT point1;
```

---

# Interview Questions

## What is a transaction?

> **A transaction is a logical unit of work consisting of one or more database operations that should be completed reliably as a single unit.**

## What is COMMIT?

> **COMMIT permanently saves the changes made by the transaction.**

## What is ROLLBACK?

> **ROLLBACK undoes the uncommitted changes made by the current transaction.**

## What is a SAVEPOINT?

> **A SAVEPOINT creates a checkpoint inside a transaction so we can roll back to that point without cancelling the entire transaction.**

## Why are transactions important?

> **Transactions ensure that related database operations are handled as one reliable unit, preventing incomplete or inconsistent updates.**

## What are isolation levels?

> **Isolation levels define how much one concurrent transaction can see or be affected by changes made by other transactions.**

---

# Quick Revision

```text
TRANSACTION
     ↓
  BEGIN
     ↓
SQL Operations
     ↓
 ┌───────────────┐
 ↓               ↓
Success         Failure
 ↓               ↓
COMMIT        ROLLBACK
 ↓               ↓
Save            Undo
```

```text
BEGIN     → Start transaction

COMMIT    → Save changes

ROLLBACK  → Undo uncommitted changes

SAVEPOINT → Create checkpoint
```

# Golden Rule

> **A transaction is a unit of work. COMMIT saves it, ROLLBACK undoes it, and ACID properties make it reliable.**
