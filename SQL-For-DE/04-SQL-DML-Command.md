# 📝 SQL DML Commands - Data Manipulation Mastery

## 🎯 Learning Objectives
Master Data Manipulation Language (DML) commands to insert, update, and delete data efficiently and safely.

> 💡 **Core Concept:** DML commands modify data content while preserving table structure.

---

## 🔧 DML Command Overview

**Data Manipulation Language (DML)** commands work with data inside tables:

| Command | Purpose | Risk Level | Affects |
|---------|---------|------------|---------|
| **INSERT** | Add new rows | 🟢 Low | Data volume |
| **UPDATE** | Modify existing rows | 🟡 Medium | Data accuracy |
| **DELETE** | Remove rows | 🔴 High | Data availability |
| **TRUNCATE** | Remove all rows | 🔴 Very High | Entire table data |

---

## ➕ INSERT Command (0:31-13:42)

### 🎯 **Purpose**
Adds new rows of data to existing tables.

### 📋 **Basic Syntax**
```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

### 🏗️ **Single Row Insert**
```sql
INSERT INTO Customers (CustomerName, Country, Score)
VALUES ('John Doe', 'USA', 85);
```

### 📊 **Multiple Row Insert**
```sql
INSERT INTO Customers (CustomerName, Country, Score)
VALUES 
    ('Alice Smith', 'Canada', 92),
    ('Bob Johnson', 'UK', 78),
    ('Carol Davis', 'Australia', 88);
```

### 🔍 **Insert Variations**

#### **All Columns (Column List Optional)**
```sql
-- With column specification (recommended)
INSERT INTO Customers (CustomerName, Country, Score)
VALUES ('Jane Doe', 'USA', 90);

-- Without column specification (risky)
INSERT INTO Customers 
VALUES ('Jane Doe', 'USA', 90);
```

#### **Partial Column Insert**
```sql
INSERT INTO Customers (CustomerName, Country)
VALUES ('New Customer', 'Germany');
-- Score will be NULL or default value
```

### 📈 **Insert from Another Table**
```sql
-- Copy data from source to target table
INSERT INTO CustomerBackup (CustomerName, Country, Score)
SELECT CustomerName, Country, Score 
FROM Customers 
WHERE Score > 80;
```

### ✅ **INSERT Best Practices**
| Practice | Benefit | Example |
|----------|---------|---------|
| **Specify Columns** | Clarity & safety | `INSERT INTO table (col1, col2)` |
| **Validate Data** | Data integrity | Check constraints before insert |
| **Batch Inserts** | Performance | Multiple VALUES in one statement |
| **Handle Duplicates** | Avoid errors | Use `INSERT IGNORE` or `ON DUPLICATE KEY` |

---

## ✏️ UPDATE Command (13:47-20:10)

### 🎯 **Purpose**
Modifies existing data in table rows.

### 📋 **Basic Syntax**
```sql
UPDATE table_name 
SET column1 = value1, column2 = value2
WHERE condition;
```

### 🎯 **Single Column Update**
```sql
UPDATE Customers 
SET Score = 95 
WHERE CustomerName = 'John Doe';
```

### 📊 **Multiple Column Update**
```sql
UPDATE Customers 
SET Score = 88, Country = 'United States'
WHERE CustomerName = 'John Doe';
```

### 🔍 **Conditional Updates**

#### **Update Based on Conditions**
```sql
-- Update all customers from a specific country
UPDATE Customers 
SET Score = Score + 5 
WHERE Country = 'USA';

-- Update NULL values
UPDATE Customers 
SET Score = 0 
WHERE Score IS NULL;
```

#### **Calculated Updates**
```sql
-- Increase scores by 10%
UPDATE Customers 
SET Score = Score * 1.1 
WHERE Score < 90;
```

### ⚠️ **WHERE Clause Importance**
```sql
-- ✅ SAFE: Updates specific rows
UPDATE Customers 
SET Score = 100 
WHERE CustomerName = 'John Doe';

-- ❌ DANGEROUS: Updates ALL rows
UPDATE Customers 
SET Score = 100;
-- This affects every single row in the table!
```

### 🛡️ **UPDATE Safety Measures**
| Safety Check | Purpose | Example |
|--------------|---------|---------|
| **WHERE Clause** | Target specific rows | `WHERE CustomerID = 123` |
| **Test First** | Verify with SELECT | `SELECT * FROM table WHERE condition` |
| **Backup** | Recovery option | Backup before bulk updates |
| **Transaction** | Rollback capability | `BEGIN TRANSACTION` |

---

## 🗑️ DELETE Command (20:23-24:09)

### 🎯 **Purpose**
Removes existing rows from tables.

### 📋 **Basic Syntax**
```sql
DELETE FROM table_name 
WHERE condition;
```

### 🎯 **Targeted Delete**
```sql
DELETE FROM Customers 
WHERE CustomerName = 'John Doe';
```

### 📊 **Conditional Delete**
```sql
-- Delete customers with low scores
DELETE FROM Customers 
WHERE Score < 50;

-- Delete customers from specific country
DELETE FROM Customers 
WHERE Country = 'Inactive';
```

### ⚠️ **Critical DELETE Warnings**
```sql
-- ✅ SAFE: Deletes specific rows
DELETE FROM Customers 
WHERE CustomerID = 123;

-- ❌ EXTREMELY DANGEROUS: Deletes ALL rows
DELETE FROM Customers;
-- This removes every single row from the table!
```

### 🔄 **DELETE vs TRUNCATE**
| Aspect | DELETE | TRUNCATE |
|--------|--------|----------|
| **WHERE Clause** | ✅ Supported | ❌ Not supported |
| **Speed** | Slower | Much faster |
| **Logging** | Full logging | Minimal logging |
| **Rollback** | ✅ Possible | ⚠️ Limited |
| **Identity Reset** | ❌ No | ✅ Yes |
| **Triggers** | ✅ Fired | ❌ Not fired |

### ⚡ **TRUNCATE Command**
```sql
-- Fast way to delete all rows
TRUNCATE TABLE Customers;
```

**Use TRUNCATE when:**
- Removing all data from table
- Performance is critical
- No need for conditional deletion
- Identity columns should reset

---

## 🔄 Advanced DML Patterns

### 📊 **Bulk Operations**
```sql
-- Bulk insert with data validation
INSERT INTO CustomerArchive 
SELECT * FROM Customers 
WHERE LastActivity < '2023-01-01';

-- Bulk update with joins
UPDATE c 
SET c.Score = c.Score + 10
FROM Customers c
INNER JOIN PremiumMembers p ON c.CustomerID = p.CustomerID;

-- Bulk delete with subquery
DELETE FROM Orders 
WHERE CustomerID IN (
    SELECT CustomerID FROM Customers 
    WHERE Status = 'Inactive'
);
```

### 🔒 **Transaction Safety**
```sql
BEGIN TRANSACTION;

    UPDATE Customers 
    SET Score = Score + 5 
    WHERE Country = 'USA';
    
    -- Verify the changes
    SELECT COUNT(*) FROM Customers WHERE Score > 100;
    
    -- If everything looks good
    COMMIT;
    -- If something went wrong
    -- ROLLBACK;
```

---

## 📊 DML Performance Optimization

### 🚀 **Performance Tips**
| Operation | Optimization | Impact |
|-----------|--------------|--------|
| **INSERT** | Batch multiple rows | 10x faster |
| **UPDATE** | Use indexes on WHERE columns | 5x faster |
| **DELETE** | Index WHERE conditions | 3x faster |
| **TRUNCATE** | Use instead of DELETE (all rows) | 100x faster |

### 📈 **Batch Size Guidelines**
```sql
-- Good: Batch insert
INSERT INTO table VALUES 
    (val1), (val2), (val3), ..., (val1000);

-- Avoid: Single row inserts in loops
-- INSERT INTO table VALUES (val1);
-- INSERT INTO table VALUES (val2);
-- ... (repeated 1000 times)
```

---

## 🎯 Real-World Scenarios

### 🏢 **Data Migration**
```sql
-- 1. Insert new data
INSERT INTO NewCustomers 
SELECT * FROM OldCustomers 
WHERE MigrationFlag = 0;

-- 2. Update migration status
UPDATE OldCustomers 
SET MigrationFlag = 1 
WHERE CustomerID IN (SELECT CustomerID FROM NewCustomers);

-- 3. Clean up old data
DELETE FROM OldCustomers 
WHERE MigrationFlag = 1 AND MigrationDate < DATEADD(month, -6, GETDATE());
```

### 📊 **Data Maintenance**
```sql
-- Regular cleanup routine
DELETE FROM LogTable 
WHERE LogDate < DATEADD(day, -30, GETDATE());

-- Data quality updates
UPDATE Products 
SET Price = 0 
WHERE Price IS NULL OR Price < 0;

-- Archive old records
INSERT INTO CustomerArchive 
SELECT * FROM Customers 
WHERE LastOrderDate < DATEADD(year, -2, GETDATE());
```

---

## 🎯 Key Takeaways

- ✅ **INSERT** adds new data safely with proper validation
- ✅ **UPDATE** modifies existing data - always use WHERE clause
- ✅ **DELETE** removes data - extremely dangerous without WHERE
- ✅ **TRUNCATE** is faster than DELETE for removing all rows
- ✅ Batch operations improve performance significantly
- ✅ Always test DML commands with SELECT first
- ✅ Use transactions for complex operations
- ✅ Backup critical data before bulk modifications

---

## 🚨 Safety Checklist

Before executing DML commands:
- [ ] WHERE clause specified (for UPDATE/DELETE)
- [ ] Test query with SELECT first
- [ ] Backup created (for bulk operations)
- [ ] Transaction boundaries defined
- [ ] Expected row count verified
- [ ] Rollback plan prepared

---

*Manipulate data with precision, modify with confidence! 📝*