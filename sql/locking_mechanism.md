# Database Locking Mechanism — Cheat Sheet

## What is Locking?

**Locking is a database mechanism used to control how multiple transactions access the same data at the same time.**

### Simple Definition

> **Locking = Control who can read or change data when multiple transactions are running at the same time.**

Without proper locking, two transactions could modify the same data incorrectly.

---

# Why Do We Need Locking?

Suppose a bank account has:

```text
Balance = ₹10,000
```

Two transactions run at the same time.

```text
Transaction A → Withdraw ₹7,000
Transaction B → Withdraw ₹5,000
```

Without proper concurrency control, both transactions might read:

```text
₹10,000
```

and both may think enough money is available.

This can produce an incorrect result.

Locking helps the database coordinate these operations.

---

# Basic Idea

Think of a lock like a room key.

```text
Transaction A
     ↓
Takes the lock
     ↓
Changes data
     ↓
Releases the lock
     ↓
Transaction B can continue
```

While A holds an exclusive lock, B may have to wait.

---

# Types of Locks

The two most important basic lock types are:

```text
Shared Lock     → Read
Exclusive Lock  → Write
```

---

# 1. Shared Lock

A **Shared Lock** is generally used when a transaction wants to read data while preventing conflicting modifications.

It is often represented as:

```text
S Lock
```

### Simple Definition

> **Shared Lock = I want to read this data.**

Multiple transactions can usually hold shared locks on the same data at the same time.

Example:

```text
Transaction A → Read → Shared Lock
Transaction B → Read → Shared Lock
```

Both can read.

```text
A → S Lock
B → S Lock
     ↓
Both can read
```

But a conflicting write generally has to wait.

---

# 2. Exclusive Lock

An **Exclusive Lock** is used when a transaction wants to modify data.

It is often represented as:

```text
X Lock
```

### Simple Definition

> **Exclusive Lock = I want to change this data, so conflicting access must wait.**

Example:

```text
Transaction A
     ↓
Exclusive Lock
     ↓
UPDATE
     ↓
COMMIT
     ↓
Release Lock
```

While A holds the exclusive lock, another transaction normally cannot acquire a conflicting lock on that same data.

---

# Shared vs Exclusive Lock

| Lock              | Purpose | Multiple transactions?                    |
| ----------------- | ------- | ----------------------------------------- |
| **Shared (S)**    | Read    | Multiple shared locks can usually coexist |
| **Exclusive (X)** | Write   | Conflicts with other locks                |

Easy memory:

```text
S → SELECT / Read
X → eXclusive / Write
```

---

# Lock Compatibility

A simplified compatibility view:

| Existing Lock | New Shared Lock | New Exclusive Lock |
| ------------- | --------------: | -----------------: |
| **Shared**    |         Allowed |            Blocked |
| **Exclusive** |         Blocked |            Blocked |

Think:

```text
S + S → Can coexist

S + X → Conflict

X + S → Conflict

X + X → Conflict
```

---

# Example: Shared Lock

Suppose:

```text
Account A = ₹10,000
```

Transaction 1 reads the account:

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR SHARE;
```

The transaction obtains a shared lock according to the database's locking rules.

Another transaction can generally read the same row, but a conflicting update may have to wait.

---

# Example: Exclusive Lock

Suppose we want to update the account:

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

COMMIT;
```

`FOR UPDATE` requests a lock suitable for protecting the selected rows from conflicting updates.

Conceptually:

```text
Transaction A
     ↓
Lock row
     ↓
Update row
     ↓
COMMIT
     ↓
Release lock
```

---

# Locking and Transactions

Locks are closely connected with transactions.

Typical flow:

```text
BEGIN
  ↓
Acquire Lock
  ↓
Read / Modify Data
  ↓
COMMIT or ROLLBACK
  ↓
Lock Released
```

In many database systems, transaction-ending statements release transaction-scoped locks, though exact behavior depends on the lock type and database.

---

# What Happens When Two Transactions Want the Same Row?

Suppose:

```text
Account A = ₹10,000
```

Transaction A:

```text
UPDATE account
SET balance = balance - 1000
WHERE id = 1;
```

Transaction B tries to update the same row at the same time.

Conceptually:

```text
Transaction A
     ↓
Gets Exclusive Lock
     ↓
Updates row
     ↓
Transaction B waits
```

After A commits:

```text
A → COMMIT
     ↓
Lock released
     ↓
B can continue
```

This prevents both transactions from modifying the same row at the exact same time without coordination.

---

# Row-Level Lock

A **row-level lock** locks specific rows.

Example:

```text
Table: accounts

Row 1 → Locked
Row 2 → Available
Row 3 → Available
```

Transaction A can work with Row 1 while another transaction may work with Row 2.

### Advantage

More concurrency.

### Simple Definition

> **Row Lock = Lock only the required rows.**

---

# Table-Level Lock

A **table-level lock** locks the entire table.

```text
accounts
-----------------
Row 1 → Locked
Row 2 → Locked
Row 3 → Locked
Row 4 → Locked
```

Other transactions may have to wait depending on the lock mode.

### Advantage

Simple to manage.

### Disadvantage

Less concurrency.

### Simple Definition

> **Table Lock = Lock the entire table.**

---

# Row Lock vs Table Lock

| Row-Level Lock              | Table-Level Lock                       |
| --------------------------- | -------------------------------------- |
| Locks specific rows         | Locks entire table                     |
| More concurrency            | Less concurrency                       |
| More fine-grained           | More coarse-grained                    |
| Useful for specific records | Useful when many/all rows are affected |

---

# Pessimistic Locking

### Definition

**Pessimistic locking assumes that conflicts may happen, so the transaction locks the data before modifying it.**

In simple words:

> **Lock first, then work.**

Example:

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

COMMIT;
```

Flow:

```text
Read row
   ↓
Lock row
   ↓
Modify row
   ↓
Commit
```

Useful when you expect concurrent conflicts and want to prevent them proactively.

---

# Optimistic Locking

### Definition

**Optimistic locking assumes conflicts are uncommon, so it does not necessarily lock the row for the entire operation. Instead, it checks whether the data changed before saving.**

A common approach uses a version column.

Example:

```text
id | balance | version
1  | 10000   | 5
```

Application reads:

```text
version = 5
```

Later it updates:

```sql
UPDATE accounts
SET balance = 9000,
    version = 6
WHERE id = 1
  AND version = 5;
```

If another transaction already changed the row:

```text
version = 6
```

then:

```text
WHERE version = 5
```

matches zero rows.

The application knows that someone else changed the data.

### Simple Definition

> **Optimistic Locking = Work first, check for conflict before saving.**

---

# Pessimistic vs Optimistic Locking

| Pessimistic                    | Optimistic                                          |
| ------------------------------ | --------------------------------------------------- |
| Locks data before modification | Usually doesn't hold a lock throughout the workflow |
| Assumes conflicts may happen   | Assumes conflicts are uncommon                      |
| Other transactions may wait    | Conflicts are detected during update                |
| Uses database locking          | Often uses version/timestamp checks                 |

Easy memory:

```text
Pessimistic → Lock first
Optimistic  → Check later
```

---

# Deadlock

A **deadlock** happens when two or more transactions wait for each other indefinitely.

### Example

Transaction A:

```text
Locks Row 1
   ↓
Waits for Row 2
```

Transaction B:

```text
Locks Row 2
   ↓
Waits for Row 1
```

Now:

```text
A → Waiting for B

B → Waiting for A
```

Neither can continue.

This is a **deadlock**.

---

# Deadlock Visual

```text
Transaction A
     |
     | Locks Row 1
     ↓
   Row 1
     |
     | Waiting for Row 2
     ↓
   Row 2
     ↑
     | Locked by B
     |
Transaction B
     |
     | Waiting for Row 1
     ↓
   Row 1
```

The database may detect the deadlock and abort one transaction so the other can continue.

---

# How to Reduce Deadlocks?

A common strategy is:

> **Always acquire locks in the same order.**

Bad:

```text
Transaction A:
Lock Row 1 → Lock Row 2

Transaction B:
Lock Row 2 → Lock Row 1
```

Better:

```text
Transaction A:
Lock Row 1 → Lock Row 2

Transaction B:
Lock Row 1 → Lock Row 2
```

Other techniques include:

```text
Keep transactions short
Access resources in a consistent order
Avoid unnecessary locks
Handle deadlock errors with retry logic when appropriate
```

---

# Locking vs Isolation Level

These concepts are related but not the same.

### Locking

Controls:

> **Who can access or modify particular data at a particular time.**

### Isolation Level

Controls:

> **What one transaction can see from other concurrent transactions and what concurrency effects are allowed.**

For example:

```text
Isolation Level
      ↓
Defines concurrency guarantees

Locking
      ↓
One mechanism used to enforce/manage those guarantees
```

Modern databases may also use **MVCC (Multi-Version Concurrency Control)**, where readers can often see snapshots rather than simply waiting on write locks.

---

# Locking vs Transaction

### Transaction

```text
Unit of Work
```

### Lock

```text
Concurrency Control
```

Together:

```text
Transaction
    ↓
Needs to safely access data
    ↓
Database uses locks and/or other
concurrency mechanisms
    ↓
Other transactions are coordinated
```

---

# Important SQL Examples

## Lock a row for update

PostgreSQL:

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

COMMIT;
```

## Lock a row for sharing

PostgreSQL:

```sql
BEGIN;

SELECT *
FROM accounts
WHERE id = 1
FOR SHARE;

COMMIT;
```

The exact lock modes and behavior differ across database systems.

---

# Real-Life Example

Imagine an online ticket booking system.

Only **one seat** is available:

```text
Seat A1 → Available
```

Two users try to book it simultaneously.

```text
User A → Book A1
User B → Book A1
```

With proper concurrency control:

```text
User A
   ↓
Locks / reserves A1
   ↓
Completes booking
   ↓
A1 = Booked

User B
   ↓
Must wait or receives failure
   ↓
A1 unavailable
```

Without proper concurrency control, both users might incorrectly believe they successfully booked the same seat.

---

# Interview Questions

## What is locking?

> **Locking is a concurrency-control mechanism used by databases to coordinate access to data when multiple transactions run simultaneously.**

## What is a shared lock?

> **A shared lock is generally used for reading and allows compatible shared locks from multiple transactions while preventing conflicting modifications.**

## What is an exclusive lock?

> **An exclusive lock is used for modifications and prevents conflicting concurrent access to the locked data.**

## What is deadlock?

> **A deadlock occurs when two or more transactions wait for resources locked by each other, so none of them can proceed.**

## What is pessimistic locking?

> **Pessimistic locking acquires a lock before modifying data because it assumes conflicts may occur.**

## What is optimistic locking?

> **Optimistic locking detects conflicts when saving, commonly by checking a version or timestamp.**

## Row lock vs table lock?

> **A row lock protects specific rows, while a table lock protects the entire table. Row-level locking generally provides greater concurrency.**

---

# Quick Revision

```text
LOCKING
   ↓
Controls concurrent access to data
```

```text
Shared Lock
   ↓
Read
   ↓
Multiple compatible readers can coexist
```

```text
Exclusive Lock
   ↓
Write
   ↓
Conflicting access waits
```

```text
Row Lock
   ↓
Specific rows

Table Lock
   ↓
Entire table
```

```text
Pessimistic
   ↓
Lock first

Optimistic
   ↓
Check for conflict before saving
```

```text
Deadlock
   ↓
A waits for B
B waits for A
```

# Golden Rule

> **Locking controls concurrent access to shared data. Shared locks are mainly for compatible reads, exclusive locks protect writes, and good locking strategies help prevent race conditions and inconsistent updates.**
