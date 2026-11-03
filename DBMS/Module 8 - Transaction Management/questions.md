# Module 8 — Questions (Transaction Management)

Conceptual / scenario / tricky questions only — no basic definitions.

**Q1.** Rahul has ₹500 and tries to transfer ₹1,000 to Amit. The transaction stops midway. Which state is it in, and what must DBMS do next?
→ **Failed** state; DBMS must **rollback** the partial changes, moving it to **Aborted**, then **Terminated**.

**Q2.** A transaction executed all its operations but the power fails during the final commit. Which two states did it pass through, and where does it go now?
→ It was **Partially Committed**; on failure it goes **Failed → Aborted → Terminated** (rollback undoes the operations).

**Q3.** Why is "Partially Committed" not the same as "Committed"? Give a failure scenario where the difference matters.
→ Partially committed = operations done, commit *pending*. If the system crashes in this window, the transaction is not durable — it must be rolled back. Committed = commit succeeded = durable, survives crashes.

**Q4.** Two transactions both read A = 100, then both write A (T1 writes 120, T2 writes 150). Final value is 150 but should reflect both updates. Name the problem and the violated property.
→ **Lost update problem**; violates **Isolation**.

**Q5.** A committed order disappears after a server restart. Which ACID property failed, and what mechanism normally prevents this?
→ **Durability** failed. Normally prevented by **Write-Ahead Logging (WAL)** + stable storage.

**Q6.** Schedule: `R1(X), R2(Y), W1(X), W2(Y)`. Is it conflict serializable? Justify with the precedence graph.
→ T1 touches only X, T2 only Y — no conflicting pairs, no edges, no cycle → **Yes**, equivalent to T1 → T2 (or T2 → T1).

**Q7.** Schedule: `W1(A), R2(A), W2(B), R1(B)`. Draw the precedence graph mentally and decide conflict serializability.
→ W1(A)→R2(A): T1→T2. W2(B)→R1(B): T2→T1. **Cycle → NOT conflict serializable.**

**Q8.** In Q7's schedule, T2 commits before T1. Is the schedule recoverable? What could go wrong?
→ **No — unrecoverable.** T2 read T1's uncommitted write and committed first; if T1 aborts, T2's commit used invalid data.

**Q9.** Give a schedule that is view serializable but NOT conflict serializable, and explain why the difference exists.
→ `W1(A), W2(A), W3(A), R2(A)`: cycle T2→T3→T2 (not conflict serializable), but view-equivalent to T1→T3→T2 — T2's write to A was a **blind write** overwritten before anyone read it.

**Q10.** `W1(A), C1, R2(A), W2(A), C2` — classify as recoverable / cascadeless / strict (all that apply).
→ **All three**: recoverable (C2 after C1), cascadeless (reads only committed data), **strict** (no read/write of A until T1 committed).

**Q11.** `W1(A), R2(A), C1, C2` — which of the three is it, and which is it NOT?
→ **Recoverable only.** NOT cascadeless (T2 read uncommitted data) and NOT strict.

**Q12.** Why does a strict schedule make database recovery easier than a merely recoverable one?
→ No dirty reads or writes: aborting T1 never forces undoing other transactions' work — no cascading rollback, no undo of dirty writes.

**Q13.** A ticket-booking system sells the last seat twice under concurrency. Which schedule property was missing, and which mechanism fixes it?
→ The schedule wasn't **serializable** (lost update on seat count). Fix with concurrency control — e.g. **exclusive locks / 2PL** on the seat row (Isolation).

**Q14.** `ROLLBACK TO SAVEPOINT sp1` is executed, then `COMMIT`. Which operations survive?
→ All operations **before sp1** survive; operations **after sp1** are undone. The transaction then commits the remainder.

**Q15.** Can a schedule be serializable but still unrecoverable? Give an example.
→ **Yes.** `R1(A), W1(A), R2(A), W2(A), C2, C1`: conflict serializable (≡ T1→T2) but T2 commits before T1 → unrecoverable.

**Q16.** Three transactions: T1 writes A, T2 reads A and writes B, T3 reads B. Draw the precedence graph for `W1(A), R2(A), W2(B), R3(B)` and give an equivalent serial order.
→ Edges: W1(A)→R2(A) = T1→T2; W2(B)→R3(B) = T2→T3. No cycle → serializable, order **T1 → T2 → T3**.
