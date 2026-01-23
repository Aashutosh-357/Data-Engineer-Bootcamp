# 🔍 SQL WHERE Conditions - Data Filtering Mastery

## 🎯 Learning Objectives
Master SQL WHERE conditions and operators to filter data with precision and efficiency.

> 💡 **Core Concept:** WHERE conditions are the gatekeepers of your data - they determine which rows make it into your results.

---

## 🎯 WHERE Conditions Overview

SQL WHERE conditions use **operators** to filter data based on specific criteria. These operators fall into five main categories:

```sql
SELECT * FROM customers 
WHERE [condition using operators];
```

---

## ⚖️ Comparison Operators (1:07-9:19)

### 📊 **Operator Reference Table**
| Operator | Symbol | Purpose | Example |
|----------|--------|---------|---------|
| **Equal** | `=` | Exact match | `country = 'Germany'` |
| **Not Equal** | `!=` or `<>` | Exclude matches | `country != 'Germany'` |
| **Greater Than** | `>` | Values above | `score > 500` |
| **Greater/Equal** | `>=` | Values above/equal | `score >= 500` |
| **Less Than** | `<` | Values below | `score < 500` |
| **Less/Equal** | `<=` | Values below/equal | `score <= 500` |

### 🎯 **Practical Examples**

#### **Equality Filtering (5:05-5:49)**
```sql
-- Find customers from Germany
SELECT * FROM customers 
WHERE country = 'Germany';

-- Find customers NOT from Germany  
SELECT * FROM customers 
WHERE country != 'Germany';
-- Alternative syntax
SELECT * FROM customers 
WHERE country <> 'Germany';
```

#### **Numeric Comparisons (6:27-8:53)**
```sql
-- High-score customers
SELECT * FROM customers 
WHERE score > 500;

-- Premium tier (500 and above)
SELECT * FROM customers 
WHERE score >= 500;

-- Below average performers
SELECT * FROM customers 
WHERE score < 500;

-- Budget tier (500 and below)
SELECT * FROM customers 
WHERE score <= 500;
```

---

## 🔗 Logical Operators (9:19-20:45)

### 🧠 **Logic Gate Behavior**
| Operator | Logic | Result Condition |
|----------|-------|------------------|
| **AND** | All must be true | Every condition satisfied |
| **OR** | At least one true | Any condition satisfied |
| **NOT** | Reverse logic | Opposite of condition |

### 🎯 **AND Operator (9:33-14:17)**
**Rule:** ALL conditions must be true for row inclusion.

```sql
-- Premium US customers
SELECT * FROM customers 
WHERE country = 'USA' AND score > 500;

-- Multiple AND conditions
SELECT * FROM customers 
WHERE country = 'USA' 
  AND score > 500 
  AND registration_date > '2023-01-01';
```

**Truth Table for AND:**
| Condition 1 | Condition 2 | Result |
|-------------|-------------|--------|
| True | True | ✅ Include |
| True | False | ❌ Exclude |
| False | True | ❌ Exclude |
| False | False | ❌ Exclude |

### 🎯 **OR Operator (14:17-17:03)**
**Rule:** AT LEAST ONE condition must be true for row inclusion.

```sql
-- US customers OR high scorers
SELECT * FROM customers 
WHERE country = 'USA' OR score > 500;

-- Multiple OR conditions
SELECT * FROM customers 
WHERE country = 'USA' 
   OR country = 'Canada' 
   OR score > 800;
```

**Truth Table for OR:**
| Condition 1 | Condition 2 | Result |
|-------------|-------------|--------|
| True | True | ✅ Include |
| True | False | ✅ Include |
| False | True | ✅ Include |
| False | False | ❌ Exclude |

### 🎯 **NOT Operator (17:03-20:45)**
**Rule:** Reverses the truth value of conditions.

```sql
-- Customers NOT from Germany
SELECT * FROM customers 
WHERE NOT country = 'Germany';

-- Scores NOT less than 500 (equivalent to >= 500)
SELECT * FROM customers 
WHERE NOT score < 500;
```

---

## 📏 Range Operator (20:45-24:54)

### 🎯 **BETWEEN Operator**
**Purpose:** Check if values fall within a specified range (inclusive).

```sql
-- Customers with scores between 100 and 500 (inclusive)
SELECT * FROM customers 
WHERE score BETWEEN 100 AND 500;

-- Equivalent using AND
SELECT * FROM customers 
WHERE score >= 100 AND score <= 500;
```

### 📊 **BETWEEN vs AND Comparison**
| Method | Readability | Performance | Use Case |
|--------|-------------|-------------|----------|
| `BETWEEN` | ✅ High | ✅ Optimized | Range queries |
| `AND` | ⚠️ Verbose | ✅ Good | Complex conditions |

### 🗓️ **Date Range Examples**
```sql
-- Orders from last quarter
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-03-31';

-- Working hours
SELECT * FROM time_logs 
WHERE hour_logged BETWEEN 9 AND 17;
```

---

## 📋 Membership Operators (24:54-29:09)

### 🎯 **IN Operator (25:05-27:39)**
**Purpose:** Check if value exists in a defined list.

```sql
-- Customers from specific countries
SELECT * FROM customers 
WHERE country IN ('Germany', 'USA', 'Canada');

-- Equivalent using OR (more verbose)
SELECT * FROM customers 
WHERE country = 'Germany' 
   OR country = 'USA' 
   OR country = 'Canada';
```

### 🎯 **NOT IN Operator (26:25)**
```sql
-- Exclude specific countries
SELECT * FROM customers 
WHERE country NOT IN ('Germany', 'France', 'Spain');
```

### 📊 **IN vs OR Comparison**
| Method | Readability | Performance | Maintenance |
|--------|-------------|-------------|-------------|
| `IN` | ✅ Excellent | ✅ Optimized | ✅ Easy |
| `OR` | ❌ Poor | ⚠️ Slower | ❌ Difficult |

### 🔢 **Numeric IN Examples**
```sql
-- Specific score tiers
SELECT * FROM customers 
WHERE score IN (100, 200, 300, 500, 1000);

-- Exclude test accounts
SELECT * FROM customers 
WHERE customer_id NOT IN (999, 998, 997);
```

---

## 🔍 Search Operator (29:09-39:00)

### 🎯 **LIKE Operator with Wildcards**
**Purpose:** Pattern matching in text data.

### 🃏 **Wildcard Characters**
| Wildcard | Meaning | Example | Matches |
|----------|---------|---------|---------|
| `%` | Zero or more characters | `'M%'` | Mike, Mary, Mountain |
| `_` | Exactly one character | `'M_ke'` | Mike, Make, Moke |

### 📝 **Pattern Examples**

#### **Starts With Pattern (35:55)**
```sql
-- Names starting with 'M'
SELECT * FROM customers 
WHERE customer_name LIKE 'M%';
-- Matches: Mike, Mary, Mountain, etc.
```

#### **Ends With Pattern (36:41)**
```sql
-- Names ending with 'N'
SELECT * FROM customers 
WHERE customer_name LIKE '%N';
-- Matches: John, Susan, Martin, etc.
```

#### **Contains Pattern (37:23)**
```sql
-- Names containing 'R' anywhere
SELECT * FROM customers 
WHERE customer_name LIKE '%R%';
-- Matches: Robert, Mary, Christopher, etc.
```

#### **Position-Specific Pattern (38:16)**
```sql
-- 'R' in third position
SELECT * FROM customers 
WHERE customer_name LIKE '__R%';
-- Matches: Aaron, Boris, Carol, etc.
```

### 🔍 **Advanced LIKE Patterns**
```sql
-- Email validation pattern
SELECT * FROM customers 
WHERE email LIKE '%@%.%';

-- Phone number pattern (US format)
SELECT * FROM customers 
WHERE phone LIKE '___-___-____';

-- Product codes starting with 'PRD'
SELECT * FROM products 
WHERE product_code LIKE 'PRD%';
```

---

## 🔄 Combining Operators

### 🧩 **Complex Condition Examples**
```sql
-- Premium customers from specific regions
SELECT * FROM customers 
WHERE (country = 'USA' OR country = 'Canada')
  AND score > 500
  AND registration_date BETWEEN '2023-01-01' AND '2023-12-31';

-- Search with exclusions
SELECT * FROM customers 
WHERE customer_name LIKE 'A%'
  AND country NOT IN ('Germany', 'France')
  AND score >= 300;

-- Range with pattern matching
SELECT * FROM products 
WHERE price BETWEEN 10.00 AND 100.00
  AND product_name LIKE '%Pro%'
  AND category_id IN (1, 2, 3);
```

### 🎯 **Operator Precedence**
```sql
-- Parentheses control evaluation order
SELECT * FROM customers 
WHERE (country = 'USA' OR country = 'Canada') 
  AND score > 500;

-- Without parentheses (different result!)
SELECT * FROM customers 
WHERE country = 'USA' OR country = 'Canada' 
  AND score > 500;
```

---

## 📊 Performance Optimization Tips

### 🚀 **Index-Friendly Conditions**
| Good Practice | Poor Practice | Reason |
|---------------|---------------|--------|
| `WHERE id = 123` | `WHERE id + 1 = 124` | Avoid functions on columns |
| `WHERE name LIKE 'John%'` | `WHERE name LIKE '%John%'` | Leading wildcards prevent index use |
| `WHERE date >= '2024-01-01'` | `WHERE YEAR(date) = 2024` | Direct comparison vs function |

### 📈 **Condition Ordering**
```sql
-- Most selective conditions first
SELECT * FROM customers 
WHERE customer_id = 123        -- Most selective
  AND country = 'USA'          -- Moderately selective  
  AND status = 'Active';       -- Least selective
```

---

## 🎯 Key Takeaways

- ✅ **Comparison operators** handle exact matches and numeric ranges
- ✅ **Logical operators** (AND, OR, NOT) combine multiple conditions
- ✅ **BETWEEN** simplifies range queries with inclusive boundaries
- ✅ **IN/NOT IN** efficiently handle multiple value matching
- ✅ **LIKE** with wildcards enables flexible text pattern matching
- ✅ **Parentheses** control operator precedence in complex conditions
- ✅ **Index-friendly** conditions improve query performance
- ✅ **Combine operators** for sophisticated data filtering

---

## 🚀 Next Steps
Master these fundamentals before advancing to:
- Subqueries in WHERE clauses
- EXISTS and NOT EXISTS
- Advanced pattern matching with REGEX
- NULL handling in conditions

---

*Filter with precision, query with confidence! 🔍*