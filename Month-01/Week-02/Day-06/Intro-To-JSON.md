# 🗂️ Introduction to JSON & Nested Data Structures

## 🎯 Learning Objectives
Master nested dictionaries, lists of dictionaries, and JSON handling - essential skills for modern data engineering and API integration.

> 💡 **Why This Matters:** JSON is the universal language of web APIs, configuration files, and data exchange in modern applications.

---

# 📚 TOPIC 1: NESTED DICTIONARIES

## 🔍 **Concept Overview**
**Nested Dictionaries** are dictionaries that contain other dictionaries as values, creating hierarchical data structures.

### 🏗️ **Basic Structure**
```python
# Simple nested dictionary
user_profile = {
    "personal_info": {
        "name": "Alice Johnson",
        "age": 28,
        "email": "alice@company.com"
    },
    "work_info": {
        "department": "Engineering",
        "position": "Senior Developer",
        "salary": 95000
    },
    "preferences": {
        "theme": "dark",
        "notifications": True,
        "language": "en"
    }
}
```

### 🎯 **Accessing Nested Data**
```python
# Method 1: Direct access (risky if keys don't exist)
name = user_profile["personal_info"]["name"]
department = user_profile["work_info"]["department"]

# Method 2: Safe access with .get()
name = user_profile.get("personal_info", {}).get("name", "Unknown")
salary = user_profile.get("work_info", {}).get("salary", 0)

# Method 3: Multiple levels of safety
email = user_profile.get("personal_info", {}).get("email")
if email:
    print(f"User email: {email}")
else:
    print("Email not found")
```

### 🔧 **Modifying Nested Data**
```python
# Adding new nested data
user_profile["contact_info"] = {
    "phone": "+1-555-0123",
    "address": {
        "street": "123 Main St",
        "city": "San Francisco",
        "state": "CA",
        "zip": "94105"
    }
}

# Updating existing nested values
user_profile["work_info"]["salary"] = 100000
user_profile["preferences"]["theme"] = "light"

# Adding to existing nested dictionary
user_profile["personal_info"]["linkedin"] = "linkedin.com/in/alice-johnson"
```

---

# 📋 TOPIC 2: LISTS OF DICTIONARIES

## 🔍 **Concept Overview**
**Lists of Dictionaries** represent collections of structured data - like database records or API responses.

### 🏗️ **Basic Structure**
```python
# List of user dictionaries
users = [
    {
        "id": 1,
        "name": "Alice Johnson",
        "email": "alice@company.com",
        "department": "Engineering",
        "active": True
    },
    {
        "id": 2,
        "name": "Bob Smith",
        "email": "bob@company.com",
        "department": "Marketing",
        "active": True
    },
    {
        "id": 3,
        "name": "Carol Davis",
        "email": "carol@company.com",
        "department": "Sales",
        "active": False
    }
]
```

### 🎯 **Accessing Data in Lists of Dictionaries**
```python
# Access by index
first_user = users[0]
second_user_email = users[1]["email"]  # "bob@company.com"

# Safe access with bounds checking
def get_user_email(users, index):
    if 0 <= index < len(users):
        return users[index].get("email", "Email not found")
    return "User not found"

# Usage
email = get_user_email(users, 1)  # Safe access to second user
```

### 🔄 **Iterating Through Lists of Dictionaries**
```python
# Method 1: Simple iteration
for user in users:
    print(f"Name: {user['name']}, Email: {user['email']}")

# Method 2: With index
for i, user in enumerate(users):
    print(f"User {i+1}: {user['name']} ({user['email']})")

# Method 3: Filtering active users
active_users = [user for user in users if user.get("active", False)]
for user in active_users:
    print(f"Active user: {user['name']}")
```

### 🔍 **Searching and Filtering**
```python
# Find user by email
def find_user_by_email(users, email):
    for user in users:
        if user.get("email") == email:
            return user
    return None

# Find users by department
def find_users_by_department(users, department):
    return [user for user in users if user.get("department") == department]

# Usage examples
user = find_user_by_email(users, "bob@company.com")
engineering_users = find_users_by_department(users, "Engineering")
```

---

# 🌐 TOPIC 3: INTRODUCTION TO JSON

## 🔍 **What is JSON?**
**JSON (JavaScript Object Notation)** is a lightweight, text-based data interchange format that's easy for humans to read and write.

### 📊 **JSON vs Python Dictionary Comparison**
| Aspect | JSON | Python Dictionary |
|--------|------|-------------------|
| **Data Type** | Text string | Python object |
| **Booleans** | `true`, `false` | `True`, `False` |
| **Null Values** | `null` | `None` |
| **Strings** | Double quotes only | Single or double quotes |
| **Comments** | Not allowed | Not applicable |

### 🔧 **JSON Syntax Rules**
```json
{
    "name": "Alice Johnson",
    "age": 28,
    "active": true,
    "skills": ["Python", "JavaScript", "SQL"],
    "address": {
        "city": "San Francisco",
        "state": "CA"
    },
    "spouse": null
}
```

---

# 🐍 TOPIC 4: PYTHON JSON MODULE

## 📦 **Core Functions**
The `json` module provides four main functions for JSON handling:

### 📤 **Serialization (Python → JSON)**
```python
import json

# json.dumps() - Convert Python object to JSON string
python_data = {
    "name": "Alice",
    "age": 28,
    "skills": ["Python", "SQL"]
}

json_string = json.dumps(python_data)
print(json_string)  # {"name": "Alice", "age": 28, "skills": ["Python", "SQL"]}

# json.dump() - Write Python object to JSON file
with open("user_data.json", "w") as file:
    json.dump(python_data, file, indent=2)
```

### 📥 **Deserialization (JSON → Python)**
```python
# json.loads() - Parse JSON string to Python object
json_string = '{"name": "Bob", "age": 30, "active": true}'
python_data = json.loads(json_string)
print(python_data)  # {'name': 'Bob', 'age': 30, 'active': True}

# json.load() - Read JSON file to Python object
with open("user_data.json", "r") as file:
    data = json.load(file)
    print(data)
```

### 🎨 **Pretty Printing JSON**
```python
# Format JSON with indentation
data = {"users": [{"name": "Alice", "email": "alice@example.com"}]}

# Compact format
compact = json.dumps(data)
print(compact)  # {"users":[{"name":"Alice","email":"alice@example.com"}]}

# Pretty format
pretty = json.dumps(data, indent=2)
print(pretty)
# {
#   "users": [
#     {
#       "name": "Alice",
#       "email": "alice@example.com"
#     }
#   ]
# }
```

---

# 🛠️ DRILL COMPLETION: MULTIPLE USERS SYSTEM

## 📋 **Drill Requirements**
Create a list of dictionaries representing multiple users and access the email of the second user.

### 💻 **Complete Implementation**
```python
import json
from datetime import datetime

# Create comprehensive user data structure
users = [
    {
        "id": 1,
        "personal_info": {
            "name": "Alice Johnson",
            "email": "alice.johnson@techcorp.com",
            "age": 28,
            "phone": "+1-555-0101"
        },
        "work_info": {
            "department": "Engineering",
            "position": "Senior Software Engineer",
            "salary": 95000,
            "start_date": "2022-03-15"
        },
        "preferences": {
            "theme": "dark",
            "notifications": True,
            "language": "en"
        },
        "skills": ["Python", "JavaScript", "React", "SQL"],
        "active": True
    },
    {
        "id": 2,
        "personal_info": {
            "name": "Bob Smith",
            "email": "bob.smith@techcorp.com",
            "age": 32,
            "phone": "+1-555-0102"
        },
        "work_info": {
            "department": "Marketing",
            "position": "Marketing Manager",
            "salary": 78000,
            "start_date": "2021-08-20"
        },
        "preferences": {
            "theme": "light",
            "notifications": False,
            "language": "en"
        },
        "skills": ["Digital Marketing", "Analytics", "SEO"],
        "active": True
    },
    {
        "id": 3,
        "personal_info": {
            "name": "Carol Davis",
            "email": "carol.davis@techcorp.com",
            "age": 29,
            "phone": "+1-555-0103"
        },
        "work_info": {
            "department": "Sales",
            "position": "Sales Representative",
            "salary": 65000,
            "start_date": "2023-01-10"
        },
        "preferences": {
            "theme": "auto",
            "notifications": True,
            "language": "es"
        },
        "skills": ["Sales", "CRM", "Negotiation"],
        "active": False
    }
]

# DRILL SOLUTION: Access email of second user
def get_second_user_email(users_list):
    """
    Safely access the email of the second user
    
    Args:
        users_list: List of user dictionaries
    
    Returns:
        str: Email of second user or error message
    """
    try:
        # Check if we have at least 2 users
        if len(users_list) < 2:
            return "Error: Less than 2 users in the list"
        
        # Access second user (index 1)
        second_user = users_list[1]
        
        # Safely access nested email
        email = second_user.get("personal_info", {}).get("email")
        
        if email:
            return email
        else:
            return "Error: Email not found for second user"
            
    except (IndexError, TypeError) as e:
        return f"Error accessing user data: {e}"

# Execute the drill
second_user_email = get_second_user_email(users)
print(f"Second user's email: {second_user_email}")

# Additional demonstrations
print("\n" + "="*50)
print("ADDITIONAL DEMONSTRATIONS")
print("="*50)

# 1. Display all user emails
print("\n1. All User Emails:")
for i, user in enumerate(users):
    email = user.get("personal_info", {}).get("email", "No email")
    name = user.get("personal_info", {}).get("name", "Unknown")
    print(f"   User {i+1}: {name} - {email}")

# 2. Convert to JSON and back
print("\n2. JSON Conversion:")
json_string = json.dumps(users, indent=2)
print("   Data converted to JSON format (first 200 chars):")
print(f"   {json_string[:200]}...")

# Parse back from JSON
parsed_users = json.loads(json_string)
print(f"   Parsed back: {len(parsed_users)} users loaded from JSON")

# 3. Save to file and load back
print("\n3. File Operations:")
filename = "users_data.json"

# Save to file
with open(filename, "w") as file:
    json.dump(users, file, indent=2)
print(f"   Data saved to {filename}")

# Load from file
with open(filename, "r") as file:
    loaded_users = json.load(file)
    
# Verify loaded data
loaded_second_email = get_second_user_email(loaded_users)
print(f"   Loaded second user email: {loaded_second_email}")

# 4. Advanced filtering
print("\n4. Advanced Operations:")

# Find active users
active_users = [user for user in users if user.get("active", False)]
print(f"   Active users: {len(active_users)}")

# Find users by department
engineering_users = [
    user for user in users 
    if user.get("work_info", {}).get("department") == "Engineering"
]
print(f"   Engineering users: {len(engineering_users)}")

# Calculate average salary
salaries = [
    user.get("work_info", {}).get("salary", 0) 
    for user in users
]
avg_salary = sum(salaries) / len(salaries) if salaries else 0
print(f"   Average salary: ${avg_salary:,.2f}")
```

---

# 🔍 ADVANCED JSON OPERATIONS

## 🛠️ **Error Handling**
```python
import json

def safe_json_operations(data):
    """Demonstrate safe JSON operations with error handling"""
    
    try:
        # Safe JSON serialization
        json_string = json.dumps(data, indent=2)
        print("✅ JSON serialization successful")
        
        # Safe JSON deserialization
        parsed_data = json.loads(json_string)
        print("✅ JSON deserialization successful")
        
        return parsed_data
        
    except TypeError as e:
        print(f"❌ Serialization error: {e}")
        return None
    except json.JSONDecodeError as e:
        print(f"❌ JSON parsing error: {e}")
        return None
    except Exception as e:
        print(f"❌ Unexpected error: {e}")
        return None

# Test with valid data
valid_data = {"name": "Alice", "age": 28}
result = safe_json_operations(valid_data)

# Test with invalid data (contains function - not JSON serializable)
invalid_data = {"name": "Bob", "func": lambda x: x}
result = safe_json_operations(invalid_data)
```

## 🎯 **Custom JSON Encoding**
```python
import json
from datetime import datetime

class DateTimeEncoder(json.JSONEncoder):
    """Custom encoder to handle datetime objects"""
    
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

# Usage with datetime objects
user_with_dates = {
    "name": "Alice",
    "created_at": datetime.now(),
    "last_login": datetime(2024, 1, 15, 10, 30, 0)
}

# Serialize with custom encoder
json_string = json.dumps(user_with_dates, cls=DateTimeEncoder, indent=2)
print("JSON with datetime objects:")
print(json_string)
```

---

# 🌐 REAL-WORLD APPLICATIONS

## 📊 **API Response Handling**
```python
import json
import requests  # Note: This would need to be installed

def process_api_response():
    """Example of processing JSON API response"""
    
    # Simulated API response
    api_response = '''
    {
        "status": "success",
        "data": {
            "users": [
                {
                    "id": 1,
                    "name": "Alice Johnson",
                    "email": "alice@example.com",
                    "profile": {
                        "avatar": "https://example.com/avatar1.jpg",
                        "bio": "Software Engineer"
                    }
                }
            ]
        },
        "pagination": {
            "page": 1,
            "total_pages": 5,
            "total_users": 50
        }
    }
    '''
    
    try:
        # Parse JSON response
        response_data = json.loads(api_response)
        
        # Extract relevant information
        status = response_data.get("status")
        users = response_data.get("data", {}).get("users", [])
        pagination = response_data.get("pagination", {})
        
        print(f"API Status: {status}")
        print(f"Users received: {len(users)}")
        print(f"Total users: {pagination.get('total_users', 0)}")
        
        # Process each user
        for user in users:
            name = user.get("name", "Unknown")
            email = user.get("email", "No email")
            bio = user.get("profile", {}).get("bio", "No bio")
            
            print(f"  - {name} ({email}): {bio}")
            
    except json.JSONDecodeError as e:
        print(f"Error parsing API response: {e}")

# Execute example
process_api_response()
```

## ⚙️ **Configuration Management**
```python
import json
import os

class ConfigManager:
    """Manage application configuration using JSON"""
    
    def __init__(self, config_file="config.json"):
        self.config_file = config_file
        self.config = self.load_config()
    
    def load_config(self):
        """Load configuration from JSON file"""
        default_config = {
            "database": {
                "host": "localhost",
                "port": 5432,
                "name": "myapp"
            },
            "api": {
                "base_url": "https://api.example.com",
                "timeout": 30,
                "retries": 3
            },
            "logging": {
                "level": "INFO",
                "file": "app.log"
            }
        }
        
        if os.path.exists(self.config_file):
            try:
                with open(self.config_file, "r") as file:
                    return json.load(file)
            except (json.JSONDecodeError, IOError) as e:
                print(f"Error loading config: {e}. Using defaults.")
                return default_config
        else:
            # Create default config file
            self.save_config(default_config)
            return default_config
    
    def save_config(self, config=None):
        """Save configuration to JSON file"""
        config_to_save = config or self.config
        try:
            with open(self.config_file, "w") as file:
                json.dump(config_to_save, file, indent=2)
            print(f"Configuration saved to {self.config_file}")
        except IOError as e:
            print(f"Error saving config: {e}")
    
    def get(self, key_path, default=None):
        """Get configuration value using dot notation"""
        keys = key_path.split(".")
        value = self.config
        
        for key in keys:
            if isinstance(value, dict) and key in value:
                value = value[key]
            else:
                return default
        
        return value
    
    def set(self, key_path, value):
        """Set configuration value using dot notation"""
        keys = key_path.split(".")
        config = self.config
        
        # Navigate to the parent of the target key
        for key in keys[:-1]:
            if key not in config:
                config[key] = {}
            config = config[key]
        
        # Set the value
        config[keys[-1]] = value
        self.save_config()

# Usage example
config = ConfigManager()

# Get configuration values
db_host = config.get("database.host", "localhost")
api_timeout = config.get("api.timeout", 30)

print(f"Database host: {db_host}")
print(f"API timeout: {api_timeout}")

# Update configuration
config.set("api.timeout", 60)
config.set("features.new_feature", True)
```

---

# 🎯 Key Takeaways

## ✅ **Nested Dictionaries Mastery**
- **Structure:** Dictionaries within dictionaries for hierarchical data
- **Access:** Use `.get()` for safe nested access
- **Modification:** Direct assignment or nested updates
- **Use Cases:** User profiles, configuration data, API responses

## ✅ **Lists of Dictionaries Proficiency**
- **Structure:** Collections of structured records
- **Access:** Index-based with bounds checking
- **Iteration:** Multiple patterns for different needs
- **Operations:** Filtering, searching, and data transformation

## ✅ **JSON Handling Expertise**
- **Serialization:** `json.dumps()` and `json.dump()`
- **Deserialization:** `json.loads()` and `json.load()`
- **Error Handling:** Proper exception management
- **Custom Encoding:** Handle special data types

## 💼 **Real-World Applications**
- **API Integration:** Processing REST API responses
- **Configuration Management:** Application settings and parameters
- **Data Exchange:** Inter-service communication
- **File Storage:** Persistent data storage in human-readable format

---

## 🚀 Next Steps
- Practice with real API responses
- Implement configuration systems
- Handle complex nested structures
- Optimize JSON processing for large datasets

---
