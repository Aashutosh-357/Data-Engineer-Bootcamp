<div align="center">

# 620. Not Boring Movies

**Difficulty:** Easy 🟢

</div>

---

## 📊 Database Schema

### Table: `Cinema`

| Column Name | Type    |
|:------------|:--------|
| id          | int     |
| movie       | varchar |
| description | varchar |
| rating      | float   |

<br>

**Key Information:**
- `id` is the **primary key** (column with unique values) for this table.
- Each row contains information about the name of a movie, its genre, and its rating.
- `rating` is a 2 decimal places float in the range `[0, 10]`

<br>

---

## 📝 Problem Statement

Write a solution to report the movies with an **odd-numbered ID** and a description that is **not** `"boring"`.

Return the result table ordered by `rating` in **descending order**.

The result format is in the following example.

<br>

---

## 💡 Example

### Input:

**Cinema table:**

| id | movie       | description | rating |
|:---|:------------|:------------|:-------|
| 1  | War         | great 3D    | 8.9    |
| 2  | Science     | fiction     | 8.5    |
| 3  | Irish       | boring      | 6.2    |
| 4  | Ice song    | Fantasy     | 8.6    |
| 5  | House card  | Interesting | 9.1    |

<br>

### Output:

| id | movie      | description | rating |
|:---|:-----------|:------------|:-------|
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |

<br>

### Explanation:

We have three movies with odd-numbered IDs: **1**, **3**, and **5**.
- The movie with ID = **3** is boring so we don't include it in the answer.

<br>

---

## 💻 Solution (PostgreSQL)

```sql
-- Write your PostgreSQL query statement below
SELECT * FROM Cinema
WHERE (id % 2 = 1) AND (description != 'boring')
ORDER BY rating DESC;
```

<br>

---

## 🔍 Solution Breakdown

| Step | Logic | Explanation |
|:-----|:------|:------------|
| **1** | `WHERE (id % 2 = 1)` | Filter rows where the ID is **odd** (remainder when divided by 2 equals 1) |
| **2** | `AND (description != 'boring')` | Exclude movies with the description **"boring"** |
| **3** | `ORDER BY rating DESC` | Sort the results by rating in **descending order** (highest first) |
| **4** | `SELECT *` | Return all columns from the filtered and sorted results |

<br>

---

## 🎯 Key Concepts

<br>

### ✅ Modulo Operator (`%`)
- Used to find the **remainder** of division
- `id % 2 = 1` identifies **odd numbers**
- `id % 2 = 0` would identify **even numbers**

<br>

### ✅ String Comparison
- `!=` operator checks for **inequality**
- SQL is **case-sensitive** for string comparisons (depending on collation)
- Alternative syntax: `description <> 'boring'`

<br>

### ✅ ORDER BY
- `DESC` = Descending (highest to lowest)
- `ASC` = Ascending (lowest to highest) - this is the default

<br>

---

## 📊 Performance Stats

<div align="center">

**✅ Accepted** | Runtime: **271 ms**

</div>

<br>

---

<div align="center">

**🎬 Happy Querying! 🎬**

</div>
