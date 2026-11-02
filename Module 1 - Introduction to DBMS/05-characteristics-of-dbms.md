# 5. Characteristics of DBMS

- **Data independence** — storage/schema changes don't break apps
- **Data security** — blocks unauthorized access
- **Data integrity** — keeps data accurate & consistent
- **Data sharing** — multiple users, one source
- **Reduced redundancy** — minimal duplication
- **Concurrency control** — handles simultaneous transactions
- **Backup & recovery** — restore after failures
- **Transaction management** — reliable execution
- **Multiple views** — different users see different views
- **Query processing** — efficient retrieval (SQL)

## ⭐ ACID Properties (interview favourite)
- **Atomicity** — all operations happen, or none (no half-done)
- **Consistency** — DB stays valid before & after transaction
- **Isolation** — concurrent transactions don't mess each other up
- **Durability** — committed changes survive crashes

> Example: bank transfer ₹1000 → deduct from A **and** add to B must both succeed; if one fails → rollback.
