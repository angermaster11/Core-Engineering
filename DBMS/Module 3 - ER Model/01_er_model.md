# Module 3: ER Model (Entity-Relationship Model)

> ER Model database design ka foundation hai — sirf definitions mat ratna, design me kaise use hota hai ye samajhna zaroori hai. Hum College Management System ka example lekar chalenge.

---

## 3.1 ER Model Fundamentals

### 1. Entity
- **Entity** = real-world object jiske baare me hum database me information store karna chahte hain.
- Examples: Student, Teacher, Course, Department, Employee.
- Example: Roll_No 101, Rahul, 21 — yahan **Rahul ek entity** hai.
> 🎤 Interview line: *"An entity is a distinguishable real-world object that has an independent existence."*

### 2. Entity Set
- **Entity Set** = same type ki multiple entities ka collection.
- Example: college ke 1000 students (Rahul, Aman, Priya, Rohit...) — sab individual entities, aur inka collection = **Student Entity Set**.

### 3. Attributes
- **Attributes** = entity ki properties/characteristics.
- Student ke attributes: Roll_No, Name, Age, Email, Phone, Address.
- ER symbol: **Oval** = Attribute | **Rectangle** = Entity.

### 4. Relationship
- **Relationship** = two or more entities ke beech association (connection).
- Examples: Student **ENROLLS_IN** Course | Teacher **TEACHES** Course | Employee **WORKS_FOR** Department.
- Example: Rahul (Student) —ENROLLS_IN→ DBMS (Course).

### 5. Relationship Set
- **Relationship Set** = same type ke multiple relationships ka collection.
- Example: Rahul→DBMS, Aman→OS, Priya→CN, Rohit→DBMS — ye saare ENROLLS_IN relationships milkar relationship set banate hain.
- Yaad rakhna: **Relationship** = "Rahul enrolls in DBMS" (one) | **Relationship Set** = "all students' enrollments" (all).

### 6. Entity vs Entity Set
| Entity | Entity Set |
|---|---|
| Single object | Collection of objects |
| Rahul | All students |
| One teacher | All teachers |
| One course | All courses |

---

## 3.2 Types of Attributes ⭐ (interviews me examples dekar poochte hain)

### 1. Simple Attribute
- Cannot be divided into smaller meaningful parts.
- Examples: Age, Gender, Salary, Roll_No.

### 2. Composite Attribute
- **Can** be divided into smaller meaningful sub-attributes.
- Example: **Address** → House No, Street, City, State, PIN Code.
- Other examples: Name → First/Middle/Last Name; Date → Day/Month/Year.

### 3. Single-Valued Attribute
- Only **one value** per entity.
- Example: har student ki ek Age (Rahul→21, Aman→22). Also: Roll Number, Date of Birth.

### 4. Multi-Valued Attribute
- **Multiple values** for a single entity.
- Example: Rahul ke do phone numbers (9876543210, 9123456780). Also: Skills, Email addresses, Certifications.
- ER symbol: **Double oval**.

### 5. Derived Attribute
- Value **calculated from another** attribute. Store karna compulsory nahi — jab chahiye calculate kar lo.
- Example: DOB = 01-01-2004 → Age = 22 (as of Nov 2026). Also: Total = Quantity × Price; Experience = Current Date − Joining Date.
- ER symbol: **Dashed oval**.

### 6. Key Attribute
- **Uniquely identifies** each entity in the entity set.
- Example: do students ka naam Rahul ho sakta hai, par Roll_No (101 vs 102) different → Roll_No uniquely identifies.
- Other examples: Employee_ID, Customer_ID.
- ER symbol: **Underlined** attribute name.

### 7. Null Attribute
- Value **missing, unknown, unavailable, or not applicable**.
- Example: Middle Name = NULL for Rahul and Priya, "Kumar" for Aman.
- ⭐ Interview point: **NULL ≠ 0 ≠ ''** (empty string). NULL = unknown/missing; 0 = actual numeric value; '' = empty string.

### Quick Revision Table
| Attribute Type | Example | ER Symbol |
|---|---|---|
| Simple | Age | Oval |
| Composite | Address | Oval with sub-ovals |
| Single-Valued | Roll_No | Oval |
| Multi-Valued | Phone Numbers | Double oval |
| Derived | Age from DOB | Dashed oval |
| Key | Student_ID | Underlined |
| Null | Missing Middle Name | Oval (value = NULL) |

---

## ER Diagram Symbols — Chen Notation ⭐ (exams + interviews dono ke liye)

### Basic Symbols
| Component | Symbol | Example |
|---|---|---|
| Entity | Rectangle | Student, Teacher, Course |
| Weak Entity | Double rectangle | Dependent |
| Attribute | Oval | Name, Age, Email |
| Key Attribute | Underlined oval text | Roll_No |
| Composite Attribute | Oval with sub-ovals | Name → First/Middle/Last |
| Multi-Valued Attribute | Double oval | Phone, Skills |
| Derived Attribute | Dashed oval | Age (from DOB) |
| Relationship | Diamond | Enrolls, Teaches |
| Identifying Relationship | Double diamond | Weak entity ↔ owner |
| Total Participation | Double line | Every entity must participate |
| Partial Participation | Single line | Participation optional |

### Cardinality Ratio Symbols
| Symbol | Meaning |
|---|---|
| 1 — 1 | One to One |
| 1 — N | One to Many |
| N — 1 | Many to One |
| M — N | Many to Many |

### Complete ER Diagram — College Management System (kaise padhna hai)
- **Rectangle** → Entity (Student, Course, Teacher)
- **Oval** → Attribute (Name, Age, Email)
- **Underlined attribute** → Key (Roll_No)
- **Diamond** → Relationship (Enrolls, Teaches)
- **Lines** → connections between entities and relationships

### Exam Tips
- Sabse pehle **ye 10+ symbols draw karna practice karo** — identification aur correct usage dono poochte hain.
- ⚠️ Chen notation me relationship = **diamond**. Crow's Foot notation me relationships ko line endings (crow's foot) se show karte hain — **dono notations mix mat karna**.

---

## 3.3 Types of Entities

### 1. Strong Entity
- **Own primary key** hota hai; **independently exist** kar sakta hai; kisi aur entity par dependent nahi.
- Example: **Student** — Student_ID (101, 102, 103) har student ko uniquely identify karta hai.
- ER symbol: **Single rectangle**.
- Other examples: Employee, Department, Product, Customer.

### 2. Weak Entity
- **Apna sufficient key nahi hota** — records ko uniquely identify nahi kar sakta; **strong (owner) entity par dependent** hai.
- Example: Student ke **Dependents** (insurance records):
```
Student_ID | Dependent_Name | Age
101        | Rahul Jr.      | 5
101        | Riya           | 3
102        | Aman Jr.       | 4
```
- `Dependent_Name` alone globally unique nahi hai — lekin **Student_ID + Dependent_Name** milkar record identify ho jata hai.
- Characteristics: no complete PK of its own | depends on owner | cannot be identified independently | existence-dependent on owner.
- ER symbol: **Double rectangle**.

### 3. Identifying Relationship
- Weak entity ko uske **owner (strong) entity se connect** karne wala relationship.
- Owner ki PK + weak entity ki partial key milkar weak entity ko identify karte hain.
- Example: Student **HAS** Dependent → (Student_ID, Dependent_Name) together uniquely identify a dependent.
- ER symbol: **Double diamond**.
- ⭐ Important: weak entity ka apne identifying relationship me **Total Participation** hota hai — har weak entity ka owner hona hi chahiye.

### 4. Partial Key
- Weak entity ka attribute jo **sirf apne owner ke context me** records ko uniquely identify kare.
- Example: `Dependent_Name` — "Rahul Jr." 101 aur 102 dono ke liye hai, so globally unique nahi. Par **101 + Rahul Jr.** vs **102 + Rahul Jr.** = two different records.
- ER symbol: **Dashed underline**.

### Strong vs Weak Entity
| Feature | Strong Entity | Weak Entity |
|---|---|---|
| Primary Key | Own primary key | No complete key of its own |
| Dependency | Independent | Depends on owner entity |
| Existence | Independent | Existence-dependent |
| ER Symbol | Single rectangle | Double rectangle |
| Relationship type | Normal relationship | Identifying relationship |
| Key used | Primary key | Partial key + Owner's PK |

### How All Four Work Together (flow yaad rakho)
1. **Student** = Strong Entity (single rectangle, PK = Student_ID)
2. **Dependent** = Weak Entity (double rectangle)
3. **HAS** = Identifying Relationship (double diamond)
4. **Dependent_Name** = Partial Key (dashed underline)
5. **Student_ID + Dependent_Name** = full identity of a dependent

### Interview Q&A
- **Q: Which entity has its own primary key?** → Strong entity.
- **Q: Which symbol represents a weak entity?** → Double rectangle.
- **Q: What is an identifying relationship?** → The relationship connecting a weak entity to its owner (strong) entity.
- **Q: How is a partial key represented?** → Dashed underline.

---

> 🔜 **Next:** 3.4 Relationship Constraints — Cardinality Ratios (1:1, 1:N, N:1, M:N), Participation Constraints (Total vs Partial). Ye ER diagrams aur SQL table relationships ka foundation hai — especially important!
