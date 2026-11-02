# Module 1 (Part 3): Types of DBMS & Three-Schema Architecture

---

## 1. Types of DBMS (short)

### 1. Hierarchical DBMS
- Data stored in a **Tree structure** (Parent → Child); each child has **only one parent**.
- Example: IBM IMS. Use: organization charts, file systems.

### 2. Network DBMS
- Data stored in a **Graph-like structure**; a child can have **multiple parents** (many-to-many).
- Example: IDMS.

### 3. Relational DBMS (RDBMS)
- Data in **Tables** (rows & columns); tables linked via **Primary Keys & Foreign Keys**; uses **SQL**.
- Examples: MySQL, PostgreSQL, Oracle.

### 4. Object-Oriented DBMS (OODBMS)
- Data stored as **Objects** — classes, inheritance, encapsulation (like OOP).
- Examples: ObjectDB, db4o.

### 5. NoSQL Database
- **Flexible, non-tabular** formats for unstructured/semi-structured data.
- Types: Document, Key-Value, Graph, Column-Family.
- Examples: MongoDB, Redis, Cassandra.

### Quick Revision
| DBMS Type | Data Structure |
|---|---|
| Hierarchical | Tree |
| Network | Graph |
| Relational | Tables |
| Object-Oriented | Objects |
| NoSQL | Flexible formats |

> 🎤 Interview tip: remember the chain — **Tree → Graph → Tables → Objects → Flexible**. And note: NoSQL is a *category* of data models, not one single structure.

---

## 2. Three-Schema Architecture ⭐ (important — read carefully)

### The problem it solves
- University database: Students, Teachers, Courses, Marks.
- A **student** needs only their marks; a **teacher** needs to update marks; the **DBA** manages everything.
- Not everyone should see the complete internal structure → Three-Schema Architecture divides the database into **3 levels**.

### Level 1: External Level (View Level)
- The **highest level** — different users get **different views** of the database.
- Each user sees only what they need; the rest stays hidden.

| User | What they see |
|---|---|
| Student | Name, Roll No, Marks |
| Teacher | Student details, Marks |
| Accountant | Student name, Fee details |

- A student never sees a teacher's salary or another student's private details.
- **One-liner: External = What the user sees.**

### Level 2: Conceptual Level (Logical Level)
- Defines the **complete logical structure**: which tables exist, their columns, relationships, constraints, data types.
- Example — university database:
```
STUDENT(student_id, name, email, course_id)
COURSE(course_id, course_name)
MARKS(student_id, subject, marks)
```
- It says *what* is stored and *how it's related* — but NOT how it's stored on disk.
- **One-liner: Conceptual = What data is stored + logical relationships.**

### Level 3: Internal Level (Physical Level)
- The **lowest level** — defines **how data is physically stored**.
- Covers: disk files & pages structure, indexing (B-Tree, Hash), compression, storage allocation.
- Example: STUDENT table has 10 lakh records → DB decides page layout, whether to index `student_id`, how to retrieve efficiently.
- **One-liner: Internal = How data is physically stored.**

### Three Levels — Comparison
| Level | Main question | Example |
|---|---|---|
| External | User kya dekhega? | Student marks view |
| Conceptual | Database mein kya hai? | Tables & relationships |
| Internal | Data store kaise hoga? | Files, indexes, pages |

### Data Abstraction
- **Meaning:** hide unnecessary implementation details, show only what's needed.
- Real-life example: ATM — you enter amount, see balance; you never know how the bank's server stores data.
- Three levels of abstraction:
  - **Physical** — hides how data is stored physically.
  - **Logical** — hides the internal logical design.
  - **View** — shows each user only their required data.
- Note: abstraction and Three-Schema are closely related, but **not exactly the same concept**.

### Data Independence ⭐ (top interview topic)
- **Meaning:** changing one level should have **minimum or zero impact** on upper levels.
- Two types:

**A. Physical Data Independence** *(easier)*
- Changes at the **Internal level** don't affect the **Conceptual level**.
- Example: move storage from HDD → SSD; tables, columns and queries stay exactly the same.
- Other examples: adding/removing an index, changing file organization, enabling compression.

**B. Logical Data Independence** *(harder)*
- Changes at the **Conceptual level** don't affect the **External level** (user views).
- Example: add a `phone` column to STUDENT — existing student views showing only id/name/email don't need any change.
- Other examples: adding a new table, splitting tables, adding relationships.

**Physical vs Logical — head to head:**
| Basis | Physical | Logical |
|---|---|---|
| Changes in | Internal level | Conceptual level |
| Upper level protected | Conceptual | External |
| Difficulty | Easier | Harder |
| Example | Add an index | Add a new column |

> 🎤 *Which is harder to achieve?* → **Logical.** Conceptual changes can ripple across multiple user views and applications.

### Complete Picture — One Flow
```
External Level          →  External/Conceptual Mapping
(Student / Teacher / Accountant views)
                            ↓
Conceptual Level        →  Conceptual/Internal Mapping
(STUDENT, TEACHER, COURSE, MARKS — tables & relationships)
                            ↓
Internal Level
(Data files, indexes, pages, disk storage — HDD/SSD)
```

### Interview Q&A — Test Yourself
- **Q1. Which level defines physical storage?** → Internal (files, pages, indexes).
- **Q2. Which level gives different views to users?** → External.
- **Q3. Adding an index without changing tables = ?** → Physical data independence (physical structures changed, logical schema untouched).
- **Q4. Which independence is harder?** → Logical (affects multiple views/apps).
- **Q5. Main purpose of data abstraction?** → Hide unnecessary implementation details.

> 💡 Advice: don't just memorize definitions. Master the **External → Conceptual → Internal flow** and the **Physical vs Logical** examples — interviews test concepts, not rattas.
