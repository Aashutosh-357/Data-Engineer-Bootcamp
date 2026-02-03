# 🎯 LeetCode SQL Solutions - Day 09

> **Focus Area:** SQL Joins, Aggregations & Performance Optimization  
> **Difficulty Level:** Medium  
> **Target Role:** Data Engineer I (L4)

---

## 📚 Table of Contents

1. [Problem 1: Product Sales Analysis](#problem-1-product-sales-analysis)
2. [Problem 2: Products Sold in Q1 2019](#problem-2-products-sold-in-q1-2019)

---

## 🔷 Problem 1: Product Sales Analysis

### 📋 Problem Statement

This is a straightforward join problem that tests your ability to handle **Relational Mapping** and **Data Denormalization**. In a production environment at Amazon, we often store descriptive metadata (like Product Names) in dimension tables and transactional data (like Prices and Years) in fact tables to save storage space.

### 💡 The Approach

To solve this, I will perform a simple `INNER JOIN` between the `Sales` table and the `Product` table. Since we only need the product names for sales that actually occurred, an `INNER JOIN` is the most **"Frugal"** and efficient choice.

### ✅ The SQL Solution

**Platform Compatibility:** PostgreSQL / Redshift

```sql
/* 
   📌 Requirement: Fetch product_name, year, and price for every sale.
   ⚡ Optimization: Standard Join on product_id.
*/

SELECT 
    p.product_name, 
    s.year, 
    s.price
FROM 
    Sales s
JOIN 
    Product p ON s.product_id = p.product_id;
```

### 🔍 Technical Deep Dive for Data Engineer I (L4)

#### 1️⃣ **Joins vs. Subqueries**
I chose a `JOIN` over a subquery here because modern SQL optimizers (especially in Redshift/Postgres) can execute joins much more efficiently by using **"Hash Joins."** This is critical when the `Sales` table contains millions of transaction records.

#### 2️⃣ **Indexing & Performance**
In a real-world scenario, I would ensure that `product_id` is an **Index** or a **Sort Key**. For a large-scale data warehouse:

- ✅ I would set `product_id` as the **Distribution Key** on both tables.
- ✅ This ensures that sales records for a specific product are stored on the same compute node as the product's name, eliminating **"Data Shuffle"** across the network during the join.

#### 3️⃣ **Readability**
I've used clear aliases (`s` for Sales, `p` for Product). This reflects **Ownership**—writing code that is easy for the next engineer to maintain or debug.

### ❓ Why not use `LEFT JOIN`?

I used an `INNER JOIN` because the schema implies that a `sale` must have a corresponding `product_id`. If we used a `LEFT JOIN`, we might end up with `NULL` product names if the data integrity was compromised. However, given the constraints of this problem, an `INNER JOIN` is the most direct way to get the required result.

> 💭 **Question:** Does this meet the performance constraints for the dataset size you're expecting?

---

## 🔷 Problem 2: Products Sold in Q1 2019

### 📋 Problem Breakdown

We need to find products that were sold **only** in the first quarter of 2019 (2019-01-01 to 2019-03-31). If a product was sold on 2019-02-01 (inside the range) but also on 2019-05-01 (outside the range), it must be **excluded**.

### 💡 My Approach

To solve this, I will use **Aggregation with a HAVING clause**. This is more efficient than using subqueries or `NOT IN` because it allows us to scan the `Sales` table once, group by the product, and verify the entire date range for that product in a single pass.

### ✅ The SQL Solution

**Platform Compatibility:** PostgreSQL / Redshift

```sql
/* 
   📌 Approach: 
   1. JOIN Product and Sales to get product names.
   2. GROUP BY product_id.
   3. Use HAVING to ensure the MIN date is >= Jan 1st AND the MAX date is <= March 31st.
   
   ⚡ This ensures NO sales exist outside the specified window.
*/

SELECT 
    p.product_id, 
    p.product_name
FROM 
    Product p
JOIN 
    Sales s ON p.product_id = s.product_id
GROUP BY 
    p.product_id, 
    p.product_name
HAVING 
    MIN(s.sale_date) >= '2019-01-01' 
    AND MAX(s.sale_date) <= '2019-03-31';
```

### 🏆 Why this approach is "Strong Hire" (L4) quality

#### 1️⃣ **Efficiency (One-Pass Filtering)**
By using `MIN` and `MAX` in the `HAVING` clause, we avoid expensive `EXCEPT` clauses or correlated subqueries. We simply define the **"boundary"** for each product. If a product has even one sale on `2019-04-01`, its `MAX(sale_date)` will exceed the limit, and the entire group will be dropped.

#### 2️⃣ **Handling "Ownership" of Data**
I included `p.product_name` in both the `SELECT` and `GROUP BY`. While some SQL dialects allow you to omit non-aggregated columns from `GROUP BY` if the PK is present, standard ANSI SQL (and Redshift) requires it for strictness and clarity.

#### 3️⃣ **Data Integrity**
This logic correctly handles the **"Only"** constraint. If a product was never sold at all, it won't appear (which is correct). If it was sold in Q1 and Q2, it's excluded.

### 🚀 Technical Deep Dive (Scale)

If the `Sales` table contained **billions of rows** (typical for Amazon's global orders), I would suggest:

| **Optimization Strategy** | **Implementation** |
|---------------------------|-------------------|
| 📊 **Partitioning** | Ensure the `Sales` table is partitioned by `sale_date` |
| 🔑 **Distribution Keys** | Set `product_id` as the **Dist Key** for both tables to colocate data on the same nodes |
| ⚡ **Performance Gain** | Makes `JOIN` and `GROUP BY` significantly faster by avoiding **"Network Broadcast"** of data |

> 💭 **Question:** Does this grouping logic align with the reporting requirements you're looking for, or should we consider an approach that includes products with zero sales?

---

## 📊 Key Takeaways

| **Concept** | **Best Practice** |
|-------------|-------------------|
| 🔗 **Joins** | Prefer `INNER JOIN` for required relationships; use `LEFT JOIN` only when nulls are acceptable |
| 📈 **Aggregations** | Use `HAVING` with `MIN/MAX` for efficient boundary checks |
| 🎯 **Performance** | Always consider distribution keys and partitioning for large-scale data |
| 📝 **Code Quality** | Use clear aliases and comments for maintainability |

---

**📅 Last Updated:** Day 09 - Week 02  
**🎓 Learning Path:** Data Engineer Bootcamp - Month 01