# Database Isolation Levels — Cheat Sheet

## What are Isolation Levels?

**Isolation levels define how much one transaction can see the changes made by other transactions running at the same time.**

### Simple Definition

> **Isolation Level = Rules that control how concurrent transactions interact with each other.**

Suppose two transactions are running:

```text id="8f5v1k"
Transaction A
      +
Transaction B
      ↓
Same Database
```

The isolation level decides things like:

```text id="4h6b3p"
Can A see B's changes?

Can B see A's changes?

Can the same query return different results?

Can new rows appear during a transaction?
```

---

# Why Do We Need Isolation Levels?

Suppose a bank account has:

```text id="8s8l1k"
Balance = ₹10,000
```

Two transactions are running at the same time.

```text id="h1j8s3"
Transaction A → Withdraw ₹2,000

Transaction B → Check balance
```

If transactions are not properly isolated, one transaction could see data in an unexpected state.

Isolation levels allow the database to choose a balance between:

```text id="1t9v0w"
Data Consistency
      ↕
Concurrency / Performance
```

---

# The Four Standard Isolation Levels

The SQL standard defines four commonly discussed isolation levels:

```text id="9r7y7w"
1. READ UNCOMMITTED
2. READ COMMITTED
3. REPEATABLE READ
4. SERIALIZABLE
```

From weaker to stronger isolation:

```text id="5x8z5q"
READ UNCOMMITTED
        ↓
READ COMMITTED
        ↓
REPEATABLE READ
        ↓
SERIALIZABLE
```

Generally:

```text id="k9q5u0"
Higher Isolation
      ↓
Stronger Consistency
      ↓
Less Concurrency / More Overhead
```

The exact behavior can vary by database system, especially because some databases use **MVCC** rather than relying only on traditional locking.

---

# Problems Isolation Levels Try to Control

Before understanding the levels, understand these three common concurrency problems.

---

# 1. Dirty Read

### Definition

A **dirty read** happens when one transaction reads data that another transaction has changed but not committed.

Example:

```text id="u8n3b5"
Transaction A
     ↓
Balance = ₹10,000
     ↓
Changes balance to ₹5,000
     ↓
Not committed
```

Transaction B reads:

```text id="q9w4y6"
₹5,000
```

Then A rolls back:

```text id="6f3r1k"
ROLLBACK
```

Actual balance becomes:

```text id="6b7k2a"
₹10,000
```

B read data that was never committed.

That is a:

> **Dirty Read**

---

# 2. Non-Repeatable Read

### Definition

A **non-repeatable read** happens when the same transaction reads the same row twice and gets different values because another transaction committed a change between the reads.

Example:

```text id="4o2l6v"
Transaction A:
Read balance → ₹10,000
```

Then Transaction B changes it:

```text id="g7m5q1"
Transaction B:
Balance → ₹8,000
COMMIT
```

Transaction A reads again:

```text id="5h4k3n"
Read balance → ₹8,000
```

Same transaction, same row, different value.

That is a:

> **Non-Repeatable Read**

---

# 3. Phantom Read

### Definition

A **phantom read** happens when the same query returns a different set of rows because another transaction inserted, deleted, or changed rows that match the query.

Example:

Transaction A:

```sql id="7h6q1m"
SELECT *
FROM employees
WHERE salary > 50000;
```

Result:

```text id="l8s2v4"
5 employees
```

Transaction B inserts another employee:

```text id="b4j9c6"
salary = 60000
```

and commits.

Transaction A runs the same query again:

```sql id="4z7k3q"
SELECT *
FROM employees
WHERE salary > 50000;
```

Result:

```text id="m5p8x2"
6 employees
```

The new row is a:

> **Phantom Row**

---

# Isolation Level 1: READ UNCOMMITTED

## Definition

**READ UNCOMMITTED allows a transaction to potentially read changes made by other transactions before those changes are committed.**

### Simple Definition

> **READ UNCOMMITTED = You may see uncommitted data.**

Example:

```text id="4n7q2w"
Transaction A
    ↓
Update balance
    ↓
Not committed

Transaction B
    ↓
Can potentially read that change
```

If A rolls back:

```text id="x2f8m7"
B read data
that never became permanent
```

This allows:

```text id="n4w8k1"
Dirty Reads
```

### Use

Rarely appropriate for correctness-sensitive applications.

---

# Isolation Level 2: READ COMMITTED

## Definition

**READ COMMITTED allows a transaction to see only data committed by other transactions.**

### Simple Definition

> **READ COMMITTED = Only read committed data.**

Example:

```text id="7m3p9q"
Transaction A
    ↓
UPDATE
    ↓
Not committed

Transaction B
    ↓
Cannot see A's uncommitted change
```

After A commits:

```text id="v6k1s8"
COMMIT
  ↓
B can see the committed change
```

### Prevents

```text id="f8q2l5"
Dirty Reads
```

### But Non-Repeatable Reads?

They can still happen.

Example:

```text id="0j5k7p"
A → Read ₹10,000

B → Update ₹8,000
     COMMIT

A → Read again
     ₹8,000
```

---

# Isolation Level 3: REPEATABLE READ

## Definition

**REPEATABLE READ provides stronger guarantees so that repeated reads of the same data within a transaction remain consistent according to the database's isolation semantics.**

### Simple Definition

> **REPEATABLE READ = Once you read a row, repeated reads give a consistent view of that row.**

Example:

```text id="6s8h2n"
Transaction A:
Read balance → ₹10,000
```

Transaction B:

```text id="g4q1z8"
Update balance → ₹8,000
COMMIT
```

Transaction A reads again.

Under repeatable-read semantics, A continues to see its transaction-consistent value:

```text id="1x6j9v"
₹10,000
```

The exact behavior for other phenomena, especially phantom reads, can differ between database implementations.

---

# Isolation Level 4: SERIALIZABLE

## Definition

**SERIALIZABLE is the strongest standard isolation level.**

It provides behavior equivalent to executing concurrent transactions in some serial order.

### Simple Definition

> **SERIALIZABLE = Transactions behave as if they ran one after another.**

Imagine:

```text id="y4n6c2"
Transaction A
      ↓
Complete
      ↓
Transaction B
      ↓
Complete
```

Instead of unsafe concurrent behavior:

```text id="z3k8m1"
A ────────┐
          ├── Same time
B ────────┘
```

The database ensures the final result is equivalent to some valid serial ordering.

### Advantage

Strongest standard isolation.

### Disadvantage

It can reduce concurrency and may cause transactions to be aborted and retried when conflicts occur.

---

# Isolation Levels Comparison

| Isolation Level      | Dirty Read | Non-Repeatable Read |                 Phantom Read |
| -------------------- | ---------: | ------------------: | ---------------------------: |
| **READ UNCOMMITTED** |   Possible |            Possible |                     Possible |
| **READ COMMITTED**   |  Prevented |            Possible |                     Possible |
| **REPEATABLE READ**  |  Prevented |           Prevented | Depends on DB implementation |
| **SERIALIZABLE**     |  Prevented |           Prevented |                    Prevented |

Important:

> **The SQL standard describes isolation levels in terms of allowed phenomena, but actual database behavior can be stronger or different because database engines implement isolation differently.**

---

# Easy Memory Table

```text id="1f5j8d"
READ UNCOMMITTED
        ↓
Can read uncommitted data

READ COMMITTED
        ↓
Only committed data

REPEATABLE READ
        ↓
Repeated reads remain consistent

SERIALIZABLE
        ↓
Behaves like serial execution
```

---

# Isolation Level vs Locking

These two concepts are often confused.

## Isolation Level

Defines:

> **What concurrency behavior the transaction should provide.**

## Locking

Is one mechanism databases may use to control concurrent access.

```text id="7j3n5c"
Isolation Level
      ↓
Concurrency Rules
      ↓
Locks and/or MVCC
      ↓
Database controls transactions
```

Modern databases commonly use **MVCC (Multi-Version Concurrency Control)**, so isolation is not implemented purely through blocking locks.

---

# Isolation Level vs Transaction

### Transaction

A transaction is:

> **A logical unit of database work.**

### Isolation Level

Isolation level defines:

> **How that transaction interacts with other concurrent transactions.**

Example:

```sql id="7h3q2k"
BEGIN;

SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

COMMIT;
```

Here:

```text id="4y9p7v"
BEGIN
  ↓
Transaction starts
  ↓
Isolation Level = SERIALIZABLE
  ↓
SQL operations
  ↓
COMMIT
```

The exact syntax can vary by database.

---

# PostgreSQL Example

PostgreSQL supports:

```text id="0x4v7k"
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```

PostgreSQL's default isolation level is:

```text id="7b2w9m"
READ COMMITTED
```

Example:

```sql id="w6j2x9"
BEGIN;

SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

SELECT *
FROM accounts
WHERE id = 1;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

COMMIT;
```

---

# MySQL Example

In MySQL with InnoDB, commonly available isolation levels include:

```text id="3k8m2p"
READ UNCOMMITTED
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```

MySQL/InnoDB commonly uses:

```text id="r7v4s1"
REPEATABLE READ
```

as its default isolation level.

Example:

```sql id="8q2x6k"
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;

SELECT *
FROM accounts
WHERE id = 1;

COMMIT;
```

---

# Real-Life Example: Bank Account

Suppose:

```text id="5m8v2n"
Balance = ₹10,000
```

Two transactions run simultaneously.

```text id="c6k4x9"
Transaction A → Withdraw ₹2,000
Transaction B → Check balance
```

The isolation level determines what B can see while A is working.

```text id="h7p1s3"
READ UNCOMMITTED
→ B may see uncommitted changes

READ COMMITTED
→ B sees only committed changes

REPEATABLE READ
→ A gets a consistent transaction view

SERIALIZABLE
→ Operations behave as if properly serialized
```

---

# Trade-Off

Higher isolation is not automatically "better" for every application.

```text id="9k2w6d"
Higher Isolation
      ↓
Stronger Consistency
      ↓
Potentially More Blocking / Conflicts
      ↓
Lower Concurrency
```

Lower isolation:

```text id="1x7m4q"
Lower Isolation
      ↓
More Concurrency
      ↓
Better Throughput in some workloads
      ↓
More Concurrency Anomalies
```

The correct level depends on the application's requirements.

---

# Interview Questions

## What is an isolation level?

> **An isolation level defines how a transaction behaves when other transactions are running concurrently, including what changes it can see and what concurrency anomalies are possible.**

## What are the four standard isolation levels?

> **READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, and SERIALIZABLE.**

## What is a dirty read?

> **Reading uncommitted data written by another transaction.**

## What is a non-repeatable read?

> **Reading the same row twice in one transaction and getting different values because another transaction committed a change between the reads.**

## What is a phantom read?

> **Running the same query twice and getting a different set of matching rows because another transaction changed the rows that satisfy the query.**

## Which is the strongest isolation level?

> **SERIALIZABLE is the strongest standard SQL isolation level.**

## Which isolation level is usually the default?

There is **no universal default**. It depends on the database.

For example:

```text id="8w2m5v"
PostgreSQL → READ COMMITTED
MySQL/InnoDB → REPEATABLE READ
```

---

# Quick Revision

```text id="1g5x8v"
READ UNCOMMITTED
→ Dirty reads can occur

READ COMMITTED
→ Dirty reads prevented

REPEATABLE READ
→ Non-repeatable reads prevented
→ Exact phantom behavior depends on DB

SERIALIZABLE
→ Strongest isolation
→ Equivalent to a valid serial execution
```

# Final Memory Trick

```text id="4j8s2q"
RU → Read anything
RC → Read committed
RR → Repeatable view
S  → Serial behavior
```

### Golden Rule

> **Isolation levels control the trade-off between concurrency and consistency when multiple transactions run at the same time.**
