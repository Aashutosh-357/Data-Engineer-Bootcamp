# 🔀 SQL Set Operators Mastery

> **A comprehensive guide to combining query results using SQL Set Operators for Data Engineers**

---

## 📚 Table of Contents

1. [Overview](#-overview)
2. [What are Set Operators?](#-what-are-set-operators)
3. [Set Operator Syntax](#-set-operator-syntax)
4. [The Six Golden Rules](#-the-six-golden-rules)
5. [Set Operators Deep Dive](#-set-operators-deep-dive)
   - [UNION](#1-union)
   - [UNION ALL](#2-union-all)
   - [EXCEPT](#3-except)
   - [INTERSECT](#4-intersect)
6. [Comparison Chart](#-comparison-chart)
7. [Real-World Use Cases](#-real-world-use-cases)
8. [Best Practices](#-best-practices)

---

## 🎯 Overview

SQL Set Operators are powerful tools that allow you to **combine rows** from multiple queries into a single result set. Unlike JOINs (which combine columns), Set Operators work vertically to merge data.

### 🔑 Key Characteristics

| Feature | Set Operators | JOINs |
|---------|---------------|-------|
| **Combines** | Rows (vertical) | Columns (horizontal) |
| **Requires Key Column** | ❌ No | ✅ Yes |
| **Column Requirement** | Same columns in all queries | Can have different columns |
| **Primary Use** | Merging similar datasets | Relating different datasets |

---

## 🧩 What are Set Operators?

Set Operators combine rows from **multiple queries** into a single result set without requiring a key column for matching.

### 📊 Visual Representation

```
Query 1 Results          Query 2 Results
┌──────────────┐        ┌──────────────┐
│   Row 1      │        │   Row A      │
│   Row 2      │        │   Row B      │
│   Row 3      │        │   Row C      │
└──────────────┘        └──────────────┘
        │                       │
        └───────────┬───────────┘
                    │
              SET OPERATOR
                    │
                    ▼
         ┌──────────────────┐
         │  Combined Result │
         └──────────────────┘
```

### 🎭 The Four Set Operators

| Operator | Purpose | Duplicates |
|----------|---------|------------|
| **UNION** | Combines all unique rows | Removed |
| **UNION ALL** | Combines all rows | Kept |
| **EXCEPT** | Rows in first query but not in second | Removed |
| **INTERSECT** | Rows common to both queries | Removed |

> **💡 Note:** `EXCEPT` is also called `MINUS` in some database systems (e.g., Oracle).

---

## 📝 Set Operator Syntax

The syntax is straightforward and consistent across all set operators:

```sql
SELECT column1, column2, ...
FROM table1
WHERE condition

SET_OPERATOR

SELECT column1, column2, ...
FROM table2
WHERE condition
ORDER BY column1;  -- Only at the very end!
```

### 💻 Basic Example

```sql
-- Combine employees and customers
SELECT first_name, last_name, 'Employee' AS source
FROM employees

UNION

SELECT first_name, last_name, 'Customer' AS source
FROM customers
ORDER BY last_name;
```

---

## 📏 The Six Golden Rules

Understanding these rules is **critical** for using set operators correctly!

### 1️⃣ **SQL Clauses**

✅ **Allowed in individual SELECT statements:**
- `WHERE`
- `JOIN`
- `GROUP BY`
- `HAVING`

⚠️ **ORDER BY** can only be used **once** at the very end of the entire query.

```sql
-- ✅ Correct
SELECT name FROM employees WHERE dept = 'IT'
UNION
SELECT name FROM customers WHERE status = 'Active'
ORDER BY name;

-- ❌ Wrong
SELECT name FROM employees ORDER BY name
UNION
SELECT name FROM customers ORDER BY name;
```

---

### 2️⃣ **Number of Columns**

All queries must have the **same number of columns**.

```sql
-- ✅ Correct - Both queries have 2 columns
SELECT first_name, last_name FROM employees
UNION
SELECT first_name, last_name FROM customers;

-- ❌ Wrong - Different number of columns
SELECT first_name, last_name FROM employees
UNION
SELECT first_name, last_name, email FROM customers;
```

---

### 3️⃣ **Data Types**

Corresponding columns must have **matching or compatible** data types.

| Position | Query 1 Type | Query 2 Type | Compatible? |
|----------|--------------|--------------|-------------|
| Column 1 | VARCHAR | VARCHAR | ✅ Yes |
| Column 2 | INT | INT | ✅ Yes |
| Column 3 | DATE | DATETIME | ✅ Yes (compatible) |
| Column 4 | VARCHAR | INT | ❌ No |

```sql
-- ✅ Correct - Compatible types
SELECT employee_id, hire_date FROM employees
UNION
SELECT customer_id, signup_date FROM customers;

-- ❌ Wrong - Incompatible types
SELECT employee_name, hire_date FROM employees
UNION
SELECT customer_id, signup_date FROM customers;
```

---

### 4️⃣ **Order of Columns**

Columns are mapped **positionally**, so the order must be the same.

```sql
-- ✅ Correct - Same order
SELECT first_name, last_name FROM employees
UNION
SELECT first_name, last_name FROM customers;

-- ❌ Wrong - Different order (will produce incorrect results)
SELECT first_name, last_name FROM employees
UNION
SELECT last_name, first_name FROM customers;
```

---

### 5️⃣ **Column Aliases**

The final output uses column names/aliases from the **first query only**.

```sql
SELECT 
    first_name AS fname,
    last_name AS lname
FROM employees

UNION

SELECT 
    first_name AS given_name,  -- This alias is ignored!
    last_name AS family_name   -- This alias is ignored!
FROM customers;

-- Result columns will be: fname, lname
```

---

### 6️⃣ **Matching Information**

**You** are responsible for ensuring logical correctness. SQL won't validate if the data makes sense.

```sql
-- ⚠️ Syntactically valid but logically WRONG!
SELECT first_name, salary FROM employees
UNION
SELECT product_name, price FROM products;
-- This will run but produces meaningless results!

-- ✅ Logically correct
SELECT first_name, last_name FROM employees
UNION
SELECT first_name, last_name FROM customers;
```

---

## 🔍 Set Operators Deep Dive

### 1️⃣ UNION

**Purpose:** Returns all **distinct (unique)** rows from both queries.

#### 🎯 Use Case
Creating a unique list of all individuals from employees and customers.

#### 💻 Example

```sql
-- Get unique list of all people
SELECT 
    first_name,
    last_name,
    email
FROM employees

UNION

SELECT 
    first_name,
    last_name,
    email
FROM customers;
```

#### 🔍 How It Works

```
Employees              Customers              UNION Result
┌─────────────┐       ┌─────────────┐        ┌─────────────┐
│ John Smith  │       │ Jane Doe    │        │ John Smith  │
│ Jane Doe    │       │ Bob Wilson  │   →    │ Jane Doe    │ (deduplicated)
│ Alice Brown │       │ John Smith  │        │ Alice Brown │
└─────────────┘       └─────────────┘        │ Bob Wilson  │
                                              └─────────────┘
```

#### ⚡ Performance Note
UNION performs deduplication, which requires additional processing time.

---

### 2️⃣ UNION ALL

**Purpose:** Returns **all rows** from both queries, including duplicates.

#### 🎯 Use Cases
- When you know there are no duplicates
- When you want to see duplicates for data quality checks
- When performance is critical

#### 💻 Example

```sql
-- Get all records including duplicates
SELECT 
    first_name,
    last_name,
    'Employee' AS source
FROM employees

UNION ALL

SELECT 
    first_name,
    last_name,
    'Customer' AS source
FROM customers;
```

#### 🔍 How It Works

```
Employees              Customers              UNION ALL Result
┌─────────────┐       ┌─────────────┐        ┌─────────────────┐
│ John Smith  │       │ Jane Doe    │        │ John Smith (E)  │
│ Jane Doe    │       │ Bob Wilson  │   →    │ Jane Doe (E)    │
│ Alice Brown │       │ John Smith  │        │ Alice Brown (E) │
└─────────────┘       └─────────────┘        │ Jane Doe (C)    │
                                              │ Bob Wilson (C)  │
                                              │ John Smith (C)  │
                                              └─────────────────┘
```

#### ⚡ Performance Comparison

| Operator | Removes Duplicates | Performance |
|----------|-------------------|-------------|
| UNION | ✅ Yes | Slower |
| UNION ALL | ❌ No | **Faster** ⚡ |

---

### 3️⃣ EXCEPT

**Purpose:** Returns distinct rows from the **first query** that are **not found** in the second query.

> **⚠️ Important:** The order of queries matters! `A EXCEPT B` ≠ `B EXCEPT A`

#### 🎯 Use Case
Finding employees who are **not** customers.

#### 💻 Example

```sql
-- Find employees who are NOT customers
SELECT 
    first_name,
    last_name,
    email
FROM employees

EXCEPT

SELECT 
    first_name,
    last_name,
    email
FROM customers;
```

#### 🔍 How It Works

```
Employees              Customers              EXCEPT Result
┌─────────────┐       ┌─────────────┐        ┌─────────────┐
│ John Smith  │       │ Jane Doe    │        │ John Smith  │
│ Jane Doe    │  ─    │ Bob Wilson  │   →    │ Alice Brown │
│ Alice Brown │       │ John Smith  │        └─────────────┘
└─────────────┘       └─────────────┘        
                                              (Jane Doe removed)
```

#### 🔄 Order Matters!

```sql
-- Query 1: Employees NOT in Customers
SELECT first_name, last_name FROM employees
EXCEPT
SELECT first_name, last_name FROM customers;

-- Query 2: Customers NOT in Employees (DIFFERENT RESULT!)
SELECT first_name, last_name FROM customers
EXCEPT
SELECT first_name, last_name FROM employees;
```

---

### 4️⃣ INTERSECT

**Purpose:** Returns only rows that are **common to both queries**.

> **💡 Note:** Similar to INNER JOIN but removes duplicates. Order doesn't matter!

#### 🎯 Use Case
Finding employees who are **also** customers.

#### 💻 Example

```sql
-- Find people who are both employees AND customers
SELECT 
    first_name,
    last_name,
    email
FROM employees

INTERSECT

SELECT 
    first_name,
    last_name,
    email
FROM customers;
```

#### 🔍 How It Works

```
Employees              Customers              INTERSECT Result
┌─────────────┐       ┌─────────────┐        ┌─────────────┐
│ John Smith  │       │ Jane Doe    │        │ Jane Doe    │
│ Jane Doe    │  ∩    │ Bob Wilson  │   →    │ John Smith  │
│ Alice Brown │       │ John Smith  │        └─────────────┘
└─────────────┘       └─────────────┘        
                                              (Only common rows)
```

#### 🔄 Order Doesn't Matter

```sql
-- Both queries produce the same result
A INTERSECT B  =  B INTERSECT A
```

---

## 📊 Comparison Chart

### Quick Reference Table

| Operator | Returns | Duplicates | Order Matters | Similar To |
|----------|---------|------------|---------------|------------|
| **UNION** | All unique rows from both | Removed | ❌ No | OR (Set Theory) |
| **UNION ALL** | All rows from both | Kept | ❌ No | Concatenation |
| **EXCEPT** | Rows in A but not in B | Removed | ✅ Yes | Subtraction |
| **INTERSECT** | Rows in both A and B | Removed | ❌ No | AND / INNER JOIN |

### Visual Comparison

```
Dataset A: {1, 2, 3, 4}
Dataset B: {3, 4, 5, 6}

UNION:        {1, 2, 3, 4, 5, 6}
UNION ALL:    {1, 2, 3, 4, 3, 4, 5, 6}
A EXCEPT B:   {1, 2}
B EXCEPT A:   {5, 6}
INTERSECT:    {3, 4}
```

---

## 🌍 Real-World Use Cases

### 1️⃣ **Combine Information**

Consolidate similar tables before analysis to create a unified view.

#### Scenario A: Combining Similar Entities

```sql
-- Create a unified "persons" table from multiple sources
SELECT 
    employee_id AS person_id,
    first_name,
    last_name,
    email,
    'Employee' AS person_type
FROM employees

UNION ALL

SELECT 
    customer_id AS person_id,
    first_name,
    last_name,
    email,
    'Customer' AS person_type
FROM customers

UNION ALL

SELECT 
    supplier_id AS person_id,
    contact_first_name AS first_name,
    contact_last_name AS last_name,
    contact_email AS email,
    'Supplier' AS person_type
FROM suppliers;
```

#### Scenario B: Combining Partitioned Tables

```sql
-- Combine yearly order tables into a single dataset
SELECT 
    order_id,
    order_date,
    customer_id,
    total_amount
FROM orders_2022

UNION ALL

SELECT 
    order_id,
    order_date,
    customer_id,
    total_amount
FROM orders_2023

UNION ALL

SELECT 
    order_id,
    order_date,
    customer_id,
    total_amount
FROM orders_2024
ORDER BY order_date DESC;
```

#### Scenario C: Merging Current and Archive Data

```sql
-- Combine active and archived orders for complete analysis
SELECT 
    order_id,
    order_date,
    customer_id,
    status,
    total_amount,
    'Active' AS data_source
FROM orders

UNION ALL

SELECT 
    order_id,
    order_date,
    customer_id,
    status,
    total_amount,
    'Archive' AS data_source
FROM orders_archive
ORDER BY order_date DESC;
```

---

### 2️⃣ **Delta Detection**

Identify changes between two datasets (e.g., before and after an update).

```sql
-- Find new customers added today
SELECT customer_id, customer_name, email
FROM customers_current

EXCEPT

SELECT customer_id, customer_name, email
FROM customers_yesterday;
```

```sql
-- Find customers removed today
SELECT customer_id, customer_name, email
FROM customers_yesterday

EXCEPT

SELECT customer_id, customer_name, email
FROM customers_current;
```

---

### 3️⃣ **Data Completeness Check**

Verify data integrity across related tables.

```sql
-- Find orders without corresponding customers (orphaned records)
SELECT DISTINCT customer_id
FROM orders

EXCEPT

SELECT customer_id
FROM customers;
```

```sql
-- Find products that have never been ordered
SELECT product_id, product_name
FROM products

EXCEPT

SELECT DISTINCT product_id, product_name
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id;
```

---

### 4️⃣ **Data Quality Auditing**

```sql
-- Find duplicate email addresses across employees and customers
SELECT email, COUNT(*) AS occurrence_count
FROM (
    SELECT email FROM employees
    UNION ALL
    SELECT email FROM customers
) AS all_emails
GROUP BY email
HAVING COUNT(*) > 1;
```

---

## 💎 Best Practices

### 1. **Choose the Right Operator**

| Goal | Use |
|------|-----|
| Merge unique records | `UNION` |
| Merge all records (faster) | `UNION ALL` |
| Find differences | `EXCEPT` |
| Find commonalities | `INTERSECT` |

---

### 2. **Optimize Performance**

```sql
-- ✅ Good - Use UNION ALL when duplicates don't matter
SELECT * FROM orders_2023
UNION ALL
SELECT * FROM orders_2024;

-- ❌ Slower - UNION removes duplicates (unnecessary overhead)
SELECT * FROM orders_2023
UNION
SELECT * FROM orders_2024;
```

---

### 3. **Use Meaningful Column Aliases**

```sql
-- ✅ Good - Clear aliases in first query
SELECT 
    emp_id AS id,
    emp_name AS name,
    'Employee' AS type
FROM employees
UNION
SELECT cust_id, cust_name, 'Customer'
FROM customers;
-- Result columns: id, name, type
```

---

### 4. **Add Source Identifiers**

```sql
-- ✅ Good - Track data source
SELECT 
    product_id,
    product_name,
    price,
    'Store A' AS source
FROM store_a_products
UNION ALL
SELECT 
    product_id,
    product_name,
    price,
    'Store B' AS source
FROM store_b_products;
```

---

### 5. **Validate Data Types**

```sql
-- ✅ Good - Explicit type casting for compatibility
SELECT 
    CAST(employee_id AS VARCHAR) AS id,
    hire_date
FROM employees
UNION
SELECT 
    customer_code AS id,
    signup_date
FROM customers;
```

---

### 6. **Use Parentheses for Complex Queries**

```sql
-- ✅ Good - Clear precedence with parentheses
(SELECT name FROM employees WHERE dept = 'IT')
UNION
(SELECT name FROM employees WHERE dept = 'HR')
EXCEPT
(SELECT name FROM terminated_employees);
```

---

### 7. **Document Your Logic**

```sql
-- ✅ Good - Comments explain the business logic
-- Combine all active contacts from different sources
-- for the monthly newsletter distribution list
SELECT email, 'Employee' AS source FROM employees WHERE active = 1
UNION
SELECT email, 'Customer' AS source FROM customers WHERE subscribed = 1
UNION
SELECT email, 'Partner' AS source FROM partners WHERE status = 'Active';
```

---

## 🎓 Summary

### Key Takeaways

✅ **Set Operators combine rows** (not columns like JOINs)  
✅ **UNION** removes duplicates, **UNION ALL** keeps them  
✅ **EXCEPT** finds differences (order matters!)  
✅ **INTERSECT** finds commonalities (order doesn't matter)  
✅ **Follow the 6 Golden Rules** for correct results  
✅ **Use UNION ALL** when possible for better performance  

### Performance Hierarchy

```
Fastest  →  UNION ALL
            ↓
            INTERSECT
            ↓
            EXCEPT
            ↓
Slowest  →  UNION (due to deduplication)
```

---

> **🚀 Next Steps:** Practice combining different datasets using set operators and experiment with real-world scenarios to master these powerful SQL tools!
