# Module 2: Data Models & SQL

---

## 1. Conceptual vs Logical vs Physical Data Models

### Conceptual Model — WHAT do we need?
- Business-level view: **what entities and relationships** does the business require?
- No tables, no keys, no data types — just *what exists*.
- Example (College Management System): we need **Student**, **Course**, **Teacher** — and "a student enrolls in a course."

### Logical Model — HOW is it organized?
- **How data is arranged and connected:** attributes, tables, relationships, primary keys, foreign keys, constraints, normalization.
- Example:
  - `Student(Student_ID, Name, Email)`
  - `Course(Course_ID, Course_Name)`
  - `Enrollment(Student_ID, Course_ID)`
  - Plus: PKs, FKs, one-to-many / many-to-many relationships, referential integrity.
- Still **no physical details** — no indexes, no disk talk, no data types yet.

### Physical Model — HOW is it implemented and stored?
- **Data types, indexes, storage structures, partitioning**, disk-level implementation.
- Example:
```sql
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(150) UNIQUE
);

CREATE INDEX idx_student_name ON Student(name);
```
- Decisions: `INT`, `VARCHAR`, `DATE`; B-Tree / Hash indexes; partitioning; file organization.
- Note: disk location and disk type *can* be physical details, but they aren't specified in every physical model.

### One-liners (memorize these)
- **Conceptual** = What do we need? (entities & relationships)
- **Logical** = How is data structured and related? (tables, keys, constraints)
- **Physical** = How do we implement and store it? (types, indexes, storage)

---

## 2. Database Languages — 5 Categories of SQL Commands

| Language | Purpose | Commands | Remember |
|---|---|---|---|
| **DDL** | Define/modify structure | CREATE, ALTER, DROP, TRUNCATE | Structure |
| **DML** | Insert, update, delete data | INSERT, UPDATE, DELETE | Manipulate |
| **DQL** | Retrieve data | SELECT | Query |
| **DCL** | Control access & permissions | GRANT, REVOKE | Control |
| **TCL** | Manage transactions | COMMIT, ROLLBACK, SAVEPOINT | Transactions |

> 🎤 Interview tip: SELECT is classified as **DQL** by most references (some older ones put it under DML).

---

## 3. SQL Commands — All 13 with Examples ⭐

We'll use one `Student` table throughout. **Practice all 13 on a real table — that's how this sticks.**

### DDL Commands

**1. CREATE** — creates a new table/database.
```sql
CREATE TABLE Student (id INT, name VARCHAR(100), age INT);
```

**2. ALTER** — modifies existing table structure.
```sql
ALTER TABLE Student ADD email VARCHAR(100);
```

**3. DROP** — permanently deletes the table *including its structure*.
```sql
DROP TABLE Student;
```

**4. TRUNCATE** — deletes **all rows** but keeps the table structure.
```sql
TRUNCATE TABLE Student;
```

### DML Commands

**5. INSERT** — adds new records.
```sql
INSERT INTO Student (id, name, age) VALUES (1, 'Rahul', 21);
```

**6. UPDATE** — modifies existing records (**always use WHERE!**).
```sql
UPDATE Student SET age = 22 WHERE id = 1;
```

**7. DELETE** — deletes specific records (or all, if no WHERE).
```sql
DELETE FROM Student WHERE id = 1;
```

### DQL Command

**8. SELECT** — retrieves data.
```sql
SELECT * FROM Student;
SELECT name, age FROM Student;
```

### DCL Commands

**9. GRANT** — gives permissions to a user.
```sql
GRANT SELECT, INSERT ON Student TO user1;
```

**10. REVOKE** — removes previously granted permissions.
```sql
REVOKE INSERT ON Student FROM user1;
```

### TCL Commands

**11. COMMIT** — permanently saves the current transaction.
```sql
UPDATE Student SET age = 23 WHERE id = 1;
COMMIT;
```

**12. ROLLBACK** — undoes uncommitted changes.
```sql
DELETE FROM Student WHERE id = 1;
ROLLBACK;  -- deletion is undone
```

**13. SAVEPOINT** — a checkpoint *inside* a transaction you can roll back to.
```sql
START TRANSACTION;
INSERT INTO Student VALUES (1, 'Rahul', 21);
SAVEPOINT sp1;
INSERT INTO Student VALUES (2, 'Aman', 22);
ROLLBACK TO sp1;  -- Aman's insert removed, Rahul's stays
COMMIT;
```

### Complete Revision Table
| Command | Category | Purpose |
|---|---|---|
| CREATE | DDL | Create structure |
| ALTER | DDL | Modify structure |
| DROP | DDL | Delete structure |
| TRUNCATE | DDL | Remove all rows, keep structure |
| INSERT | DML | Add records |
| UPDATE | DML | Modify records |
| DELETE | DML | Delete records |
| SELECT | DQL | Retrieve records |
| GRANT | DCL | Give permissions |
| REVOKE | DCL | Remove permissions |
| COMMIT | TCL | Save transaction permanently |
| ROLLBACK | TCL | Undo uncommitted changes |
| SAVEPOINT | TCL | Checkpoint inside a transaction |

### ⭐ DELETE vs TRUNCATE vs DROP (top interview question)
| Basis | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| Removes | Rows (WHERE supported) | All rows (no WHERE) | Entire table |
| Table remains? | Yes | Yes | No |
| Category | DML | DDL | DDL |
| Logging | Row-by-row (heavier) | Minimal | Object removed |

> ⚠️ DBMS-dependent detail: exact rollback behaviour, identity reset and trigger firing vary — e.g. PostgreSQL supports transactional TRUNCATE, while MySQL DDL causes implicit commits.

---

## 4. DBMS vs RDBMS

### The one line to remember
**Every RDBMS is a DBMS, but every DBMS is not an RDBMS.** RDBMS is one *type* of DBMS.

### What is DBMS?
- General software to store, manage, retrieve and manipulate data — using **any** data model.
- Examples: hierarchical DBMS (IBM IMS), XML databases (BaseX), file-based systems.

### What is RDBMS?
- A DBMS that stores data in **tables** (rows & columns) and connects them via **relationships**.
- Examples: MySQL, PostgreSQL, Oracle, Microsoft SQL Server.
- College example: `Student` ↔ `Enrollment` ↔ `Course`, linked by foreign keys:
```
Student(Student_ID PK, Name)        Course(Course_ID PK, Course_Name)
Enrollment(Student_ID FK, Course_ID FK)
```

### Differences Table
| Basis | DBMS | RDBMS |
|---|---|---|
| Full form | Database Management System | Relational DBMS |
| Storage | Depends on data model | Tables (rows & columns) |
| Relationships | Not necessarily supported | Supported between tables |
| Keys | Not necessarily relational keys | Primary & Foreign Keys |
| Normalization | Not necessarily applicable | Supported |
| Integrity | Depends on system | Relational integrity constraints |
| Redundancy | May be higher | Reduced via normalization |
| Transactions | Varies by system | Commonly ACID |
| Examples | XML DB, hierarchical DBMS | MySQL, PostgreSQL, Oracle |

### Features of RDBMS (beyond basic DBMS)
- Table-based storage, **Primary Key** (unique identity), **Foreign Key** (relationships), **normalization**, **referential integrity**, **SQL**, **ACID** transactions.

### Limitations (know the trade-offs)
- **DBMS:** some models handle relationships poorly; redundancy varies; querying can be awkward.
- **RDBMS:** complex joins get expensive at scale; schema migrations need care; horizontal scaling is harder; not ideal for highly unstructured data.
- Honest note: modern RDBMSs (PostgreSQL, MySQL) support JSON, scaling and distributed setups too — these are *general* trade-offs, not absolute walls.

### Practical Examples
- Hierarchical: IBM IMS (tree structure) | XML: BaseX | RDBMS: MySQL/PostgreSQL (related tables).

### Interview Quick Revision
- **DBMS** = general system for managing databases.
- **RDBMS** = DBMS built on the relational model (tables).
- **PK + FK** = identify records + connect tables.

> 🎤 *Can an RDBMS store unstructured data?* → **Yes.** PostgreSQL and MySQL store JSON and semi-structured data — but their *core model* remains relational.
