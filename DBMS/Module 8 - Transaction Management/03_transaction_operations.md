# 8.3 Transaction Operations

Six operations manage a transaction. Setup for all examples: Rahul = ₹5,000 (account 101), Amit = ₹2,000 (account 102). Rahul transfers ₹1,000 to Amit.

## 1. BEGIN — Start a transaction

```sql
BEGIN;
```
Tells DBMS a new transaction is starting. After BEGIN, we can READ and WRITE.

## 2. READ — Retrieve data

```sql
SELECT balance FROM accounts WHERE account_id = 101;
-- Output: 5000
```
READ = fetch data from the database. (Note: READ/WRITE describe operations; they are not literal SQL keywords like BEGIN/COMMIT.)

## 3. WRITE — Modify data

```sql
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 101;  -- Rahul: 5000 → 4000
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 102;  -- Amit:  2000 → 3000
```
WRITE = modify/insert data.

## 4. COMMIT — Save permanently

```sql
COMMIT;
```
All changes become permanent. Even a server crash afterward cannot undo them.

## 5. ROLLBACK — Undo uncommitted changes

```sql
ROLLBACK;
```
Cancels the current transaction and restores previous values. *Example:* ₹1,000 deducted from Rahul, then an error before crediting Amit → ROLLBACK restores Rahul ₹5,000, Amit ₹2,000.

**Important:** After a transaction commits, its own ROLLBACK can no longer undo it.

## 6. SAVEPOINT — Checkpoint inside a transaction

Creates a temporary checkpoint so you can roll back **part** of a transaction instead of all of it.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 101;  -- 10000 → 9000
SAVEPOINT sp1;                                                        -- checkpoint
UPDATE accounts SET balance = balance - 2000 WHERE account_id = 101;  -- 9000 → 7000
UPDATE accounts SET balance = balance - 3000 WHERE account_id = 101;  -- 7000 → 4000

ROLLBACK TO SAVEPOINT sp1;   -- undoes only the last two deductions → balance back to 9000
COMMIT;
```

## Complete Example (all six together)

```sql
BEGIN;
SELECT balance FROM accounts WHERE account_id = 101;          -- READ
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 101;  -- WRITE
SAVEPOINT sp1;                                                -- checkpoint
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 102;  -- WRITE
COMMIT;                                                       -- save
-- If an error occurred before COMMIT: ROLLBACK;
```

## Quick Revision Table

| Operation | Purpose |
|---|---|
| BEGIN | Start transaction |
| READ | Retrieve data |
| WRITE | Modify data |
| COMMIT | Save changes permanently |
| ROLLBACK | Undo uncommitted changes |
| SAVEPOINT | Checkpoint for partial rollback |

## Test Your Understanding

**Q1.** Which operation starts a new transaction? → **BEGIN**
**Q2.** Which operation permanently saves changes? → **COMMIT**
**Q3.** Undo only the last two operations, not the whole transaction? → **SAVEPOINT with ROLLBACK TO**
**Q4.** Can you rollback after a successful commit? → **No** (not with the same transaction)

> COMMIT saves everything, ROLLBACK cancels uncommitted changes, SAVEPOINT allows partial rollback — the trio most asked in interviews and practical SQL.
