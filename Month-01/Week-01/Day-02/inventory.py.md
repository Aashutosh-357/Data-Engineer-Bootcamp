# 📦 Inventory Management System

## 🎯 Project Overview
A comprehensive exercise in **string parsing** and **dictionary mutation** - essential skills for processing unstructured text data in production environments.

> 🏢 **Real-World Application:** Production systems frequently receive unstructured text that needs normalization for database updates.

---

## 📋 Requirements & Specifications

### 📥 **Input Data**
```python
inventory = {'apple': 5, 'banana': 2}
user_input = "Bought 3 apple"  # or "Bought 3 apples"
```

### 🧠 **Business Logic**
- Extract numeric quantity from user string
- Parse item name (handle pluralization)
- Update inventory dictionary dynamically
- Add new items if they don't exist

### ⚠️ **Edge Cases to Handle**
| Scenario | Challenge | Solution |
|----------|-----------|----------|
| New fruit | Item not in inventory | Add with `.get(fruit, 0)` |
| Pluralization | "apples" vs "apple" | Use `.rstrip('s')` |
| Invalid input | Non-numeric quantity | `try/except` blocks |
| Malformed string | Missing parts | Length validation |

---

## 🔧 Technical Approach

### 🚀 **Algorithm Design**
1. **String Parsing:** `.split()` method for tokenization
2. **Data Extraction:** Index-based access to parts
3. **Safe Updates:** `.get()` method for graceful handling
4. **Error Handling:** Comprehensive exception management

### 📊 **Complexity Analysis**
- ⏱️ **Time:** `O(W)` where W = words in input string
- 💾 **Space:** `O(K)` where K = unique fruits in inventory
- 🔄 **Dictionary Update:** `O(1)` constant time

---

## 💻 Implementation

### 📁 **File: `inventory.py`**

```python
"""
inventory.py
Logic: Parse user transaction strings to update fruit inventory.
"""

def update_inventory():
    # Initial Inventory
    inventory = {'apple': 5, 'banana': 2}
    print(f"Current Inventory: {inventory}")

    # Ask for user input
    # Expected format: "Bought 3 apple"
    user_input = input("Enter transaction (e.g., 'Bought 3 apple'): ")

    try:
        # Step 1: Split the string into parts
        parts = user_input.split()
        
        # Basic validation: ensure we have enough parts
        if len(parts) < 3:
            print("Error: Please use the format 'Bought [number] [item]'")
            return

        # Step 2: Extract data
        # We assume index 1 is quantity and index 2 is the fruit name
        quantity = int(parts[1])
        fruit = parts[2].lower().rstrip('s') # Convert to lowercase and remove trailing 's' for 'apples'

        # Step 3: Update Dictionary
        # .get(fruit, 0) handles new fruits gracefully
        current_stock = inventory.get(fruit, 0)
        inventory[fruit] = current_stock + quantity

        print(f"\nSuccessfully updated!")
        print(f"New Inventory: {inventory}")

    except ValueError:
        print("Error: Could not find a valid number for the quantity.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")

if __name__ == "__main__":
    update_inventory()
```

---

## 🔍 Step-by-Step Walkthrough

### 📊 **Test Scenario: "Bought 10 bananas"**

| Step | Process | Input/Output |
|------|---------|-------------|
| 1 | **String Split** | `['Bought', '10', 'bananas']` |
| 2 | **Extract Quantity** | `int('10')` → `10` |
| 3 | **Normalize Fruit** | `'bananas'.lower().rstrip('s')` → `'banana'` |
| 4 | **Current Stock Lookup** | `inventory.get('banana', 0)` → `2` |
| 5 | **Update Inventory** | `inventory['banana'] = 2 + 10` → `12` |
| 6 | **Final Result** | `{'apple': 5, 'banana': 12}` |

### 🖥️ **Expected Output**
```
Current Inventory: {'apple': 5, 'banana': 2}
Enter transaction (e.g., 'Bought 3 apple'): Bought 10 bananas

Successfully updated!
New Inventory: {'apple': 5, 'banana': 12}
```

---

## 💡 Expert Insights

> 🔧 **Production Pattern:** The `.rstrip('s')` approach is a quick heuristic for simple plurals. In enterprise NLP systems, proper lemmatizers would be used.

> 🛡️ **Error Resilience:** Using `try...except` around `int()` conversion is crucial - user input is notoriously unreliable and should never crash production services.

> 📈 **Scalability:** The `.get(fruit, 0)` pattern gracefully handles new items, making the system extensible without code changes.

---

## 🎯 Key Learning Outcomes
- ✅ String parsing and tokenization
- ✅ Safe dictionary updates with `.get()`
- ✅ Robust error handling patterns
- ✅ Input validation and normalization
- ✅ Production-ready code practices

---

## 🚀 Enhancement Ideas
**Ready for more?** Try extending this system to handle:
- "Sold" transactions (subtraction logic)
- Multiple items in one transaction
- Inventory alerts for low stock

---

*Build robust systems, handle the unexpected! 🛠️*