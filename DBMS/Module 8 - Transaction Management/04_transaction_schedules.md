# 8.4 Transaction Schedules

A **Transaction Schedule** is the order in which operations of multiple transactions are executed by the DBMS.

**Notation used below:**

| Symbol | Meaning |
|---|---|
| R1(A) | T1 reads data item A |
| W1(A) | T1 writes data item A |
| C1 / A1 | T1 commits / aborts |

---

## 1. Serial Schedule

One transaction **completes entirely** before the next starts. No interleaving.

```
T1:  R1(A) → W1(A) → C1
T2:  (waits) → R2(A) → W2(A) → C2
```

- ✅ Easy to understand, no conflicts, consistency easy
- ❌ Poor performance — transactions wait; concurrency wasted

**Remember: one finishes, then the next starts.**

## 2. Non-Serial Schedule

Operations of multiple transactions are **interleaved**:

```
R1(A), R2(A), W1(A), W2(A), C1, C2
```

- ✅ Better CPU use, faster, real concurrency
- ❌ Can produce **incorrect results** if transactions interfere

**Remember: operations are mixed.**

## 3. Serializable Schedule

A non-serial schedule whose **final result equals some serial schedule**. Transactions run mixed, but the outcome is the same as if they ran one after another.

*Example:* `R1(A), W1(A), R2(A), W2(A)` ≡ serial order **T1 → T2**. So it is serializable.

**Why it matters:** DBMS wants concurrency *without* wrong results. Two types:

- **Conflict Serializability** (→ file 05)
- **View Serializability** (→ file 06)

**Remember: concurrent execution, serial-equivalent result.**

---

## Quick Comparison

| Schedule | Meaning | Safe? |
|---|---|---|
| Serial | One after another | Always safe, slow |
| Non-serial | Interleaved | Fast, may be wrong |
| Serializable | Interleaved but serial-equivalent | Fast AND correct ✅ |

## Practice

**Q1.** Which schedule runs transactions one after another? → **Serial**
**Q2.** Why does DBMS want serializable schedules instead of purely serial ones? → **Concurrency + correctness together: speed of interleaving with the safety of serial execution.**
