# 📊 SQL Deep-Dive: Boolean Logic in `SELECT` vs. Aggregations

This repository documents an analysis of SQL query behavior when using comparison operators (`>=`, `>`, `<`) directly inside a `SELECT` clause versus filtering aggregates using the `HAVING` clause.

---

## 1. Input Dataset (`STUDENTS`)

Below is the database schema and sample data used for this test:

```sql
-- Schema Creation
CREATE TABLE STUDENTS (
    ST_ID INT PRIMARY KEY,
    STUDENT_NAME VARCHAR(50),
    COURSE VARCHAR(50),
    MARKS INT
);

-- Data Insertion
INSERT INTO STUDENTS (ST_ID, STUDENT_NAME, COURSE, MARKS) VALUES
(1, 'ARUN', 'MYSQL', 85),
(2, 'ANIL', 'BI', 40),
(3, 'RAM', 'DA', 90),
(4, 'TOM', 'BA', 35),
(5, 'SWETHA', 'MYSQL', 95),
(6, 'SANVI', 'MYSQL', 75);

### Raw Table Data
| ST_ID | STUDENT_NAME | COURSE | MARKS |
| :--- | :--- | :--- | :--- |
| **1** | ARUN | MYSQL | 85 |
| **2** | ANIL | BI | 40 |
| **3** | RAM | DA | 90 |
| **4** | TOM | BA | 35 |
| **5** | SWETHA | MYSQL | 95 |
| **6** | SANVI | MYSQL | 75 |

---

## **2. Query Comparison**

### ❌ Query A: Comparison Operator inside `SELECT`
```sql
SELECT 
    COURSE, 
    AVG(MARKS) >= 50 AS AVG_MARKS
FROM STUDENTS
GROUP BY COURSE 
HAVING AVG(MARKS) > 80;

Query B: Standard Aggregation (Corrected)
SELECT 
    COURSE, 
    AVG(MARKS) AS AVG_MARKS
FROM STUDENTS
GROUP BY COURSE 
HAVING AVG(MARKS) > 80;

Execution Results
COURSE           Output: Query A (Boolean Flag)Output:       Query B (Calculated Average)
MYSQL              1                                              85.0000
DA                 1                                              90.0000


**Key Takeaways & Explanation**

Why Query A returns 1:
In SQL, placing >= 50 inside the SELECT clause evaluates the expression as a Boolean test (Is the group average >= 50?).
Since the average for MYSQL ($85$) and DA ($90$) are both greater than or equal to $50$, SQL returns 1 (representing TRUE).

Why Query B returns the numerical average:
Removing comparison operators from the SELECT clause tells SQL to compute and return the actual mathematical mean (85.0000 and 90.0000).

Best Practice:

SELECT Clause: Reserved for computing and projecting column values.
WHERE / HAVING Clauses: Reserved for filtering dataset rows or groups using conditions.

