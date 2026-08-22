# ACID Properties — SQL Cheat Sheet

ACID is a set of four properties that make **database transactions reliable and safe**.

**ACID = Atomicity + Consistency + Isolation + Durability**

A **transaction** is a group of database operations that should be treated as one complete unit.

---

## 1. Atomicity

### Simple Definition

**Atomicity means: A transaction either happens completely or does not happen at all.**

There is no half-completed transaction.

### Example

Suppose Alok transfers **₹1,000** from Account A to Account B.

Two operations are required:

```sql
UPDATE account
SET balance = balance - 1000
WHERE id = 'A';

UPDATE account
SET balance = balance + 1000
WHERE id = 'B';
```

If ₹1,000 is deducted from A but adding ₹1,000 to B fails, the database will **roll back the entire transaction**.

So:

```text
Before:
A = ₹5000
B = ₹3000

After successful transaction:
A = ₹4000
B = ₹4000
```

If something fails:

```text
A = ₹5000
B = ₹3000
```

### Remember

> **Atomicity = All or Nothing**

---

# 2. Consistency

### Simple Definition

**Consistency means a transaction must keep the database in a valid state.**

After a transaction finishes, all database rules and constraints should still be satisfied.

### Example

Suppose an account cannot have a negative balance.

```text
Account Balance = ₹500
```

Trying to withdraw ₹1000 should not result in:

```text
Balance = -₹500
```

The database should reject or roll back the operation if it violates the defined rules.

Other examples of rules:

```text
Primary Key must be unique
Foreign Key must be valid
Balance cannot be negative
Email cannot be NULL
Age must be >= 18
```

### Remember

> **Consistency = Database Rules Stay Correct**

---

# 3. Isolation

### Simple Definition

**Isolation means multiple transactions running at the same time should not incorrectly interfere with each other.**

Each transaction should behave as if it is running independently.

### Example

Suppose two transactions are running at the same time:

```text
Transaction 1:
Withdraw ₹1000

Transaction 2:
Check account balance
```

The database manages these transactions so that one transaction does not see incorrect or partially completed changes from another transaction.

For example:

```text
Transaction 1:
₹5000 → ₹4000

Transaction 2:
Should not see some half-completed state
```

### Real-Life Example

Imagine two people trying to buy the **last available ticket** at the same time.

Without proper isolation:

```text
Person A → buys ticket
Person B → also buys the same ticket
```

With proper isolation:

```text
Person A → gets the ticket
Person B → gets "Ticket unavailable"
```

### Remember

> **Isolation = Transactions Don't Mess With Each Other**

---

# 4. Durability

### Simple Definition

**Durability means once a transaction is successfully committed, its changes are permanently saved.**

Even if the database server crashes immediately afterward, the committed data should not be lost.

### Example

Suppose:

```sql
COMMIT;
```

is executed after transferring ₹1,000.

Then:

```text
A = ₹4000
B = ₹4000
```

If the database server suddenly crashes:

```text
Database Crash
     ↓
Database Restarts
     ↓
A = ₹4000
B = ₹4000
```

The committed transaction remains saved.

### Remember

> **Durability = Committed Data Stays Saved**

---

# ACID in One Example

Consider an **online banking transfer**:

```text
Transfer ₹1000 from A → B
```

| Property        | What it means                                               |
| --------------- | ----------------------------------------------------------- |
| **Atomicity**   | Both debit and credit happen, or neither happens            |
| **Consistency** | Database rules remain valid                                 |
| **Isolation**   | Other transactions don't see incorrect intermediate data    |
| **Durability**  | After COMMIT, the transfer remains saved even after a crash |

---

# Easy Way to Remember ACID

```text
A → All or Nothing
C → Correct State
I → Independent Transactions
D → Data Stays Saved
```

Or simply:

```text
Atomicity   → Complete or Rollback
Consistency → Rules remain correct
Isolation   → Transactions stay independent
Durability  → Committed data is permanent
```

---

# Definition

**"What is ACID in DBMS?"**

You can answer:

> **ACID is a set of four properties that ensure database transactions are reliable and safe. Atomicity ensures all-or-nothing execution, Consistency keeps the database in a valid state, Isolation prevents concurrent transactions from interfering incorrectly, and Durability ensures committed changes are permanently saved.**

---

# Quick Revision

```text
                 ACID
                  |
      ┌───────────┼───────────┐
      ↓           ↓           ↓
 Atomicity   Consistency   Isolation   Durability
    ↓            ↓             ↓          ↓
All or       Rules remain   Transactions  Changes
Nothing         valid        stay safe    stay saved
```

### One-Line Cheat Sheet

**Atomicity** → All or nothing
**Consistency** → Database remains correct
**Isolation** → Concurrent transactions don't interfere incorrectly
**Durability** → Committed changes are permanent
