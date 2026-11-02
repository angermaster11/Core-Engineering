# 10. Interview Questions — Test Yourself

**Q1. Data vs Information?**
→ Data = raw facts (`80, 90, 70`). Information = processed data with meaning (`Average = 80`).

**Q2. What is DBMS in one line?**
→ Software that sits between applications and the database to create, store, retrieve & manage data.

**Q3. Why DBMS over file system?**
→ Files cause redundancy, inconsistency, weak security, no concurrency control. DBMS fixes all via central management + transactions + constraints.

**Q4. What are ACID properties?**
→ Atomicity (all-or-nothing), Consistency (valid before/after), Isolation (no interference), Durability (survives crashes). Example: bank transfer.

**Q5. Is DBMS always better than file system?**
→ No — overkill for simple static data. Worth it when you need structure, concurrency, security, transactions.

**Q6. What is data integrity?**
→ Data stays accurate, valid & reliable — enforced via constraints (PRIMARY KEY, UNIQUE, NOT NULL, CHECK, FOREIGN KEY).

**Q7. Integrity vs Security vs Consistency?**
→ Integrity = data is correct. Security = data is protected. Consistency = data isn't contradictory.
