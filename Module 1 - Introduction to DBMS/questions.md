# Module 1: Interview Questions (with Answers)

> 16 hand-picked interview questions — no "what is data" definitions (wo notes me hai). Har question concept test karta hai, ratta nahi.

---

### Q1. A college stores 10,000 student records in Excel. The principal asks why they should pay for a DBMS. What's your pitch?

**Answer:** Excel breaks at scale. Same student entered twice with different CPIs (inconsistency), two clerks overwriting each other's edits (no concurrency control), anyone with the file sees everything (no security), one corrupt file = total data loss (no recovery). A DBMS gives one controlled source: constraints block bad data, transactions prevent half-done updates, permissions control access, backups enable recovery.

**Why they ask:** They want problems → solutions mapping, not a feature list. Always answer with *what breaks without it*.

---

### Q2. "DBMS is always better than a file system." Agree or disagree?

**Answer:** Disagree. For small, static data (app config files, a personal todo list), a DBMS adds cost, memory overhead and complexity for zero benefit. DBMS earns its keep when you need concurrent access, security, integrity rules and transactions.

**Why they ask:** Tests judgment over dogma. Strong engineers know when *not* to use a tool.

---

### Q3. A bank transfer deducts ₹1000 from account A but the system crashes before crediting B. What guarantees the money isn't lost?

**Answer:** ACID — specifically **Atomicity** and **Durability**. The transfer is a single transaction: either both steps commit or the whole thing rolls back (no half-deducted money). Once committed, **Durability** guarantees the result survives crashes.

**Why they ask:** The classic ACID application question. If you can explain it with the bank example, you own the topic.

---

### Q4. In 2-tier architecture the client connects directly to the database. Walk me through how an attacker could exploit this.

**Answer:** Database credentials live inside the client application. An attacker reverse-engineers the app, extracts the credentials, and connects straight to the DB — bypassing every rule the app enforced (like "only admins can update CPI"). Depending on the DB user's permissions, they can read or modify anything directly.

**Why they ask:** Tests whether you understand *why* 3-tier exists, not just its diagram. Security reasoning > memorized layers.

---

### Q5. You're building React + Node.js + PostgreSQL. Why shouldn't React talk to PostgreSQL directly?

**Answer:** That collapses the design into 2-tier. DB credentials would ship to the browser (visible to anyone via devtools), there'd be no central place for authentication and business rules, and every client would hold direct database access. The Node layer hides credentials, enforces roles and validates input before anything touches the DB.

**Why they ask:** Same concept as Q4, framed as a real design decision — extremely common in backend interviews.

---

### Q6. People mix up 3-tier architecture, distributed databases, and client-server. Untangle them.

**Answer:** They describe different things. **3-tier** = how an *application* is layered (presentation / app / database). **Distributed database** = where *data* physically lives (many locations, one logical view). **Client-server** = the *communication pattern* (request/response). They're orthogonal — a 3-tier app can absolutely use a distributed database.

**Why they ask:** Directly punishes rote learning. If you memorized diagrams without understanding, this question exposes it.

---

### Q7. Your app's database is in Mumbai; Delhi users complain reads are slow. What are your options?

**Answer:** **Replication** — place read copies (replicas) in Delhi so reads go local while writes go to Mumbai. Or **fragmentation** — if Delhi users mostly need Delhi data, store that partition locally. Often both are combined.

**Why they ask:** Applied replication vs fragmentation. They want to see you reach for the right tool, not recite definitions.

---

### Q8. When would you pick replication over fragmentation?

**Answer:** Replication when the *same* data is read from many places and availability matters — copies everywhere, any copy can serve. Fragmentation when different locations need *different* subsets — each place holds only its own slice, reducing what each server stores.

**Why they ask:** Trade-off reasoning. "It depends" is a good start; the split criterion (same data vs different data) is the real answer.

---

### Q9. A `phone` column is added to the Students table, but the student portal keeps working unchanged. Which concept is this, and why does it matter?

**Answer:** **Logical data independence** — a conceptual-level change didn't break external views. It matters because schemas evolve constantly; without it, every database change would force application rewrites.

**Why they ask:** Applied definition. "Define logical data independence" is a college question; *recognizing it in a scenario* is an interview question.

---

### Q10. You add an index on `student_id` and nothing else changes. Which independence is this — and why is this one easier?

**Answer:** **Physical data independence** — only physical access structures changed, the logical schema is untouched. It's easier because applications never see physical storage, so changes there are naturally contained. Logical changes are harder because they can ripple across views, queries and app code.

**Why they ask:** Pairs with Q9. Tests the full comparison, not one side of it.

---

### Q11. Why is logical data independence *harder* to achieve than physical?

**Answer:** Physical storage is invisible to applications by design, so changing it (HDD→SSD, new index) can't break what apps can't see. Logical changes — new columns, split tables, renamed relations — alter the very structure that views, queries and application code were written against. Shielding every dependent from that is genuinely hard.

**Why they ask:** The "why" behind the comparison table. Shows you think in terms of blast radius.

---

### Q12. Rule: "only admins can update student CPI." Should the DBA or the application programmer enforce it?

**Answer:** Both, in layers — but the *guarantee* belongs near the data (DB permissions/constraints, DBA's domain), because app-level checks can be bypassed, especially in 2-tier where clients reach the DB directly. Defense in depth: the app validates for UX, the database enforces the truth.

**Why they ask:** Connects user roles with the 2-tier security lesson. "Defense in depth" is a phrase interviewers like hearing.

---

### Q13. The college DB has duplicate roll numbers and negative ages. Who failed — the designer or the DBA?

**Answer:** The **designer**. Constraints like PRIMARY KEY and CHECK are design decisions that prevent bad data at entry — that's schema design, the designer's job. The DBA operates and maintains; they *can* add constraints later, but the gap originated in design.

**Why they ask:** Role distinction through a concrete failure, not "list the responsibilities of a DBA."

---

### Q14. Give an example of data that is valid but inconsistent — and data that is consistent but invalid.

**Answer:** **Valid but inconsistent:** age `25` passes the CHECK constraint, but two files show the same student as `25` and `30` — each value is individually valid, yet they contradict. **Consistent but invalid:** every copy agrees the age is `-5` — perfectly consistent across the system, logically impossible.

**Why they ask:** The trickiest question on this list. It separates *integrity* (is each value valid?) from *consistency* (do copies agree?). Nailing this signals deep understanding.

---

### Q15. A NoSQL fan says "schemas are dead, three-schema architecture is obsolete." Your response?

**Answer:** Three-schema was never about SQL schemas — it's about **separating concerns**: user views (external), logical organization (conceptual), physical storage (internal). MongoDB has the same three: app-level projections (external), document structure (conceptual), the WiredTiger storage engine's files (internal). The data model changed; the principle survived.

**Why they ask:** Tests whether you grasp the *idea* behind the diagram. Great closer for an interview.

---

### Q16. In a distributed DB, Delhi and Mumbai both hold copies of product stock. A Delhi user buys the last item. What can go wrong?

**Answer:** **Replication lag** — Mumbai's copy still shows one item in stock, so a Mumbai user buys the same "last" item → overselling. Distributed systems trade consistency for availability; you need a sync strategy (synchronous replication, conflict resolution, or single-writer design).

**Why they ask:** Real distributed-systems thinking. This is how you go from "DBMS student" to "backend engineer" in an interviewer's eyes.

---

> 💡 **How to use this file:** Cover the answers, say each one aloud. If you hesitate on Q6, Q11 or Q14 — those are the ones interviewers use to separate candidates. Revise them twice.
