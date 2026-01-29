# 📅 SQL Date & Time Functions Mastery Guide

## 🎯 Learning Objectives
Master SQL date and time functions for data analysis, reporting, and data quality management in production environments.

> 💡 **Why This Matters:** Date/time manipulation is crucial for time-series analysis, reporting, and data pipeline operations in data engineering.

---

# 🕐 PART 1: DATE & TIME FUNDAMENTALS

## 📊 Core Data Types

### 🗓️ **Date Components**
| Component | Description | Example | SQL Type |
|-----------|-------------|---------|----------|
| **DATE** | Year, Month, Day | 2024-01-15 | `DATE` |
| **TIME** | Hours, Minutes, Seconds | 14:30:25 | `TIME` |
| **TIMESTAMP** | Date + Time combined | 2024-01-15 14:30:25 | `DATETIME` |

### 📥 **Data Sources**
```sql
-- 1. Database columns
SELECT order_date FROM orders;

-- 2. Hardcoded strings
SELECT '2024-01-15' as fixed_date;

-- 3. Current date/time
SELECT GETDATE() as current_timestamp;
```

---

## 🔧 Function Categories Overview

### 📋 **Function Classification**
| Category | Purpose | Functions | Use Case |
|----------|---------|-----------|----------|
| **Part Extraction** | Get specific components | `DAY()`, `MONTH()`, `YEAR()` | Filtering, grouping |
| **Format & Casting** | Change display/type | `FORMAT()`, `CONVERT()`, `CAST()` | Reporting, standardization |
| **Calculations** | Date arithmetic | `DATEADD()`, `DATEDIFF()` | Age calculation, intervals |
| **Validation** | Check validity | `ISDATE()` | Data quality checks |

---

# 🔍 PART 2: EXTRACTION FUNCTIONS

## 📊 Basic Extraction Functions

### 🎯 **Simple Component Extraction**
```sql
-- Basic component functions
SELECT 
    order_date,
    DAY(order_date) as day_number,        -- Returns: 15
    MONTH(order_date) as month_number,    -- Returns: 1  
    YEAR(order_date) as year_number       -- Returns: 2024
FROM orders;
```

**Output Type:** `INTEGER`

### 🔧 **Advanced DATEPART() Function**
```sql
-- More granular extraction
SELECT 
    order_date,
    DATEPART(YEAR, order_date) as year,
    DATEPART(QUARTER, order_date) as quarter,
    DATEPART(WEEK, order_date) as week_of_year,
    DATEPART(WEEKDAY, order_date) as day_of_week,
    DATEPART(HOUR, order_date) as hour
FROM orders;
```

**Available Parts:**
- `YEAR`, `QUARTER`, `MONTH`, `WEEK`
- `DAY`, `WEEKDAY`, `HOUR`, `MINUTE`, `SECOND`

### 📝 **Human-Readable DATENAME() Function**
```sql
-- String-based extraction
SELECT 
    order_date,
    DATENAME(MONTH, order_date) as month_name,     -- Returns: "January"
    DATENAME(WEEKDAY, order_date) as day_name,     -- Returns: "Monday"
    DATENAME(QUARTER, order_date) as quarter_name  -- Returns: "1"
FROM orders;
```

**Output Type:** `VARCHAR` (String)

---

## 🔄 Date Truncation & Boundaries

### ✂️ **DATETRUNC() Function**
**Purpose:** Reset less significant parts to minimum values for aggregation.

```sql
-- Truncate to different levels
SELECT 
    order_date,
    DATETRUNC(YEAR, order_date) as year_start,     -- 2024-01-01 00:00:00
    DATETRUNC(MONTH, order_date) as month_start,   -- 2024-01-01 00:00:00
    DATETRUNC(DAY, order_date) as day_start,       -- 2024-01-15 00:00:00
    DATETRUNC(HOUR, order_date) as hour_start      -- 2024-01-15 14:00:00
FROM orders;
```

**Use Cases:**
- **Data Aggregation:** Group by month/quarter/year
- **Time Series Analysis:** Standardize time periods
- **Reporting:** Create consistent time buckets

### 📅 **EOMONTH() Function**
```sql
-- End of month calculations
SELECT 
    order_date,
    EOMONTH(order_date) as month_end,              -- Last day of month
    EOMONTH(order_date, 1) as next_month_end,      -- Last day of next month
    EOMONTH(order_date, -1) as prev_month_end      -- Last day of previous month
FROM orders;

-- First day of month trick
SELECT 
    order_date,
    CAST(DATETRUNC(MONTH, order_date) AS DATE) as month_start
FROM orders;
```

---

# 🎨 PART 3: FORMATTING & CONVERSION

## 🖼️ FORMAT() Function

### 🎯 **Custom Date Formatting**
```sql
-- Various format examples
SELECT 
    order_date,
    FORMAT(order_date, 'dddd, MMMM dd, yyyy') as full_format,     -- Monday, January 15, 2024
    FORMAT(order_date, 'MM/dd/yy') as usa_format,                 -- 01/15/24
    FORMAT(order_date, 'dd/MM/yyyy') as european_format,          -- 15/01/2024
    FORMAT(order_date, 'yyyy-MM') as year_month                   -- 2024-01
FROM orders;
```

### 📊 **Format Specifiers Reference**
| Specifier | Description | Example |
|-----------|-------------|---------|
| `yyyy` | 4-digit year | 2024 |
| `MM` | 2-digit month | 01 |
| `dd` | 2-digit day | 15 |
| `MMMM` | Full month name | January |
| `dddd` | Full day name | Monday |
| `HH` | 24-hour format | 14 |
| `mm` | Minutes | 30 |

### 🌍 **Culture-Specific Formatting**
```sql
-- Different cultures
SELECT 
    order_date,
    FORMAT(order_date, 'd', 'en-US') as usa_short,        -- 1/15/2024
    FORMAT(order_date, 'd', 'de-DE') as german_short,     -- 15.01.2024
    FORMAT(order_date, 'd', 'ja-JP') as japanese_short    -- 2024/01/15
FROM orders;
```

---

## 🔄 CONVERT() Function

### 🎯 **Type Conversion with Styling**
```sql
-- Data type conversion
SELECT 
    '123' as string_value,
    CONVERT(INT, '123') as integer_value,
    
    '2024-01-15' as date_string,
    CONVERT(DATE, '2024-01-15') as date_value,
    
    GETDATE() as current_datetime,
    CONVERT(DATE, GETDATE()) as current_date_only
FROM orders;
```

### 📅 **Date Style Codes**
```sql
-- Popular style formats
SELECT 
    order_date,
    CONVERT(VARCHAR, order_date, 101) as usa_format,       -- mm/dd/yyyy
    CONVERT(VARCHAR, order_date, 103) as british_format,   -- dd/mm/yyyy
    CONVERT(VARCHAR, order_date, 104) as german_format,    -- dd.mm.yyyy
    CONVERT(VARCHAR, order_date, 120) as iso_format        -- yyyy-mm-dd hh:mi:ss
FROM orders;
```

### 📊 **Common Style Codes**
| Code | Format | Example |
|------|--------|---------|
| 101 | mm/dd/yyyy | 01/15/2024 |
| 103 | dd/mm/yyyy | 15/01/2024 |
| 104 | dd.mm.yyyy | 15.01.2024 |
| 110 | mm-dd-yyyy | 01-15-2024 |
| 120 | yyyy-mm-dd hh:mi:ss | 2024-01-15 14:30:25 |

---

## ⚡ CAST() Function

### 🎯 **Simple Type Conversion**
```sql
-- Basic casting operations
SELECT 
    '123' as original_string,
    CAST('123' AS INT) as as_integer,
    
    123 as original_number,
    CAST(123 AS VARCHAR(10)) as as_string,
    
    '2024-01-15' as date_string,
    CAST('2024-01-15' AS DATE) as as_date,
    
    GETDATE() as full_datetime,
    CAST(GETDATE() AS DATE) as date_only
FROM orders;
```

### 📊 **Function Comparison**
| Function | Type Conversion | Formatting | Output Types |
|----------|----------------|------------|--------------|
| **CAST()** | ✅ Any to Any | ❌ No | Any data type |
| **CONVERT()** | ✅ Any to Any | ✅ Date/Time only | Any data type |
| **FORMAT()** | ✅ Any to String | ✅ Date/Time & Numbers | VARCHAR only |

---

# 🧮 PART 4: CALCULATION FUNCTIONS

## ➕ DATEADD() Function

### 🎯 **Adding/Subtracting Time Intervals**
```sql
-- Date arithmetic examples
SELECT 
    order_date,
    DATEADD(YEAR, 2, order_date) as two_years_later,
    DATEADD(MONTH, 3, order_date) as three_months_later,
    DATEADD(DAY, -10, order_date) as ten_days_earlier,
    DATEADD(HOUR, 24, order_date) as twenty_four_hours_later
FROM orders;
```

### 📊 **Time Unit Options**
| Unit | Abbreviation | Example Usage |
|------|--------------|---------------|
| YEAR | yy, yyyy | `DATEADD(YEAR, 1, date)` |
| QUARTER | qq, q | `DATEADD(QUARTER, 2, date)` |
| MONTH | mm, m | `DATEADD(MONTH, 6, date)` |
| WEEK | wk, ww | `DATEADD(WEEK, 2, date)` |
| DAY | dd, d | `DATEADD(DAY, 30, date)` |
| HOUR | hh | `DATEADD(HOUR, 12, date)` |

### 🎯 **Practical Applications**
```sql
-- Business scenarios
SELECT 
    customer_id,
    subscription_date,
    DATEADD(YEAR, 1, subscription_date) as renewal_date,
    DATEADD(DAY, 30, subscription_date) as trial_end_date,
    DATEADD(MONTH, -1, GETDATE()) as last_month_cutoff
FROM subscriptions;
```

---

## 📏 DATEDIFF() Function

### 🎯 **Calculating Date Differences**
```sql
-- Age and duration calculations
SELECT 
    employee_id,
    birth_date,
    hire_date,
    DATEDIFF(YEAR, birth_date, GETDATE()) as age_years,
    DATEDIFF(MONTH, hire_date, GETDATE()) as tenure_months,
    DATEDIFF(DAY, hire_date, GETDATE()) as tenure_days
FROM employees;
```

### 📊 **Advanced Applications**
```sql
-- Shipping duration analysis
SELECT 
    MONTH(order_date) as order_month,
    AVG(DATEDIFF(DAY, order_date, ship_date)) as avg_shipping_days,
    MAX(DATEDIFF(DAY, order_date, ship_date)) as max_shipping_days,
    MIN(DATEDIFF(DAY, order_date, ship_date)) as min_shipping_days
FROM orders
WHERE ship_date IS NOT NULL
GROUP BY MONTH(order_date)
ORDER BY order_month;
```

### 🔄 **Time Between Orders**
```sql
-- Days between consecutive orders
SELECT 
    customer_id,
    order_date,
    LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) as prev_order_date,
    DATEDIFF(DAY, 
        LAG(order_date) OVER (PARTITION BY customer_id ORDER BY order_date), 
        order_date
    ) as days_since_last_order
FROM orders
ORDER BY customer_id, order_date;
```

---

# ✅ PART 5: VALIDATION & DATA QUALITY

## 🔍 ISDATE() Function

### 🎯 **Date Validation**
```sql
-- Testing various inputs
SELECT 
    '2024-01-15' as test_value,
    ISDATE('2024-01-15') as is_valid,          -- Returns: 1 (True)
    
    '2024-13-01' as invalid_month,
    ISDATE('2024-13-01') as is_valid,          -- Returns: 0 (False)
    
    'not a date' as text_value,
    ISDATE('not a date') as is_valid,          -- Returns: 0 (False)
    
    '20240115' as numeric_format,
    ISDATE('20240115') as is_valid             -- Returns: 1 (True)
```

### 🧹 **Data Cleaning Applications**
```sql
-- Clean and convert date strings
SELECT 
    raw_date_string,
    CASE 
        WHEN ISDATE(raw_date_string) = 1 
        THEN CAST(raw_date_string AS DATE)
        ELSE NULL 
    END as cleaned_date
FROM raw_data_table;

-- Alternative with default date
SELECT 
    raw_date_string,
    CASE 
        WHEN ISDATE(raw_date_string) = 1 
        THEN CAST(raw_date_string AS DATE)
        ELSE '1900-01-01'  -- Default date for invalid entries
    END as cleaned_date
FROM raw_data_table;
```

### 📊 **Data Quality Assessment**
```sql
-- Assess data quality in batch
SELECT 
    COUNT(*) as total_records,
    SUM(CASE WHEN ISDATE(date_column) = 1 THEN 1 ELSE 0 END) as valid_dates,
    SUM(CASE WHEN ISDATE(date_column) = 0 THEN 1 ELSE 0 END) as invalid_dates,
    ROUND(
        100.0 * SUM(CASE WHEN ISDATE(date_column) = 1 THEN 1 ELSE 0 END) / COUNT(*), 
        2
    ) as data_quality_percentage
FROM raw_data_table;
```

---

# 🎯 REAL-WORLD APPLICATIONS

## 📊 Data Aggregation & Reporting

### 📈 **Time-Based Grouping**
```sql
-- Sales by month and quarter
SELECT 
    YEAR(order_date) as sales_year,
    DATENAME(MONTH, order_date) as month_name,
    DATEPART(QUARTER, order_date) as quarter,
    COUNT(*) as order_count,
    SUM(total_amount) as total_sales,
    FORMAT(SUM(total_amount), 'C', 'en-US') as formatted_sales
FROM orders
GROUP BY 
    YEAR(order_date),
    MONTH(order_date),
    DATENAME(MONTH, order_date),
    DATEPART(QUARTER, order_date)
ORDER BY sales_year, MONTH(order_date);
```

### 📅 **Cohort Analysis**
```sql
-- Customer cohort analysis by registration month
SELECT 
    FORMAT(registration_date, 'yyyy-MM') as cohort_month,
    COUNT(DISTINCT customer_id) as customers_registered,
    COUNT(DISTINCT CASE 
        WHEN DATEDIFF(MONTH, registration_date, first_order_date) = 0 
        THEN customer_id 
    END) as customers_ordered_same_month,
    COUNT(DISTINCT CASE 
        WHEN DATEDIFF(MONTH, registration_date, first_order_date) <= 3 
        THEN customer_id 
    END) as customers_ordered_within_3_months
FROM customer_summary
GROUP BY FORMAT(registration_date, 'yyyy-MM')
ORDER BY cohort_month;
```

---

## 🔍 Advanced Filtering Techniques

### 📊 **Performance-Optimized Filtering**
```sql
-- Efficient date range filtering
SELECT *
FROM orders
WHERE YEAR(order_date) = 2024          -- Less efficient
  AND MONTH(order_date) = 1;           -- Function on column

-- Better approach
SELECT *
FROM orders
WHERE order_date >= '2024-01-01'       -- More efficient
  AND order_date < '2024-02-01';       -- Direct comparison
```

### 🎯 **Complex Date Conditions**
```sql
-- Business day filtering (Monday-Friday)
SELECT *
FROM orders
WHERE DATEPART(WEEKDAY, order_date) BETWEEN 2 AND 6;  -- Mon=2, Fri=6

-- Last 30 days excluding weekends
SELECT *
FROM orders
WHERE order_date >= DATEADD(DAY, -30, GETDATE())
  AND DATEPART(WEEKDAY, order_date) BETWEEN 2 AND 6;

-- Quarterly reporting periods
SELECT *
FROM orders
WHERE DATEPART(QUARTER, order_date) = DATEPART(QUARTER, GETDATE())
  AND YEAR(order_date) = YEAR(GETDATE());
```

---

# 📊 FUNCTION DECISION MATRIX

## 🎯 When to Use Which Function

### 📋 **Quick Reference Guide**
| Need | Function | Output Type | Example |
|------|----------|-------------|---------|
| **Integer component** | `DAY()`, `MONTH()`, `YEAR()` | INT | `MONTH(date) = 1` |
| **Flexible extraction** | `DATEPART()` | INT | `DATEPART(QUARTER, date)` |
| **Human-readable name** | `DATENAME()` | VARCHAR | `DATENAME(MONTH, date)` |
| **Time period grouping** | `DATETRUNC()` | DATETIME | `DATETRUNC(MONTH, date)` |
| **Month boundaries** | `EOMONTH()` | DATE | `EOMONTH(date)` |
| **Custom formatting** | `FORMAT()` | VARCHAR | `FORMAT(date, 'MM/dd/yyyy')` |
| **Type conversion + style** | `CONVERT()` | Various | `CONVERT(VARCHAR, date, 101)` |
| **Simple type conversion** | `CAST()` | Various | `CAST(date AS VARCHAR)` |
| **Date arithmetic** | `DATEADD()` | DATETIME | `DATEADD(MONTH, 1, date)` |
| **Date differences** | `DATEDIFF()` | INT | `DATEDIFF(DAY, start, end)` |
| **Validation** | `ISDATE()` | INT (0/1) | `ISDATE(string_value)` |

### 🎯 **Performance Considerations**
| Scenario | Recommendation | Reason |
|----------|---------------|--------|
| **WHERE clause filtering** | Use direct comparison | Avoids function evaluation on every row |
| **GROUP BY aggregation** | Use `DATETRUNC()` | Efficient time period grouping |
| **Reporting display** | Use `FORMAT()` | Flexible, readable output |
| **Data type conversion** | Use `CAST()` for simple, `CONVERT()` for styled | Clear intent, appropriate complexity |

---

# 🎯 Key Takeaways

## ✅ **Essential Skills Mastered**
- **Component Extraction:** Get specific date/time parts for analysis
- **Formatting & Conversion:** Present data in required formats
- **Date Arithmetic:** Calculate intervals and differences
- **Data Validation:** Ensure data quality in ETL processes
- **Performance Optimization:** Choose efficient functions for large datasets

## 💼 **Production Applications**
- **Time Series Analysis:** Group and aggregate data by time periods
- **Reporting:** Format dates for business stakeholders
- **Data Quality:** Validate and clean date inputs
- **Business Logic:** Calculate ages, tenures, and intervals
- **ETL Processes:** Transform and standardize date formats

## 🚀 **Next Steps**
- Practice with real datasets
- Combine functions for complex scenarios
- Optimize queries for large-scale data processing
- Implement in data pipeline workflows

---

*Master time, master data! ⏰*