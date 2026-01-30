# 🚀 Week 02: Dictionaries, String Cleaning & The Art of JOIN

---

## 🎉 Congratulations!

**Excellent work!** You have successfully completed Week 1. The foundation for data manipulation in Python and basic SQL querying is now being laid. You're building momentum! 💪

---

## 📋 Your Tactical Battle Plan for Week 2

<table>
<tr>
<td><strong>🎨 Theme</strong></td>
<td>"Dictionaries, String Cleaning, and the Art of the JOIN"</td>
</tr>
<tr>
<td><strong>🎯 Objective</strong></td>
<td>By Friday, you should be able to work with structured data (like JSON), clean messy text, and retrieve information from multiple tables.</td>
</tr>
</table>

---

## 📅 Daily Breakdown

### 📘 **Day 6: Monday** — Dictionaries Deep Dive & JSON

> **🎯 Focus:** Nested structures and data interchange format

#### ⏰ **1 hour 45 minutes** | 📚 Learn

- **📖 Topic:** 
  - Nested Dictionaries (dictionaries within dictionaries)
  - Lists of Dictionaries
  - Accessing nested data
  - Introduction to JSON (`json` module in Python)

- **🔨 Drill:** 
  ```python
  # Create a list of dictionaries representing multiple users
  # Access the email of the second user
  ```

#### ⏰ **45 minutes** | 💪 Gym - LeetCode Easy

- **🧩 LeetCode Easy:** [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
  - *Uses a Set/Dictionary effectively*

#### ⏰ **1 hour 15 minutes** | 🧪 Lab

- **✅ Task:** Write `user_processor.py`

- **📥 Input:** A JSON string representing a list of user orders:
  ```json
  '[{"user_id": 101, "orders": [{"item": "laptop", "price": 1200}, {"item": "mouse", "price": 25}]}, {"user_id": 102, "orders": [{"item": "keyboard", "price": 75}]}]'
  ```

- **⚙️ Logic:** Parse the JSON string into Python objects. Loop through users and print their total spending.

---

### 📗 **Day 7: Tuesday** — Advanced String Cleaning

> **🎯 Focus:** Real-world messy data needs more than `.strip()`

#### ⏰ **1 hour 45 minutes** | 📚 Learn

- **📖 Topic:** 
  - String methods: `.replace()`, `.find()`, `.startswith()`, `.endswith()`
  - Introduction to Regular Expressions (`re` module - basic usage for patterns)

- **🔨 Drill:** 
  ```python
  # Extract a phone number from a string like:
  # "Contact us at: 123-456-7890 or call 9876543210"
  ```

#### ⏰ **45 minutes** | 💪 Gym - LeetCode Easy

- **🧩 LeetCode Easy:** [Reverse String](https://leetcode.com/problems/reverse-string/)
  - *Simple string manipulation*

#### ⏰ **1 hour 15 minutes** | 🧪 Lab

- **✅ Task:** Write `log_parser.py`

- **📥 Input:** A multi-line string representing server logs. Each line might look like:
  ```
  "INFO: User logged in at 2023-01-15 10:30:00"
  "WARN: Disk space low."
  ```

- **⚙️ Logic:** Loop through lines. Extract log level (INFO, WARN) and timestamp (if present). Print a summary.

---

### 📙 **Day 8: Wednesday** — SQL: The Essential JOIN

> **🎯 Focus:** Retrieving data spread across multiple tables. This is the bread and butter of DE! 🍞🧈

#### ⏰ **1 hour 45 minutes** | 📚 Learn

- **📖 Topic:** 
  - `INNER JOIN`
  - `LEFT JOIN`
  - Understanding the difference

- **💡 Concept:** 
  ```sql
  -- Relate two tables
  -- Example: Customers table and Orders table
  -- Show me all customers AND their orders
  ```

#### ⏰ **45 minutes** | 💪 Gym - LeetCode SQL 50

Solve exactly these two from the **"Select & Join"** category:

1. **🔗 Combine Two Tables** (problem 1757)
   - *Teaches basic INNER JOIN*

2. **🔗 Customer Who Bought All Products** (problem 1767)
   - *This is a Medium, might be challenging, but teaches complex join logic*
   - *If too hard, substitute with problem 1756: "Lowest Price in Each Category"*

#### ⏰ **1 hour 15 minutes** | 🧪 Lab

- **✅ Task:** Setup your local PostgreSQL again (or use a free tier on ElephantSQL/Supabase)

- **📊 Database Schema:**
  - Create two tables:
    - `Employees` (id, name, department_id)
    - `Departments` (id, name)
  - Insert sample data
  - Write a query to list each employee's name and their department's name

---

### 📕 **Day 9: Thursday** — SQL: More Joins & Aggregation

> **🎯 Focus:** Combining information and summarizing it

#### ⏰ **1 hour 45 minutes** | 📚 Learn

- **📖 Topic:** 
  - `RIGHT JOIN`
  - `FULL OUTER JOIN`
  - Combining `JOIN` with `GROUP BY` and aggregate functions (`COUNT`, `SUM`, `AVG`)

- **💡 Concept:** 
  ```sql
  -- Find departments with no employees
  ```

#### ⏰ **45 minutes** | 💪 Gym - LeetCode SQL 50

Solve exactly these two:

1. **📊 Employees With High Salary** (problem 1783)
   - *Teaches simple join and aggregation*

2. **📊 Product Sales Analysis I** (problem 1084)
   - *Teaches joining with aggregation and filtering*

#### ⏰ **1 hour 15 minutes** | 🧪 Lab

- **✅ Task:** Extend yesterday's `Employees` and `Departments` schema

- **📊 New Schema:**
  - Add a `Salaries` table (employee_id, amount)
  - Write a query to find the average salary per department

---

### 📓 **Day 10: Friday** — Putting it Together: The First ETL

> **🎯 Focus:** Combining Python and SQL 🐍 + 🗄️

#### ⏰ **1 hour 45 minutes** | 📚 Learn

- **📖 Topic:** 
  - Python's `sqlite3` module (for simple databases)
  - `psycopg2` (for PostgreSQL)
  - Basic SQL execution from Python

- **💡 Concept:** 
  ```python
  # How to connect to a DB, execute a query, and fetch results
  ```

#### ⏰ **45 minutes** | 💪 Gym - LeetCode SQL 50

Solve exactly these two:

1. **🔍 The Difference: Between two tables** (problem 1764)
   - *Teaches set operations (LEFT JOIN + WHERE IS NULL)*

2. **📅 Find Dates With An Event That Had Multiple Consecutive Days** (problem 1158)
   - *This is a Medium. If too hard, substitute with problem 1149: "Lowest Product Price" - teaches `MAX` with `GROUP BY`*

#### ⏰ **1 hour 15 minutes** | 🧪 Lab

- **✅ Task:** Write `simple_etl.py`

- **📥 Input:** A JSON file (create a sample one with user data)

- **⚙️ Logic:**
  1. Read JSON file into a list of dictionaries (using Python)
  2. Connect to your local PostgreSQL DB
  3. Execute an `INSERT` statement for each user from the JSON
  4. Execute a `SELECT` query to verify the data was inserted

---

## 🏆 The Weekend Mission (Jan 10-11)

> **🎯 Combined Goal:** Robust data processing with Python and basic database interaction

### 🎯 **Project: "Customer Data Aggregator"**

#### 📌 **Goal**
Simulate a simplified ETL process that pulls data from a "source" (a JSON file) and loads it into a "data warehouse" (your PostgreSQL database).

#### ⚙️ **Logic**

1. **📄 Create** a `customers.json` file with sample customer data:
   ```json
   {
     "id": 1,
     "name": "John Doe",
     "signup_date": "2024-01-15",
     "city": "NY"
   }
   ```

2. **🐍 Write** a Python script (`etl_customer.py`):
   - ✅ Reads `customers.json`
   - ✅ Cleans any messy fields (e.g., standardize city names: "NY" → "New York")
   - ✅ Connects to PostgreSQL using `psycopg2`
   - ✅ Inserts the cleaned data into a `customers` table (create the table first)
   - ✅ Executes a `SELECT COUNT(*)` query to show how many records were inserted

#### 📦 **Deliverable**
The Python script and the SQL `CREATE TABLE` statement, both pushed to your `data-engineering-bootcamp` GitHub repo.

---

## ✅ Checklist for Week 2 Success

Track your progress:

- [ ] 📊 Did you successfully parse and process a JSON string in Python?
- [ ] 🔗 Did you write a SQL query that successfully JOINs two tables?
- [ ] 🚀 Did your `simple_etl.py` script run without errors and insert data into your DB?
- [ ] 📤 Is your "Customer Data Aggregator" project (Python script + SQL DDL) pushed to GitHub?

---

<div align="center">

### 💪 Execute the plan, and embrace the learning curves.

**They are steep but rewarding.** 🏔️✨

</div>