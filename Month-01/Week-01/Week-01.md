# 📅 Week 1: Python Fundamentals & SQL Introduction

## 🎯 Weekly Objectives
Master Python data structures, control flow, and basic SQL querying - the foundation of data engineering.

> 💡 **Success Metric:** Code confidently with lists, dictionaries, and basic SQL without constant documentation lookup.

---

## 📋 Daily Schedule

### 🗓️ **Day 1: Monday - Lists & Loops**
**Focus:** Storing multiple items and iterating through them efficiently.

#### ⏰ **Time Allocation**
| Session | Duration | Activity |
|---------|----------|----------|
| **Morning Learn** | 105 minutes | Core concepts & practice |
| **Gym Break** | 45 minutes | LeetCode problem |
| **Evening Lab** | 75 minutes | Hands-on project |

#### 📚 **Learning Session (105 minutes)**
**Core Topics:**
- **Python Lists (`[]`):** Creation, indexing, slicing
- **Iteration Patterns:** `for` loops, `while` loops, `range()`
- **List Operations:** `.append()`, `.extend()`, `.remove()`
- **Practical Drill:** Build and manipulate dynamic lists

```python
# Key concepts to master
numbers = [1, 2, 3, 4, 5]
for i, num in enumerate(numbers):
    print(f"Index {i}: {num}")
```

#### 🏋️ **Gym Session (45 minutes)**
**LeetCode Challenge:** Choose one
- "Build Array from Permutation" (Easy)
- "Concatenation of Array" (Easy)

**Focus:** Apply list manipulation concepts in problem-solving context.

#### 🔬 **Lab Session (75 minutes)**
**Project:** `grades.py` - Grade Processing System

```python
# Requirements:
# Input: [50, 80, 90, 45]
# Logic: Loop through scores
# Output: "Pass"/"Fail" + calculate average
```

**Skills Practiced:**
- List iteration
- Conditional logic
- Mathematical operations
- Output formatting

---

### 🗓️ **Day 2: Tuesday - Dictionaries (Crucial)**
**Focus:** Key-value pairs - foundation of JSON and NoSQL databases.

#### ⏰ **Time Allocation**
| Session | Duration | Activity |
|---------|----------|----------|
| **Morning Learn** | 105 minutes | Dictionary mastery |
| **Gym Break** | 45 minutes | HashMap problem |
| **Evening Lab** | 75 minutes | Inventory system |

#### 📚 **Learning Session (105 minutes)**
**Core Topics:**
- **Dictionary Syntax (`{}`):** Creation and access patterns
- **Safe Access:** `.get()` method vs direct access
- **Modification:** Adding, updating, deleting values
- **Iteration:** `.items()`, `.keys()`, `.values()`
- **Practical Drill:** User profile management

```python
# Master these patterns
user = {"name": "Alex", "id": 1024, "email": "alex@company.com"}
for key, value in user.items():
    print(f"{key}: {value}")
```

#### 🏋️ **Gym Session (45 minutes)**
**LeetCode Challenge:** "Two Sum" (Easy)
**Focus:** Understanding HashMap/Dictionary usage in algorithms.

#### 🔬 **Lab Session (75 minutes)**
**Project:** `inventory.py` - Dynamic Inventory Manager

```python
# Requirements:
# Input: {'apple': 5, 'banana': 2}
# User input: "Bought 3 apples"
# Output: Updated inventory {'apple': 8, 'banana': 2}
```

**Skills Practiced:**
- Dictionary manipulation
- String parsing
- User input handling
- Dynamic updates

---

### 🗓️ **Day 3: Wednesday - Strings & Cleaning**
**Focus:** Data cleaning fundamentals - 90% of data engineering work.

#### ⏰ **Time Allocation**
| Session | Duration | Activity |
|---------|----------|----------|
| **Morning Learn** | 105 minutes | String manipulation |
| **Gym Break** | 45 minutes | String algorithms |
| **Evening Lab** | 75 minutes | Data cleaning project |

#### 📚 **Learning Session (105 minutes)**
**Core Topics:**
- **String Slicing:** `text[0:4]`, negative indexing
- **Essential Methods:** `.strip()`, `.split()`, `.replace()`, `.lower()`
- **Pattern Matching:** Basic regex concepts
- **Practical Drill:** Clean messy data `" JOhn Doe "` → `"John Doe"`

```python
# Master these cleaning patterns
messy_text = " JOhn Doe "
clean_text = messy_text.strip().title()
```

#### 🏋️ **Gym Session (45 minutes)**
**LeetCode Challenge:** "Defanging an IP Address" (Easy)
**Focus:** String manipulation and replacement techniques.

#### 🔬 **Lab Session (75 minutes)**
**Project:** `cleaner.py` - Email Data Normalizer

```python
# Requirements:
# Input: [" ALEX@GMAIL.COM", "bob@yahoo.co.in "]
# Logic: Normalize → lowercase, trim, extract domain
# Output: Clean emails + domain extraction
```

**Skills Practiced:**
- Batch string processing
- Data normalization
- Pattern extraction
- List comprehensions

---

### 🗓️ **Day 4: Thursday - Logic & Error Handling**
**Focus:** Building robust, production-ready code.

#### ⏰ **Time Allocation**
| Session | Duration | Activity |
|---------|----------|----------|
| **Morning Learn** | 105 minutes | Control flow & exceptions |
| **Gym Break** | 45 minutes | Logic problems |
| **Evening Lab** | 75 minutes | Error-proof existing code |

#### 📚 **Learning Session (105 minutes)**
**Core Topics:**
- **Complex Logic:** `if/elif/else` chains
- **Boolean Operations:** `and`, `or`, `not`
- **Exception Handling:** `try/except/finally` blocks
- **Practical Drill:** Handle division by zero gracefully

```python
# Master error handling patterns
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
    result = 0
```

#### 🏋️ **Gym Session (45 minutes)**
**LeetCode Challenge:** "Palindrome Number" (Easy)
**Focus:** Complex conditional logic implementation.

#### 🔬 **Lab Session (75 minutes)**
**Project:** Enhance `inventory.py` with Error Handling

```python
# Requirements:
# Add try/except blocks
# Handle invalid user input gracefully
# Provide helpful error messages
# Ensure program never crashes
```

**Skills Practiced:**
- Exception handling
- Input validation
- User experience design
- Code robustness

---

### 🗓️ **Day 5: Friday - SQL Foundation**
**Focus:** Switching to database thinking mode.

#### ⏰ **Time Allocation**
| Session | Duration | Activity |
|---------|----------|----------|
| **Morning Learn** | 105 minutes | SQL fundamentals |
| **Gym Break** | 45 minutes | HackerRank SQL |
| **Evening Lab** | 75 minutes | Interactive SQL practice |

#### 📚 **Learning Session (105 minutes)**
**Core Topics:**
- **Query Structure:** `SELECT column FROM table WHERE condition ORDER BY column`
- **Logical Operators:** `AND`, `OR`, `NOT`, `IN`
- **Filtering Patterns:** Complex WHERE clauses
- **Sorting:** `ORDER BY` with multiple columns

```sql
-- Master these query patterns
SELECT name, age 
FROM users 
WHERE age > 25 AND city IN ('NYC', 'LA')
ORDER BY age DESC;
```

#### 🏋️ **Gym Session (45 minutes)**
**HackerRank SQL Challenges:**
1. "Select All" - Basic SELECT syntax
2. "Select By ID" - WHERE clause usage
3. "Weather Observation Station 1" - Column selection

#### 🔬 **Lab Session (75 minutes)**
**Interactive Practice:** SQLBolt.com Lessons 1-4
- Faster than video tutorials
- Immediate feedback
- Progressive difficulty
- Hands-on learning

---

## 🏁 Weekend Mission: The Desktop Cleaner

### 🎯 **Project Goal**
Build a file organization system that demonstrates mastery of Week 1 concepts.

#### 📋 **Project Requirements**
**Name:** `desktop_cleaner.py`
**Purpose:** Organize Downloads folder by file type

#### 🔧 **Technical Specifications**
```python
import os
import shutil

# Core functionality:
# 1. Scan Downloads folder
# 2. Identify file extensions
# 3. Create organized folder structure
# 4. Move files to appropriate locations

file_types = {
    'images': ['.jpg', '.png', '.gif', '.bmp'],
    'documents': ['.pdf', '.doc', '.txt', '.xlsx'],
    'videos': ['.mp4', '.avi', '.mov', '.mkv'],
    'audio': ['.mp3', '.wav', '.flac']
}
```

#### ✅ **Success Criteria**
- [ ] Uses loops for file iteration
- [ ] Implements string manipulation for extensions
- [ ] Includes error handling for file operations
- [ ] Creates folders dynamically
- [ ] Provides user feedback
- [ ] Handles edge cases gracefully

#### 🏗️ **Skills Integration**
| Concept | Application |
|---------|-------------|
| **Lists** | File extension categories |
| **Dictionaries** | File type mappings |
| **Strings** | Extension extraction |
| **Loops** | Directory traversal |
| **Conditions** | File type classification |
| **Error Handling** | File operation safety |

---

## ✅ Week 1 Success Checklist

### 📊 **Daily Completion Tracking**
- [ ] **Monday:** Completed grades.py with proper list handling
- [ ] **Tuesday:** Built inventory.py with dictionary operations
- [ ] **Wednesday:** Created cleaner.py for string processing
- [ ] **Thursday:** Enhanced code with error handling
- [ ] **Friday:** Solved 3+ SQL problems on HackerRank

### 🎯 **Technical Milestones**
- [ ] **Code Quality:** All scripts run without errors
- [ ] **Version Control:** Code pushed to GitHub repository
- [ ] **Problem Solving:** Completed 5+ LeetCode/HackerRank problems
- [ ] **Integration:** Weekend project demonstrates concept mastery

### 📈 **Learning Outcomes**
- [ ] **Python Fluency:** Write basic scripts without constant reference
- [ ] **Data Structures:** Comfortable with lists and dictionaries
- [ ] **Error Handling:** Build robust, crash-resistant code
- [ ] **SQL Basics:** Construct simple queries confidently
- [ ] **File Operations:** Manipulate file systems programmatically

---

## 🚀 Success Tips

### 💡 **Daily Habits**
- **Consistency:** Code at the same time daily
- **Documentation:** Comment your code thoroughly
- **Testing:** Run code multiple times with different inputs
- **Version Control:** Commit changes regularly

### 🎯 **Learning Strategy**
- **Practice First:** Hands-on before theory
- **Build Projects:** Apply concepts immediately
- **Debug Actively:** Understand error messages
- **Teach Others:** Explain concepts to solidify understanding

---

## 🔗 Resources

### 📚 **Learning Platforms**
- **LeetCode:** Algorithm practice
- **HackerRank:** SQL challenges
- **SQLBolt:** Interactive SQL tutorial
- **Python.org:** Official documentation

### 🛠️ **Development Tools**
- **VS Code:** Code editor
- **Git:** Version control
- **Python 3.8+:** Programming language
- **SQLite:** Local database practice

---

*Master the fundamentals, build the foundation! 🚀*