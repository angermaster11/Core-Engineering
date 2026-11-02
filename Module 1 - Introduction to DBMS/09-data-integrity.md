# 9. Data Integrity (deep dive)

- **Simple meaning:** data in the DB must be **accurate, correct & reliable** — no wrong/invalid/illogical data.

## Problems it prevents
- **Duplicates** — two students with Roll No `101` ❌ (must be unique)
- **Invalid values** — age `-5` ❌ (logically impossible)
- **Missing required data** — name `NULL` when mandatory ❌

## Valid vs Invalid (quick sense)
- Age `20` ✅ valid | `-5`, `150` ❌ invalid (out of range)
- Mobile `9876543210` ✅ | `98765`, `ABCDEFGHIJ` ❌
- Roll No `101` twice ❌ (already taken by Rahul)

## DBMS enforces it via Constraints
| Constraint | Stops... |
|---|---|
| PRIMARY KEY | Duplicate & NULL ids |
| UNIQUE | Duplicate values |
| NOT NULL | Empty values |
| CHECK | Invalid values (e.g. `age >= 18`) |
| FOREIGN KEY | Broken links between tables |

```sql
CREATE TABLE Students (
  roll_no INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  age INT CHECK (age >= 18),
  email VARCHAR(100) UNIQUE
);
-- INSERT (102,'Aman',-5,...) → DBMS rejects it ✅
```

> 🎤 Interview: *What is data integrity?* → Accuracy, consistency & reliability of data — ensured via constraints so only valid data lives in the DB.

## Don't mix these up
- **Integrity** = data is valid & correct
- **Security** = data is protected from unauthorized access
- **Consistency** = data isn't contradictory across the DB
