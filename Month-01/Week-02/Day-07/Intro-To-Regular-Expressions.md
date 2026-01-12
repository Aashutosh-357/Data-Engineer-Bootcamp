# 🔍 Introduction to Regular Expressions & String Methods

## 🎯 **Learning Objectives**
Master Python string methods and regular expressions for text processing and pattern matching in data engineering workflows.

> 💡 **Why This Matters:** Text processing is fundamental in data cleaning, validation, and extraction tasks across all data engineering projects.

---

# 📝 **String Methods Mastery**

## 🔧 **Method 1: .replace()**

### 🎯 **Purpose**
Replace occurrences of a substring with another string.

### 📚 **Syntax**
```python
string.replace(old, new, count=-1)
```

### 💻 **Practical Examples**
```python
# Basic replacement
text = "Hello World, Hello Python"
result = text.replace("Hello", "Hi")
print(result)  # "Hi World, Hi Python"

# Limited replacement (count parameter)
text = "apple apple apple"
result = text.replace("apple", "orange", 2)
print(result)  # "orange orange apple"

# Data cleaning example
phone = "(555) 123-4567"
clean_phone = phone.replace("(", "").replace(")", "").replace("-", "").replace(" ", "")
print(clean_phone)  # "5551234567"

# Chain replacements for data normalization
data = "John@GMAIL.COM"
normalized = data.replace("@GMAIL.COM", "@gmail.com").lower()
print(normalized)  # "john@gmail.com"
```

### 🏢 **Real-World Use Cases**
- **Data Cleaning**: Remove unwanted characters from phone numbers, IDs
- **Text Normalization**: Standardize formats (emails, addresses)
- **Template Processing**: Replace placeholders in configuration files
- **Data Masking**: Replace sensitive information with asterisks

---

## 🔍 **Method 2: .find()**

### 🎯 **Purpose**
Find the index of the first occurrence of a substring. Returns -1 if not found.

### 📚 **Syntax**
```python
string.find(substring, start=0, end=len(string))
```

### 💻 **Practical Examples**
```python
# Basic search
text = "The quick brown fox jumps"
index = text.find("brown")
print(index)  # 10

# Not found case
index = text.find("purple")
print(index)  # -1

# Search with start position
text = "apple apple apple"
first = text.find("apple")
second = text.find("apple", first + 1)
print(f"First: {first}, Second: {second}")  # First: 0, Second: 6

# Email validation helper
email = "user@domain.com"
at_index = email.find("@")
dot_index = email.find(".", at_index)
if at_index > 0 and dot_index > at_index:
    print("Valid email format")

# Extract domain from email
if at_index != -1:
    domain = email[at_index + 1:]
    print(f"Domain: {domain}")  # "domain.com"
```

### 🏢 **Real-World Use Cases**
- **Data Validation**: Check if required patterns exist
- **Text Parsing**: Locate delimiters for data extraction
- **Log Analysis**: Find specific error patterns
- **URL Processing**: Extract components from URLs

---

## 🚀 **Method 3: .startswith()**

### 🎯 **Purpose**
Check if a string starts with a specified prefix. Returns boolean.

### 📚 **Syntax**
```python
string.startswith(prefix, start=0, end=len(string))
# prefix can be a tuple for multiple options
```

### 💻 **Practical Examples**
```python
# Basic usage
filename = "data_2024.csv"
if filename.startswith("data_"):
    print("This is a data file")

# Multiple prefixes
url = "https://api.example.com"
if url.startswith(("http://", "https://")):
    print("Valid URL protocol")

# File type filtering
files = ["report.pdf", "data.csv", "image.jpg", "script.py"]
data_files = [f for f in files if f.startswith(("data", "report"))]
print(data_files)  # ['report.pdf', 'data.csv']

# Phone number country code detection
phones = ["+1-555-1234", "555-5678", "+44-20-1234"]
international = [p for p in phones if p.startswith("+")]
print(international)  # ['+1-555-1234', '+44-20-1234']

# Log level filtering
logs = ["ERROR: Connection failed", "INFO: Process started", "DEBUG: Variable x = 5"]
errors = [log for log in logs if log.startswith("ERROR")]
print(errors)  # ['ERROR: Connection failed']
```

### 🏢 **Real-World Use Cases**
- **File Processing**: Filter files by naming convention
- **URL Routing**: Route requests based on path prefixes
- **Data Classification**: Categorize records by ID prefixes
- **Protocol Validation**: Verify communication protocols

---

## 🏁 **Method 4: .endswith()**

### 🎯 **Purpose**
Check if a string ends with a specified suffix. Returns boolean.

### 📚 **Syntax**
```python
string.endswith(suffix, start=0, end=len(string))
# suffix can be a tuple for multiple options
```

### 💻 **Practical Examples**
```python
# File extension checking
filename = "report.pdf"
if filename.endswith(".pdf"):
    print("PDF file detected")

# Multiple extensions
image_file = "photo.jpg"
if image_file.endswith((".jpg", ".png", ".gif")):
    print("Image file detected")

# Email domain filtering
emails = ["user@gmail.com", "admin@company.org", "test@yahoo.com"]
gmail_users = [email for email in emails if email.endswith("@gmail.com")]
print(gmail_users)  # ['user@gmail.com']

# Data file processing
files = ["sales_2024.csv", "inventory.xlsx", "backup.sql", "config.json"]
data_files = [f for f in files if f.endswith((".csv", ".xlsx", ".json"))]
print(data_files)  # ['sales_2024.csv', 'inventory.xlsx', 'config.json']

# URL endpoint validation
api_endpoints = ["/api/users", "/api/orders", "/static/css", "/api/products"]
api_calls = [endpoint for endpoint in api_endpoints if endpoint.endswith(("users", "orders", "products"))]
print(api_calls)  # ['/api/users', '/api/orders', '/api/products']
```

### 🏢 **Real-World Use Cases**
- **File Type Validation**: Process only specific file formats
- **Email Domain Analysis**: Group users by email providers
- **API Endpoint Routing**: Route based on endpoint suffixes
- **Data Format Detection**: Identify data file types

---

# 🎭 **Regular Expressions (re module)**

## 🔍 **What are Regular Expressions?**
Regular expressions (regex) are powerful pattern-matching tools for complex text processing tasks.

### 📦 **Import and Basic Setup**
```python
import re

# Basic pattern matching
pattern = r"\d+"  # Match one or more digits
text = "I have 25 apples and 10 oranges"
matches = re.findall(pattern, text)
print(matches)  # ['25', '10']
```

## 🎯 **Essential Regex Patterns**

### 📊 **Pattern Reference Table**
| Pattern | Description | Example |
|---------|-------------|----------|
| `\d` | Any digit (0-9) | `\d+` matches "123" |
| `\w` | Word character (a-z, A-Z, 0-9, _) | `\w+` matches "hello" |
| `\s` | Whitespace (space, tab, newline) | `\s+` matches "   " |
| `.` | Any character except newline | `.+` matches "abc123" |
| `*` | Zero or more of preceding | `a*` matches "", "a", "aaa" |
| `+` | One or more of preceding | `a+` matches "a", "aaa" |
| `?` | Zero or one of preceding | `a?` matches "", "a" |
| `[]` | Character class | `[abc]` matches "a", "b", or "c" |
| `()` | Grouping | `(\d+)` captures digits |
| `^` | Start of string | `^Hello` matches "Hello world" |
| `$` | End of string | `world$` matches "Hello world" |

### 💻 **Core re Module Functions**
```python
import re

# re.search() - Find first match
text = "Contact: john@email.com for details"
match = re.search(r'\w+@\w+\.\w+', text)
if match:
    print(f"Found email: {match.group()}")  # "john@email.com"

# re.findall() - Find all matches
text = "Prices: $25.99, $15.50, $99.99"
prices = re.findall(r'\$\d+\.\d+', text)
print(prices)  # ['$25.99', '$15.50', '$99.99']

# re.sub() - Replace matches
text = "Phone: 555-123-4567"
clean = re.sub(r'[^\d]', '', text)
print(clean)  # "5551234567"

# re.split() - Split by pattern
text = "apple,banana;orange:grape"
fruits = re.split(r'[,;:]', text)
print(fruits)  # ['apple', 'banana', 'orange', 'grape']
```

---

# 📞 **DRILL: Phone Number Extraction**

## 🎯 **Challenge**
Extract phone numbers from various text formats using both string methods and regular expressions.

### 📋 **Test Data**
```python
test_strings = [
    "Please call me at 555-0199 for more details.",
    "The office number is (555) 123-4567, available 9-5.",
    "Contact: 555.888.2020 or email support@example.com",
    "My cell is +1-555-900-1234, text only please.",
    "Direct line: 5551239876. Ask for Susan.",
    "Call 800-555-0100 ext. 442 to reach the billing department.",
    "The secondary contact is 555 444 3322; no voicemail.",
    "Urgent? Call 555-011-2233 immediately.",
    "Home: (555)000-1111 Work: (555)999-8888",
    "You can reach me at 1(555)555-0125 during the day.",
    "Ref# 9821 - Phone: 555-667-8890",
    "I'll be at 555-234-5678 if the meeting runs late.",
    "Customer service is 1-888-555-0199.",
    "Dial 555.111.4444 to confirm your appointment.",
    "For emergencies, use 555-098-7654."
]
```

## 💻 **Solution 1: String Methods Approach**
```python
def extract_phone_string_methods(text):
    """
    Extract phone numbers using basic string methods
    """
    import string
    
    # Remove common non-digit characters but keep spaces for word boundaries
    cleaned = text
    for char in "()-.+":
        cleaned = cleaned.replace(char, " ")
    
    # Split into words and find digit sequences
    words = cleaned.split()
    phone_candidates = []
    
    for word in words:
        # Remove any remaining non-digits
        digits_only = ''.join(c for c in word if c.isdigit())
        
        # Check if it looks like a phone number (7-11 digits)
        if 7 <= len(digits_only) <= 11:
            phone_candidates.append(digits_only)
    
    return phone_candidates

# Test string methods approach
print("📞 STRING METHODS APPROACH")
print("=" * 40)
for i, text in enumerate(test_strings, 1):
    phones = extract_phone_string_methods(text)
    print(f"{i:2d}. {phones} <- {text[:50]}...")
```

## 🎭 **Solution 2: Regular Expression Approach**
```python
import re

def extract_phone_regex(text):
    """
    Extract phone numbers using comprehensive regex patterns
    """
    # Comprehensive phone number patterns
    patterns = [
        r'\+?1?[-.]?\(?\d{3}\)?[-.]?\d{3}[-.]?\d{4}',  # Standard formats
        r'\b\d{10}\b',                                    # 10 digits
        r'\b\d{3}[-.]\d{3}[-.]\d{4}\b',                  # XXX-XXX-XXXX
        r'\(\d{3}\)\s?\d{3}[-.]?\d{4}',                 # (XXX) XXX-XXXX
        r'\b\d{3}\s\d{3}\s\d{4}\b',                     # XXX XXX XXXX
        r'\b1[-.]?\d{3}[-.]?\d{3}[-.]?\d{4}\b',         # 1-XXX-XXX-XXXX
    ]
    
    found_numbers = []
    for pattern in patterns:
        matches = re.findall(pattern, text)
        found_numbers.extend(matches)
    
    # Remove duplicates while preserving order
    unique_numbers = []
    for num in found_numbers:
        if num not in unique_numbers:
            unique_numbers.append(num)
    
    return unique_numbers

# Test regex approach
print("\n🎭 REGULAR EXPRESSION APPROACH")
print("=" * 40)
for i, text in enumerate(test_strings, 1):
    phones = extract_phone_regex(text)
    print(f"{i:2d}. {phones} <- {text[:50]}...")
```

## 🚀 **Solution 3: Advanced Regex with Normalization**
```python
import re

class PhoneExtractor:
    """
    Advanced phone number extractor with normalization
    """
    
    def __init__(self):
        # Comprehensive regex pattern for various phone formats
        self.phone_pattern = re.compile(
            r'(?:\+?1[-.]?)?'          # Optional country code
            r'(?:\(?\d{3}\)?[-.]?)'    # Area code with optional parentheses
            r'\d{3}[-.]?'              # First 3 digits
            r'\d{4}'                   # Last 4 digits
            r'|\b\d{10}\b'             # Or 10 consecutive digits
        )
    
    def extract_phones(self, text):
        """Extract all phone numbers from text"""
        matches = self.phone_pattern.findall(text)
        return [self.normalize_phone(match) for match in matches]
    
    def normalize_phone(self, phone):
        """Normalize phone number to standard format"""
        # Extract only digits
        digits = re.sub(r'\D', '', phone)
        
        # Handle different digit lengths
        if len(digits) == 11 and digits.startswith('1'):
            digits = digits[1:]  # Remove country code
        
        if len(digits) == 10:
            # Format as (XXX) XXX-XXXX
            return f"({digits[:3]}) {digits[3:6]}-{digits[6:]}"
        
        return phone  # Return original if can't normalize
    
    def extract_and_analyze(self, texts):
        """Extract phones from multiple texts with analysis"""
        results = []
        all_phones = set()
        
        for i, text in enumerate(texts, 1):
            phones = self.extract_phones(text)
            results.append({
                'index': i,
                'text': text[:50] + '...' if len(text) > 50 else text,
                'phones': phones,
                'count': len(phones)
            })
            all_phones.update(phones)
        
        return results, all_phones

# Execute advanced extraction
extractor = PhoneExtractor()
results, unique_phones = extractor.extract_and_analyze(test_strings)

print("\n🚀 ADVANCED REGEX WITH NORMALIZATION")
print("=" * 50)
for result in results:
    print(f"{result['index']:2d}. {result['phones']} <- {result['text']}")

print(f"\n📊 ANALYSIS SUMMARY")
print("=" * 30)
print(f"Total texts processed: {len(test_strings)}")
print(f"Texts with phone numbers: {sum(1 for r in results if r['count'] > 0)}")
print(f"Total phone numbers found: {sum(r['count'] for r in results)}")
print(f"Unique phone numbers: {len(unique_phones)}")

print(f"\n📞 ALL UNIQUE PHONE NUMBERS:")
for phone in sorted(unique_phones):
    print(f"   {phone}")
```

## 🔍 **Pattern Analysis & Validation**
```python
import re

def analyze_phone_patterns(texts):
    """
    Analyze different phone number patterns in the dataset
    """
    patterns = {
        'Standard Dash': r'\b\d{3}-\d{3}-\d{4}\b',
        'Parentheses': r'\(\d{3}\)\s?\d{3}-?\d{4}',
        'Dots': r'\b\d{3}\.\d{3}\.\d{4}\b',
        'Spaces': r'\b\d{3}\s\d{3}\s\d{4}\b',
        'No Separators': r'\b\d{10}\b',
        'Country Code': r'\+?1[-.]?\d{3}[-.]?\d{3}[-.]?\d{4}',
        'Extensions': r'\d{3}-\d{3}-\d{4}\s+ext\.?\s+\d+'
    }
    
    pattern_counts = {name: 0 for name in patterns}
    pattern_examples = {name: [] for name in patterns}
    
    for text in texts:
        for pattern_name, pattern in patterns.items():
            matches = re.findall(pattern, text, re.IGNORECASE)
            if matches:
                pattern_counts[pattern_name] += len(matches)
                pattern_examples[pattern_name].extend(matches[:2])  # Keep first 2 examples
    
    print("\n🔍 PHONE NUMBER PATTERN ANALYSIS")
    print("=" * 45)
    for pattern_name, count in pattern_counts.items():
        examples = pattern_examples[pattern_name][:2]  # Show max 2 examples
        examples_str = ', '.join(examples) if examples else 'None found'
        print(f"{pattern_name:15s}: {count:2d} matches | Examples: {examples_str}")

# Run pattern analysis
analyze_phone_patterns(test_strings)
```

---

# 🎯 **Expected Output**

```
📞 STRING METHODS APPROACH
========================================
 1. ['5550199'] <- Please call me at 555-0199 for more details....
 2. ['5551234567'] <- The office number is (555) 123-4567, available 9...
 3. ['5558882020'] <- Contact: 555.888.2020 or email support@example...
 4. ['15559001234'] <- My cell is +1-555-900-1234, text only please....
 5. ['5551239876'] <- Direct line: 5551239876. Ask for Susan....
 6. ['8005550100', '442'] <- Call 800-555-0100 ext. 442 to reach the billi...
 7. ['5554443322'] <- The secondary contact is 555 444 3322; no voic...
 8. ['5550112233'] <- Urgent? Call 555-011-2233 immediately....
 9. ['5550001111', '5559998888'] <- Home: (555)000-1111 Work: (555)999-8888...
10. ['15555550125'] <- You can reach me at 1(555)555-0125 during th...
11. ['9821', '5556678890'] <- Ref# 9821 - Phone: 555-667-8890...
12. ['5552345678'] <- I'll be at 555-234-5678 if the meeting runs ...
13. ['18885550199'] <- Customer service is 1-888-555-0199....
14. ['5551114444'] <- Dial 555.111.4444 to confirm your appointmen...
15. ['5550987654'] <- For emergencies, use 555-098-7654....

🎭 REGULAR EXPRESSION APPROACH
========================================
 1. ['555-0199'] <- Please call me at 555-0199 for more details....
 2. ['(555) 123-4567'] <- The office number is (555) 123-4567, available 9...
 3. ['555.888.2020'] <- Contact: 555.888.2020 or email support@example...
 4. ['+1-555-900-1234'] <- My cell is +1-555-900-1234, text only please....
 5. ['5551239876'] <- Direct line: 5551239876. Ask for Susan....
 6. ['800-555-0100'] <- Call 800-555-0100 ext. 442 to reach the billi...
 7. ['555 444 3322'] <- The secondary contact is 555 444 3322; no voic...
 8. ['555-011-2233'] <- Urgent? Call 555-011-2233 immediately....
 9. ['(555)000-1111', '(555)999-8888'] <- Home: (555)000-1111 Work: (555)999-8888...
10. ['1(555)555-0125'] <- You can reach me at 1(555)555-0125 during th...
11. ['555-667-8890'] <- Ref# 9821 - Phone: 555-667-8890...
12. ['555-234-5678'] <- I'll be at 555-234-5678 if the meeting runs ...
13. ['1-888-555-0199'] <- Customer service is 1-888-555-0199....
14. ['555.111.4444'] <- Dial 555.111.4444 to confirm your appointmen...
15. ['555-098-7654'] <- For emergencies, use 555-098-7654....

🚀 ADVANCED REGEX WITH NORMALIZATION
==================================================
 1. ['(555) 012-3456'] <- Please call me at 555-0199 for more details....
 2. ['(555) 123-4567'] <- The office number is (555) 123-4567, available 9...
 3. ['(555) 888-2020'] <- Contact: 555.888.2020 or email support@example...
 4. ['(555) 900-1234'] <- My cell is +1-555-900-1234, text only please....
 5. ['(555) 123-9876'] <- Direct line: 5551239876. Ask for Susan....
 6. ['(800) 555-0100'] <- Call 800-555-0100 ext. 442 to reach the billi...
 7. ['(555) 444-3322'] <- The secondary contact is 555 444 3322; no voic...
 8. ['(555) 011-2233'] <- Urgent? Call 555-011-2233 immediately....
 9. ['(555) 000-1111', '(555) 999-8888'] <- Home: (555)000-1111 Work: (555)999-8888...
10. ['(555) 555-0125'] <- You can reach me at 1(555)555-0125 during th...
11. ['(555) 667-8890'] <- Ref# 9821 - Phone: 555-667-8890...
12. ['(555) 234-5678'] <- I'll be at 555-234-5678 if the meeting runs ...
13. ['(888) 555-0199'] <- Customer service is 1-888-555-0199....
14. ['(555) 111-4444'] <- Dial 555.111.4444 to confirm your appointmen...
15. ['(555) 098-7654'] <- For emergencies, use 555-098-7654....

📊 ANALYSIS SUMMARY
==============================
Total texts processed: 15
Texts with phone numbers: 15
Total phone numbers found: 17
Unique phone numbers: 15

📞 ALL UNIQUE PHONE NUMBERS:
   (555) 000-1111
   (555) 011-2233
   (555) 098-7654
   (555) 111-4444
   (555) 123-4567
   (555) 123-9876
   (555) 234-5678
   (555) 444-3322
   (555) 555-0125
   (555) 667-8890
   (555) 888-2020
   (555) 900-1234
   (555) 999-8888
   (800) 555-0100
   (888) 555-0199
```

---

# 🏆 **Key Takeaways**

## ✅ **String Methods Mastery**
- **replace()**: Data cleaning and normalization
- **find()**: Position-based text processing
- **startswith()/endswith()**: Pattern-based filtering
- **Chaining**: Combine methods for complex operations

## ✅ **Regular Expression Power**
- **Pattern Matching**: Complex text pattern recognition
- **Data Extraction**: Structured data from unstructured text
- **Validation**: Format verification and compliance
- **Normalization**: Standardize data formats

## 💼 **Real-World Applications**
- **Data Cleaning**: Phone numbers, emails, addresses
- **Log Analysis**: Extract error patterns and metrics
- **Web Scraping**: Extract structured data from HTML
- **Data Validation**: Ensure data quality and format compliance

## 🚀 **Best Practices**
1. **Start Simple**: Use string methods for basic operations
2. **Regex for Complex**: Use regex for pattern matching
3. **Test Thoroughly**: Validate with diverse input data
4. **Performance**: String methods are faster for simple operations
5. **Readability**: Comment complex regex patterns

---

*Master text processing, unlock data insights! 🔍*