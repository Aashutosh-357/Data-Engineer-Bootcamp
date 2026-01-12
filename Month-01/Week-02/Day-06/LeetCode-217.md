
# 🏆 LEETCODE PROBLEMS: JSON & DATA STRUCTURES

## 📊 **Problem Categories**
| Category | Problems | Difficulty | Key Concepts |
|----------|----------|------------|-------------|
| **Hash Tables** | 217, 1, 242, 49 | Easy-Medium | Dictionary operations, frequency counting |

---

## 🎯 **Problem 1: Contains Duplicate (LC-217)**

### 📝 **Problem Statement**
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

### 💡 **Solution Approaches**

#### **Method 1: Set Comparison (Optimal)**
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        return len(nums) != len(set(nums))
```
**Time:** O(n) | **Space:** O(n)

#### **Method 2: Sorting Approach**
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        if len(nums) <= 1:
            return False
        
        nums.sort()
        for i in range(len(nums) - 1):
            if nums[i] == nums[i + 1]:
                return True
        return False
```
**Time:** O(n log n) | **Space:** O(1)

#### **Method 3: Hash Map Approach**
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for num in nums:
            if num in seen:
                return True
            seen.add(num)
        return False
```
**Time:** O(n) | **Space:** O(n)

---

## 🚀 **Performance Tips**

### ⚡ **Optimization Strategies**
1. **Use `set()` for O(1) lookups** instead of `list` with `in` operator
2. **Pre-build hash maps** for repeated queries
3. **Use `defaultdict`** to avoid key existence checks
4. **Consider memory vs time tradeoffs** in nested structures

### 📊 **Complexity Analysis**
```python
# Common operations complexity
operations = {
    "dict[key]": "O(1) average, O(n) worst",
    "key in dict": "O(1) average, O(n) worst",
    "dict.get(key)": "O(1) average, O(n) worst",
    "set.add(item)": "O(1) average, O(n) worst",
    "list.append(item)": "O(1) amortized",
    "json.loads(string)": "O(n) where n is string length"
}
```

---

*Master the patterns, ace the interviews! 🏆*