# Module 3: ER Model — 3.4 Relationships

> Relationships me **do important concepts** hain, inhe confuse mat karna:
> 1. **Cardinality Ratio** — ek entity ka doosri entity ke saath *kitna* connection ho sakta hai (1:1, 1:N, M:N).
> 2. **Degree of Relationship** — relationship me *kitni entity sets* participate kar rahi hain (unary, binary, ternary).

---

## Cardinality Ratios

### 1. One-to-One (1:1)
- Ek entity of A → at most one entity of B, **aur vice versa**.
- Example: **Person ↔ Passport**
  - One person has one passport; one passport belongs to one person.
```
Person_ID | Passport_ID
P101      | PP101
P102      | PP102
```
- Other examples: Person ↔ Aadhaar (one-to-one mapping) | Employee ↔ Company ID Card.
- Yaad rakho: **One record ↔ One record.**

### 2. One-to-Many (1:N)
- One entity of A → multiple entities of B; but each entity of B → at most one entity of A.
- Example: **Department → Employees**
  - One department has many employees; one employee works in one department.
```
Department | Employee
IT         | Rahul
IT         | Aman
HR         | Priya
```
- Other examples: University → Students | Customer → Orders | Teacher → Students.
- Yaad rakho: **One parent → Multiple children.**

### 3. Many-to-One (N:1)
- Many entities of A → one entity of B.
- Example: **Employees → Department** (many employees work in one department).
- ⭐ Key insight: **1:N aur N:1 same relationship hai, bas direction opposite** —
  - Department → Employee = **1:N**
  - Employee → Department = **N:1**

### 4. Many-to-Many (M:N)
- Multiple entities of A ↔ multiple entities of B.
- Example: **Students ↔ Courses** — one student multiple courses me enroll; one course me multiple students.
```
Student | Course
Rahul   | DBMS
Rahul   | OS
Aman    | DBMS
Priya   | CN
```
- ⭐ **Practical SQL implementation:** M:N ko relational database me **junction table** (associative table) se implement karte hain:
```sql
CREATE TABLE Student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Course (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Student(student_id),
    FOREIGN KEY (course_id) REFERENCES Course(course_id)
);
```
`Enrollment` table hi M:N relationship ko implement karti hai.

---

## Degree of Relationship

### 5. Unary Relationship (Recursive) — Degree = 1
- Jab ek entity **khud ke saath** relationship banaye.
- Example: **Employee MANAGES Employee** (ek employee doosre ko manage karta hai).
```
Employee | Manager
Rahul    | Amit
Priya    | Amit
Aman     | Rahul
```
- Other examples: Employee supervises Employee | Person is parent of Person | Category contains Category (nested categories).
- Degree = **1** (sirf ek entity set participate karti hai).

### 6. Binary Relationship — Degree = 2
- Jab **do entity sets** relationship me participate karein.
- Example: **Student ENROLLS_IN Course** (Student + Course = 2 entities).
- Other examples: Employee works in Department | Customer places Order.
- Degree = **2**. (Sabse common type.)

### 7. Ternary Relationship — Degree = 3
- Jab **teen entity sets** ek hi relationship me participate karein.
- Example: **Supplier SUPPLIES Product to Project** — ABC Company supplies Laptop to College ERP project.
- Three entities: Supplier + Product + Project. Relationship = SUPPLIES.
- Degree = **3**.
- ⚠️ Important: ternary relationship ko **blindly teen binary relationships me break mat karna** — original meaning ya constraints lose ho sakte hain.

### Degree Summary
| Degree | Relationship | Meaning |
|---|---|---|
| 1 | Unary | One entity set |
| 2 | Binary | Two entity sets |
| 3 | Ternary | Three entity sets |
| N | N-ary | N entity sets |

---

## ⭐ Cardinality vs Degree — Bahut Important (dono confuse mat karna)

| Cardinality | Degree |
|---|---|
| Number of associated entities **per entity** | Number of **participating entity sets** |
| 1:1, 1:N, M:N | Unary, Binary, Ternary |
| Relationship **mapping** | Relationship **structure** |

Examples (ek relationship ke dono aspects):
- Student ENROLLS_IN Course → Cardinality = **M:N**, Degree = **Binary** (2)
- Employee MANAGES Employee → Cardinality = 1:N, Degree = **Unary** (1)
- Supplier SUPPLIES Product to Project → Degree = **Ternary** (3)

---

## Interview Practice Q&A
- **Q: One department has multiple employees. Which cardinality?** → 1:N
- **Q: Student enrolls in multiple courses, each course has multiple students?** → M:N
- **Q: Employee manages another employee. What is the degree?** → Unary (1)
- **Q: Supplier supplies Product to Project. What is the degree?** → Ternary (3)
- **Q: Student enrolls in Course, cardinality M:N — what is its degree?** → Binary (2)
