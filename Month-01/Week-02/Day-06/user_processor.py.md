# 🛒 User Order Processor

## 📋 **Task Overview**
Create `user_processor.py` to parse JSON user order data and calculate total spending per user.

---

## 📥 **Input Data**
```json
[
  {
    "user_id": 101,
    "orders": [
      {"item": "laptop", "price": 1200},
      {"item": "mouse", "price": 25}
    ]
  },
  {
    "user_id": 102,
    "orders": [
      {"item": "keyboard", "price": 75}
    ]
  }
]
```

---

## 🎯 **Requirements**
1. **Parse JSON** string into Python objects
2. **Loop through users** and calculate total spending
3. **Print results** in a user-friendly format

---

## 💻 **Complete Implementation**

### 📄 **user_processor.py**
```python
import json
from typing import List, Dict, Any

def process_user_orders(json_string: str) -> None:
    """
    Process user orders from JSON string and calculate total spending
    
    Args:
        json_string: JSON string containing user order data
    """
    try:
        # Parse JSON string to Python objects
        users_data = json.loads(json_string)
        
        print("🛒 User Order Processing Results")
        print("=" * 40)
        
        total_all_users = 0
        
        # Process each user
        for user in users_data:
            user_id = user.get("user_id")
            orders = user.get("orders", [])
            
            # Calculate total spending for this user
            user_total = sum(order.get("price", 0) for order in orders)
            total_all_users += user_total
            
            # Print user details
            print(f"👤 User ID: {user_id}")
            print(f"📦 Orders: {len(orders)} items")
            
            # Print individual orders
            for order in orders:
                item = order.get("item", "Unknown")
                price = order.get("price", 0)
                print(f"   • {item}: ${price:,.2f}")
            
            print(f"💰 Total Spending: ${user_total:,.2f}")
            print("-" * 30)
        
        print(f"🏆 Grand Total (All Users): ${total_all_users:,.2f}")
        
    except json.JSONDecodeError as e:
        print(f"❌ Error parsing JSON: {e}")
    except Exception as e:
        print(f"❌ Error processing data: {e}")

def main():
    """Main function to run the user processor"""
    
    # Input JSON string
    json_data = '''[
        {
            "user_id": 101,
            "orders": [
                {"item": "laptop", "price": 1200},
                {"item": "mouse", "price": 25}
            ]
        },
        {
            "user_id": 102,
            "orders": [
                {"item": "keyboard", "price": 75}
            ]
        }
    ]'''
    
    # Process the orders
    process_user_orders(json_data)

if __name__ == "__main__":
    main()
```

---

## 🚀 **Expected Output**
```
🛒 User Order Processing Results
========================================
👤 User ID: 101
📦 Orders: 2 items
   • laptop: $1,200.00
   • mouse: $25.00
💰 Total Spending: $1,225.00
------------------------------
👤 User ID: 102
📦 Orders: 1 items
   • keyboard: $75.00
💰 Total Spending: $75.00
------------------------------
🏆 Grand Total (All Users): $1,300.00
```

---

## 🔧 **Enhanced Version with Analytics**

```python
import json
from typing import List, Dict, Any, Tuple
from collections import defaultdict

class UserOrderProcessor:
    """Enhanced user order processor with analytics"""
    
    def __init__(self, json_string: str):
        self.users_data = json.loads(json_string)
        self.analytics = self._calculate_analytics()
    
    def _calculate_analytics(self) -> Dict[str, Any]:
        """Calculate comprehensive analytics"""
        total_users = len(self.users_data)
        total_orders = 0
        total_revenue = 0
        item_frequency = defaultdict(int)
        user_spending = []
        
        for user in self.users_data:
            orders = user.get("orders", [])
            user_total = sum(order.get("price", 0) for order in orders)
            
            total_orders += len(orders)
            total_revenue += user_total
            user_spending.append(user_total)
            
            # Track item frequency
            for order in orders:
                item = order.get("item", "Unknown")
                item_frequency[item] += 1
        
        # Calculate averages
        avg_spending = total_revenue / total_users if total_users > 0 else 0
        avg_orders = total_orders / total_users if total_users > 0 else 0
        
        return {
            "total_users": total_users,
            "total_orders": total_orders,
            "total_revenue": total_revenue,
            "avg_spending_per_user": avg_spending,
            "avg_orders_per_user": avg_orders,
            "most_popular_item": max(item_frequency.items(), key=lambda x: x[1]) if item_frequency else ("None", 0),
            "user_spending_range": (min(user_spending), max(user_spending)) if user_spending else (0, 0)
        }
    
    def process_orders(self) -> None:
        """Process and display user orders with analytics"""
        print("🛒 Enhanced User Order Processing")
        print("=" * 50)
        
        # Display individual users
        for user in self.users_data:
            user_id = user.get("user_id")
            orders = user.get("orders", [])
            user_total = sum(order.get("price", 0) for order in orders)
            
            print(f"👤 User ID: {user_id} | Total: ${user_total:,.2f}")
            for order in orders:
                item = order.get("item", "Unknown")
                price = order.get("price", 0)
                print(f"   📦 {item}: ${price:,.2f}")
            print()
        
        # Display analytics
        self._display_analytics()
    
    def _display_analytics(self) -> None:
        """Display comprehensive analytics"""
        a = self.analytics
        
        print("📊 ORDER ANALYTICS")
        print("=" * 30)
        print(f"👥 Total Users: {a['total_users']}")
        print(f"📦 Total Orders: {a['total_orders']}")
        print(f"💰 Total Revenue: ${a['total_revenue']:,.2f}")
        print(f"📈 Avg Spending/User: ${a['avg_spending_per_user']:,.2f}")
        print(f"📊 Avg Orders/User: {a['avg_orders_per_user']:.1f}")
        print(f"🏆 Most Popular Item: {a['most_popular_item'][0]} ({a['most_popular_item'][1]} orders)")
        print(f"💸 Spending Range: ${a['user_spending_range'][0]:,.2f} - ${a['user_spending_range'][1]:,.2f}")

# Usage
def main_enhanced():
    json_data = '''[
        {
            "user_id": 101,
            "orders": [
                {"item": "laptop", "price": 1200},
                {"item": "mouse", "price": 25}
            ]
        },
        {
            "user_id": 102,
            "orders": [
                {"item": "keyboard", "price": 75}
            ]
        }
    ]'''
    
    processor = UserOrderProcessor(json_data)
    processor.process_orders()

if __name__ == "__main__":
    main_enhanced()
```

---

## 🎯 **Key Features**

### ✅ **Core Functionality**
- **JSON Parsing**: Safe parsing with error handling
- **Order Processing**: Loop through users and calculate totals
- **Formatted Output**: User-friendly display with emojis

### 🚀 **Enhanced Features**
- **Analytics Dashboard**: Comprehensive order statistics
- **Item Tracking**: Most popular items and frequency
- **Spending Analysis**: Average spending and ranges
- **Error Handling**: Robust exception management

### 💡 **Best Practices**
- **Type Hints**: Clear function signatures
- **Modular Design**: Separate functions for different tasks
- **Safe Access**: Using `.get()` for dictionary access
- **Professional Output**: Formatted currency and statistics

---

## 🔄 **Usage Examples**

### 📝 **Basic Usage**
```bash
python user_processor.py
```

### 🔧 **Custom JSON Input**
```python
custom_data = '[{"user_id": 103, "orders": [{"item": "tablet", "price": 500}]}]'
process_user_orders(custom_data)
```

---

*Process orders, analyze data, drive insights! 📈*