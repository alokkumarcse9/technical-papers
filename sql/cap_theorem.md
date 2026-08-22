# CAP Theorem — Database Cheat Sheet

## What is CAP Theorem?

The **CAP Theorem** explains a fundamental limitation of **distributed databases**.

It says that when a network problem occurs, a distributed system can guarantee **at most two of these three properties**:

```text
C → Consistency
A → Availability
P → Partition Tolerance
```

> **CAP = Consistency + Availability + Partition Tolerance**

---

# 1. Consistency (C)

### Simple Definition

**Every read gets the most recent write or an error.**

In simple words:

> **All users should see the same latest data.**

### Example

Suppose we have two database servers:

```text
Server A          Server B
₹500              ₹500
```

You update the balance:

```text
₹500 → ₹1000
```

After the update, if someone reads from either server, they should get:

```text
₹1000
```

Not:

```text
Server A → ₹1000
Server B → ₹500
```

### Remember

> **Consistency = Everyone sees the same latest data**

---

# 2. Availability (A)

### Simple Definition

**Every request gets a response, even if the response may not contain the latest data.**

In simple words:

> **The system should always be ready to respond.**

### Example

Suppose you are using an online shopping application.

Even if one database server has a problem, another server can respond:

```text
User
 ↓
Server A ❌
 ↓
Server B ✅
 ↓
Response
```

The user still gets a response instead of:

```text
"Service unavailable"
```

### Remember

> **Availability = System keeps responding**

---

# 3. Partition Tolerance (P)

### Simple Definition

**The system continues working even when communication between database nodes is broken.**

This is called a **network partition**.

### Example

Suppose we have two database servers:

```text
Server A  ←──── Network ────→  Server B
```

Suddenly the network connection breaks:

```text
Server A  ←──── ❌ ────→  Server B
```

The two servers cannot communicate with each other.

A partition-tolerant system is designed to continue operating despite this failure.

### Remember

> **Partition Tolerance = System survives network failure**

---

# The Important Part of CAP

The most important thing to understand is:

> **In a distributed system, when a network partition occurs, you generally have to choose between Consistency and Availability.**

Why?

Imagine:

```text
        Network Partition
               ↓
      ┌────────┴────────┐
      ↓                 ↓
  Server A          Server B
```

Server A and Server B cannot communicate.

Now a user sends a request.

The system has two choices.

---

## Choice 1: Choose Consistency

The system says:

> "I cannot confirm the latest data from the other server, so I will not return potentially stale data."

```text
Request
   ↓
Server
   ↓
Cannot confirm latest data
   ↓
Error / Wait
```

Result:

```text
Consistency ✅
Availability ❌
Partition Tolerance ✅
```

This is commonly called a:

**CP system**

---

## Choice 2: Choose Availability

The system says:

> "I will respond using the data I have, even if it might not be the latest."

```text
Request
   ↓
Server
   ↓
Return available data
```

Result:

```text
Consistency ❌
Availability ✅
Partition Tolerance ✅
```

This is commonly called an:

**AP system**

---

# CP vs AP

| Type   | Consistency              | Availability       | Partition Tolerance |
| ------ | ------------------------ | ------------------ | ------------------- |
| **CP** | ✅                        | ❌ during partition | ✅                   |
| **AP** | ❌ may temporarily differ | ✅                  | ✅                   |
| **CA** | ✅                        | ✅                  | ❌                   |

### Important

**CA is generally not a realistic choice for a distributed system that must tolerate network partitions.**

If your system has multiple distributed nodes, network partitions are a real possibility. Therefore, in practice, the important trade-off during a partition is usually:

```text
CP ↔ AP
```

---

# Real-Life Example

Imagine a food-delivery application with two servers:

```text
          Food Delivery System

        ┌───────────────┐
        │   Database    │
        └───────┬───────┘
                │
        ┌───────┴───────┐
        ↓               ↓
    Server A         Server B
```

Now the network connection between them breaks.

A restaurant has **only 1 pizza left**.

### CP Approach

The system may stop accepting an order until it can confirm the correct inventory.

```text
Consistency ✅
Availability ❌
Partition Tolerance ✅
```

This avoids selling the same pizza twice.

### AP Approach

Both servers may continue accepting requests.

```text
Consistency ❌ temporarily
Availability ✅
Partition Tolerance ✅
```

The system stays available, but the inventory may temporarily become inconsistent.

---

# CAP vs ACID

Don't confuse **CAP** with **ACID**.

| ACID                                          | CAP                                              |
| --------------------------------------------- | ------------------------------------------------ |
| Focuses on **transactions**                   | Focuses on **distributed systems**               |
| Commonly discussed in databases               | Mainly relevant to distributed databases/systems |
| Ensures reliable transactions                 | Explains distributed-system trade-offs           |
| Atomicity, Consistency, Isolation, Durability | Consistency, Availability, Partition Tolerance   |

### Easy Difference

```text
ACID → "Is my transaction reliable?"

CAP  → "How does my distributed system behave
        when network communication fails?"
```

---

# Easy Way to Remember CAP

```text
C → Consistency
    Everyone sees correct/latest data

A → Availability
    System keeps responding

P → Partition Tolerance
    System survives network failure
```

When a partition happens:

```text
             Network Partition
                    ↓
              Choose the trade-off
                 /       \
                ↓         ↓
              CP         AP
              ↓           ↓
       Consistency    Availability
          first           first
```

---

# Interview Definition

If an interviewer asks:

**"What is CAP Theorem?"**

You can answer:

> **CAP Theorem states that a distributed system cannot simultaneously guarantee Consistency, Availability, and Partition Tolerance during a network partition. When a partition occurs, the system must trade off between Consistency and Availability.**

---

# One-Line Cheat Sheet

**Consistency** → Everyone sees the latest correct data.
**Availability** → Every request gets a response.
**Partition Tolerance** → System continues despite network failures.
**CP** → Choose consistency during partition.
**AP** → Choose availability during partition.

---

# Quick Revision

```text
                    CAP
                     |
          ┌──────────┼──────────┐
          ↓          ↓          ↓
     Consistency Availability Partition
                              Tolerance
          ↓          ↓          ↓
     Latest data   Responds   Survives
     everywhere    to users   network split


During a network partition:

        CP                     AP
        ↓                      ↓
  Consistency first      Availability first
  May reject/wait        May return stale data
```

> **Golden Rule:** CAP is mainly about what a distributed system does **when a network partition occurs**. You don't simply "pick any two" in normal operation; the key trade-off is **Consistency vs Availability when Partition Tolerance is required**.
