# 🏆 LEETCODE SQL PROBLEMS: BASIC JOINS

## 📊 **Problem Categories**
| Category | Problems | Difficulty | Key Concepts |
|----------|----------|------------|-------------|
| **Basic Joins** | 1068, 1378, 1581, 1661 | Easy | INNER JOIN, LEFT JOIN |
| **Multiple Joins** | 1280, 1294, 1303 | Easy-Medium | Multiple table joins, aggregations |
| **Join with Conditions** | 1050, 1141, 1148 | Easy-Medium | WHERE clauses, filtering |

---

## 🎯 **Problem 1: Product Sales Analysis I (LC-1068)**

### 📝 **Problem Statement**
Write an SQL query that reports the product_name, year, and price for each sale_id in the Sales table.

### 📊 **Schema**
```sql
-- Sales table
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+

-- Product table
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    p.product_name,
    s.year,
    s.price
FROM Sales s
INNER JOIN Product p ON s.product_id = p.product_id;
```
**Concept:** Basic INNER JOIN to combine sales data with product information

---

## 🎯 **Problem 2: Replace Employee ID With Unique Identifier (LC-1378)**

### 📝 **Problem Statement**
Write an SQL query to show the unique ID of each user. If a user does not have a unique ID, show null.

### 📊 **Schema**
```sql
-- Employees table
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+

-- EmployeeUNI table
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    eu.unique_id,
    e.name
FROM Employees e
LEFT JOIN EmployeeUNI eu ON e.id = eu.id;
```
**Concept:** LEFT JOIN to include all employees, even those without unique IDs

---

## 🎯 **Problem 3: Visits and Transactions (LC-1581)**

### 📝 **Problem Statement**
Write an SQL query to find the IDs of users who visited without making any transactions and the number of times they made these types of visits.

### 📊 **Schema**
```sql
-- Visits table
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+

-- Transactions table
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    v.customer_id,
    COUNT(v.visit_id) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;
```
**Concept:** LEFT JOIN with NULL check to find visits without transactions

---

## 🎯 **Problem 4: Average Time of Process per Machine (LC-1661)**

### 📝 **Problem Statement**
Calculate the average time each machine takes to complete a process.

### 📊 **Schema**
```sql
-- Activity table
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    a1.machine_id,
    ROUND(AVG(a2.timestamp - a1.timestamp), 3) AS processing_time
FROM Activity a1
INNER JOIN Activity a2 
    ON a1.machine_id = a2.machine_id 
    AND a1.process_id = a2.process_id
    AND a1.activity_type = 'start'
    AND a2.activity_type = 'end'
GROUP BY a1.machine_id;
```
**Concept:** Self-join to match start and end activities

---

## 🎯 **Problem 5: Students and Examinations (LC-1280)**

### 📝 **Problem Statement**
Write an SQL query to find the number of times each student attended each exam.

### 📊 **Schema**
```sql
-- Students table
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+

-- Subjects table
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+

-- Examinations table
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    s.student_id,
    s.student_name,
    sub.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e 
    ON s.student_id = e.student_id 
    AND sub.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
```
**Concept:** CROSS JOIN to create all combinations, then LEFT JOIN for actual data

---

## 🎯 **Problem 6: Managers with at Least 5 Direct Reports (LC-570)**

### 📝 **Problem Statement**
Write an SQL query to find managers with at least five direct reports.

### 📊 **Schema**
```sql
-- Employee table
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    m.name
FROM Employee e
INNER JOIN Employee m ON e.managerId = m.id
GROUP BY m.id, m.name
HAVING COUNT(e.id) >= 5;
```
**Concept:** Self-join to connect employees with their managers

---

## 🎯 **Problem 7: Customer Who Visited but Did Not Make Transactions (LC-1050)**

### 📝 **Problem Statement**
Find customers who visited the mall but did not make any transactions.

### 💡 **Solution**
```sql
SELECT 
    customer_id,
    COUNT(*) AS count_no_trans
FROM Visits
WHERE visit_id NOT IN (
    SELECT DISTINCT visit_id 
    FROM Transactions
)
GROUP BY customer_id;
```
**Concept:** Subquery approach as alternative to LEFT JOIN

---

## 🎯 **Problem 8: Rising Temperature (LC-197)**

### 📝 **Problem Statement**
Find all dates' IDs with higher temperatures compared to their previous dates.

### 📊 **Schema**
```sql
-- Weather table
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
```

### 💡 **Solution**
```sql
SELECT 
    w1.id
FROM Weather w1
INNER JOIN Weather w2 
    ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;
```
**Concept:** Self-join with date comparison

---

## 💡 **Key SQL Join Patterns**

### 🎯 **Pattern 1: Basic INNER JOIN**
```sql
-- Template for combining related tables
SELECT 
    t1.column1,
    t2.column2
FROM table1 t1
INNER JOIN table2 t2 ON t1.key = t2.key;
```

### 🎯 **Pattern 2: LEFT JOIN with NULL Check**
```sql
-- Template for finding missing relationships
SELECT 
    t1.column1,
    COUNT(*) AS count
FROM table1 t1
LEFT JOIN table2 t2 ON t1.key = t2.key
WHERE t2.key IS NULL
GROUP BY t1.column1;
```

### 🎯 **Pattern 3: Self-Join**
```sql
-- Template for comparing rows within same table
SELECT 
    t1.column1
FROM table t1
INNER JOIN table t2 
    ON t1.key = t2.key 
    AND condition
WHERE t1.value > t2.value;
```

### 🎯 **Pattern 4: Multiple Joins**
```sql
-- Template for combining multiple tables
SELECT 
    t1.column1,
    t2.column2,
    t3.column3
FROM table1 t1
INNER JOIN table2 t2 ON t1.key1 = t2.key1
LEFT JOIN table3 t3 ON t2.key2 = t3.key2;
```

---

## 🚀 **Performance Tips**

### ⚡ **Optimization Strategies**
1. **Use appropriate join types** - INNER vs LEFT vs CROSS
2. **Index join columns** for faster lookups
3. **Filter early** with WHERE before joins when possible
4. **Avoid SELECT *** - specify only needed columns
5. **Use EXPLAIN** to analyze query performance

### 📊 **Join Type Selection**
```sql
-- Join type decision guide
Join_Types = {
    "INNER JOIN": "Only matching rows from both tables",
    "LEFT JOIN": "All rows from left + matching from right",
    "RIGHT JOIN": "All rows from right + matching from left",
    "CROSS JOIN": "Cartesian product of both tables",
    "SELF JOIN": "Join table with itself for comparisons"
}
```

---

## 🎯 **Practice Problems by Difficulty**

### 🟢 **Easy Level**
1. **Product Sales Analysis I (LC-1068)** - Basic INNER JOIN
2. **Replace Employee ID (LC-1378)** - LEFT JOIN with NULL
3. **Customer Referee (LC-584)** - Simple filtering
4. **Big Countries (LC-595)** - Basic WHERE conditions

### 🟡 **Medium Level**
1. **Visits and Transactions (LC-1581)** - LEFT JOIN + aggregation
2. **Students and Examinations (LC-1280)** - CROSS JOIN + LEFT JOIN
3. **Average Time per Machine (LC-1661)** - Self-join with calculations
4. **Managers with 5+ Reports (LC-570)** - Self-join + HAVING

---

## 🏆 **Interview Tips**

### 📝 **Problem-Solving Approach**
1. **Understand the schema** - Identify primary/foreign keys
2. **Identify relationships** - One-to-many, many-to-many
3. **Choose join type** - Based on required output
4. **Consider edge cases** - NULL values, missing data
5. **Test with examples** - Verify logic with sample data

### 🎯 **Common Mistakes to Avoid**
- **Wrong join type** - Using INNER when LEFT is needed
- **Missing GROUP BY** - When using aggregate functions
- **Cartesian products** - Forgetting join conditions
- **NULL handling** - Not considering NULL values
- **Duplicate results** - Not using DISTINCT when needed

---

*Master SQL joins, query with confidence! 🎯*