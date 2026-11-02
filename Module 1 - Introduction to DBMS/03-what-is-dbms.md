# 3. What is DBMS?

- **DBMS** = software to create, store, retrieve, update, manage data in a database.
- Simple words: **DBMS is the middleman between application/user and the database.**

## Examples
- MySQL, PostgreSQL, Oracle, SQL Server → Relational (RDBMS)
- MongoDB → NoSQL (document DB)

## How it works (college app example)
1. User enters student details
2. App sends request → DBMS
3. DBMS validates & processes
4. DBMS stores in database
5. Retrieve later anytime

## Quick SQL
```sql
CREATE TABLE Students (
  student_id INT PRIMARY KEY,
  name VARCHAR(100),
  course VARCHAR(50),
  cpi DECIMAL(3,2)
);

INSERT INTO Students VALUES (101, 'Rahul', 'CSE', 8.50);
SELECT * FROM Students;
```
