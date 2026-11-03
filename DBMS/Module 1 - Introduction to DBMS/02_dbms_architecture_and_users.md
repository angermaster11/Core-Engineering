# Module 1 (Part 2): DBMS Architecture & Database Users

---

## 1. DBMS Architecture

- **Meaning:** how a database system's components are organized and how they communicate with each other.
- Example flow (college app login):
  1. You open the application
  2. App sends request → server
  3. Server fetches user details from database
  4. Database returns result
  5. App shows you the dashboard

### 1-Tier Architecture
- **Definition:** user, application, DBMS and database — **all on the same machine**.
- Example: MySQL/SQLite on your laptop; you run queries directly.
- How it works: user → DBMS → database → result (no network involved).
- ✅ Simple, easy to develop, no network needed, great for learning.
- ❌ Not scalable, not for multiple users, weak security isolation.
> 🎤 *Example of 1-tier?* → Running MySQL locally and querying from the same machine.

### 2-Tier Architecture
- **Definition:** two layers — **Client** ↔ **Database Server** (over a network).
- Example: college desktop app (client) connecting to a MySQL server.
- Flow: user enters student ID → client sends SQL → server executes → returns result.
- Technologies: **JDBC** (Java), **ODBC** — connect client apps to databases.
- ✅ Easy to develop, fast for small systems, simpler than 3-tier.
- ❌ DB directly exposed to clients, hard to manage many clients, limited scalability, business logic lives in the client.
> 🎤 *1-tier vs 2-tier?* → 1-tier: everything on one machine. 2-tier: client and DB server talk over a network.

### 3-Tier Architecture ⭐ (backend interviews love this)
- **Definition:** three independent layers:
  1. **Presentation Layer** — UI (login page, dashboard). Tech: HTML, CSS, JS, React, Next.js.
  2. **Application Layer** — the brain/business logic (auth, JWT, payments, validation). Tech: Node.js, Express, Django, FastAPI, Spring Boot.
  3. **Database Layer** — actual data (users, orders). Tech: MySQL, PostgreSQL, MongoDB, Oracle.
- Practical example (login): React form → HTTP → Node/Express validates → queries MongoDB/PostgreSQL → response travels back up.
- ✅ Better security (DB hidden behind app layer), easy maintenance, scalable, separation of concerns.
- ❌ More complex, higher cost, extra network latency.
> 🎤 *Why better for large apps?* → Each layer can be maintained, secured and scaled independently.

### Centralized Database System
- **Definition:** entire database at **one central location**; users access it over the network.
- Example: university with 5 departments, one DB server in the main data center.
- ✅ Easy management, centralized security, easier backup, less duplication.
- ❌ Single point of failure, heavy load = slow, network failure = no access.
> 🎤 *If the central server goes down?* → Users lose access until restore/failover kicks in.

### Distributed Database System
- **Definition:** data stored across **multiple locations**, but presented as one logical database.
- Example: e-commerce company with DB servers in Delhi, Mumbai, Bangalore, Kolkata.
- Flow: user request → system finds where the data lives → processes there → returns result.
- **Types:** Homogeneous (same DBMS everywhere, e.g. all PostgreSQL) | Heterogeneous (different DBMS per location).
- **Key concepts:**
  - **Replication** = copies of the same data at multiple locations (better availability, fault tolerance).
  - **Fragmentation** = database split into parts, each part stored at a different location (Delhi students in Delhi, Mumbai students in Mumbai).
- ✅ Better availability, faster local access, geographic scale, fault tolerance.
- ❌ Complex management, sync difficulties, network dependency, hard distributed transactions.
> 🎤 *Centralized vs distributed?* → One location vs many; easier management vs complex; single-point risk vs built-in fault tolerance.

### Client-Server Architecture
- **Definition:** clients request services, servers process and respond.
- Example: YouTube — mobile app (client) → backend servers → database (user details, video metadata) + CDN (video files).
- **Types:** Two-tier (client → DB directly, e.g. desktop app + MySQL) | Three-tier (client → app server → DB, e.g. React → Node → PostgreSQL).
- ✅ Centralized resources, easy multi-client management, access control.
- ❌ Server failure hits everyone, needs network, infrastructure costs.

### Interview Revision — Key Differences

**1-Tier vs 2-Tier vs 3-Tier:**
| Basis | 1-Tier | 2-Tier | 3-Tier |
|---|---|---|---|
| Layers | 1 | 2 | 3 |
| Communication | Direct | Client ↔ DB | Client ↔ App ↔ DB |
| Network | Not needed | Usually needed | Usually needed |
| Security | Limited | Moderate | Better |
| Scalability | Low | Limited | Better |
| Example | Local SQLite | Desktop + MySQL | React + Node + PostgreSQL |

**Centralized vs Distributed:**
| Basis | Centralized | Distributed |
|---|---|---|
| Data location | One place | Multiple places |
| Management | Easier | Complex |
| Fault tolerance | Needs extra redundancy | Built-in via replication |
| Synchronization | Simple | Complex |

**Don't confuse these three:**
- **3-Tier** = application divided into presentation/app/database layers.
- **Distributed DB** = data stored across physical locations.
- **Client-Server** = how clients and servers communicate.
- (A 3-tier application can also use a distributed database!)

### Deep Dive: Why is direct DB exposure in 2-tier dangerous?
- In 2-tier, the client app holds database credentials:
```js
const connection = mysql.createConnection({
  host: "192.168.1.100", user: "admin",
  password: "admin123", database: "college"
});
```
- **Problems:** (1) credentials can be extracted by reverse-engineering the app; (2) direct access → a compromised client could run `DELETE FROM Students`; (3) business logic on the client can be bypassed (e.g. `UPDATE Students SET cpi = 10.0` skipping the "only admin" rule).
- **3-tier fix:** client talks HTTP to Node.js; Node authenticates, checks the role, then queries the DB. Credentials stay on the backend:
```js
app.put("/students/:id/cpi", authenticate, async (req, res) => {
  if (req.user.role !== "admin") return res.status(403).json({ message: "Access denied" });
  await db.query("UPDATE students SET cpi = $1 WHERE id = $2", [req.body.cpi, req.params.id]);
  res.json({ message: "CPI updated" });
});
```
- Note: 2-tier isn't *automatically* insecure — restricted DB permissions, firewalls and VPNs help. But direct access = bigger attack surface.

### Deep Dive: Replication vs Fragmentation
| Basis | Replication | Fragmentation |
|---|---|---|
| Meaning | Copies of data | Division of data |
| Storage | Same data, multiple places | Different parts, different places |
| Example | 9,000 records copied on 3 servers | 3,000 records per server |
| Main benefit | Availability | Local access + distribution |
> Interview point: distributed databases can use replication, fragmentation, or both.

---

## 2. Database Users

### 1. Database Administrator (DBA) — *the caretaker*
- Manages, secures and monitors the database system.
- Responsibilities: installation, security, user accounts & permissions, backup/recovery, performance tuning, monitoring.
```sql
CREATE USER faculty WITH PASSWORD 'secure123';
GRANT SELECT ON Students TO faculty;
REVOKE SELECT ON Students FROM faculty;
```
> 🎤 *Database crashes?* → DBA investigates and restores via backups + transaction logs.

### 2. Database Designer — *decides the structure*
- Designs tables, relationships, keys, constraints, normalization.
- Example: Students (`student_id` PK, `course_id` FK) ↔ Courses (`course_id` PK, `course_name`).
> 🎤 *Who decides the schema?* → Database Designer.

### 3. Application Programmer — *builds on top*
- Develops apps that talk to the DB: SQL queries, business logic, auth.
```js
app.post("/students", async (req, res) => {
  const { name, course_id } = req.body;
  const result = await db.query(
    "INSERT INTO students (name, course_id) VALUES ($1, $2) RETURNING *",
    [name, course_id]);
  res.json(result.rows[0]);
});
```
> Designer = structure; Programmer = software using that structure.

### 4. End Users — *use the application*
- Students checking results, teachers uploading attendance, accountants checking fees.
- **Types:** Naive/parametric (bank cashier — repetitive tasks) | Casual (principal checking reports occasionally) | Sophisticated (data analysts writing complex queries) | Standalone (shopkeeper using MS Access).

### 5. Database Analyst — *extracts insights*
- Analyzes data, writes reporting queries, finds trends, builds dashboards.
```sql
SELECT course, COUNT(*) AS total_students, AVG(cpi) AS average_cpi
FROM Students GROUP BY course;
```

### All Users — One Table (exams love this)
| Role | Main responsibility |
|---|---|
| DBA | Manages & maintains the database |
| Database Designer | Designs structure & relationships |
| Application Programmer | Builds database applications |
| End User | Uses applications |
| Database Analyst | Analyzes data, generates insights |

### Remember Everything — One Example (College Management System)
- **Designer** → designs Students, Courses, Fees tables + relationships.
- **DBA** → creates users, permissions, backups, monitors performance.
- **Programmer** → builds the portal (React + Node + PostgreSQL).
- **End User** → students check attendance and results.
- **Analyst** → placement statistics and performance reports.
