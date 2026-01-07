Here is a comprehensive summary of Chapter 6: Manipulating Strings, including all examples, code implementations, and practice questions.

### **String Basics and Syntax**
*   **String Literals:** Strings can be enclosed in single quotes (`'`) or double quotes (`"`). Using double quotes allows the use of single quotes inside the string without escaping (e.g., `"That is Alice's cat."`).
*   **Escape Characters:** Used to insert characters that are illegal in a string.
    *   `\'`: Single quote
    *   `\"`: Double quote
    *   `\t`: Tab
    *   `\n`: Newline
    *   `\\`: Backslash
*   **Raw Strings:** Prefixed with `r`. Ignores escape characters (useful for file paths and regex).
    ```python
    print(r'That is Carol\'s cat.') # Prints: That is Carol\'s cat.
    ```
*   **Multiline Strings:** Enclosed by triple quotes (`'''` or `"""`). They preserve newlines and tabs literally. Often used for multiline comments.

### **Indexing, Slicing, and Operators**
*   **Indexing/Slicing:** Works identical to lists.
    *   `spam[0]`: First character.
    *   `spam[0:5]`: Characters from index 0 up to (but not including) 5.
    *   Strings are immutable; slicing creates a new string copy.
*   **Membership:** `in` and `not in` check if a specific substring exists within another string.

### **String Interpolation**
*   **`%s` Formatting:** Old style. ` 'My name is %s.' % (name) `
*   **f-strings (Python 3.6+):** Prefixed with `f`. Expressions inside braces `{}` are evaluated.
    ```python
    name = 'Al'
    age = 4000
    f'My name is {name}. Next year I will be {age + 1}.'
    # Result: 'My name is Al. Next year I will be 4001.'
    ```

### **Useful String Methods**
Methods return new string values; they do not modify the original string in place.

#### **Case Manipulation**
*   `upper()`: Converts to uppercase.
*   `lower()`: Converts to lowercase. (Useful for case-insensitive comparisons).
*   `isupper()` / `islower()`: Returns `True` if all letters are uppercase or lowercase, respectively.

#### **Validation (isX Methods)**
These return `True` if the string matches the criteria and is not blank:
*   `isalpha()`: Letters only.
*   `isalnum()`: Letters and numbers.
*   `isdecimal()`: Numbers only.
*   `isspace()`: Whitespace only (spaces, tabs, newlines).
*   `istitle()`: Title case (Word capitalized, following letters lowercase).

**Example: `validateInput.py`**
```python
while True:
    print('Enter your age:')
    age = input()
    if age.isdecimal():
        break
    print('Please enter a number for your age.')

while True:
    print('Select a new password (letters and numbers only):')
    password = input()
    if password.isalnum():
        break
    print('Passwords can only have letters and numbers.')
```

#### **Start and End Checks**
*   `startswith()`: Returns `True` if string starts with the argument.
*   `endswith()`: Returns `True` if string ends with the argument.

#### **Join, Split, and Partition**
*   `join()`: Called on the separator string, takes a list of strings.
    ```python
    ', '.join(['cats', 'rats', 'bats']) # Returns 'cats, rats, bats'
    ```
*   `split()`: Called on the string, returns a list. Default splits by whitespace.
    ```python
    'My name is Simon'.split() # Returns ['My', 'name', 'is', 'Simon']
    ```
*   `partition()`: Splits string at the first occurrence of the separator. Returns a tuple: `(before, separator, after)`.

#### **Text Alignment**
Used to justify text within a given width. Optional second argument specifies the fill character.
*   `rjust()`: Right-justify.
*   `ljust()`: Left-justify.
*   `center()`: Center text.

**Example: `picnicTable.py`**
```python
def printPicnic(itemsDict, leftWidth, rightWidth):
    print('PICNIC ITEMS'.center(leftWidth + rightWidth, '-'))
    for k, v in itemsDict.items():
        print(k.ljust(leftWidth, '.') + str(v).rjust(rightWidth))

picnicItems = {'sandwiches': 4, 'apples': 12, 'cups': 4, 'cookies': 8000}
printPicnic(picnicItems, 12, 5)
printPicnic(picnicItems, 20, 6)
```

#### **Trimming Whitespace**
*   `strip()`: Removes whitespace from both ends.
*   `lstrip()` / `rstrip()`: Removes whitespace from left or right ends.
*   Optional string argument removes specific characters (e.g., `spam.strip('ampS')`).

### **Numeric Values of Characters**
*   `ord(char)`: Returns the Unicode code point (int) of a character.
*   `chr(int)`: Returns the character of a Unicode code point.

### **Clipboard Access (`pyperclip` Module)**
Requires installation (`pip install pyperclip`).
*   `pyperclip.copy(text)`: Sends text to clipboard.
*   `pyperclip.paste()`: Retrieves text from clipboard.

---

### **Project 1: Multi-Clipboard (`mclip.py`)**
A program to store multiple phrases and copy them to the clipboard based on a command-line keyword.

**Code:**
```python
#! python3
# mclip.py - A multi-clipboard program.

TEXT = {'agree': """Yes, I agree. That sounds fine to me.""",
        'busy': """Sorry, can we do this later this week or next week?""",
        'upsell': """Would you consider making this a monthly donation?"""}

import sys, pyperclip
if len(sys.argv) < 2:
    print('Usage: py mclip.py [keyphrase] - copy phrase text')
    sys.exit()

keyphrase = sys.argv[1]    # first command line arg is the keyphrase

if keyphrase in TEXT:
    pyperclip.copy(TEXT[keyphrase])
    print('Text for ' + keyphrase + ' copied to clipboard.')
else:
    print('There is no text for ' + keyphrase)
```
*Note: On Windows, a batch file (`mclip.bat`) can be created to run this easily from the Run window.*

### **Project 2: Adding Bullets to Wiki Markup (`bulletPointAdder.py`)**
Gets text from the clipboard, adds a star and space to the start of every line, and pastes it back to the clipboard.

**Code:**
```python
#! python3
# bulletPointAdder.py - Adds Wikipedia bullet points to the start
# of each line of text on the clipboard.

import pyperclip
text = pyperclip.paste()

# Separate lines and add stars.
lines = text.split('\n')
for i in range(len(lines)):    # loop through all indexes for "lines" list
    lines[i] = '* ' + lines[i] # add star to each string in "lines" list
text = '\n'.join(lines)
pyperclip.copy(text)
```

### **Short Program: Pig Latin (`pigLat.py`)**
Translates English to Pig Latin (moving initial consonants to end + "ay", or adding "yay" if it starts with a vowel). Handles punctuation and capitalization.

**Code:**
```python
# English to Pig Latin
print('Enter the English message to translate into Pig Latin:')
message = input()

VOWELS = ('a', 'e', 'i', 'o', 'u', 'y')

pigLatin = [] # A list of the words in Pig Latin.
for word in message.split():
    # Separate the non-letters at the start of this word:
    prefixNonLetters = ''
    while len(word) > 0 and not word[0].isalpha():
        prefixNonLetters += word[0]
        word = word[1:]
    if len(word) == 0:
        pigLatin.append(prefixNonLetters)
        continue

    # Separate the non-letters at the end of this word:
    suffixNonLetters = ''
    while not word[-1].isalpha():
        suffixNonLetters = word[-1] + suffixNonLetters
        word = word[:-1]

    # Remember if the word was in uppercase or title case.
    wasUpper = word.isupper()
    wasTitle = word.istitle()

    word = word.lower() # Make the word lowercase for translation.

    # Separate the consonants at the start of this word:
    prefixConsonants = ''
    while len(word) > 0 and not word[0] in VOWELS:
        prefixConsonants += word[0]
        word = word[1:]

    # Add the Pig Latin ending to the word:
    if prefixConsonants != '':
        word += prefixConsonants + 'ay'
    else:
        word += 'yay'

    # Set the word back to uppercase or title case:
    if wasUpper:
        word = word.upper()
    if wasTitle:
        word = word.title()

    # Add the non-letters back to the start or end of the word.
    pigLatin.append(prefixNonLetters + word + suffixNonLetters)

# Join all the words back together into a single string:
print(' '.join(pigLatin))
```

---

### **Practice Questions**

1.  What are escape characters?
2.  What do the `\n` and `\t` escape characters represent?
3.  How can you put a `\` backslash character in a string?
4.  The string value `"Howl's Moving Castle"` is a valid string. Why isn’t it a problem that the single quote character in the word `Howl's` isn’t escaped?
5.  If you don’t want to put `\n` in your string, how can you write a string with newlines in it?
6.  What do the following expressions evaluate to?
    *   `'Hello, world!'[1]`
    *   `'Hello, world!'[0:5]`
    *   `'Hello, world!'[:5]`
    *   `'Hello, world!'[3:]`
7.  What do the following expressions evaluate to?
    *   `'Hello'.upper()`
    *   `'Hello'.upper().isupper()`
    *   `'Hello'.upper().lower()`
8.  What do the following expressions evaluate to?
    *   `'Remember, remember, the fifth of November.'.split()`
    *   `'-'.join('There can be only one.'.split())`
9.  What string methods can you use to right-justify, left-justify, and center a string?
10. How can you trim whitespace characters from the beginning or end of a string?