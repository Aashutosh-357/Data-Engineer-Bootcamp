# 🛠️ SQL Functions: The Engine of Data Transformation

> **🎓 Tailored for Junior Data Engineers at Mid-Product Companies**  
> *Transform messy data into production-ready datasets with confidence*

---

## 🎯 Career-Focused Learning Path

| Aspect | Details |
|--------|---------|
| **📌 Topic** | SQL Data Manipulation & Aggregation |
| **👨‍💼 Role** | Data Engineer (Mid-Product Company) |
| **🎯 Goal** | Master the difference between **Cleaning Data** (Single-Row) and **Summarizing Data** (Multi-Row) to build reliable ETL pipelines |
| **⏱️ Time to Master** | 3-4 weeks with daily practice |
| **💰 Impact** | Reduce data quality issues by 80%, enable self-service analytics |

### 💡 Industry Reality Check

> **You will spend 70% of your SQL time using Single-Row Functions.**  
> 
> As a Junior DE at a mid-product company, you'll use functions like `TRIM`, `COALESCE`, `CAST` to fix messy CSV uploads, API responses, and user-generated content. This ensures Data Analysts can actually use the **Multi-Row Functions** (`SUM`, `AVG`) without their charts breaking.
>
> **The harsh truth:** If you can't clean data, you can't be a Data Engineer. Period.

---

## 💼 What You'll Actually Do at a Mid-Product Company

In a product company (100-1000 employees), raw data is **never** perfect. Here's your daily reality:

### 📊 Real-World Use Cases

| Concept | Daily Task | Business Impact | Example Scenario |
|---------|------------|-----------------|------------------|
| **🔤 Single-Row Functions** | `LOWER(TRIM(email))` | Ensuring `User@Gmail.com ` and `user@gmail.com` are treated as the same user | User deduplication for marketing campaigns |
| **📊 Multi-Row (Aggregate)** | `MAX(login_date)` | Finding the last time a user was active to calculate "Churn Risk" | Customer retention dashboards |
| **🔗 Nesting Functions** | `ROUND(AVG(price), 2)` | Formatting financial data for export to the accounting system | Monthly revenue reports |
| **🛡️ NULL Handling** | `COALESCE(phone, 'Unknown')` | Preventing the "Send SMS" pipeline from crashing on empty values | Notification systems |
| **✂️ String Extraction** | `SUBSTRING(sku, 1, 3)` | Extracting category codes from product IDs | Inventory categorization |

### 🎬 A Day in the Life

**9:00 AM** - Data Analyst: *"Why are there duplicate users in the dashboard?"*  
**Without Functions:** Manually export, clean in Excel, re-upload (2 hours)  
**With Functions:** `SELECT DISTINCT LOWER(TRIM(email)) FROM users` (30 seconds) ✨

**2:00 PM** - Product Manager: *"What's our average order value by region?"*  
**Without Functions:** Write 5 separate queries, combine in spreadsheet  
**With Functions:** One query with `AVG()` and `GROUP BY` (1 minute) 🚀

---

## 📚 Technical Deep Dive

### 🔍 Part 1: The Two Main Engines

Understanding the **"Grain"** (Granularity) of your output is critical for writing correct SQL.

#### 1️⃣ Single-Row (Scalar) Functions

**Concept:** One row in → One row out (row count stays the same)

| Attribute | Details |
|-----------|---------|
| **Input** | 1 Row → **Output:** 1 Row |
| **Usage** | Transformation, Cleaning, Formatting |
| **Examples** | `UPPER()`, `SUBSTR()`, `LENGTH()`, `CAST()`, `COALESCE()` |
| **Rule** | The number of rows in the result equals the number of rows in the source |
| **When to Use** | Data cleaning, standardization, type conversion |

**Example:**
```sql
-- Input: 5 rows → Output: 5 rows (same count)
SELECT 
    customer_id,
    UPPER(TRIM(email)) as clean_email,        -- Single-row function
    COALESCE(phone, 'N/A') as clean_phone     -- Single-row function
FROM customers;
```

#### 2️⃣ Multi-Row (Aggregate) Functions

**Concept:** Many rows in → One row out (data collapses)

| Attribute | Details |
|-----------|---------|
| **Input** | N Rows → **Output:** 1 Row (per group) |
| **Usage** | Reporting, KPI calculation, summarization |
| **Examples** | `SUM()`, `COUNT()`, `MIN()`, `MAX()`, `AVG()` |
| **Rule** | Collapses data. Usually requires a `GROUP BY` clause if other columns are present |
| **When to Use** | Creating reports, calculating totals, finding extremes |

**Example:**
```sql
-- Input: 1000 rows → Output: 1 row (collapsed)
SELECT 
    COUNT(*) as total_orders,                 -- Multi-row function
    SUM(amount) as total_revenue,             -- Multi-row function
    AVG(amount) as avg_order_value            -- Multi-row function
FROM orders;
```

#### 📊 Comparison Table

| Feature | Single-Row Functions | Multi-Row (Aggregate) Functions |
|---------|---------------------|--------------------------------|
| **Row Count** | Preserved (1:1) | Reduced (N:1) |
| **GROUP BY Required?** | No | Yes (if selecting other columns) |
| **Use in WHERE?** | Yes | No (use HAVING instead) |
| **Examples** | `TRIM()`, `CAST()`, `SUBSTRING()` | `SUM()`, `COUNT()`, `AVG()` |
| **Primary User** | Data Engineers (cleaning) | Data Analysts (reporting) |
| **Performance** | Fast (row-by-row) | Slower (requires grouping) |

---

### 🧅 Part 2: The Logic of Nesting

Functions work like an **Onion**: You process from the **Inside Out**.

#### 🎯 Execution Order

```sql
-- Reading order (for humans): Left to Right
-- Execution order (for SQL): Inside to Outside

SELECT CAST(TRIM(REPLACE(price_str, '$', '')) AS INT) as clean_price
FROM products;

-- Step-by-step execution:
-- 1. REPLACE(' $100 ', '$', '')  → ' 100 '   (Remove dollar sign)
-- 2. TRIM(' 100 ')                → '100'     (Remove spaces)
-- 3. CAST('100' AS INT)           → 100       (Convert to integer)
```

#### 💡 Real-World Nesting Patterns

```sql
-- Pattern 1: Clean → Transform → Aggregate
SELECT 
    ROUND(AVG(CAST(TRIM(price) AS DECIMAL)), 2) as avg_price
FROM products;
-- Execution: TRIM → CAST → AVG → ROUND

-- Pattern 2: Extract → Standardize → Group
SELECT 
    UPPER(LEFT(TRIM(category), 3)) as category_code,
    COUNT(*) as product_count
FROM products
GROUP BY UPPER(LEFT(TRIM(category), 3));
-- Execution: TRIM → LEFT → UPPER → COUNT

-- Pattern 3: Handle NULLs → Calculate → Format
SELECT 
    ROUND(SUM(COALESCE(revenue, 0)), 2) as total_revenue
FROM sales;
-- Execution: COALESCE → SUM → ROUND
```

---

### 🚀 Part 3: Window Functions (The "Hybrid")

**Advanced concept:** Perform Multi-Row calculations *without* collapsing rows.

#### 🎯 Key Concept

Window functions let you add aggregated columns while keeping the detail rows intact.

```sql
-- Traditional Aggregate (collapses rows)
SELECT 
    customer_id,
    SUM(amount) as total_spent
FROM orders
GROUP BY customer_id;
-- Result: 1 row per customer

-- Window Function (keeps all rows)
SELECT 
    order_id,
    customer_id,
    amount,
    SUM(amount) OVER (PARTITION BY customer_id) as customer_total
FROM orders;
-- Result: All original rows + running total column
```

#### 📊 Common Window Functions

| Function | Purpose | Example Use Case |
|----------|---------|------------------|
| `ROW_NUMBER()` | Assign unique row numbers | Deduplication, pagination |
| `RANK()` | Rank with gaps | Top N products by sales |
| `DENSE_RANK()` | Rank without gaps | Leaderboards |
| `SUM() OVER()` | Running total | Cumulative revenue |
| `AVG() OVER()` | Moving average | 7-day average metrics |
| `LAG()` / `LEAD()` | Previous/next row value | Month-over-month growth |

**Example:**
```sql
-- Calculate running total and rank
SELECT 
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) as running_total,
    RANK() OVER (ORDER BY amount DESC) as amount_rank
FROM orders;
```

---

## 🛠️ Hands-On Lab: The "Dirty Data" Scrubber

### 📋 Scenario

You're a Junior DE at a mid-sized e-commerce company. The marketing team uploaded a CSV of customer transactions with these issues:
1. **Names** have inconsistent casing and trailing spaces
2. **Amounts** are NULL for failed transactions
3. **Dates** are stored as strings
4. **Emails** have mixed case

**Your mission:** Clean this data and generate a summary report for the CFO.

### 🎯 Learning Objectives

1. ✅ Apply single-row functions for data cleaning
2. ✅ Use multi-row functions for aggregation
3. ✅ Handle NULL values gracefully
4. ✅ Combine multiple function types in one query

### 🐍 The Code (Production-Style)

```python
import sqlite3
import pandas as pd
import logging
from datetime import datetime

# Setup Professional Logging
logging.basicConfig(
    level=logging.INFO, 
    format='%(asctime)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)

class DataScrubber:
    """
    Production-style data cleaning pipeline demonstrating:
    - Single-Row Functions (cleaning)
    - Multi-Row Functions (aggregation)
    - Function nesting
    - NULL handling
    """

    def __init__(self):
        """Initialize in-memory SQLite database."""
        self.conn = sqlite3.connect(':memory:')
        self.cursor = self.conn.cursor()
        logger.info("🔌 Database connection established")

    def setup_messy_data(self):
        """
        Creates a realistic table with common data quality issues:
        - Inconsistent casing
        - Leading/trailing spaces
        - NULL values
        - Mixed formats
        """
        self.cursor.execute('''
            CREATE TABLE raw_sales (
                id INTEGER PRIMARY KEY,
                customer_name TEXT,
                customer_email TEXT,
                transaction_amount REAL,
                txn_date TEXT,
                status TEXT
            )
        ''')
        
        # Realistic messy data
        data = [
            (1, ' alice ', 'Alice@GMAIL.com ', 100.50, '2023-10-01', 'completed'),
            (2, 'BOB', 'bob@yahoo.com', 200.00, '2023-10-01', 'COMPLETED'),
            (3, 'Alice', 'alice@gmail.com', None, '2023-10-02', 'failed'),
            (4, ' charlie', 'Charlie@Hotmail.COM ', 50.25, '2023-10-02', 'completed'),
            (5, 'Bob', 'BOB@yahoo.com ', 150.00, '2023-10-03', 'pending'),
            (6, 'ALICE ', 'alice@GMAIL.com', 75.00, '2023-10-03', 'completed'),
        ]
        self.cursor.executemany(
            'INSERT INTO raw_sales VALUES (?,?,?,?,?,?)', 
            data
        )
        self.conn.commit()
        logger.info(f"✅ Loaded {len(data)} rows of messy data")

    def run_single_row_cleaning(self):
        """
        Step 1: Row-level cleaning using Single-Row Functions.
        
        Demonstrates:
        - String standardization (LOWER, TRIM)
        - NULL handling (COALESCE)
        - String extraction (SUBSTR)
        - Function nesting
        
        Input: 6 rows → Output: 6 rows (same count)
        """
        query = """
        SELECT 
            id,
            
            -- 1. NESTED STRING FUNCTIONS: Standardize name
            -- Execution: TRIM first, then LOWER
            LOWER(TRIM(customer_name)) as clean_name,
            
            -- 2. COMPLEX NESTING: Clean and standardize email
            -- Execution: TRIM → LOWER
            LOWER(TRIM(customer_email)) as clean_email,
            
            -- 3. NULL HANDLING: Replace NULL amounts with 0
            COALESCE(transaction_amount, 0) as clean_amount,
            
            -- 4. STRING EXTRACTION: Get day from date
            -- SQL is 1-indexed: position 9, length 2
            SUBSTR(txn_date, 9, 2) as day_part,
            
            -- 5. STANDARDIZATION: Lowercase status
            LOWER(TRIM(status)) as clean_status,
            
            -- 6. CONDITIONAL LOGIC: Flag high-value transactions
            CASE 
                WHEN COALESCE(transaction_amount, 0) >= 100 THEN 'High Value'
                WHEN COALESCE(transaction_amount, 0) > 0 THEN 'Standard'
                ELSE 'Failed'
            END as value_tier
            
        FROM raw_sales
        ORDER BY id
        """
        
        logger.info("🧹 Running single-row cleaning functions...")
        df = pd.read_sql_query(query, self.conn)
        
        print("\n" + "="*100)
        print("SINGLE-ROW CLEANING RESULTS (Row count preserved: 6 → 6)")
        print("="*100)
        print(df.to_string(index=False))
        print("="*100 + "\n")
        
        return df

    def run_multi_row_aggregation(self):
        """
        Step 2: Aggregation using Multi-Row Functions.
        
        Demonstrates:
        - Grouping (GROUP BY)
        - Aggregation (COUNT, SUM, AVG)
        - Nested functions (ROUND + SUM)
        - NULL-safe aggregation
        
        Input: 6 rows → Output: 3 rows (collapsed by customer)
        """
        query = """
        SELECT 
            -- GROUP BY key (cleaned)
            LOWER(TRIM(customer_name)) as customer,
            LOWER(TRIM(customer_email)) as email,
            
            -- AGGREGATE: Count transactions
            COUNT(id) as txn_count,
            
            -- NESTED AGGREGATION: Sum with NULL handling, then round
            -- Execution: COALESCE → SUM → ROUND
            ROUND(SUM(COALESCE(transaction_amount, 0)), 2) as total_spend,
            
            -- AGGREGATE: Average (ignores NULLs automatically)
            ROUND(AVG(transaction_amount), 2) as avg_transaction,
            
            -- AGGREGATE: Max transaction
            MAX(transaction_amount) as largest_transaction,
            
            -- CONDITIONAL AGGREGATION: Count completed only
            SUM(CASE WHEN LOWER(status) = 'completed' THEN 1 ELSE 0 END) as completed_count
            
        FROM raw_sales
        GROUP BY LOWER(TRIM(customer_name)), LOWER(TRIM(customer_email))
        ORDER BY total_spend DESC
        """
        
        logger.info("📊 Running multi-row aggregation functions...")
        df = pd.read_sql_query(query, self.conn)
        
        print("\n" + "="*100)
        print("MULTI-ROW AGGREGATION RESULTS (Row count reduced: 6 → 3)")
        print("="*100)
        print(df.to_string(index=False))
        print("="*100 + "\n")
        
        return df

    def run_combined_analysis(self):
        """
        Step 3: Advanced analysis combining both function types.
        
        Demonstrates real-world reporting scenario.
        """
        query = """
        WITH cleaned_data AS (
            -- CTE: Clean the data first
            SELECT 
                LOWER(TRIM(customer_name)) as customer,
                COALESCE(transaction_amount, 0) as amount,
                LOWER(status) as status,
                SUBSTR(txn_date, 1, 7) as month
            FROM raw_sales
        )
        SELECT 
            month,
            COUNT(*) as total_transactions,
            SUM(amount) as total_revenue,
            ROUND(AVG(amount), 2) as avg_transaction,
            SUM(CASE WHEN status = 'completed' THEN amount ELSE 0 END) as completed_revenue,
            ROUND(
                100.0 * SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END) / COUNT(*),
                1
            ) as completion_rate_pct
        FROM cleaned_data
        GROUP BY month
        ORDER BY month
        """
        
        logger.info("🎯 Running combined analysis...")
        df = pd.read_sql_query(query, self.conn)
        
        print("\n" + "="*100)
        print("COMBINED ANALYSIS (Monthly Summary)")
        print("="*100)
        print(df.to_string(index=False))
        print("="*100 + "\n")
        
        return df

    def close(self):
        """Clean up database connection."""
        self.conn.close()
        logger.info("🔌 Database connection closed")

# ============================================================================
# MAIN EXECUTION
# ============================================================================

if __name__ == "__main__":
    print("\n" + "🚀 SQL Functions - Production Demo".center(100, "="))
    print("Demonstrating Single-Row vs Multi-Row Functions\n")
    
    scrubber = DataScrubber()
    
    try:
        # Step 1: Setup messy data
        scrubber.setup_messy_data()
        
        # Step 2: Single-row cleaning (preserves row count)
        scrubber.run_single_row_cleaning()
        
        # Step 3: Multi-row aggregation (reduces row count)
        scrubber.run_multi_row_aggregation()
        
        # Step 4: Combined analysis
        scrubber.run_combined_analysis()
        
        logger.info("✅ All queries executed successfully!")
        
    except Exception as e:
        logger.error(f"❌ Error: {e}")
        raise
    finally:
        scrubber.close()
    
    print("\n" + "="*100)
    print("💡 Key Insight: Single-Row functions clean, Multi-Row functions summarize!")
    print("="*100 + "\n")
```

### 🎯 Expected Output

```
SINGLE-ROW CLEANING RESULTS (Row count preserved: 6 → 6)
====================================================================================================
 id  clean_name     clean_email        clean_amount  day_part  clean_status  value_tier
  1       alice  alice@gmail.com             100.50        01     completed  High Value
  2         bob   bob@yahoo.com              200.00        01     completed  High Value
  3       alice  alice@gmail.com                0.00        02        failed      Failed
  4     charlie  charlie@hotmail.com           50.25        02     completed    Standard
  5         bob   bob@yahoo.com              150.00        03       pending  High Value
  6       alice  alice@gmail.com               75.00        03     completed    Standard
====================================================================================================

MULTI-ROW AGGREGATION RESULTS (Row count reduced: 6 → 3)
====================================================================================================
customer              email  txn_count  total_spend  avg_transaction  largest_transaction  completed_count
   alice  alice@gmail.com          3       175.50            87.75               100.50                2
     bob   bob@yahoo.com           2       350.00           175.00               200.00                1
 charlie  charlie@hotmail.com      1        50.25            50.25                50.25                1
====================================================================================================
```

---

## 🎯 Common Interview Questions & Professional Answers

### Q1: What is the difference between `WHERE` and `HAVING`?

> **Professional Answer:**  
> "It comes down to *when* the filtering happens in the SQL execution order. `WHERE` filters data **before** any grouping or aggregation occurs (Single-Row context). `HAVING` filters data **after** the aggregation is complete (Multi-Row context).
> 
> For example, I use `WHERE` to filter for 'Sales in 2023' before grouping, and `HAVING` to filter for 'Customers with SUM(Sales) > 1000' after grouping. Using `WHERE` when possible is more efficient because it reduces the data before the expensive GROUP BY operation."

**Example:**
```sql
-- ❌ WRONG: Can't use aggregate in WHERE
SELECT customer_id, SUM(amount)
FROM orders
WHERE SUM(amount) > 1000  -- ERROR!
GROUP BY customer_id;

-- ✅ CORRECT: Use HAVING for aggregates
SELECT customer_id, SUM(amount) as total
FROM orders
WHERE order_date >= '2023-01-01'  -- Filter before grouping
GROUP BY customer_id
HAVING SUM(amount) > 1000;        -- Filter after grouping
```

---

### Q2: How do NULL values affect Aggregate functions?

> **Professional Answer:**  
> "Most aggregate functions like `SUM`, `AVG`, and `MAX` **ignore NULLs completely**. The exception is `COUNT(*)`, which counts rows regardless of content. `COUNT(column_name)` ignores NULLs in that specific column.
> 
> This is why I often use `COALESCE(col, 0)` inside an aggregate if I want NULLs to be treated as zero. Otherwise, you might get unexpected results like `AVG()` excluding NULL rows from the calculation, which could skew your averages."

**Example:**
```sql
-- Data: [10, 20, NULL, 30]

SELECT 
    COUNT(*) as total_rows,              -- 4 (counts all rows)
    COUNT(amount) as non_null_count,     -- 3 (ignores NULL)
    SUM(amount) as sum_amount,           -- 60 (ignores NULL)
    AVG(amount) as avg_amount,           -- 20 (60/3, not 60/4!)
    SUM(COALESCE(amount, 0)) as sum_with_null  -- 60 (treats NULL as 0)
FROM sales;
```

---

### Q3: What determines the order of execution in a nested function?

> **Professional Answer:**  
> "SQL evaluates nested functions from the **innermost** parentheses outwards, just like mathematical expressions. If I write `ROUND(AVG(COALESCE(price, 0)), 2)`, SQL executes:
> 1. `COALESCE(price, 0)` - Replace NULLs
> 2. `AVG(...)` - Calculate average
> 3. `ROUND(..., 2)` - Round the result
> 
> Understanding this is critical for debugging. If you get an error, trace from the inside out to find which function is failing."

---

### Q4: Can I use aggregate functions without GROUP BY?

> **Professional Answer:**  
> "Yes, but only when you want to aggregate the **entire table** into a single row. If you include any non-aggregated columns in your SELECT without GROUP BY, you'll get an error in most databases (except MySQL with certain settings, which is dangerous).
> 
> The rule is: Every column in SELECT must either be:
> 1. Inside an aggregate function, OR
> 2. Listed in the GROUP BY clause"

**Example:**
```sql
-- ✅ VALID: Aggregate entire table
SELECT COUNT(*), SUM(amount)
FROM orders;

-- ❌ INVALID: Mixing aggregated and non-aggregated
SELECT customer_id, SUM(amount)
FROM orders;
-- Error: customer_id not in GROUP BY

-- ✅ VALID: Add GROUP BY
SELECT customer_id, SUM(amount)
FROM orders
GROUP BY customer_id;
```

---

### Q5: What's the difference between `LENGTH()` and `LEN()`?

> **Professional Answer:**  
> "This is database-specific. PostgreSQL and SQLite use `LENGTH()`, while SQL Server uses `LEN()`. MySQL supports both. I always check the documentation for the specific database I'm using.
> 
> Additionally, `LEN()` in SQL Server ignores trailing spaces, while `LENGTH()` in PostgreSQL counts them. For production code, I always `TRIM()` first to ensure consistent behavior across databases."

---

## 🎓 Best Practices for Production Code

### ✅ DO's

| Practice | Why | Example |
|----------|-----|---------|
| **Always TRIM before comparing** | Prevents silent join failures | `WHERE TRIM(email) = TRIM(input)` |
| **Use COALESCE for NULL safety** | Prevents NULL propagation | `COALESCE(amount, 0)` |
| **Lowercase emails before storing** | Ensures uniqueness | `LOWER(TRIM(email))` |
| **Round financial data** | Prevents floating-point errors | `ROUND(price, 2)` |
| **Comment complex nesting** | Helps future maintainers | `-- Step 1: Remove spaces` |
| **Use CTEs for readability** | Breaks complex queries into steps | `WITH cleaned AS (...)` |
| **Test with NULL values** | Catches edge cases | Include NULLs in test data |

### ❌ DON'Ts

| Anti-Pattern | Why It's Bad | Better Approach |
|--------------|--------------|-----------------|
| **Hardcoding string positions** | Breaks when data format changes | Use dynamic `LENGTH()` calculations |
| **Mixing aggregated and non-aggregated columns** | Causes SQL errors | Always use GROUP BY |
| **Using aggregate in WHERE** | Syntax error | Use HAVING instead |
| **Ignoring NULL handling** | Silent data quality issues | Explicit COALESCE or IS NULL checks |
| **Nesting too deeply** | Unmaintainable code | Use CTEs to break into steps |
| **Assuming 0-indexing** | SQL is 1-indexed | Remember: `SUBSTRING(col, 1, 3)` |

---

## 📊 Function Cheat Sheet

### 🔤 String Functions Quick Reference

| Function | Purpose | Example | Result |
|----------|---------|---------|--------|
| `UPPER(str)` | Convert to uppercase | `UPPER('hello')` | `'HELLO'` |
| `LOWER(str)` | Convert to lowercase | `LOWER('HELLO')` | `'hello'` |
| `TRIM(str)` | Remove leading/trailing spaces | `TRIM(' hi ')` | `'hi'` |
| `LENGTH(str)` | Count characters | `LENGTH('hello')` | `5` |
| `SUBSTRING(str, start, len)` | Extract substring | `SUBSTRING('hello', 2, 3)` | `'ell'` |
| `LEFT(str, n)` | First n characters | `LEFT('hello', 2)` | `'he'` |
| `RIGHT(str, n)` | Last n characters | `RIGHT('hello', 2)` | `'lo'` |
| `CONCAT(a, b)` | Combine strings | `CONCAT('hi', ' there')` | `'hi there'` |
| `REPLACE(str, old, new)` | Replace substring | `REPLACE('hi-there', '-', ' ')` | `'hi there'` |

### 🔢 Number Functions Quick Reference

| Function | Purpose | Example | Result |
|----------|---------|---------|--------|
| `ROUND(num, decimals)` | Round to n decimals | `ROUND(10.556, 2)` | `10.56` |
| `ABS(num)` | Absolute value | `ABS(-10)` | `10` |
| `CEIL(num)` | Round up | `CEIL(10.1)` | `11` |
| `FLOOR(num)` | Round down | `FLOOR(10.9)` | `10` |
| `MOD(a, b)` | Modulo (remainder) | `MOD(10, 3)` | `1` |
| `POWER(base, exp)` | Exponentiation | `POWER(2, 3)` | `8` |

### 📊 Aggregate Functions Quick Reference

| Function | Purpose | NULL Handling | Example |
|----------|---------|---------------|---------|
| `COUNT(*)` | Count all rows | Includes NULLs | `COUNT(*)` |
| `COUNT(col)` | Count non-NULL values | Ignores NULLs | `COUNT(email)` |
| `SUM(col)` | Sum values | Ignores NULLs | `SUM(amount)` |
| `AVG(col)` | Average | Ignores NULLs | `AVG(price)` |
| `MIN(col)` | Minimum value | Ignores NULLs | `MIN(date)` |
| `MAX(col)` | Maximum value | Ignores NULLs | `MAX(score)` |

---

## 🚀 Key Takeaways for Junior Data Engineers

### 🏆 The 3 Pillars of SQL Functions Mastery

1. **🧹 DEs Clean, DAs Analyze**  
   As a Data Engineer, your mastery of **Single-Row functions** (`TRIM`, `CAST`, `COALESCE`) is what ensures the Data Analysts receive clean tables. This is 70% of your job.

2. **⚖️ Don't Mix Granularities**  
   You cannot put a non-aggregated column (like `customer_name`) next to an aggregated column (`SUM(price)`) without including it in the `GROUP BY` clause. This is the #1 error for beginners.

3. **🛡️ NULL Safety First**  
   Always use `COALESCE` or `IFNULL` when performing math on columns that might contain empty data. Otherwise, your pipeline might propagate NULLs downstream and break dashboards.

### 📈 Career Progression

| Level | Function Skills |
|-------|-----------------|
| **Junior** | Basic string cleaning (`TRIM`, `LOWER`), simple aggregates (`SUM`, `COUNT`) |
| **Mid-Level** | Complex nesting, window functions, performance optimization |
| **Senior** | Custom functions, query optimization, teaching others |

### 🎓 Next Steps

1. ✅ **Practice daily:** Add function calls to every query you write this week
2. ✅ **Build a cleaning library:** Create a collection of common cleaning patterns
3. ✅ **Learn your database:** Study the specific functions available in your company's database
4. ✅ **Master CTEs:** Use Common Table Expressions to make complex queries readable

---

## 📚 Additional Resources

### 📖 Recommended Reading
- **"SQL Cookbook"** by Anthony Molinaro - Advanced function patterns
- **Database-specific documentation:**
  - [PostgreSQL String Functions](https://www.postgresql.org/docs/current/functions-string.html)
  - [MySQL String Functions](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)
  - [SQL Server String Functions](https://docs.microsoft.com/en-us/sql/t-sql/functions/string-functions-transact-sql)

### 🎥 Video Tutorials
- **"SQL String Functions Deep Dive"** - Advanced cleaning techniques
- **"Aggregate Functions Masterclass"** - Beyond basic SUM and COUNT

### 🔗 Useful Links
- [SQL Function Reference (W3Schools)](https://www.w3schools.com/sql/sql_ref_sqlserver.asp)
- [Mode Analytics SQL Tutorial](https://mode.com/sql-tutorial/) - Interactive exercises
- [LeetCode SQL Problems](https://leetcode.com/problemset/database/) - Practice challenges

---

## 💡 Final Pro Tip

> **"The best data engineers write SQL that is:**
> - ✅ **Defensive** (handles NULLs and edge cases)
> - ✅ **Readable** (uses CTEs and comments)
> - ✅ **Efficient** (minimizes table scans)
> - ✅ **Maintainable** (future-you will thank you)"

**Master these function patterns:**
1. **Cleaning:** `LOWER(TRIM(column))`
2. **NULL handling:** `COALESCE(column, default)`
3. **Extraction:** `SUBSTRING(column, start, LENGTH(column) - offset)`
4. **Aggregation:** `ROUND(SUM(COALESCE(column, 0)), 2)`

Practice these daily, and you'll be writing production-grade SQL that impresses senior engineers! 🚀

---

*📝 Document Version: 2.0*  
*🎯 Target Audience: Junior Data Engineers (0-2 years experience)*  
*🏢 Context: Mid-Product Companies (100-1000 employees)*  
*⏱️ Last Updated: January 2026*
