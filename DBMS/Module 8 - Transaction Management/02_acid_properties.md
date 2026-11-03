# 8.2 ACID Properties

ACID = four properties that keep database transactions **reliable, correct, and safe**.

| Letter | Property | One-line meaning |
|---|---|---|
| A | Atomicity | All or Nothing |
| C | Consistency | Database remains correct |
| I | Isolation | Transactions don't interfere |
| D | Durability | Committed changes stay permanent |

---

## 1. Atomicity — All or Nothing

A transaction must complete **entirely** or **not at all**.

*Example:* Rahul transfers ₹1,000 to Amit (deduct from Rahul + add to Amit). If the system crashes mid-transfer, DBMS rolls back — Rahul's money is restored. Without atomicity, Rahul could lose money that Amit never receives.

**Remember: All or Nothing.**

## 2. Consistency — Database Must Remain Correct

A transaction must take the database from **one valid state to another valid state**. All rules and constraints stay satisfied before and after.

*Example:* Bank rule — total money in both accounts must stay unchanged during a transfer.

| | Rahul | Amit | Total |
|---|---|---|---|
| Before | ₹5,000 | ₹2,000 | ₹7,000 |
| After | ₹4,000 | ₹3,000 | ₹7,000 ✓ |

Other rules: balance can't be negative, email must be unique, primary key can't be NULL, foreign key must reference a valid record.

**Remember: Database rules must never be violated by a successful transaction.**

## 3. Isolation — Transactions Should Not Interfere

Concurrent transactions must behave **as if each is executing alone**.

*Example:* Rahul has ₹5,000. Two transactions run at the same time:
- T1: Rahul transfers ₹2,000 to Amit
- T2: Rahul withdraws ₹3,000

If both read the old balance ₹5,000 simultaneously without isolation, the final balance can come out wrong. DBMS uses concurrency control — **locks** (shared/exclusive), **Two-Phase Locking (2PL)**, **timestamp ordering**, **MVCC**.

**Remember: Transactions should not interfere with each other's execution.**

## 4. Durability — Committed Data Must Stay Safe

Once a transaction **commits**, its changes must survive even a system crash or power failure.

*Example:* Rahul → Amit transfer commits (₹4,000 / ₹3,000). Server crashes. After restart, balances must still show ₹4,000 / ₹3,000. DBMS achieves this with **transaction logs (Write-Ahead Logging)**, stable storage, recovery mechanisms, and backups.

**Remember: Once committed, changes must survive system failures.**

---

## Real-Life Example: Online Shopping (all four together)

You buy a phone for ₹20,000:

- **A — Atomicity:** Payment deduction + order creation must succeed together, else rollback.
- **C — Consistency:** Stock, payment, and order rules must remain valid.
- **I — Isolation:** Two customers buying the last phone must not corrupt stock counts.
- **D — Durability:** Once the order is committed, it stays saved even if the server crashes.

## Interview Questions

**Q1.** What is ACID in DBMS?
→ Four properties ensuring reliable transactions: Atomicity, Consistency, Isolation, Durability.

**Q2.** Which ACID property handles system crashes after commit?
→ **Durability.**

**Q3.** Which property ensures two transactions don't interfere?
→ **Isolation.**

**Q4.** Difference between Atomicity and Consistency?
→ Atomicity = all operations succeed or none do. Consistency = database rules stay valid before and after.

**Q5.** Which property is related to rollback?
→ **Atomicity.**

## Study Checkpoint

- Atomicity = All or Nothing
- Consistency = Valid data before and after
- Isolation = Independent transaction execution
- Durability = Committed data is permanent

> Don't just memorize definitions — understand the bank transfer and online shopping examples. They unlock the next topics: Concurrency Control, Serializability, Deadlocks, Recovery Management.
