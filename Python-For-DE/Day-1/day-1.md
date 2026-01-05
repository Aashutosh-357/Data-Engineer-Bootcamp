# 📚 Day 1: Python Fundamentals for Data Engineering

## 🎯 Learning Objectives
**Topics Covered:** Python Lists (`[]`), Indexing, `for` loops, `while` loops, `range()`  
**Practical Drill:** Master list manipulation and iteration techniques

---

## 🔍 Core Concepts Explained

### 🗂️ **Python Lists (`[]`)**
Think of this as a **digital container** or "shopping list" that:
- Keeps items in a specific order
- Can store anything: numbers, strings, or even other lists
- Is mutable (you can change it after creation)

### 📍 **Indexing**
The **address system** for list items:
> 💡 **Pro Tip:** Python uses **zero-based indexing**
> - First item: `list[0]`
> - Second item: `list[1]`
> - Last item: `list[-1]`

### 🔄 **`for` Loops - The "Automatic Runner"**
Python's way of saying: *"For every item in this collection, do something with it"*
- Automatically stops at the end
- Most Pythonic way to iterate

### ⏰ **`while` Loops - The "As Long As" Rule**
Keeps executing code while a condition remains true:
- More control over iteration
- ⚠️ **Caution:** Must include a way to stop or it runs forever!

### 🔢 **`range()` - The Number Generator**
Creates sequences of numbers (0, 1, 2, 3...):
- Perfect for controlling loop iterations
- Memory efficient (generates numbers on-demand)

---

## 🛠️ Hands-On Drill: List Building & Iteration

### 📋 **Scenario**
Building a "Completed Interview Modules" tracker:
1. Start with an empty list
2. Add modules dynamically
3. Display using multiple iteration methods

### ⚖️ **Technical Approach & Trade-offs**

| Method | Description | Performance | Use Case |
|--------|-------------|-------------|----------|
| `.append()` | Adds item to end | O(1) | Standard addition |
| `for` loop | Pythonic iteration | O(N) | Clean, readable code |
| `while` loop | Manual control | O(N) | When you need index control |
| `range()` loop | Index-based | O(N) | Need both index and value |

**Complexity Analysis:**
- ⏱️ **Time:** O(N) for displaying all items
- 💾 **Space:** O(N) for storing N items

---

## 💻 Implementation Examples

```python
# Initialize an empty list
completed_modules = []

# Using .append() to add items dynamically
completed_modules.append("SQL Fundamentals")
completed_modules.append("Advanced Joins")
completed_modules.append("Window Functions")

print("--- Method 1: The 'for' loop (Pythonic) ---")
# This is the cleanest way to iterate in Python
for module in completed_modules:
    print(f"Module Name: {module}")

print("\n--- Method 2: The 'while' loop with indexing ---")
# We manually track the 'index' to show how indexing works
index = 0
while index < len(completed_modules):
    current_item = completed_modules[index]
    print(f"Index {index}: {current_item}")
    index += 1  # Crucial: Increment to avoid an infinite loop!

print("\n--- Method 3: for loop with range() ---")
# Good if you need both the index AND the value
for i in range(len(completed_modules)):
    print(f"Processing Item #{i+1}: {completed_modules[i]}")
```

---

## 🔍 Code Walkthrough: While Loop Logic

**Initial State:** `completed_modules = ["SQL Fundamentals", "Advanced Joins", "Window Functions"]`

| Step | Index | Condition Check | Action | Next Index |
|------|-------|----------------|--------|-----------|
| 1 | 0 | `0 < 3` ✅ | Print "SQL Fundamentals" | 1 |
| 2 | 1 | `1 < 3` ✅ | Print "Advanced Joins" | 2 |
| 3 | 2 | `2 < 3` ✅ | Print "Window Functions" | 3 |
| 4 | 3 | `3 < 3` ❌ | **Loop terminates** | - |

---

## 🎓 Key Takeaways

> 💡 **Best Practice:** Use `for item in list` syntax in Python—it's cleaner and prevents off-by-one errors common in `while` loops.

> 🔧 **When to use indexing:** Essential when you need to:
> - Modify the list while iterating
> - Compare adjacent elements
> - Track position information

---

## 🚀 Next Challenge
** Task: Write a script grades.py. **
Input: A list of integers: [50, 80, 90, 45].
Logic: Loop through them. If score < 50, print "Fail". Else "Pass". Calculate the Average.

---

*Happy coding! 🐍*