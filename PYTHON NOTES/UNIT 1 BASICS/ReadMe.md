# Unit 1: Python Basics

> **Notes roadmap:** From beginner fundamentals to confident use of core Python.  
> **Scope:** Python basics only. Object-oriented programming and file handling are covered in separate units.

## Table of Contents

1. [Introduction to Python](#1-introduction-to-python)
2. [History and Features of Python](#2-history-and-features-of-python)
3. [Installing Python and Setting Up the Environment](#3-installing-python-and-setting-up-the-environment)
4. [Python Syntax and Indentation](#4-python-syntax-and-indentation)
5. [Comments and Documentation Strings](#5-comments-and-documentation-strings)
6. [Keywords and Identifiers](#6-keywords-and-identifiers)
7. [Variables and Constants](#7-variables-and-constants)
8. [Data Types in Python](#8-data-types-in-python)
9. [Type Checking and Type Conversion](#9-type-checking-and-type-conversion)
10. [Input and Output Functions](#10-input-and-output-functions)
11. [Operators in Python](#11-operators-in-python)
12. [Operator Precedence](#12-operator-precedence)
13. [Conditional Statements](#13-conditional-statements)
14. [Loops in Python](#14-loops-in-python)
15. [Strings](#15-strings)
16. [Lists](#16-lists)
17. [Tuples](#17-tuples)
18. [Sets and Frozensets](#18-sets-and-frozensets)
19. [Dictionaries](#19-dictionaries)
20. [Functions](#20-functions)
21. [Built-in Functions](#21-built-in-functions)
22. [Modules and Packages](#22-modules-and-packages)
23. [Importing Modules](#23-importing-modules)
24. [Exception Handling Basics](#24-exception-handling-basics)
25. [Python Practice Programs](#25-python-practice-programs)

---

## 1. Introduction to Python

- What Python is
- Where Python is used
- How Python programs run
- Python interpreter and interactive shell
- Writing and running a first program
- Example: `print("Hello, World!")`

## 2. History and Features of Python

- Brief history and creator
- Python 2 vs Python 3
- Readable, high-level syntax
- Interpreted execution model
- Dynamic typing
- Cross-platform support
- Large standard library and package ecosystem
- Common applications: web development, automation, data analysis, AI, and scripting

## 3. Installing Python and Setting Up the Environment

- Installing Python on Windows, macOS, or Linux
- Checking the installation with `python --version` or `python3 --version`
- Understanding PATH
- Choosing an editor or IDE (IDLE, VS Code, or PyCharm)
- Running a script from the terminal
- Interactive mode vs script mode
- Basic troubleshooting

## 4. Python Syntax and Indentation

- Statements and expressions
- Case sensitivity
- Indentation and code blocks
- Line breaks and continuation
- Using parentheses for readable multiline expressions
- Syntax errors

```python
if 5 > 2:
    print("Five is greater than two")
```

## 5. Comments and Documentation Strings

- Single-line comments with `#`
- Inline comments
- Writing useful comments
- Triple-quoted strings
- Docstrings for modules, classes, and functions
- Comments vs docstrings
- Good documentation habits

```python
# Calculate the total price
price = 100
quantity = 3
total = price * quantity

"""A module docstring describes a module's purpose."""
```

## 6. Keywords and Identifiers

- What keywords are
- Viewing keywords with the `keyword` module
- What identifiers are
- Naming rules
- Naming conventions: `snake_case`, `PascalCase`, and constants in `UPPER_CASE`
- Valid and invalid identifiers
- Avoiding names that shadow built-in functions

## 7. Variables and Constants

- Creating and assigning variables
- Reassigning values
- Multiple assignment
- Unpacking values
- Naming variables clearly
- Dynamic typing
- Constants by convention
- Object references and the difference between a name and an object

```python
name = "Amina"
age = 20
x, y = 10, 15

MAX_RETRIES = 3  # Constant by convention
```

## 8. Data Types in Python

### 8.1 Numeric Types
- `int`
- `float`
- `complex`
- Basic numeric operations

### 8.2 Boolean Type
- `bool`
- `True` and `False`
- Truthy and falsy values

### 8.3 Text Type
- `str`
- Basic string creation and use

### 8.4 None Type
- `None`
- Meaning of “no value”
- Checking for `None` with `is None`

### 8.5 Collection Types
- `list`
- `tuple`
- `range`
- `set`
- `frozenset`
- `dict`

### 8.6 Mutable and Immutable Types
- Meaning of mutability
- Examples of mutable and immutable objects
- Why mutability matters

### 8.7 Checking Types
- `type()`
- `isinstance()`

## 9. Type Checking and Type Conversion

- Checking types with `type()` and `isinstance()`
- Explicit conversion with `int()`, `float()`, `str()`, and `bool()`
- Converting between collections
- Converting user input
- Valid and invalid conversions
- `ValueError` and `TypeError` basics

```python
age_text = "21"
age = int(age_text)
print(age + 1)  # 22
```

## 10. Input and Output Functions

- Displaying output with `print()`
- Reading input with `input()`
- `print()` arguments: `sep` and `end`
- Formatting with f-strings
- Formatting numbers and decimal places
- Converting input to numeric types
- Basic input validation

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))
print(f"Hello, {name}. Next year you will be {age + 1}.")
```

## 11. Operators in Python

### 11.1 Arithmetic Operators
- `+`, `-`, `*`, `/`, `//`, `%`, `**`
- Integer division and remainder
- Negative numbers and exponentiation

### 11.2 Comparison Operators
- `==`, `!=`, `<`, `>`, `<=`, `>=`
- Chained comparisons

### 11.3 Assignment Operators
- `=`, `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=`
- Assignment vs comparison

### 11.4 Logical Operators
- `and`, `or`, `not`
- Short-circuit evaluation

### 11.5 Bitwise Operators
- `&`, `|`, `^`, `~`, `<<`, `>>`
- Basic binary-number concepts

### 11.6 Membership Operators
- `in`, `not in`
- Membership in strings and collections

### 11.7 Identity Operators
- `is`, `is not`
- Identity vs equality

## 12. Operator Precedence

- What precedence means
- Parentheses and grouping
- Arithmetic precedence
- Comparison, logical, and bitwise precedence
- Associativity
- Why explicit parentheses improve readability
- Evaluating expressions step by step

## 13. Conditional Statements

- Boolean conditions
- `if`
- `if-else`
- `if-elif-else`
- Nested conditions
- Combining conditions with logical operators
- Conditional expressions (ternary expressions)
- Pattern matching with `match-case` (Python 3.10+)
- Truthy and falsy conditions
- Common condition mistakes

```python
marks = 78

if marks >= 80:
    grade = "A"
elif marks >= 70:
    grade = "B"
elif marks >= 60:
    grade = "C"
else:
    grade = "Needs improvement"

print(grade)
```

## 14. Loops in Python

### 14.1 `while` Loop
- Syntax and execution
- Loop conditions
- Updating counters
- Avoiding infinite loops

### 14.2 `for` Loop
- Iterating over strings and collections
- Using `range()`
- Start, stop, and step values
- Nested loops

### 14.3 Loop Control
- `break`
- `continue`
- `pass`
- `else` clauses on loops

### 14.4 Loop Practice
- Counting and summing
- Multiplication tables
- Number patterns
- Searching through a sequence
- Choosing between `for` and `while`

## 15. Strings

- Creating strings with single, double, and triple quotes
- String indexing
- Negative indexing
- Slicing and slice steps
- String immutability
- Concatenation and repetition
- Membership testing
- Useful methods: `lower()`, `upper()`, `strip()`, `replace()`, `find()`, `count()`
- Splitting and joining: `split()`, `join()`
- Prefix and suffix checks
- String formatting with f-strings
- Escape sequences and raw strings
- Unicode basics
- Common string-processing exercises

## 16. Lists

- Creating lists
- Indexing and slicing
- Updating list elements
- Adding items: `append()`, `extend()`, `insert()`
- Removing items: `remove()`, `pop()`, `clear()`
- Searching and counting
- Sorting and reversing
- Copying lists and aliasing
- Nested lists
- Iterating through lists
- List comprehensions
- Lists as mutable sequences
- Common list errors and practice exercises

## 17. Tuples

- Creating tuples
- Indexing and slicing
- Single-item tuple syntax
- Tuple immutability
- Packing and unpacking
- Nested tuples
- Tuple methods: `count()` and `index()`
- Tuples vs lists
- When tuples are useful

## 18. Sets and Frozensets

- Creating sets
- Uniqueness and unordered collections
- Adding and removing elements
- Membership tests
- Union
- Intersection
- Difference
- Symmetric difference
- Subsets and supersets
- Set comprehensions
- `frozenset` and immutability
- Common use cases: removing duplicates and comparing groups

## 19. Dictionaries

- Key-value pairs
- Creating dictionaries
- Accessing values with keys
- `get()` and handling missing keys
- Adding, updating, and deleting entries
- Methods: `keys()`, `values()`, `items()`, `update()`, `pop()`
- Iterating through dictionaries
- Nested dictionaries
- Dictionary comprehensions
- Merging dictionaries
- Key uniqueness and hashability basics
- Practical uses: profiles, settings, and records

## 20. Functions

- Why functions are useful
- Defining functions with `def`
- Calling functions
- Parameters and arguments
- Positional and keyword arguments
- Default parameter values
- Return values
- Returning multiple values
- `*args` and `**kwargs`
- Local and global scope
- The `global` and `nonlocal` keywords
- Lambda functions
- Recursion
- Function annotations
- Docstrings
- Functions as values
- Basic higher-order functions
- Good function design and common mistakes

## 21. Built-in Functions

Study common built-ins and practise using them:

- Input/output: `print()`, `input()`
- Type and conversion: `type()`, `isinstance()`, `int()`, `float()`, `str()`, `bool()`
- Numeric: `abs()`, `round()`, `pow()`, `divmod()`, `sum()`, `min()`, `max()`
- Iteration: `range()`, `len()`, `enumerate()`, `zip()`, `iter()`, `next()`
- Ordering and logic: `sorted()`, `reversed()`, `any()`, `all()`
- Object inspection: `id()`, `dir()`, `help()`, `callable()`
- Functional utilities: `map()`, `filter()`
- Representation and attributes: `repr()`, `getattr()`, `hasattr()`, `setattr()`

## 22. Modules and Packages

- What a module is
- What a package is
- Standard library vs third-party packages
- Creating a custom module
- Creating a package
- The role of `__init__.py`
- Module namespaces
- Absolute and relative imports
- Circular import basics
- Installing external packages with `pip`
- Virtual environment overview

## 23. Importing Modules

- `import module`
- `from module import name`
- Import aliases with `as`
- Importing multiple names
- Avoiding wildcard imports
- The `__name__` variable
- The `if __name__ == "__main__":` pattern
- Common import errors
- Importing from your own project files

## 24. Exception Handling Basics

- Errors vs exceptions
- Common exceptions: `ValueError`, `TypeError`, `NameError`, `IndexError`, `KeyError`, `ZeroDivisionError`
- `try` and `except`
- Catching specific exceptions
- Handling multiple exception types
- `else` and `finally`
- Raising exceptions with `raise`
- Basic assertions
- Writing helpful error messages
- Avoiding overly broad exception handling

```python
try:
    number = int(input("Enter a whole number: "))
except ValueError:
    print("Please enter a valid whole number.")
else:
    print(f"You entered {number}.")
```

## 25. Python Practice Programs

### Beginner Programs
1. Print “Hello, World!”
2. Add two numbers
3. Convert Celsius to Fahrenheit
4. Calculate the area of a rectangle and circle
5. Check whether a number is even or odd
6. Find the largest of three numbers
7. Make a simple calculator
8. Print a multiplication table
9. Calculate a factorial
10. Check whether a number is prime

### String and Collection Programs
11. Reverse a string
12. Count vowels in a string
13. Check for a palindrome
14. Find the largest number in a list
15. Remove duplicates from a list
16. Count the frequency of list elements
17. Find common elements between two sets
18. Create a word-frequency dictionary
19. Sort a list of numbers
20. Merge two dictionaries

### Function and Module Programs
21. Build a calculator using functions
22. Create reusable validation functions
23. Write a module for arithmetic operations
24. Import and use your own module
25. Build a menu-driven student grading program

---

## Suggested Study Method

For every topic, write notes in this order:

1. **Definition** — explain the concept in simple words.
2. **Syntax** — show the general structure.
3. **Example** — write a small Python program.
4. **Output** — record the result.
5. **Explanation** — describe how the code works.
6. **Common mistakes** — note errors and how to fix them.
7. **Practice** — solve at least three exercises.

## Unit 1 Completion Checklist

- [ ] I can write and run basic Python programs.
- [ ] I understand variables, data types, and operators.
- [ ] I can use conditions and loops.
- [ ] I can work with strings and all major collection types.
- [ ] I can define and call functions.
- [ ] I can import modules and use built-in functions.
- [ ] I can handle common exceptions.
- [ ] I can solve beginner-level problems without copying solutions.

**Next unit:** Unit 2 — Object-Oriented Programming (OOP).  
**After that:** Unit 3 — File Handling in Python.
