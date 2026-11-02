# Module 2: Interview Questions (with Answers)

> 16 interview-grade questions — conceptual and scenario-based. No "what is DDL" definitions (wo notes me hai).

---

### Q1. DELETE vs TRUNCATE vs DROP — when would you use each in real life?

**Answer:** **DELETE** when you need to remove *specific* rows (it supports WHERE) — e.g. deleting one graduated student. **TRUNCATE** when you want to wipe a table fast but keep its structure — e.g. clearing a staging table before a fresh data load. **DROP** when the table itself is no longer needed — e.g. removing a deprecated feature's table.

**Why they ask:** The single most-asked SQL question. They want the *decision*, not the definitions.

---

### Q2. You ran DELETE without WHERE on a production table. What saves you?

**Answer:** **Transactions.** If the DELETE ran inside an uncommitted transaction, `ROLLBACK` undoes it. That's why experienced engineers wrap risky operations in transactions and verify row counts before `COMMIT`. Without a transaction (autocommit on), you're restoring from backup.

**Why they ask:** Tests whether you understand *why* TCL exists — it's a survival skill, not theory.

---

### Q3. TRUNCATE is DDL while DELETE is DML. Why does the category actually matter?

**Answer:** Because DDL statements typically cause an **implicit commit** in many databases (notably MySQL) — so you often *cannot roll back* a TRUNCATE, while DELETE (DML) can be rolled back inside a transaction. The category predicts transactional behaviour.

**Why they ask:** Separates people who memorized the table from people who understand consequences.

---

### Q4. When would you actually use SAVEPOINT in a real transaction?

**Answer:** In multi-step operations where later steps are optional. Example: booking flow — insert the order (must stay), then try applying a coupon; if the coupon fails, `ROLLBACK TO savepoint` removes only the coupon attempt instead of killing the whole order. Checkpoints inside transactions.

**Why they ask:** SAVEPOINT is the least-used TCL command — knowing a real use case proves depth.

---

### Q5. A junior developer needs read-only access to the Students table. Walk through it.

**Answer:** `GRANT SELECT ON Students TO junior_dev;` — and nothing else. No INSERT/UPDATE/DELETE. If they later need write access, grant it explicitly; to remove, `REVOKE`. Principle: **least privilege** — give the minimum permissions the job needs.

**Why they ask:** Applied DCL. "Least privilege" is the phrase that shows security thinking.

---

### Q6. Why do most references classify SELECT as DQL instead of DML?

**Answer:** DML *changes* data (INSERT/UPDATE/DELETE); SELECT only *reads* it — no state changes, no transaction side effects to commit or roll back. The read-vs-write distinction is fundamental enough to deserve its own category.

**Why they ask:** A nuance question. It checks whether you see the *logic* behind the categories.

---

### Q7. You need to ALTER a table with 10 crore rows in production. What are the risks?

**Answer:** Many ALTER operations **rewrite the entire table** — locking it for reads/writes, spiking I/O, and potentially taking the app down. On huge tables you plan: run during low traffic, use online-schema-change tools, test on a copy first, and have a rollback plan. A one-line command can be a multi-hour operation.

**Why they ask:** Moves from textbook to production reality. Senior engineers think about blast radius.

---

### Q8. You're designing a college database. What do you actually produce at the conceptual, logical and physical stages?

**Answer:** **Conceptual:** entities and relationships — "we need Student, Course, Teacher; students enroll in courses" (maybe an ER sketch). **Logical:** the schema — `Student(Student_ID PK, Name, Email)`, `Enrollment(Student_ID FK, Course_ID FK)`, relationships and constraints, normalized. **Physical:** the implementation — `INT`/`VARCHAR` types, `CREATE TABLE` statements, indexes on `student_id`, partitioning decisions.

**Why they ask:** Applied version of the three models. If you can walk the stages, you truly understand them.

---

### Q9. Can you skip the logical model and go straight from conceptual to physical? What breaks?

**Answer:** You *can*, but you lose the DBMS-independent blueprint. The logical model is where normalization, keys and relationships are reasoned about cleanly. Skipping it means designing tables directly in physical terms — indexes and types before the relationships are right — which produces schemas that are hard to change and full of redundancy. It's like pouring concrete before the architect's drawings.

**Why they ask:** Tests whether you understand each layer's *purpose*, not just its definition.

---

### Q10. Your PostgreSQL stores JSON columns. Is it still an RDBMS? Does that make it NoSQL?

**Answer:** Still an **RDBMS** — its core model is tables, keys and relationships; JSON is just a flexible column type inside that model. It's not NoSQL, because NoSQL describes databases whose *primary* model is non-relational (documents, key-value, graphs). A relational engine with JSON support ≠ a document database.

**Why they ask:** Directly from the notes' interview question, expanded. Precision about categories matters.

---

### Q11. When would you deliberately NOT choose an RDBMS?

**Answer:** When the data has no stable relational shape — e.g. rapidly evolving event logs, social graphs with deep traversals (graph DB territory), or massive write-heavy telemetry where a wide-column store scales cheaper. Also for simple key-value caching (Redis). Choose the model that fits the access pattern, not the default.

**Why they ask:** Judgment call. "It depends on the access pattern" is the mark of a thinking engineer.

---

### Q12. Two apps share one database. App A adds a column; app B breaks. Which concept failed, and how do you prevent it?

**Answer:** **Logical data independence** failed — a conceptual change rippled into an external consumer. Prevention: app B should depend on a **view or API contract**, not raw tables; additive changes (new nullable columns) are safer than renames/deletes; and schema changes need impact review across consumers.

**Why they ask:** Connects Module 1's independence concept to a real multi-app failure. Beautiful cross-topic question.

---

### Q13. Explain why normalization reduces redundancy — with a concrete before/after.

**Answer:** **Before:** one table `Student(Student_ID, Name, Course_ID, Course_Name)` — "DBMS" is repeated for every enrolled student. **After:** split into `Student(Student_ID, Name)` and `Course(Course_ID, Course_Name)` + `Enrollment` — each course name stored once, linked by keys. Update "DBMS" → "Database Systems" in one row instead of a thousand. Less duplication = fewer inconsistency bugs.

**Why they ask:** They don't want the textbook line "normalization reduces redundancy" — they want you to *show* it.

---

### Q14. What's the real difference between a PRIMARY KEY and a UNIQUE constraint?

**Answer:** Both enforce uniqueness, but: a table has **exactly one** primary key (it's the table's identity, used by foreign keys to reference rows), while you can have **many** UNIQUE constraints. Also, primary key columns **cannot be NULL**; UNIQUE columns usually can (depending on DBMS). PK = identity; UNIQUE = "no duplicates" rule on any column.

**Why they ask:** Classic trick question. Everyone knows both mean "unique" — few know the *three* differences.

---

### Q15. A query is slow; you add an index and it's fast. Did you change the logical or physical model — and why didn't the app code need to change?

**Answer:** **Physical** — only storage/access structures changed; tables, columns and relationships are identical. The app didn't change because of **physical data independence**: applications never see physical storage, so physical tuning is invisible to them. That's exactly why DBAs can optimize production without redeploying apps.

**Why they ask:** Ties indexing (Module 2) to data independence (Module 1). Cross-module synthesis impresses interviewers.

---

### Q16. A table has foreign keys pointing to it from three other tables. What happens on DROP vs DELETE?

**Answer:** **DROP** will typically **fail** (or cascade dangerously) because referential integrity protects the referenced rows — most databases block dropping a table others depend on unless you use CASCADE. **DELETE** of specific rows is also blocked by FK constraints if child rows exist — you'd violate referential integrity. The safe path: delete child rows first, or define `ON DELETE CASCADE` deliberately.

**Why they ask:** Tests whether you see tables as a *connected system*, not isolated objects. FK awareness is core RDBMS thinking.

---

> 💡 **How to use this file:** Cover the answers, explain each aloud. Q1, Q12 and Q14 are the highest-frequency real interview questions — master those first. Q6, Q9 and Q15 test whether you *think* in models, which is what separates selected candidates.
