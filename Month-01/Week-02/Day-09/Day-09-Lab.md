# 📊 Day 09 Lab: Salary Analysis with SQL

> **Objective**: Extend the Employees and Departments schema by adding a Salaries table and perform salary analytics per department.

---

## 🎯 Lab Tasks

### Task Overview
1. ✅ Create a `Salaries` table with proper constraints
2. ✅ Establish foreign key relationships with the `Employees` table
3. ✅ Insert salary data for all employees
4. ✅ Write queries to analyze average salary per department

---

## 📋 Table of Contents
- [Schema Design](#schema-design)
- [Data Insertion](#data-insertion)
- [Query Solutions](#query-solutions)
- [Expected Results](#expected-results)

---

## 🗂️ Schema Design

### Salaries Table Structure

The `Salaries` table maintains a one-to-one relationship with employees, ensuring each employee has exactly one salary record.

```sql
-- Create Salaries table with comprehensive constraints
CREATE TABLE IF NOT EXISTS Salaries (
    id SERIAL PRIMARY KEY,
    employee_id INT NOT NULL UNIQUE,
    amount DECIMAL(12, 2) NOT NULL CHECK (amount > 0),
    
    -- Foreign Key Constraint
    CONSTRAINT fk_employee_salary
        FOREIGN KEY (employee_id)
        REFERENCES Employees(id)
        ON DELETE CASCADE
);
```

#### 🔑 Key Features:
- **Primary Key**: Auto-incrementing `id` for unique identification
- **Unique Constraint**: Each employee can have only one salary record
- **Check Constraint**: Ensures salary amounts are always positive
- **Foreign Key**: Maintains referential integrity with Employees table
- **Cascade Delete**: Automatically removes salary records when an employee is deleted

---

## 💾 Data Insertion

### Populating the Salaries Table

```sql
-- Insert salary data for all employees
INSERT INTO Salaries (employee_id, amount) VALUES 
    (4, 55000.00),
    (5, 92500.50),
    (6, 48000.00),
    (7, 71200.00),
    (8, 64000.00),
    (9, 105000.00),
    (10, 59000.00),
    (11, 88000.00),
    (12, 73500.25),
    (13, 62000.00),
    (14, 97000.00),
    (15, 51000.00),
    (16, 79900.00),
    (1, 85600.00),
    (3, 42000.00),
    (2, 67500.00);
```

#### 📈 Salary Statistics:
- **Total Employees**: 16
- **Salary Range**: $42,000 - $105,000
- **Highest Paid**: Employee ID 9 ($105,000)
- **Lowest Paid**: Employee ID 3 ($42,000)

---

## 🔍 Query Solutions

### Query 1: Employee Salary Report (Sorted by Amount)

```sql
-- List all employees with their department and salary, ordered by salary
SELECT 
    emp_name,
    dept_name,
    amount
FROM Employees AS e
LEFT JOIN Departments AS d ON e.department_id = d.id
JOIN Salaries AS s ON s.employee_id = e.id
ORDER BY amount;
```

#### 📝 Query Explanation:
- **LEFT JOIN**: Includes employees even if they don't have a department
- **INNER JOIN**: Only includes employees with salary records
- **ORDER BY**: Sorts results from lowest to highest salary

---

### Query 2: Average Salary Per Department

```sql
-- Calculate average salary for each department
SELECT 
    d.dept_name,
    COUNT(e.id) AS employee_count,
    ROUND(AVG(s.amount), 2) AS avg_salary,
    MIN(s.amount) AS min_salary,
    MAX(s.amount) AS max_salary
FROM Departments AS d
LEFT JOIN Employees AS e ON d.id = e.department_id
LEFT JOIN Salaries AS s ON e.id = s.employee_id
GROUP BY d.dept_name
ORDER BY avg_salary DESC;
```

#### 📊 Query Features:
- **AVG()**: Calculates average salary per department
- **COUNT()**: Shows number of employees in each department
- **MIN/MAX()**: Displays salary range within departments
- **ROUND()**: Formats average to 2 decimal places
- **GROUP BY**: Aggregates data by department
- **LEFT JOIN**: Includes departments even without employees

---

### Query 3: Departments Above Company Average

```sql
-- Find departments with average salary above company average
WITH company_avg AS (
    SELECT AVG(amount) AS avg_salary
    FROM Salaries
)
SELECT 
    d.dept_name,
    ROUND(AVG(s.amount), 2) AS dept_avg_salary,
    (SELECT ROUND(avg_salary, 2) FROM company_avg) AS company_avg_salary,
    ROUND(AVG(s.amount) - (SELECT avg_salary FROM company_avg), 2) AS difference
FROM Departments AS d
JOIN Employees AS e ON d.id = e.department_id
JOIN Salaries AS s ON e.id = s.employee_id
GROUP BY d.dept_name
HAVING AVG(s.amount) > (SELECT avg_salary FROM company_avg)
ORDER BY dept_avg_salary DESC;
```

#### 🎯 Advanced Features:
- **CTE (Common Table Expression)**: Calculates company-wide average
- **HAVING Clause**: Filters departments above average
- **Subquery**: Compares department average to company average

---

## 📈 Expected Results

### Sample Output: Employee Salary Report

| emp_name | dept_name | amount |
|----------|-----------|--------|
| John Doe | Engineering | $42,000.00 |
| Jane Smith | Marketing | $48,000.00 |
| ... | ... | ... |
| Alice Johnson | Engineering | $105,000.00 |

### Sample Output: Average Salary Per Department

| dept_name | employee_count | avg_salary | min_salary | max_salary |
|-----------|----------------|------------|------------|------------|
| Engineering | 8 | $73,450.00 | $42,000.00 | $105,000.00 |
| Marketing | 5 | $68,280.00 | $51,000.00 | $92,500.50 |
| HR | 3 | $64,500.00 | $55,000.00 | $79,900.00 |

---

## 💡 Key Learnings

### SQL Concepts Practiced:
1. ✅ **Table Creation** with multiple constraints
2. ✅ **Foreign Key Relationships** and referential integrity
3. ✅ **JOIN Operations** (INNER, LEFT)
4. ✅ **Aggregate Functions** (AVG, COUNT, MIN, MAX)
5. ✅ **GROUP BY** and **HAVING** clauses
6. ✅ **Common Table Expressions (CTEs)**
7. ✅ **Data Sorting** and formatting

### Best Practices Applied:
- 🔒 **Data Integrity**: Check constraints ensure valid data
- 🔗 **Referential Integrity**: Foreign keys maintain relationships
- 📊 **Meaningful Aliases**: Improves query readability
- 🎯 **Proper Indexing**: UNIQUE constraint on employee_id
- 🧹 **Cascade Operations**: Automatic cleanup of related records

---

## 🚀 Next Steps

1. **Experiment** with different aggregate functions
2. **Create indexes** on frequently queried columns
3. **Add views** for commonly used queries
4. **Implement triggers** for salary change auditing
5. **Practice window functions** for advanced analytics

---

## 📚 Additional Resources

- [PostgreSQL Aggregate Functions](https://www.postgresql.org/docs/current/functions-aggregate.html)
- [SQL JOIN Types Explained](https://www.postgresql.org/docs/current/tutorial-join.html)
- [Common Table Expressions (CTEs)](https://www.postgresql.org/docs/current/queries-with.html)

---

<div align="center">

**🎓 Lab Completed Successfully!**

*Keep practicing SQL to master data engineering fundamentals.*

</div>
