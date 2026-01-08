Here is a summary of the chapter on Flow Control, preserving all vital information, examples, and code implementations.

### **Flow Control Basics**
Flow control allows a program to decide which Python instructions to execute based on conditions, rather than just running strictly from top to bottom. This mirrors flowcharts where diamonds represent decisions and arrows represent the path of execution.

### **Boolean Values**
The Boolean data type has only two values: `True` and `False`. They must be capitalized and do not use quotes.
*   **Correct:** `spam = True`
*   **Incorrect:** `spam = true` (NameError) or `True = 2 + 2` (SyntaxError).

### **Comparison Operators**
Comparison operators compare two values and evaluate to a Boolean value.

| Operator | Meaning |
| :--- | :--- |
| `==` | Equal to |
| `!=` | Not equal to |
| `<` | Less than |
| `>` | Greater than |
| `<=` | Less than or equal to |
| `>=` | Greater than or equal to |

**Important Distinction:**
*   `==` checks if values are the same.
*   `=` assigns a value to a variable.

**Note on Types:**
*   Integers/floats and strings are never equal (`42 == '42'` is `False`).
*   `<`, `>`, `<=`, `>=` work properly with numbers, but not always with mixed types.

### **Boolean Operators**
Used to compare Boolean values.
1.  **`and`**: True only if **both** values are True.
2.  **`or`**: True if **at least one** value is True.
3.  **`not`**: Unary operator; flips the value (True becomes False).

**Order of Operations:**
1.  Math operators
2.  Comparison operators
3.  `not`
4.  `and`
5.  `or`

### **Elements of Flow Control**
*   **Condition:** An expression that evaluates to `True` or `False`.
*   **Block:** A group of code lines.
    *   Begins when indentation increases.
    *   Ends when indentation decreases to zero or a containing block's level.

### **Flow Control Statements**

#### **1. `if` Statements**
Executes the clause (indented block) if the condition is `True`.

```python
if name == 'Alice':
    print('Hi, Alice.')
```

#### **2. `else` Statements**
Executed only if the `if` statement's condition is `False`.

```python
if name == 'Alice':
    print('Hi, Alice.')
else:
    print('Hello, stranger.')
```

#### **3. `elif` Statements**
Stands for "Else If". It checks a condition only if previous conditions were `False`. Order matters; once a condition is met, the rest of the chain is skipped.

**Example: `vampire.py`**
```python
name = 'Carol'
age = 3000
if name == 'Alice':
    print('Hi, Alice.')
elif age < 12:
    print('You are not Alice, kiddo.')
elif age > 2000:
    print('Unlike you, Alice is not an undead, immortal vampire.')
elif age > 100:
    print('You are not Alice, grannie.')
```

**Bug warning:** If you check `age > 100` before `age > 2000`, the program will catch the first True condition and skip the specific vampire check.

**Example: `littleKid.py`** (Using `if`, `elif`, and `else`)
```python
name = 'Carol'
age = 3000
if name == 'Alice':
    print('Hi, Alice.')
elif age < 12:
    print('You are not Alice, kiddo.')
else:
    print('You are neither Alice nor a little kid.')
```

### **Loops**

#### **1. `while` Loops**
Repeats the code block as long as the condition is `True`.

**Comparison: `if` vs `while`**
*   `if`: Runs once.
    ```python
    spam = 0
    if spam < 5:
        print('Hello, world.')
        spam = spam + 1
    ```
*   `while`: Runs five times (until `spam` is 5).
    ```python
    spam = 0
    while spam < 5:
        print('Hello, world.')
        spam = spam + 1
    ```

**Example: `yourName.py`** (Input validation)
```python
name = ''
while name != 'your name':
    print('Please type your name.')
    name = input()
print('Thank you!')
```

#### **Loop Control Statements**
*   **`break`**: Immediately exits the loop.
*   **`continue`**: Immediately jumps back to the start of the loop to re-evaluate the condition.

**Example: `yourName2.py`** (Using `break` in an infinite loop)
```python
while True:
    print('Please type your name.')
    name = input()
    if name == 'your name':
        break
print('Thank you!')
```

**Example: `swordfish.py`** (Using `continue`)
```python
while True:
    print('Who are you?')
    name = input()
    if name != 'Joe':
        continue
    print('Hello, Joe. What is the password? (It is a fish.)')
    password = input()
    if password == 'swordfish':
        break
print('Access granted.')
```

**"Truthy" and "Falsey" Values:**
`0`, `0.0`, and `''` (empty string) count as `False`. All other values count as `True`.

#### **2. `for` Loops and `range()`**
Used to execute code a specific number of times.

**Example: `fiveTimes.py`**
```python
print('My name is')
for i in range(5):
    print('Jimmy Five Times (' + str(i) + ')')
```

**Calculating Sum (Gauss example):**
```python
total = 0
for num in range(101):
    total = total + num
print(total)
```

**Equivalent `while` Loop:**
```python
print('My name is')
i = 0
while i < 5:
    print('Jimmy Five Times (' + str(i) + ')')
    i = i + 1
```

**`range()` Arguments:**
1.  `range(5)`: 0 to 4.
2.  `range(12, 16)`: Starts at 12, stops at 15.
3.  `range(0, 10, 2)`: Counts 0 to 8, stepping by 2.
4.  `range(5, -1, -1)`: Counts down from 5 to 0.

### **Importing Modules**
To use built-in modules like `random` or `sys`, use `import`.

**Example: `printRandom.py`**
```python
import random
for i in range(5):
    print(random.randint(1, 10))
```

*   **`from import`**: `from random import *` allows calling `randint(1, 10)` without the `random.` prefix.
*   **Naming warning**: Never name your script the same as a module (e.g., do not name a file `random.py`).

### **Terminating Execution**
`sys.exit()` immediately stops the program.

**Example: `exitExample.py`**
```python
import sys

while True:
    print('Type exit to exit.')
    response = input()
    if response == 'exit':
        sys.exit()
    print('You typed ' + response + '.')
```

### **Major Example Programs**

#### **1. Guess the Number (`guessTheNumber.py`)**
A game where the user guesses a random number between 1 and 20.

```python
# This is a guess the number game.
import random
secretNumber = random.randint(1, 20)
print('I am thinking of a number between 1 and 20.')

# Ask the player to guess 6 times.
for guessesTaken in range(1, 7):
    print('Take a guess.')
    guess = int(input())

    if guess < secretNumber:
        print('Your guess is too low.')
    elif guess > secretNumber:
        print('Your guess is too high.')
    else:
        break    # This condition is the correct guess!

if guess == secretNumber:
    print('Good job! You guessed my number in ' + str(guessesTaken) + ' guesses!')
else:
    print('Nope. The number I was thinking of was ' + str(secretNumber))
```

#### **2. Rock, Paper, Scissors (`rpsGame.py`)**
A complete game implementation using loops, conditions, and random choices.

```python
import random, sys

print('ROCK, PAPER, SCISSORS')

# These variables keep track of the number of wins, losses, and ties.
wins = 0
losses = 0
ties = 0

while True: # The main game loop.
    print('%s Wins, %s Losses, %s Ties' % (wins, losses, ties))
    while True: # The player input loop.
        print('Enter your move: (r)ock (p)aper (s)cissors or (q)uit')
        playerMove = input()
        if playerMove == 'q':
            sys.exit() # Quit the program.
        if playerMove == 'r' or playerMove == 'p' or playerMove == 's':
            break # Break out of the player input loop.
        print('Type one of r, p, s, or q.')

    # Display what the player chose:
    if playerMove == 'r':
        print('ROCK versus...')
    elif playerMove == 'p':
        print('PAPER versus...')
    elif playerMove == 's':
        print('SCISSORS versus...')

    # Display what the computer chose:
    randomNumber = random.randint(1, 3)
    if randomNumber == 1:
        computerMove = 'r'
        print('ROCK')
    elif randomNumber == 2:
        computerMove = 'p'
        print('PAPER')
    elif randomNumber == 3:
        computerMove = 's'
        print('SCISSORS')

    # Display and record the win/loss/tie:
    if playerMove == computerMove:
        print('It is a tie!')
        ties = ties + 1
    elif playerMove == 'r' and computerMove == 's':
        print('You win!')
        wins = wins + 1
    elif playerMove == 'p' and computerMove == 'r':
        print('You win!')
        wins = wins + 1
    elif playerMove == 's' and computerMove == 'p':
        print('You win!')
        wins = wins + 1
    elif playerMove == 'r' and computerMove == 'p':
        print('You lose!')
        losses = losses + 1
    elif playerMove == 'p' and computerMove == 's':
        print('You lose!')
        losses = losses + 1
    elif playerMove == 's' and computerMove == 'r':
        print('You lose!')
        losses = losses + 1
```

### **Practice Questions**

1.  What are the two values of the Boolean data type? How do you write them?
2.  What are the three Boolean operators?
3.  Write out the truth tables of each Boolean operator (that is, every possible combination of Boolean values for the operator and what they evaluate to).
4.  What do the following expressions evaluate to?
    *   `(5 > 4) and (3 == 5)`
    *   `not (5 > 4)`
    *   `(5 > 4) or (3 == 5)`
    *   `not ((5 > 4) or (3 == 5))`
    *   `(True and True) and (True == False)`
    *   `(not False) or (not True)`
5.  What are the six comparison operators?
6.  What is the difference between the equal to operator and the assignment operator?
7.  Explain what a condition is and where you would use one.
8.  Identify the three blocks in this code:
    ```python
    spam = 0
    if spam == 10:
        print('eggs')
        if spam > 5:
            print('bacon')
        else:
            print('ham')
        print('spam')
    print('spam')
    ```
9.  Write code that prints Hello if 1 is stored in spam, prints Howdy if 2 is stored in spam, and prints Greetings! if anything else is stored in spam.
10. What keys can you press if your program is stuck in an infinite loop?
11. What is the difference between break and continue?
12. What is the difference between range(10), range(0, 10), and range(0, 10, 1) in a for loop?
13. Write a short program that prints the numbers 1 to 10 using a for loop. Then write an equivalent program that prints the numbers 1 to 10 using a while loop.
14. If you had a function named bacon() inside a module named spam, how would you call it after importing spam?


Here is a comprehensive summary of Chapter 5, organized by concept to ensure no vital information is lost.

### 1. Raising Exceptions
While Python raises exceptions automatically for errors, you can raise your own to handle invalid code usage or bad data.

*   **The `raise` Statement:** Used to trigger an exception manually.
    *   **Syntax:** `raise Exception('Error message here')`
    *   **Usage:** Often placed inside a function to validate input. If the input is invalid, the function raises an exception.
*   **Handling Custom Exceptions:**
    *   Use `try` and `except` blocks in the code *calling* the function.
    *   **Syntax for capturing messages:** `except Exception as err:` allows you to convert the error object to a string using `str(err)` to display the custom message.

### 2. Assertions
Assertions are "sanity checks" designed to detect programmer errors during development. They ensure that a condition is True; if not, the program crashes immediately ("fail fast").

*   **The `assert` Statement:**
    *   **Syntax:** `assert condition, 'Error message if False'`
    *   **Logic:** If the condition is `True`, nothing happens. If `False`, it raises an `AssertionError`.
*   **Key Rules:**
    *   **Do not use `try/except`:** Assertions are for crashes. They indicate a bug in the code that must be fixed, not a user error (like "File not found") that should be handled gracefully.
    *   **Development only:** Assertions are for the programmer, not the final user.

### 3. Logging
Logging is a superior alternative to using `print()` for debugging. It allows you to track variable values and execution flow without cluttering the final program output or requiring manual cleanup.

*   **Setup:** You must import the module and configure it at the start of your script:
    ```python
    import logging
    logging.basicConfig(level=logging.DEBUG, format=' %(asctime)s -  %(levelname)s -  %(message)s')
    ```
*   **Logging to a File:** To save logs to a text file instead of the screen, add `filename='log.txt'` to the `basicConfig` parameters.
*   **Logging Levels:** There are five levels of importance. You can set the minimum level in `basicConfig` to filter out lower-priority messages.
    1.  **DEBUG:** Small details, useful only for diagnosing problems.
    2.  **INFO:** General confirmation that things are working.
    3.  **WARNING:** Potential issues that won't stop the program yet.
    4.  **ERROR:** The program failed to do something.
    5.  **CRITICAL:** Fatal error; program stops.
*   **Disabling Logs:** You can disable all logging at a specific level and below using `logging.disable(logging.LEVEL)`.
    *   *Example:* `logging.disable(logging.CRITICAL)` disables **all** log messages.

### 4. The Debugger
The debugger (available in editors like Mu) allows you to execute code one line at a time to inspect variable values in real-time.

**Debugger Controls:**
*   **Continue:** Runs the program normally until it hits a *breakpoint* or terminates.
*   **Step In:** Executes the next line. If the line is a function call, the debugger jumps **inside** that function.
*   **Step Over:** Executes the next line. If the line is a function call, the debugger executes the function at full speed and pauses **after** the function returns (it does not go inside).
*   **Step Out:** If you are inside a function, this executes the rest of the function at full speed and pauses immediately after returning to the calling code.
*   **Stop:** Terminates the program immediately.

**Breakpoints:**
*   A breakpoint marks a specific line of code where the debugger must pause.
*   It allows you to skip clicking "Step Over" repeatedly for loops or long code blocks.
*   In Mu, you set a breakpoint by clicking the line number (a red dot appears).