# 🏆 LEETCODE PROBLEMS: TWO POINTERS & STRING MANIPULATION

## 📊 **Problem Categories**
| Category | Problems | Difficulty | Key Concepts |
|----------|----------|------------|-------------|
| **Two Pointers** | 344, 125, 167, 15 | Easy-Medium | In-place operations, pointer manipulation |
| **String Manipulation** | 344, 151, 557, 345 | Easy-Medium | Character arrays, reversing, palindromes |
| **Array Manipulation** | 344, 189, 283, 26 | Easy-Medium | In-place modifications, swapping |

---

## 🎯 **Problem 1: Reverse String (LC-344)**

### 📝 **Problem Statement**
Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array **in-place** with O(1) extra memory.

### 💡 **Solution Approaches**

#### **Method 1: Two Pointers (Optimal)**
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```
**Time:** O(n) | **Space:** O(1)

#### **Method 2: Recursive Approach**
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        def helper(left, right):
            if left >= right:
                return
            s[left], s[right] = s[right], s[left]
            helper(left + 1, right - 1)
        
        helper(0, len(s) - 1)
```
**Time:** O(n) | **Space:** O(n) - due to recursion stack

#### **Method 3: Pythonic Approach**
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        s.reverse()  # Built-in method
        # Or: s[:] = s[::-1]
```
**Time:** O(n) | **Space:** O(1)

### 🔍 **Step-by-Step Example**
```python
# Input: s = ["h","e","l","l","o"]
# Step 1: left=0, right=4 -> swap 'h' and 'o' -> ["o","e","l","l","h"]
# Step 2: left=1, right=3 -> swap 'e' and 'l' -> ["o","l","l","e","h"]
# Step 3: left=2, right=2 -> left >= right, stop
# Output: ["o","l","l","e","h"]
```

---

## 🎯 **Problem 2: Valid Palindrome (LC-125)**

### 📝 **Problem Statement**
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.

### 💡 **Two Pointers Solution**
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1
        
        while left < right:
            # Skip non-alphanumeric characters
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1
            
            # Compare characters (case-insensitive)
            if s[left].lower() != s[right].lower():
                return False
            
            left += 1
            right -= 1
        
        return True
```
**Time:** O(n) | **Space:** O(1)

---

## 🎯 **Problem 3: Two Sum II - Input Array Is Sorted (LC-167)**

### 📝 **Problem Statement**
Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number.

### 💡 **Two Pointers Solution**
```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        
        while left < right:
            current_sum = numbers[left] + numbers[right]
            
            if current_sum == target:
                return [left + 1, right + 1]  # 1-indexed
            elif current_sum < target:
                left += 1
            else:
                right -= 1
        
        return []  # No solution found
```
**Time:** O(n) | **Space:** O(1)

---

## 🎯 **Problem 4: 3Sum (LC-15)**

### 📝 **Problem Statement**
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

### 💡 **Sorting + Two Pointers Solution**
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        result = []
        
        for i in range(len(nums) - 2):
            # Skip duplicates for first number
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            left, right = i + 1, len(nums) - 1
            
            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]
                
                if current_sum == 0:
                    result.append([nums[i], nums[left], nums[right]])
                    
                    # Skip duplicates
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    
                    left += 1
                    right -= 1
                elif current_sum < 0:
                    left += 1
                else:
                    right -= 1
        
        return result
```
**Time:** O(n²) | **Space:** O(1) - excluding output

---

## 🎯 **Problem 5: Move Zeroes (LC-283)**

### 📝 **Problem Statement**
Given an integer array `nums`, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

### 💡 **Two Pointers Solution**
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        left = 0  # Position for next non-zero element
        
        # Move all non-zero elements to the front
        for right in range(len(nums)):
            if nums[right] != 0:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
```
**Time:** O(n) | **Space:** O(1)

---

## 💡 **Key Patterns for Two Pointers Problems**

### 🎯 **Pattern 1: Opposite Direction (Converging)**
```python
# Template for problems like reverse, palindrome, two sum
def two_pointers_converging(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        # Process arr[left] and arr[right]
        if condition:
            # Move both pointers
            left += 1
            right -= 1
        elif other_condition:
            left += 1
        else:
            right -= 1
```

### 🎯 **Pattern 2: Same Direction (Fast & Slow)**
```python
# Template for problems like remove duplicates, move zeros
def two_pointers_same_direction(arr):
    slow = 0
    for fast in range(len(arr)):
        if condition:
            arr[slow] = arr[fast]
            slow += 1
    return slow
```

---

## 🚀 **Performance Tips**

### ⚡ **Optimization Strategies**
1. **Use two pointers** instead of nested loops when possible
2. **Sort first** for problems involving pairs/triplets
3. **Skip duplicates** to avoid redundant computations
4. **In-place modifications** to achieve O(1) space complexity
5. **Early termination** when conditions are met

### 📊 **Complexity Analysis**
```python
# Common two-pointer operations
operations = {
    "Two pointers (converging)": "O(n) time, O(1) space",
    "Two pointers (same direction)": "O(n) time, O(1) space",
    "Sort + Two pointers": "O(n log n) time, O(1) space",
    "Array reversal": "O(n) time, O(1) space",
    "In-place swapping": "O(1) time, O(1) space"
}
```

---

## 🎯 **Practice Problems by Difficulty**

### 🟢 **Easy Level**
1. **Reverse String (LC-344)** - Basic two pointers
2. **Valid Palindrome (LC-125)** - Skip invalid characters
3. **Move Zeroes (LC-283)** - Fast/slow pointers
4. **Remove Duplicates (LC-26)** - Sorted array processing

### 🟡 **Medium Level**
1. **Two Sum II (LC-167)** - Sorted array advantage
2. **3Sum (LC-15)** - Triple pointer technique
3. **Rotate Array (LC-189)** - Multiple reversals
4. **Container With Most Water (LC-11)** - Area maximization

### 🔴 **Hard Level**
1. **Trapping Rain Water (LC-42)** - Advanced two pointers
2. **Minimum Window Substring (LC-76)** - Sliding window
3. **4Sum (LC-18)** - Quadruple pointer technique

---

*Master two pointers, solve efficiently! 🎯*
        
