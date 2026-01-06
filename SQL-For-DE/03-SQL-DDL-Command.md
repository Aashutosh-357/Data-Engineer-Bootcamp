# 🏗️ SQL DDL Commands - Database Structure Management

## 🎯 Learning Objectives
Master Data Definition Language (DDL) commands to create, modify, and manage database structures.

> ⚠️ **Critical Warning:** DDL commands modify database structure and can cause permanent data loss. Always backup before executing!

---

## 🔧 DDL Command Overview

**Data Definition Language (DDL)** commands define and modify the database schema:

| Command | Purpose | Risk Level | Reversible |
|---------|---------|------------|------------|
| **CREATE** | Build new objects | 🟢 Low | ✅ Yes |
| **ALTER** | Modify existing objects | 🟡 Medium | ⚠️ Partial |
| **DROP** | Remove objects completely | 🔴 High | ❌ No |

---

## 🆕 CREATE Command (0:29-6:00)

### 🎯 **Purpose**
Creates new database objects like tables, indexes, and schemas.

### 📋 **Basic Syntax**
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);
```

### 🏗️ **Practical Example: Persons Table**
```sql
CREATE TABLE Persons (
    ID INT PRIMARY KEY,
    PersonName VARCHAR(100) NOT NULL,
    BirthDate DATE,
    Phone VARCHAR(15)
);
```

### 🔍 **Component Breakdown**
| Element | Purpose | Example |
|---------|---------|---------|
| **Column Name** | Identifier | `PersonName` |
| **Data Type** | Storage format | `VARCHAR(100)` |
| **Constraints** | Data rules | `NOT NULL`, `PRIMARY KEY` |

### 📊 **Common Data Types**
| Type | Purpose | Example |
|------|---------|---------|
| `INT` | Whole numbers | `123` |
| `VARCHAR(n)` | Variable text | `'John Doe'` |
| `DATE` | Date values | `'2024-01-15'` |
| `DECIMAL(p,s)` | Precise numbers | `99.99` |

### ✅ **Key Features**
- **No Data Return:** DDL commands return success messages, not data
- **Structure Definition:** Establishes table blueprint
- **Constraint Enforcement:** Ensures data integrity

### 🔍 **Retrieving CREATE Statements**
```sql
-- View existing table structure
SHOW CREATE TABLE Persons;
-- or right-click table in database explorer
```

---

## ✏️ ALTER Command (6:37-9:34)

### 🎯 **Purpose**
Modifies existing database object definitions without recreating them.

### ➕ **Adding Columns**
```sql
ALTER TABLE Persons 
ADD Email VARCHAR(100);
```

**Result:** New `Email` column added to existing table structure.

### ➖ **Removing Columns**
```sql
ALTER TABLE Persons 
DROP COLUMN Phone;
```

> ⚠️ **Data Loss Warning:** Dropping columns permanently deletes all data in those columns!

### 🔄 **Modifying Columns**
```sql
-- Change data type
ALTER TABLE Persons 
ALTER COLUMN PersonName VARCHAR(150);

-- Add constraint
ALTER TABLE Persons 
ALTER COLUMN Email SET NOT NULL;
```

### 📊 **ALTER Operations Summary**
| Operation | Command | Data Impact |
|-----------|---------|-------------|
| Add Column | `ADD column_name datatype` | ✅ Safe |
| Drop Column | `DROP COLUMN column_name` | ❌ Data loss |
| Modify Column | `ALTER COLUMN column_name` | ⚠️ Depends |

---

## 🗑️ DROP Command (10:04-11:11)

### 🎯 **Purpose**
Completely removes database objects and all associated data.

### ⚡ **Basic Syntax**
```sql
DROP TABLE table_name;
```

### 🚨 **Example: Removing Persons Table**
```sql
DROP TABLE Persons;
```

**Result:** 
- ❌ Table structure deleted
- ❌ All data permanently lost
- ❌ Cannot be undone

### 🛡️ **Safety Considerations**
| Risk Factor | Impact | Mitigation |
|-------------|--------|------------|
| **Data Loss** | Permanent | Backup first |
| **Dependencies** | Cascade effects | Check references |
| **No Confirmation** | Immediate execution | Double-check syntax |

### 🔒 **Best Practices**
```sql
-- 1. Always backup first
BACKUP DATABASE YourDB TO DISK = 'backup_path';

-- 2. Check dependencies
SELECT * FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS 
WHERE TABLE_NAME = 'Persons';

-- 3. Use IF EXISTS for safety
DROP TABLE IF EXISTS Persons;
```

---

## 📊 DDL Command Comparison

| Aspect | CREATE | ALTER | DROP |
|--------|--------|-------|------|
| **Purpose** | Build new | Modify existing | Remove completely |
| **Data Safety** | ✅ Safe | ⚠️ Conditional | ❌ Dangerous |
| **Reversibility** | ✅ Easy | ⚠️ Partial | ❌ Impossible |
| **Performance** | Fast | Moderate | Instant |
| **Common Use** | Initial setup | Schema evolution | Cleanup |

---

## 🎯 Real-World Scenarios

### 🏢 **Development Lifecycle**
```sql
-- 1. Initial table creation
CREATE TABLE Users (
    UserID INT PRIMARY KEY,
    Username VARCHAR(50) NOT NULL
);

-- 2. Add new requirements
ALTER TABLE Users 
ADD Email VARCHAR(100),
ADD CreatedDate DATETIME DEFAULT GETDATE();

-- 3. Remove unused features
ALTER TABLE Users 
DROP COLUMN OldColumn;

-- 4. Clean up test tables
DROP TABLE TestUsers;
```

### 🔄 **Schema Migration Pattern**
```sql
-- Safe column addition
ALTER TABLE Products 
ADD NewColumn VARCHAR(100);

-- Data migration
UPDATE Products 
SET NewColumn = 'default_value';

-- Remove old column
ALTER TABLE Products 
DROP COLUMN OldColumn;
```

---

## ⚡ Performance Tips

### 🚀 **Optimization Strategies**
- **Batch Operations:** Group multiple ALTER statements
- **Off-Peak Timing:** Execute during low-traffic periods
- **Index Management:** Drop/recreate indexes for large changes
- **Transaction Control:** Use transactions for complex changes

### 📊 **Impact Assessment**
| Operation | Table Size Impact | Downtime |
|-----------|------------------|----------|
| CREATE | None | Minimal |
| ALTER (Add) | Minimal | Low |
| ALTER (Drop) | Significant | Medium |
| DROP | None | Minimal |

---

## 🎯 Key Takeaways

- ✅ **CREATE** builds new database structures safely
- ✅ **ALTER** modifies existing objects with varying risk levels
- ✅ **DROP** permanently removes objects and data
- ✅ Always backup before structural changes
- ✅ DDL commands return messages, not data
- ✅ Understanding constraints and data types is crucial
- ✅ Plan schema changes carefully in production

---

## 🚨 Safety Checklist

Before executing DDL commands:
- [ ] Database backup completed
- [ ] Dependencies identified
- [ ] Syntax verified in test environment
- [ ] Rollback plan prepared
- [ ] Team notification sent (for production)

---

*Structure with purpose, modify with caution! 🏗️*