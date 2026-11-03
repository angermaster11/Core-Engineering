# 8.6 View Serializability (Deep Dive)

A schedule is **View Serializable** if it is **view-equivalent** to some serial schedule — same *reads* observed and same *final writes*.

## The Three Conditions

For schedules S and S′ to be view-equivalent:

1. **Initial Read:** if a transaction reads the *initial* value of an item in S, it must read the initial value in S′ too.
2. **Read-From Relationship:** if T2 reads a value *written by* T1 in S, the same T1 → T2 relationship must hold in S′.
3. **Final Write:** the transaction performing the *final* write on each item must be the same in both.

**Why it exists:** some schedules are View Serializable but **NOT** Conflict Serializable (see Example 3). View serializability is the more general test.

---

## Worked Example 1 — View Serializable ✅

Schedule S: `R1(A), W1(A), R2(A), W2(A), C1, C2`

- Initial reads: R1(A) reads initial ✅ (serial T1→T2 also has T1 reading initial)
- Read-from: R2(A) reads W1(A)'s value → T1 → T2 in both
- Final write: W2(A) → T2 in both

≡ serial **T1 → T2** → **View Serializable** (and also conflict serializable).

## Worked Example 2 — NOT View Serializable ❌ (lost update)

Schedule S: `R1(A), R2(A), W1(A), W2(A)` (both read ₹5,000, both write)

- Initial reads: both read initial.
- Read-from: none (nobody reads another's write).
- Final write: T2.

Try serial T1 → T2: then R2(A) would read T1's written value — but in S, R2 read the *initial* value. ✗
Try serial T2 → T1: then R1(A) would read T2's written value — but in S, R1 read the *initial* value. ✗

No serial schedule matches → **NOT View Serializable**. (Also not conflict serializable — cycle. This schedule is simply wrong.)

## Worked Example 3 — View Serializable but NOT Conflict Serializable ✅/❌ (extra, interview gold)

Schedule S: `W1(A), W2(A), W3(A), R2(A)` — three blind writes, then T2 reads.

**Conflict check:** W1(A)→W2(A): T1→T2. W2(A)→W3(A): T2→T3. W3(A)→R2(A): T3→T2. Cycle T2 → T3 → T2 → **NOT conflict serializable**.

**View check** against serial order **T1 → T3 → T2**:
- Initial reads: none in either ✅
- Read-from: R2(A) reads W3(A)'s value → T3 → T2; in serial T1→T3→T2, T3 comes before T2 ✅
- Final write: W3(A) is last writer in S; in serial T1→T3→T2, writers run T1, T3 (T2 never writes A) → last writer T3 ✅

All three conditions hold → **View Serializable** ≡ T1 → T3 → T2, even though a cycle exists!

**Key insight:** T2's write to A was a *blind write* — overwritten by T3 before anyone read it, so its position doesn't affect what anyone observes. View serializability sees this; conflict serializability doesn't.

---

## Conflict vs View — the relationship

- Every **conflict-serializable** schedule is **view-serializable**.
- The reverse is **not** true (Example 3 proves it).
- So: **Conflict Serializable ⊂ View Serializable** — view is the bigger, more general class.

## Practice

**Q1.** Can a schedule be view serializable but not conflict serializable? → **Yes** (blind writes — Example 3)
**Q2.** The three view-equivalence conditions are? → **Initial read, read-from relationship, final write**
**Q3.** In Example 3, which serial order is view-equivalent? → **T1 → T3 → T2**
