# 📊 Grade Processing System

## 🎯 Project Overview
A practical drill combining **list iteration**, **conditional logic**, and **arithmetic operations** to process student grades.

---

## 📋 Requirements & Specifications

### 📥 **Input**
```python
grades = [50, 80, 90, 45]
```

### 🧮 **Business Logic**
| Score Range | Status |
|-------------|--------|
| < 50 | ❌ **Fail** |
| ≥ 50 | ✅ **Pass** |

### 🎯 **Deliverables**
1. Individual pass/fail status for each grade
2. Mathematical average of all scores
3. Graceful handling of edge cases (empty lists)

---

## 🔧 Technical Approach

### 🚀 **Algorithm Choice**
- **Iteration Method:** `for...in` loop (most readable)
- **Accumulation Strategy:** Manual accumulator for educational value
- **Error Handling:** Empty list validation

### ⚖️ **Alternative Approaches**
| Method | Pros | Cons | Use Case |
|--------|------|------|----------|
| Manual Loop | Educational, flexible | More code | Learning scenarios |
| Built-in Functions | Concise (`sum()`, `len()`) | Less transparent | Production code |
| List Comprehension | Pythonic | Single-purpose | Simple transformations |

### 📈 **Complexity Analysis**
- ⏱️ **Time:** `O(N)` - Single pass through grades
- 💾 **Space:** `O(1)` - Constant extra variables

---

## 💻 Implementation

### 📁 **File: `grades.py`**

```python
"""
grades.py
Logic: Process student scores to determine pass/fail status and average.
"""

def process_grades(scores):
    # Handle edge case of an empty list
    if not scores:
        print("No grades provided.")
        return

    total_sum = 0
    count = len(scores)

    print("--- Individual Results ---")
    for score in scores:
        # Add to our running total for average calculation
        total_sum += score
        
        # Conditional Logic for Pass/Fail
        if score < 50:
            status = "Fail"
        else:
            status = "Pass"
            
        print(f"Score: {score} -> {status}")

    # Calculate Average
    average = total_sum / count
    
    print("-" * 26)
    print(f"Total Students: {count}")
    print(f"Class Average:  {average:.2f}")

# Input List
grade_list = [50, 80, 90, 45]

# Execute
if __name__ == "__main__":
    process_grades(grade_list)
```

---

## 🔍 Step-by-Step Execution

### 📊 **Input Analysis**
```python
grade_list = [50, 80, 90, 45]
```

### 🔄 **Processing Steps**

| Step | Score | Condition | Status | Running Total |
|------|-------|-----------|--------|--------------|
| 1 | 50 | `50 < 50` → False | ✅ Pass | 50 |
| 2 | 80 | `80 < 50` → False | ✅ Pass | 130 |
| 3 | 90 | `90 < 50` → False | ✅ Pass | 220 |
| 4 | 45 | `45 < 50` → True | ❌ Fail | 265 |

### 📈 **Final Calculation**
```
Average = Total Sum ÷ Count
Average = 265 ÷ 4 = 66.25
```

### 🖥️ **Expected Output**
```
--- Individual Results ---
Score: 50 -> Pass
Score: 80 -> Pass
Score: 90 -> Pass
Score: 45 -> Fail
--------------------------
Total Students: 4
Class Average:  66.25
```

---

## 💡 Expert Insights

> 🏢 **Production Note:** The `total_sum += score` pattern inside loops is highly reliable and commonly used in enterprise systems.

> 🔄 **Scalability:** For simple averages, `sum()` and `len()` built-ins are preferred, but manual accumulation is essential when performing multiple operations per iteration.

> 🚀 **Next Level:** Consider extending this to handle student names with a dictionary structure: `{"Alice": 85, "Bob": 92}`

---

## 🎯 Key Learning Outcomes
- ✅ Single-pass data processing
- ✅ Conditional logic implementation
- ✅ Accumulator pattern mastery
- ✅ Edge case handling
- ✅ Clean output formatting

---

*Ready to level up? Try implementing this with student names! 🚀*