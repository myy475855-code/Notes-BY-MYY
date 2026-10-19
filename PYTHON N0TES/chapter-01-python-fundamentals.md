# Chapter 1: Python Fundamentals

> Learn the building blocks of Python: how to run code, store data, work with text and numbers, make comparisons, and take input from users.

## Table of Contents

- [1.1 Python HOME](#11-python-home)
- [1.2 Python Introduction](#12-python-introduction)
- [1.3 Python Get Started](#13-python-get-started)
- [1.4 Python Syntax](#14-python-syntax)
- [1.5 Python Output](#15-python-output)
- [1.6 Python Comments](#16-python-comments)
- [1.7 Python Variables](#17-python-variables)
- [1.8 Python Data Types](#18-python-data-types)
- [1.9 Python Numbers](#19-python-numbers)
- [1.10 Python Casting](#110-python-casting)
- [1.11 Python Strings](#111-python-strings)
- [1.12 Python Booleans](#112-python-booleans)
- [1.13 Python Operators](#113-python-operators)
- [1.14 Python User Input](#114-python-user-input)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 1.1 Python HOME

Welcome to Python. This chapter takes you from zero to writing small, working programs.

**What you will learn in this chapter**

| Topic | What you can do afterwards |
|---|---|
| Setup and syntax | Run your first program |
| Variables and data types | Store and label information |
| Numbers, strings, booleans | Work with the main kinds of data |
| Casting | Convert between data types |
| Operators | Calculate and compare |
| User input | Make interactive programs |

**Your first program**

```python
print("Hello, World!")
```

Output:

```
Hello, World!
```

---

## 1.2 Python Introduction

### What is Python?

Python is a popular, general-purpose programming language created by Guido van Rossum and first released in 1991. It is known for its clear, readable syntax.

### What is Python used for?

- Web development (Django, Flask, FastAPI)
- Data analysis and data science (pandas, NumPy)
- Machine learning and AI (scikit-learn, PyTorch)
- Automation and scripting
- Software testing
- Game development and desktop applications

### Why learn Python?

- **Easy to read:** the code looks close to plain English.
- **Beginner friendly:** few rules to memorize before you can build things.
- **Huge ecosystem:** thousands of ready-made libraries.
- **Cross-platform:** runs on Windows, macOS, and Linux.
- **Interpreted:** code runs line by line, with no separate compile step.

### Python versions

Always use **Python 3**. Python 2 is no longer supported.

```bash
python --version
```

Example output:

```
Python 3.12.4
```

---

## 1.3 Python Get Started

### Step 1: Check whether Python is installed

```bash
python --version
```

On some systems (macOS and Linux) the command is `python3`:

```bash
python3 --version
```

### Step 2: Install Python

Download it from [python.org/downloads](https://www.python.org/downloads/). On Windows, tick **"Add Python to PATH"** during installation.

### Step 3: Run Python code

There are three common ways.

**Way 1: The interactive shell (REPL)**

```bash
python
```

```python
>>> print("Hello")
Hello
>>> 5 + 3
8
>>> exit()
```

**Way 2: A script file**

Create a file named `hello.py`:

```python
print("Hello from a file!")
```

Run it from the terminal:

```bash
python hello.py
```

**Way 3: A code editor or IDE**

Popular choices: VS Code, PyCharm, IDLE (comes with Python), Jupyter Notebook.

### Basic workflow

```
Write code  →  Save as .py  →  Run  →  Read output  →  Fix and repeat
```

---

## 1.4 Python Syntax

### Indentation

Python uses indentation (spaces at the start of a line) to define blocks of code. Other languages use curly braces `{}`.

```python
if 5 > 2:
    print("Five is greater than two!")
```

Missing or wrong indentation causes an error:

```python
if 5 > 2:
print("Error!")
```

```
IndentationError: expected an indented block after 'if' statement
```

**Rules**

- Use the same number of spaces within a block. The standard is **4 spaces**.
- Do not mix tabs and spaces.

```python
if 5 > 2:
    print("Five is greater than two!")
    print("Same block, same indentation.")
```

### Case sensitivity

Python is case sensitive. `name`, `Name`, and `NAME` are three different names.

```python
name = "Ali"
Name = "Sara"
print(name)   # Ali
print(Name)   # Sara
```

### One statement per line

```python
x = 10
y = 20
```

Several statements can share a line using a semicolon, but this is discouraged:

```python
x = 10; y = 20
```

### Line continuation

Use a backslash `\` or wrap in brackets to split a long line.

```python
total = 1 + 2 + 3 + \
        4 + 5

numbers = [
    1, 2, 3,
    4, 5, 6,
]
```

### Keywords

Words with a special meaning, which cannot be used as variable names.

```python
import keyword
print(keyword.kwlist)
```

Some examples: `if`, `else`, `for`, `while`, `def`, `class`, `import`, `return`, `True`, `False`, `None`.

---

## 1.5 Python Output

### The `print()` function

**Syntax**

```python
print(value1, value2, ..., sep=" ", end="\n")
```

**Basic examples**

```python
print("Hello, World!")
print(42)
print(3.14)
print(True)
```

### Printing multiple values

By default, values are separated by a space.

```python
print("Name:", "Ali", "Age:", 25)
```

```
Name: Ali Age: 25
```

### The `sep` parameter

Changes the separator between values.

```python
print("2026", "10", "18", sep="-")
```

```
2026-10-18
```

### The `end` parameter

Changes what is printed at the end. The default is a newline.

```python
print("Hello", end=" ")
print("World")
```

```
Hello World
```

### Escape characters

| Code | Meaning |
|---|---|
| `\n` | New line |
| `\t` | Tab |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |

```python
print("Line 1\nLine 2")
print("Name:\tAli")
print("She said \"Hi\"")
```

```
Line 1
Line 2
Name:	Ali
She said "Hi"
```

### Printing variables and f-strings

```python
name = "Sara"
age = 22
print(f"My name is {name} and I am {age} years old.")
```

```
My name is Sara and I am 22 years old.
```

### Multi-line text

```python
print("""This is
a multi-line
message.""")
```

---

## 1.6 Python Comments

Comments explain code. Python ignores them when running.

### Single-line comments

Start with `#`.

```python
# This is a comment
print("Hello")  # This is an inline comment
```

### Multi-line comments

Python has no special multi-line comment syntax. Use several `#` lines:

```python
# This is a comment
# that spans
# multiple lines
```

Or use a triple-quoted string that is not assigned to anything:

```python
"""
This is a multi-line string
often used as a block comment.
"""
print("Hello")
```

### Docstrings

A triple-quoted string as the first statement in a function, class, or module describes it. You will use these more in later chapters.

```python
def greet():
    """Prints a greeting message."""
    print("Hello!")
```

### Commenting out code

```python
# print("This line will not run")
print("This line will run")
```

### Good commenting tips

- Explain **why**, not just what.
- Keep comments short and up to date.
- Do not comment obvious things like `x = 5  # set x to 5`.

---

## 1.7 Python Variables

A variable is a name that refers to a stored value.

### Creating variables

Python has no command to declare a variable. It is created when you assign a value.

**Syntax**

```python
variable_name = value
```

```python
x = 5
name = "Ali"
price = 19.99
is_student = True

print(x, name, price, is_student)
```

### Variables can change type

```python
x = 10        # int
x = "Ten"     # now a str
print(x)      # Ten
```

### Naming rules

- Must start with a letter or underscore `_`.
- Cannot start with a number.
- Can contain only letters, digits, and underscores.
- Case sensitive.
- Cannot be a Python keyword.

```python
# Valid
my_var = 1
_private = 2
var2 = 3
userName = "Ali"

# Invalid
# 2var = 1       # starts with a number
# my-var = 2     # hyphen not allowed
# my var = 3     # space not allowed
# class = 4      # keyword
```

### Naming style (PEP 8)

| Style | Example | Use for |
|---|---|---|
| snake_case | `user_name` | Variables and functions (recommended) |
| PascalCase | `UserName` | Classes |
| UPPER_CASE | `MAX_SIZE` | Constants |

### Assigning multiple variables

```python
# Different values
a, b, c = 1, 2, 3
print(a, b, c)    # 1 2 3

# Same value
x = y = z = 0
print(x, y, z)    # 0 0 0
```

### Unpacking a collection

```python
fruits = ["apple", "banana", "cherry"]
x, y, z = fruits
print(x)    # apple
print(y)    # banana
print(z)    # cherry
```

### Swapping variables

```python
a = 1
b = 2
a, b = b, a
print(a, b)    # 2 1
```

### Constants

Python has no true constants. By convention, use UPPER_CASE names and do not change them.

```python
PI = 3.14159
MAX_USERS = 100
```

### Global and local variables (quick preview)

```python
x = "global"

def show():
    x = "local"
    print(x)

show()      # local
print(x)    # global
```

Scope is covered in detail in Chapter 4.

### Deleting a variable

```python
x = 5
del x
# print(x)  # NameError: name 'x' is not defined
```

---

## 1.8 Python Data Types

Every value in Python has a type. Use `type()` to check it.

```python
x = 5
print(type(x))    # <class 'int'>
```

### Built-in data types

| Category | Types |
|---|---|
| Text | `str` |
| Numeric | `int`, `float`, `complex` |
| Sequence | `list`, `tuple`, `range` |
| Mapping | `dict` |
| Set | `set`, `frozenset` |
| Boolean | `bool` |
| Binary | `bytes`, `bytearray`, `memoryview` |
| None | `NoneType` |

### Examples

```python
a = "Hello"                    # str
b = 20                         # int
c = 20.5                       # float
d = 1j                         # complex
e = ["apple", "banana"]        # list
f = ("apple", "banana")        # tuple
g = range(6)                   # range
h = {"name": "Ali", "age": 25} # dict
i = {"apple", "banana"}        # set
j = True                       # bool
k = None                       # NoneType
```

### Checking types

```python
print(type("Hello"))     # <class 'str'>
print(type(10))          # <class 'int'>
print(type(3.14))        # <class 'float'>
print(type(True))        # <class 'bool'>
print(type([1, 2]))      # <class 'list'>
print(type(None))        # <class 'NoneType'>
```

### `isinstance()`

```python
x = 10
print(isinstance(x, int))      # True
print(isinstance(x, str))      # False
```

### Mutable vs immutable

| Immutable (cannot change in place) | Mutable (can change in place) |
|---|---|
| `int`, `float`, `str`, `bool`, `tuple` | `list`, `dict`, `set` |

```python
# Immutable string
s = "hello"
# s[0] = "H"   # TypeError

# Mutable list
lst = [1, 2, 3]
lst[0] = 100
print(lst)     # [100, 2, 3]
```

---

## 1.9 Python Numbers

Python has three numeric types: `int`, `float`, and `complex`.

### Integers (`int`)

Whole numbers, positive or negative, with no limit on size.

```python
x = 1
y = 35656222554887711
z = -3255522

print(type(x))   # <class 'int'>
```

Underscores can make large numbers readable:

```python
population = 1_000_000
print(population)   # 1000000
```

### Floats (`float`)

Numbers with a decimal point.

```python
x = 1.10
y = 1.0
z = -35.59

print(type(x))   # <class 'float'>
```

Scientific notation:

```python
a = 35e3     # 35000.0
b = 12E4     # 120000.0
c = -87.7e100
print(a, b)
```

### Complex numbers (`complex`)

```python
x = 3 + 5j
y = 5j
z = -5j
print(type(x))   # <class 'complex'>
print(x.real)    # 3.0
print(x.imag)    # 5.0
```

### Float precision

Floats are stored in binary, so some decimals are not exact.

```python
print(0.1 + 0.2)    # 0.30000000000000004
```

Use `round()` or the `decimal` module for exact decimals:

```python
print(round(0.1 + 0.2, 2))    # 0.3

from decimal import Decimal
print(Decimal("0.1") + Decimal("0.2"))    # 0.3
```

### Useful number functions

```python
print(abs(-7))         # 7
print(round(3.14159, 2))   # 3.14
print(pow(2, 3))       # 8
print(max(3, 9, 5))    # 9
print(min(3, 9, 5))    # 3
print(sum([1, 2, 3]))  # 6
```

### The `math` module (preview)

```python
import math

print(math.sqrt(16))    # 4.0
print(math.pi)          # 3.141592653589793
print(math.ceil(4.2))   # 5
print(math.floor(4.8))  # 4
```

### Random numbers

```python
import random

print(random.randint(1, 10))   # random int from 1 to 10
print(random.random())         # random float from 0.0 to 1.0
```

---

## 1.10 Python Casting

Casting means converting a value from one type to another.

### Casting functions

| Function | Converts to |
|---|---|
| `int()` | Integer |
| `float()` | Float |
| `str()` | String |
| `bool()` | Boolean |
| `list()`, `tuple()`, `set()` | Collections |

### `int()`

```python
print(int(2.8))        # 2 (truncates, does not round)
print(int("25"))       # 25
print(int(True))       # 1
```

### `float()`

```python
print(float(3))        # 3.0
print(float("4.5"))    # 4.5
print(float("10"))     # 10.0
```

### `str()`

```python
print(str(100))        # "100"
print(str(3.14))       # "3.14"
print(str(True))       # "True"
```

### `bool()`

```python
print(bool(1))         # True
print(bool(0))         # False
print(bool("Hello"))   # True
print(bool(""))        # False
print(bool([]))        # False
print(bool([1, 2]))    # True
```

### Implicit casting

Python sometimes converts types automatically.

```python
x = 5        # int
y = 2.5      # float
z = x + y
print(z)          # 7.5
print(type(z))    # <class 'float'>
```

### Common error: mixing text and numbers

```python
age = 25
# print("I am " + age)        # TypeError
print("I am " + str(age))     # I am 25
```

### Invalid conversions

```python
# int("hello")   # ValueError: invalid literal for int()
# int("3.5")     # ValueError (convert to float first)
print(int(float("3.5")))      # 3
```

### Converting collections

```python
print(list("abc"))             # ['a', 'b', 'c']
print(tuple([1, 2, 3]))        # (1, 2, 3)
print(set([1, 2, 2, 3]))       # {1, 2, 3}
```

---

## 1.11 Python Strings

A string is a sequence of characters written in single or double quotes.

### Creating strings

```python
a = "Hello"
b = 'World'
c = """Multi-line
string"""
```

### Quotes inside strings

```python
print("It's fine")
print('He said "Hi"')
print("She said \"Hello\"")
```

### Indexing

Each character has a position (index) starting at **0**. Negative indexes count from the end.

```
 H   e   l   l   o
 0   1   2   3   4
-5  -4  -3  -2  -1
```

```python
s = "Hello"
print(s[0])     # H
print(s[4])     # o
print(s[-1])    # o
print(s[-2])    # l
```

### Slicing

**Syntax**

```python
string[start:stop:step]
```

`start` is included, `stop` is excluded.

```python
s = "Hello, World!"
print(s[0:5])     # Hello
print(s[7:])      # World!
print(s[:5])      # Hello
print(s[-6:])     # World!
print(s[::2])     # Hlo ol!
print(s[::-1])    # !dlroW ,olleH (reversed)
```

### Length

```python
print(len("Hello"))    # 5
```

### Looping through a string

```python
for ch in "Hi!":
    print(ch)
```

```
H
i
!
```

### Checking membership

```python
text = "The quick brown fox"
print("quick" in text)       # True
print("slow" not in text)    # True
```

### String methods

Strings are immutable, so methods return a **new** string.

| Method | Purpose | Example | Result |
|---|---|---|---|
| `upper()` | Uppercase | `"hi".upper()` | `"HI"` |
| `lower()` | Lowercase | `"HI".lower()` | `"hi"` |
| `title()` | Title Case | `"hello world".title()` | `"Hello World"` |
| `capitalize()` | First letter upper | `"hello".capitalize()` | `"Hello"` |
| `strip()` | Remove outer spaces | `"  hi  ".strip()` | `"hi"` |
| `replace(a, b)` | Replace text | `"cat".replace("c", "b")` | `"bat"` |
| `split(sep)` | Split into list | `"a,b".split(",")` | `["a", "b"]` |
| `join(list)` | Join list to string | `"-".join(["a","b"])` | `"a-b"` |
| `find(x)` | Index of first match (-1 if none) | `"hello".find("l")` | `2` |
| `count(x)` | Count occurrences | `"banana".count("a")` | `3` |
| `startswith(x)` | Starts with? | `"hello".startswith("he")` | `True` |
| `endswith(x)` | Ends with? | `"hello".endswith("lo")` | `True` |
| `isdigit()` | All digits? | `"123".isdigit()` | `True` |
| `isalpha()` | All letters? | `"abc".isalpha()` | `True` |

```python
text = "  Hello, Python!  "
print(text.strip())              # Hello, Python!
print(text.upper())              # "  HELLO, PYTHON!  "
print(text.replace("Python", "World"))
print(text.split(","))           # ['  Hello', ' Python!  ']
```

### String concatenation

```python
first = "Hello"
second = "World"
print(first + " " + second)    # Hello World
print("Ha" * 3)                # HaHaHa
```

### String formatting

**f-strings (recommended)**

```python
name = "Ali"
age = 25
print(f"{name} is {age} years old.")
print(f"Next year: {age + 1}")
print(f"Pi is {3.14159:.2f}")      # Pi is 3.14
```

**`format()` method**

```python
print("{} is {} years old.".format("Ali", 25))
print("{0} {1} {0}".format("a", "b"))    # a b a
```

### Raw strings

Treat backslashes literally. Useful for file paths.

```python
print(r"C:\new_folder\test")    # C:\new_folder\test
```

### Strings are immutable

```python
s = "Hello"
# s[0] = "J"              # TypeError
s = "J" + s[1:]
print(s)                  # Jello
```

---

## 1.12 Python Booleans

A boolean has one of two values: `True` or `False` (capital first letter).

### Basic use

```python
print(10 > 9)     # True
print(10 == 9)    # False
print(10 < 9)     # False
```

### In `if` statements

```python
a = 200
b = 33

if b > a:
    print("b is greater than a")
else:
    print("b is not greater than a")
```

### The `bool()` function

Evaluates any value as `True` or `False`.

```python
print(bool("Hello"))    # True
print(bool(15))         # True
print(bool(0))          # False
```

### Truthy and falsy values

**Falsy values** (evaluate to `False`):

```python
bool(False)
bool(None)
bool(0)
bool(0.0)
bool("")
bool([])
bool(())
bool({})
bool(set())
```

**Everything else is truthy.**

```python
if "":
    print("Won't print")

if "text":
    print("Will print")
```

### Booleans are numbers

```python
print(True + True)    # 2
print(True * 5)       # 5
print(False + 1)      # 1
```

### Logical operators

| Operator | Meaning |
|---|---|
| `and` | True if both are true |
| `or` | True if at least one is true |
| `not` | Reverses the result |

```python
x = 5
print(x > 3 and x < 10)    # True
print(x > 3 or x > 10)     # True
print(not (x > 3))         # False
```

### Truth table

| A | B | A and B | A or B |
|---|---|---|---|
| True | True | True | True |
| True | False | False | True |
| False | True | False | True |
| False | False | False | False |

### Checking a type with `isinstance`

```python
print(isinstance(5, int))        # True
print(isinstance("5", int))      # False
```

---

## 1.13 Python Operators

Operators perform operations on values (operands).

### Arithmetic operators

| Operator | Name | Example | Result |
|---|---|---|---|
| `+` | Addition | `5 + 2` | `7` |
| `-` | Subtraction | `5 - 2` | `3` |
| `*` | Multiplication | `5 * 2` | `10` |
| `/` | Division (always float) | `5 / 2` | `2.5` |
| `//` | Floor division | `5 // 2` | `2` |
| `%` | Modulus (remainder) | `5 % 2` | `1` |
| `**` | Exponent | `5 ** 2` | `25` |

```python
a = 17
b = 5
print(a + b)     # 22
print(a - b)     # 12
print(a * b)     # 85
print(a / b)     # 3.4
print(a // b)    # 3
print(a % b)     # 2
print(a ** 2)    # 289
```

**Even or odd check**

```python
n = 8
print(n % 2 == 0)    # True (even)
```

### Assignment operators

| Operator | Example | Same as |
|---|---|---|
| `=` | `x = 5` | `x = 5` |
| `+=` | `x += 3` | `x = x + 3` |
| `-=` | `x -= 3` | `x = x - 3` |
| `*=` | `x *= 3` | `x = x * 3` |
| `/=` | `x /= 3` | `x = x / 3` |
| `//=` | `x //= 3` | `x = x // 3` |
| `%=` | `x %= 3` | `x = x % 3` |
| `**=` | `x **= 3` | `x = x ** 3` |

```python
x = 10
x += 5
print(x)    # 15
x *= 2
print(x)    # 30
```

**Walrus operator** (`:=`) assigns and returns a value in one expression:

```python
if (n := len("Python")) > 5:
    print(f"Length is {n}")    # Length is 6
```

### Comparison operators

| Operator | Name | Example | Result |
|---|---|---|---|
| `==` | Equal | `5 == 5` | `True` |
| `!=` | Not equal | `5 != 3` | `True` |
| `>` | Greater than | `5 > 3` | `True` |
| `<` | Less than | `5 < 3` | `False` |
| `>=` | Greater or equal | `5 >= 5` | `True` |
| `<=` | Less or equal | `5 <= 3` | `False` |

```python
print(10 == 10.0)    # True
print("a" < "b")     # True
```

**Chained comparisons**

```python
x = 7
print(1 < x < 10)    # True
```

### Logical operators

```python
age = 20
has_id = True

print(age >= 18 and has_id)     # True
print(age < 18 or has_id)       # True
print(not has_id)               # False
```

### Identity operators

Check whether two names point to the **same object**.

| Operator | Meaning |
|---|---|
| `is` | Same object |
| `is not` | Different objects |

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)    # True  (same values)
print(a is b)    # False (different objects)
print(a is c)    # True  (same object)
```

Use `is` mainly for `None`:

```python
x = None
print(x is None)    # True
```

### Membership operators

| Operator | Meaning |
|---|---|
| `in` | Value is found in a sequence |
| `not in` | Value is not found |

```python
fruits = ["apple", "banana"]
print("apple" in fruits)        # True
print("mango" not in fruits)    # True
print("a" in "banana")          # True
```

### Bitwise operators

Work on the binary form of integers.

| Operator | Name | Example | Result |
|---|---|---|---|
| `&` | AND | `5 & 3` | `1` |
| `\|` | OR | `5 \| 3` | `7` |
| `^` | XOR | `5 ^ 3` | `6` |
| `~` | NOT | `~5` | `-6` |
| `<<` | Left shift | `5 << 1` | `10` |
| `>>` | Right shift | `5 >> 1` | `2` |

### Operator precedence

From highest to lowest:

| Order | Operators |
|---|---|
| 1 | `()` parentheses |
| 2 | `**` |
| 3 | `+x`, `-x`, `~x` |
| 4 | `*`, `/`, `//`, `%` |
| 5 | `+`, `-` |
| 6 | `<<`, `>>` |
| 7 | `&`, `^`, `\|` |
| 8 | Comparisons, `is`, `in` |
| 9 | `not` |
| 10 | `and` |
| 11 | `or` |

```python
print(2 + 3 * 4)        # 14
print((2 + 3) * 4)      # 20
print(2 ** 3 ** 2)      # 512 (right to left: 2 ** 9)
```

---

## 1.14 Python User Input

### The `input()` function

**Syntax**

```python
variable = input(prompt)
```

```python
name = input("Enter your name: ")
print("Hello,", name)
```

Sample run:

```
Enter your name: Sara
Hello, Sara
```

### Input is always a string

```python
age = input("Enter your age: ")
print(type(age))    # <class 'str'>
```

### Converting input to numbers

```python
age = int(input("Enter your age: "))
print("Next year you will be", age + 1)
```

```python
price = float(input("Enter price: "))
print("With 10% tax:", price * 1.10)
```

### Why conversion matters

```python
a = input("First number: ")     # "5"
b = input("Second number: ")    # "3"
print(a + b)                    # 53  (string joining)
print(int(a) + int(b))          # 8
```

### Multiple inputs on one line

```python
x, y = input("Enter two numbers: ").split()
print(int(x) + int(y))
```

Sample run:

```
Enter two numbers: 4 6
10
```

### Reading a list of numbers

```python
numbers = list(map(int, input("Enter numbers: ").split()))
print(numbers)
print("Sum:", sum(numbers))
```

### Handling invalid input

```python
try:
    age = int(input("Enter your age: "))
    print("You entered", age)
except ValueError:
    print("Please enter a valid number.")
```

(`try...except` is covered fully in Chapter 6.)

### Mini program: simple calculator

```python
a = float(input("Enter first number: "))
b = float(input("Enter second number: "))

print(f"Sum:        {a + b}")
print(f"Difference: {a - b}")
print(f"Product:    {a * b}")
print(f"Quotient:   {a / b}")
```

### Mini program: greeting card

```python
name = input("Your name: ").strip().title()
city = input("Your city: ").strip().title()

print(f"Hello {name} from {city}!")
```

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Syntax | Indentation defines blocks; Python is case sensitive |
| Output | `print()` with `sep`, `end`, and f-strings |
| Comments | `#` for single line, triple quotes for blocks and docstrings |
| Variables | Created by assignment; use `snake_case` |
| Data types | `str`, `int`, `float`, `bool`, `list`, `tuple`, `set`, `dict`; check with `type()` |
| Numbers | `int`, `float`, `complex`; beware float precision |
| Casting | `int()`, `float()`, `str()`, `bool()` |
| Strings | Indexing, slicing, methods, f-strings; immutable |
| Booleans | `True` / `False`; truthy and falsy values |
| Operators | Arithmetic, assignment, comparison, logical, identity, membership, bitwise |
| Input | `input()` returns a string; convert when needed |

### Quick syntax cheat sheet

```python
# Variable
x = 10

# Output
print(f"x is {x}")

# Input
name = input("Name: ")
age = int(input("Age: "))

# Type check and casting
type(x)
str(x)
float(x)

# String basics
s = "Python"
s[0]          # 'P'
s[1:4]        # 'yth'
s.upper()     # 'PYTHON'
len(s)        # 6

# Operators
5 + 2, 5 - 2, 5 * 2, 5 / 2, 5 // 2, 5 % 2, 5 ** 2
5 > 2 and 3 < 4
"a" in "cat"
```

---

## Practice Exercises

### Beginner

1. Print your name, age, and city on three separate lines.
2. Create variables for `length` and `width`, then print the area of a rectangle.
3. Ask the user for their name and print a greeting.
4. Convert the string `"150"` to an integer and add 50 to it.
5. Print the first and last characters of the string `"Programming"`.

### Intermediate

6. Ask for a temperature in Celsius and convert it to Fahrenheit: `F = C * 9/5 + 32`.
7. Take a string from the user and print it reversed, in uppercase, and its length.
8. Ask for two numbers and print whether the first is divisible by the second.
9. Swap two variables without using a third variable.
10. Ask for a full name (`"ali raza khan"`) and print it in Title Case.

### Challenge

11. Write a program that asks for the price of an item and a discount percentage, then prints the final price.
12. Ask for the number of seconds and print it as hours, minutes, and seconds (use `//` and `%`).
13. Ask for a sentence, then print how many times the letter `"a"` appears and the number of words in it.

### Sample solution (Exercise 6)

```python
celsius = float(input("Temperature in Celsius: "))
fahrenheit = celsius * 9 / 5 + 32
print(f"{celsius}°C = {fahrenheit}°F")
```

### Sample solution (Exercise 12)

```python
total = int(input("Enter seconds: "))

hours = total // 3600
minutes = (total % 3600) // 60
seconds = total % 60

print(f"{hours}h {minutes}m {seconds}s")
```

---

**Next:** [Chapter 2: Python Data Structures](#)
