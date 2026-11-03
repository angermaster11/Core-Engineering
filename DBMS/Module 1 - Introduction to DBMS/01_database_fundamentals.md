# Module 1: Introduction to DBMS
### Database Fundamentals — short notes (B.Tech CSE + Interviews)

---

## 1. What is Data?
- **Data** = raw facts, figures, observations — not yet processed.
- Examples: `Rahul`, Roll No `101`, Age `21`, CPI `8.5`, Salary `45000`.
- **Types:** Structured (SQL tables) | Semi-structured (JSON, XML) | Unstructured (images, videos, PDFs)
- **Data vs Information:** Data = raw (`80, 90, 70`); Information = processed with meaning (`Average = 80`).
> 🎤 *Data vs information?* → Data is raw; information is processed data that gives insight.

## 2. What is a Database?
- **Database** = organized collection of logically related data.
- Example — `Students` table: 101/Rahul/CSE/8.5, 102/Aman/IT/7.9, 103/Priya/CSE/9.1
- A database ≠ one table → Students, Teachers, Courses, Fees... many related tables.
- Real world: Instagram stores accounts, posts, followers, messages in databases.

## 3. What is DBMS?
- **DBMS** = software to create, store, retrieve, update & manage data. The **middleman between app and database**.
- Examples: MySQL, PostgreSQL, Oracle, SQL Server (RDBMS); MongoDB (NoSQL).
- Flow: User enters data → App → DBMS validates → stores → retrieve anytime.
```sql
CREATE TABLE Students (student_id INT PRIMARY KEY, name VARCHAR(100), course VARCHAR(50), cpi DECIMAL(3,2));
INSERT INTO Students VALUES (101, 'Rahul', 'CSE', 8.50);
SELECT * FROM Students;
```

## 4. Need for DBMS
- Files (Excel/text) fail at scale — 10,000 students:
- Redundancy, inconsistency, weak security, sharing conflicts, no integrity rules, painful backup, scattered data, concurrency clashes.
- **DBMS fixes:** central management + access control + transactions + constraints + recovery.

## 5. Characteristics of DBMS
- Data independence, security, integrity, sharing, less redundancy, concurrency control, backup/recovery, transaction management, multiple views, query processing (SQL).
- ⭐ **ACID:** Atomicity (all-or-nothing) | Consistency (valid before/after) | Isolation (no interference) | Durability (survives crashes).
- Example: ₹1000 bank transfer → deduct A + add B both succeed, else rollback.

## 6. File System vs DBMS ⭐
| Basis | File System | DBMS |
|---|---|---|
| Storage | Files & folders | Managed database |
| Redundancy | High | Controlled |
| Security | Basic | Fine-grained |
| Consistency | Hard | Built-in |
| Sharing | Limited | Multi-user |
| Concurrency | Weak | Controlled |
| Backup | Manual | Built-in |
| Querying | Custom code | SQL |
| Transactions | No | Yes |

## 7. Advantages & Disadvantages
- ✅ Less redundancy, consistency, security, integrity, concurrency, backup, easy retrieval.
- ❌ Cost, needs skilled DBAs, heavy resources, overhead for tiny apps, single failure hits many apps.
> 🎤 *Always better than files?* → **No.** Files are fine for simple static data; DBMS wins for structure + concurrency + security + transactions.

## 8. Real-World Applications
- Banking (HDFC), E-commerce (Amazon), Healthcare, Education (ERP), Railway/Airline (IRCTC), Social media (Instagram).

## 9. Data Integrity (deep dive)
- **Meaning:** data must be accurate, correct & reliable — no invalid/illogical entries.
- Prevents: duplicate Roll No `101` ❌ | age `-5` ❌ | mandatory name `NULL` ❌.
- Valid vs invalid: age `20` ✅ / `-5`, `150` ❌; mobile `9876543210` ✅ / `98765` ❌; Roll `101` twice ❌.
- **Constraints:** PRIMARY KEY (no dup/NULL ids) | UNIQUE | NOT NULL | CHECK (`age >= 18`) | FOREIGN KEY.
```sql
CREATE TABLE Students (
  roll_no INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  age INT CHECK (age >= 18),
  email VARCHAR(100) UNIQUE
);
```
- Don't mix: **Integrity** = data is correct | **Security** = data is protected | **Consistency** = data isn't contradictory.

## 10. Interview Q&A
- **Q: Data vs Information?** → Raw facts vs processed meaningful data.
- **Q: DBMS in one line?** → Software between app & database managing data.
- **Q: Why DBMS over files?** → Files = redundancy/inconsistency/no concurrency; DBMS fixes via central mgmt + transactions.
- **Q: ACID?** → Atomicity, Consistency, Isolation, Durability (bank transfer example).
- **Q: Always better?** → No, overkill for simple static data.
- **Q: Data integrity?** → Accuracy & reliability via constraints.
- **Q: Integrity vs Security vs Consistency?** → Correct vs protected vs non-contradictory.
