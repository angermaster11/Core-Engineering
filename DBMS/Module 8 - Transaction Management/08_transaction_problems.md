# 8.5 Transaction Problems (Concurrency Problems)

When multiple transactions work on the same database simultaneously, data can become inconsistent. These five problems are the **reason** concurrency control exists. Learn each with its schedule — interviews test understanding through schedules, not definitions.

---

## 1. Lost Update Problem — Data Loss

**Definition:** Two transactions update the same data; one transaction's update gets **overwritten** by the other.

**Example:** Account A = ₹10,000. T1 deposits ₹1,000, T2 withdraws ₹2,000.

| Step | Transaction | Operation | Balance seen |
|---|---|---|---|
| 1 | T1 | Read(A) | 10,000 |
| 2 | T2 | Read(A) | 10,000 |
| 3 | T1 | A = A + 1,000 | 11,000 |
| 4 | T2 | A = A − 2,000 | 8,000 |
| 5 | T1 | Write(A) | 11,000 |
| 6 | T2 | Write(A) | **8,000** |

**Expected:** ₹9,000 (10,000 + 1,000 − 2,000). **Actual:** ₹8,000 — T1's ₹1,000 deposit is **lost**. T2 read the old value and overwrote T1's update.

**Solution:** Exclusive locks + serializable execution.

## 2. Dirty Read Problem — Uncommitted Data Read

**Definition:** A transaction reads **uncommitted** data written by another transaction.

**Example:** Account A = ₹10,000.

| Step | Transaction | Operation | Balance |
|---|---|---|---|
| 1 | T1 | Read(A) | 10,000 |
| 2 | T1 | Write(A = 5,000) | 5,000 |
| 3 | T2 | Read(A) | **5,000** ← dirty read! |
| 4 | T1 | ROLLBACK | 10,000 |

T2 read ₹5,000 — data that **never permanently existed** in the database.

**Solution:** Read Committed isolation level or higher.

## 3. Unrepeatable Read Problem — Same Query, Different Result

**Definition:** A transaction reads the **same row twice**, but another transaction updates and commits that row in between — the two reads return **different values**.

**Example:** Account A = ₹10,000.

| Step | Transaction | Operation | Balance |
|---|---|---|---|
| 1 | T1 | Read(A) | 10,000 |
| 2 | T2 | Update(A) → 15,000 | 15,000 |
| 3 | T2 | COMMIT | 15,000 |
| 4 | T1 | Read(A) again | **15,000** ← different! |

**Solution:** Repeatable Read or Serializable isolation level.

## 4. Phantom Read Problem — New or Missing Rows

**Definition:** A transaction runs the **same condition-based query twice**; another transaction inserts/deletes rows matching that condition in between — **new or missing rows** appear.

**Example:** Employee table:

| ID | Name | Salary |
|---|---|---|
| 1 | Rahul | 60,000 |
| 2 | Amit | 70,000 |

- T1: `SELECT * FROM Employee WHERE salary > 50000;` → **2 rows**
- T2: `INSERT INTO Employee VALUES (3, 'Raj', 80000);` + COMMIT
- T1 (same query again) → **3 rows** — Raj is a **phantom row**

**Solution:** Serializable isolation level or range/predicate locking.

## 5. Incorrect Summary Problem — Inconsistent Aggregate

**Definition:** A transaction calculates an **aggregate (SUM/AVG/COUNT)** over multiple records while another transaction updates those records simultaneously — the summary is wrong.

**Example:** A = ₹10,000, B = ₹5,000 (total ₹15,000). T1 sums balances while T2 transfers ₹2,000 from A to B.

| Step | Transaction | Operation | Value |
|---|---|---|---|
| 1 | T1 | Read(A) | 10,000 |
| 2 | T2 | A = A − 2,000 | 8,000 |
| 3 | T2 | B = B + 2,000 | 7,000 |
| 4 | T1 | Read(B) | 7,000 |

T1 computes 10,000 + 7,000 = **₹17,000** — but the real total is still **₹15,000**. T1 read A's *old* value and B's *new* value.

**Solution:** Serializable isolation or appropriate locking.

---

## All Five — Quick Comparison

| Problem | Main issue | Simple meaning |
|---|---|---|
| Lost Update | Overwriting | One update got lost |
| Dirty Read | Uncommitted data | Read data that never existed |
| Unrepeatable Read | Updated row | Same row, different values |
| Phantom Read | Insert/Delete rows | Same query, different rows |
| Incorrect Summary | Aggregate inconsistency | SUM/AVG calculated wrong |

**One-line distinction (interview favourite):**
- Lost Update → update **overwritten**
- Dirty Read → **uncommitted** value read
- Unrepeatable Read → same **existing row's** value changed
- Phantom Read → **rows added/removed** in query result
- Incorrect Summary → **aggregate** over mixed old/new values wrong

## Isolation Levels (memorize this table)

| Isolation Level | Dirty Read | Unrepeatable Read | Phantom Read |
|---|---|---|---|
| Read Uncommitted | Yes | Yes | Yes |
| Read Committed | No | Yes | Yes |
| Repeatable Read | No | No | Possible* |
| Serializable | No | No | No |

*\*Per standard SQL guarantees; actual behavior is database-specific.*

**Reading the table:** each level fixes the problems of the levels above it. Serializable fixes everything but costs the most performance.

---

## Interview Practice

**Q1.** T1 reads ₹5,000 written by T2, but T2 later rolls back. Which problem?
→ **Dirty Read** (T1 read uncommitted data that never existed)

**Q2.** Same row is read twice, but its value changes between reads. Which problem?
→ **Unrepeatable Read**

**Q3.** A SELECT returns 2 rows first and 3 rows later because of a committed INSERT. Which problem?
→ **Phantom Read**

**Q4.** Two transactions update the same account using the same old value. One update gets overwritten. Which problem?
→ **Lost Update**

> Advice: don't memorize definitions — practice drawing the transaction schedules yourself. That's what DBMS interviews actually test.
