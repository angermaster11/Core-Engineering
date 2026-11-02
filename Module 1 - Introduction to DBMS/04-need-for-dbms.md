# 4. Need for DBMS

- Why not just Excel/text files? With 10,000 students, files break down:

## Problems with File System
- **Data redundancy** — same info stored multiple times
- **Data inconsistency** — same student, different values in different files
- **Poor security** — hard to control who accesses what
- **Data sharing issues** — multiple users → conflicts
- **No integrity** — can't enforce rules like unique roll numbers
- **Backup/recovery** — painful after failure
- **Data isolation** — scattered files, hard to manage
- **Concurrency** — simultaneous edits → conflicts

## DBMS fixes this via
- Centralized data management, access control, transactions, constraints, recovery.
