# Chapter 4: Python Functions and Iteration

> Functions let you package code and reuse it. Iterators power every `for` loop. Modules organize code across files. Scope controls where names are visible. Lambdas are small one-line functions.

## Table of Contents

- [4.1 Python Functions](#41-python-functions)
- [4.2 Python Iterators](#42-python-iterators)
- [4.3 Python Modules](#43-python-modules)
- [4.4 Python Scope](#44-python-scope)
- [4.5 Python Lambda](#45-python-lambda)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 4.1 Python Functions

A function is a block of code that runs only when it is **called**. It can receive data (parameters) and give data back (a return value).

### Why use functions?

- Reuse code instead of copying it.
- Break big problems into small pieces.
- Make programs easier to read, test, and fix.

### Defining and calling a function

**Syntax**

```python
def function_name(parameters):
    # body
    return value    # optional
```

```python
def greet():
    print("Hello from a function!")

greet()    # call it
greet()    # call it again
```

```
Hello from a function!
Hello from a function!
```

### Parameters and arguments

- **Parameter**: the name in the function definition.
- **Argument**: the value you pass when calling.

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Ali")
greet("Sara")
```

```
Hello, Ali!
Hello, Sara!
```

### Multiple parameters

```python
def add(a, b):
    print(a + b)

add(3, 4)    # 7
```

Passing the wrong number of arguments causes an error:

```python
# add(3)    # TypeError: add() missing 1 required positional argument: 'b'
```

### The `return` statement

`return` sends a value back to the caller and ends the function.

```python
def add(a, b):
    return a + b

result = add(3, 4)
print(result)           # 7
print(add(10, 5) * 2)   # 30
```

A function with no `return` gives back `None`:

```python
def say_hi():
    print("Hi")

x = say_hi()
print(x)    # None
```

### Returning multiple values

Python packs them into a tuple.

```python
def min_max(nums):
    return min(nums), max(nums)

low, high = min_max([4, 8, 1, 9])
print(low, high)    # 1 9
```

### Default parameter values

```python
def greet(name="friend"):
    print(f"Hello, {name}!")

greet()          # Hello, friend!
greet("Ali")     # Hello, Ali!
```

Parameters with defaults must come **after** those without:

```python
def power(base, exponent=2):
    return base ** exponent

print(power(5))       # 25
print(power(2, 10))   # 1024
```

### Keyword arguments

Pass arguments by name, in any order.

```python
def describe(name, age, city):
    print(f"{name}, {age}, from {city}")

describe(age=25, city="Lahore", name="Ali")
```

### Positional vs keyword mix

Positional arguments come first, keyword arguments after.

```python
describe("Ali", city="Lahore", age=25)
```

### Arbitrary positional arguments: `*args`

Collects any number of extra positional arguments into a **tuple**.

```python
def total(*numbers):
    return sum(numbers)

print(total(1, 2))          # 3
print(total(1, 2, 3, 4))    # 10
```

### Arbitrary keyword arguments: `**kwargs`

Collects extra keyword arguments into a **dictionary**.

```python
def show_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

show_info(name="Ali", age=25, city="Lahore")
```

```
name: Ali
age: 25
city: Lahore
```

### Combining everything

Required order of parameter types:

```python
def func(a, b, /, c, d=0, *args, e, f=1, **kwargs):
    pass
```

| Part | Meaning |
|---|---|
| `a, b, /` | Positional-only (before `/`) |
| `c, d=0` | Normal (positional or keyword) |
| `*args` | Extra positional values |
| `e, f=1` | Keyword-only (after `*args` or `*`) |
| `**kwargs` | Extra keyword values |

**Keyword-only parameters**

```python
def connect(host, *, port=80):
    print(host, port)

connect("example.com", port=443)
# connect("example.com", 443)    # TypeError
```

### Unpacking arguments when calling

```python
def add(a, b, c):
    return a + b + c

nums = [1, 2, 3]
print(add(*nums))          # 6

values = {"a": 1, "b": 2, "c": 3}
print(add(**values))       # 6
```

### Passing different data types

```python
def show_all(items):
    for item in items:
        print(item)

show_all(["apple", "banana"])
show_all("hi")
```

### Mutable arguments can change

Lists and dictionaries are passed by reference, so a function can modify them.

```python
def add_item(items):
    items.append("new")

my_list = ["a", "b"]
add_item(my_list)
print(my_list)    # ['a', 'b', 'new']
```

### Mutable default argument trap

Never use a list or dict as a default value. It is created **once** and shared between calls.

```python
# Wrong
def add(item, items=[]):
    items.append(item)
    return items

print(add(1))    # [1]
print(add(2))    # [1, 2]   (surprise!)

# Right
def add(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

print(add(1))    # [1]
print(add(2))    # [2]
```

### The `pass` statement

A function cannot be empty. Use `pass` as a placeholder.

```python
def todo():
    pass
```

### Docstrings

Describe what a function does. View them with `help()` or `__doc__`.

```python
def area(width, height):
    """Return the area of a rectangle."""
    return width * height

print(area.__doc__)    # Return the area of a rectangle.
help(area)
```

### Type hints (optional)

Hints document the expected types. Python does not enforce them.

```python
def add(a: int, b: int) -> int:
    return a + b

def greet(name: str = "friend") -> None:
    print(f"Hello, {name}")
```

### Recursion

A function that calls itself. It needs a **base case** to stop.

```python
def factorial(n):
    if n <= 1:            # base case
        return 1
    return n * factorial(n - 1)

print(factorial(5))    # 120
```

**Fibonacci**

```python
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

print([fib(i) for i in range(10)])
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

Python limits recursion depth (about 1000 calls by default). For deep problems, use loops.

### Functions as values

Functions are objects. They can be stored, passed, and returned.

```python
def shout(text):
    return text.upper()

def whisper(text):
    return text.lower()

def speak(func, message):
    print(func(message))

speak(shout, "Hello")      # HELLO
speak(whisper, "Hello")    # hello
```

### Nested functions and closures

```python
def make_multiplier(n):
    def multiply(x):
        return x * n
    return multiply

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(5))    # 10
print(triple(5))    # 15
```

The inner function "remembers" `n`. This is called a **closure**.

### Useful built-in functions

```python
print(len("hello"))          # 5
print(max(3, 9, 5))          # 9
print(sorted([3, 1, 2]))     # [1, 2, 3]
print(abs(-7))               # 7
print(sum([1, 2, 3]))        # 6
print(list(map(str, [1, 2])))    # ['1', '2']
print(list(filter(None, [0, 1, "", "a"])))    # [1, 'a']
```

### Real-world example: grade calculator

```python
def get_grade(marks):
    """Return a letter grade for the given marks."""
    if marks >= 90:
        return "A+"
    elif marks >= 80:
        return "A"
    elif marks >= 70:
        return "B"
    elif marks >= 60:
        return "C"
    return "F"

for m in [95, 82, 67, 40]:
    print(m, get_grade(m))
```

### Real-world example: temperature converter

```python
def c_to_f(c):
    return c * 9 / 5 + 32

def f_to_c(f):
    return (f - 32) * 5 / 9

print(c_to_f(100))    # 212.0
print(f_to_c(32))     # 0.0
```

### Good function habits

- One function, one job.
- Use clear, verb-based names (`calculate_total`, `get_user`).
- Prefer `return` over `print` inside functions, so results can be reused.
- Keep parameter lists short.
- Write a docstring for non-trivial functions.

---

## 4.2 Python Iterators

An **iterator** is an object that produces values one at a time. Every `for` loop uses iterators behind the scenes.

### Iterable vs iterator

| Term | Meaning | Examples |
|---|---|---|
| **Iterable** | Can give you an iterator | list, tuple, string, set, dict, range |
| **Iterator** | Remembers its position and gives the next value | result of `iter(x)` |

### `iter()` and `next()`

```python
fruits = ["apple", "banana", "cherry"]

it = iter(fruits)

print(next(it))    # apple
print(next(it))    # banana
print(next(it))    # cherry
# print(next(it))  # StopIteration error
```

### Default value for `next()`

```python
it = iter([1])
print(next(it, "done"))    # 1
print(next(it, "done"))    # done
```

### How a `for` loop really works

```python
for x in [1, 2, 3]:
    print(x)
```

is the same as:

```python
it = iter([1, 2, 3])

while True:
    try:
        x = next(it)
    except StopIteration:
        break
    print(x)
```

### Iterating strings and dictionaries

```python
it = iter("abc")
print(next(it))    # a
print(next(it))    # b

d = {"x": 1, "y": 2}
for key in d:
    print(key)
```

### Iterators are used up

```python
it = iter([1, 2, 3])
print(list(it))    # [1, 2, 3]
print(list(it))    # []   (already consumed)
```

### Creating your own iterator

A class needs two methods: `__iter__()` and `__next__()`.

```python
class Countdown:
    def __init__(self, start):
        self.current = start

    def __iter__(self):
        return self

    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value

for n in Countdown(5):
    print(n)
```

```
5
4
3
2
1
```

### Stopping an iterator

`StopIteration` signals the end. `for` loops handle it automatically.

```python
class UpTo:
    def __init__(self, limit):
        self.limit = limit

    def __iter__(self):
        self.n = 1
        return self

    def __next__(self):
        if self.n > self.limit:
            raise StopIteration
        x = self.n
        self.n += 1
        return x

print(list(UpTo(5)))    # [1, 2, 3, 4, 5]
```

### Generators (a simpler way)

A function with `yield` becomes a **generator**, which is an easy way to build an iterator.

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for x in countdown(3):
    print(x)    # 3 2 1
```

Generators are covered in depth in Chapter 9.

### Generator expressions

Like list comprehensions, but lazy and memory friendly.

```python
squares = (x ** 2 for x in range(1, 6))
print(next(squares))     # 1
print(list(squares))     # [4, 9, 16, 25]

print(sum(x ** 2 for x in range(1000)))
```

### Helpful tools for iteration

| Function | Purpose | Example |
|---|---|---|
| `enumerate()` | Index and value | `enumerate(["a", "b"])` |
| `zip()` | Combine sequences | `zip([1, 2], ["a", "b"])` |
| `reversed()` | Go backwards | `reversed([1, 2, 3])` |
| `sorted()` | Sorted copy | `sorted({3, 1, 2})` |
| `map()` | Apply a function | `map(str, [1, 2])` |
| `filter()` | Keep matching items | `filter(None, [0, 1])` |

```python
names = ["Ali", "Sara"]
ages = [25, 22]

for i, (n, a) in enumerate(zip(names, ages), start=1):
    print(i, n, a)
```

```
1 Ali 25
2 Sara 22
```

### `itertools` (preview)

```python
from itertools import count, cycle, islice, chain

print(list(islice(count(10), 3)))          # [10, 11, 12]
print(list(islice(cycle("AB"), 5)))        # ['A', 'B', 'A', 'B', 'A']
print(list(chain([1, 2], [3, 4])))         # [1, 2, 3, 4]
```

### Checking whether something is iterable

```python
from collections.abc import Iterable

print(isinstance([1, 2], Iterable))    # True
print(isinstance(5, Iterable))         # False
```

---

## 4.3 Python Modules

A **module** is a file containing Python code (functions, variables, classes) that you can use in other files. Modules keep large programs organized.

### Creating a module

Save this as `mymodule.py`:

```python
# mymodule.py
PI = 3.14159

def greet(name):
    return f"Hello, {name}!"

def area_circle(r):
    return PI * r ** 2
```

### Using a module with `import`

```python
# main.py (same folder as mymodule.py)
import mymodule

print(mymodule.greet("Ali"))        # Hello, Ali!
print(mymodule.area_circle(2))      # 12.56636
print(mymodule.PI)                  # 3.14159
```

### Ways to import

| Style | Example | Use |
|---|---|---|
| Whole module | `import math` | `math.sqrt(16)` |
| With alias | `import numpy as np` | `np.array([1])` |
| Specific names | `from math import sqrt, pi` | `sqrt(16)` |
| Specific with alias | `from math import sqrt as sq` | `sq(16)` |
| Everything (avoid) | `from math import *` | can cause name clashes |

```python
import math
print(math.sqrt(16))        # 4.0

import math as m
print(m.pi)                 # 3.141592653589793

from math import sqrt, pi
print(sqrt(25), pi)         # 5.0 3.14159...

from mymodule import greet as hello
print(hello("Sara"))
```

### Built-in modules

Python ships with many ready-to-use modules (the "standard library").

| Module | Purpose |
|---|---|
| `math` | Math functions |
| `random` | Random numbers |
| `datetime` | Dates and times |
| `os` | Operating system tasks |
| `sys` | Interpreter details |
| `json` | JSON data |
| `re` | Regular expressions |
| `collections` | Extra data structures |
| `itertools` | Iteration tools |
| `statistics` | Mean, median, etc. |

```python
import random
print(random.randint(1, 6))
print(random.choice(["a", "b", "c"]))

import datetime
print(datetime.date.today())

import statistics
print(statistics.mean([1, 2, 3, 4]))    # 2.5

import os
print(os.getcwd())
```

### `dir()`: list what a module contains

```python
import math
print(dir(math))
```

```python
help(math.sqrt)
```

### The `__name__ == "__main__"` pattern

Runs code only when the file is executed directly, not when it is imported.

```python
# tools.py
def add(a, b):
    return a + b

if __name__ == "__main__":
    print(add(2, 3))    # runs only with: python tools.py
```

When another file does `import tools`, the `print` line does **not** run.

### Where Python looks for modules

1. The folder of the running script
2. Folders in the `PYTHONPATH` environment variable
3. The standard library and installed packages

```python
import sys
print(sys.path)
```

### Packages

A **package** is a folder of modules, normally with an `__init__.py` file.

```
myproject/
├── main.py
└── shapes/
    ├── __init__.py
    ├── circle.py
    └── square.py
```

```python
# shapes/circle.py
def area(r):
    return 3.14159 * r ** 2
```

```python
# main.py
from shapes import circle
from shapes.circle import area

print(circle.area(3))
print(area(3))
```

### Installing third-party modules

```bash
pip install requests
```

```python
import requests
response = requests.get("https://example.com")
print(response.status_code)
```

(More about `pip` and virtual environments in Chapter 5.)

### Reloading a changed module

```python
import importlib
import mymodule
importlib.reload(mymodule)
```

### Common errors

| Error | Cause | Fix |
|---|---|---|
| `ModuleNotFoundError` | Module not installed or wrong name | `pip install ...` or check spelling |
| `ImportError: cannot import name` | Name does not exist in the module | Check the name |
| Name clash | A file named like a standard module, e.g. `random.py` | Rename your file |

> Do not name your own file after a standard module (`math.py`, `random.py`). Python may import yours instead.

### Real-world example: a small utility module

```python
# stringtools.py
def is_palindrome(text):
    cleaned = "".join(c.lower() for c in text if c.isalnum())
    return cleaned == cleaned[::-1]

def word_count(text):
    return len(text.split())
```

```python
# main.py
from stringtools import is_palindrome, word_count

print(is_palindrome("A man, a plan, a canal: Panama"))    # True
print(word_count("Python is fun"))                        # 3
```

---

## 4.4 Python Scope

**Scope** decides where in your code a variable can be seen and used.

### The LEGB rule

Python looks for a name in this order:

| Level | Meaning |
|---|---|
| **L**ocal | Inside the current function |
| **E**nclosing | In outer (enclosing) functions |
| **G**lobal | At the top level of the file |
| **B**uilt-in | Names Python provides (`len`, `print`, ...) |

### Local scope

A variable created inside a function exists only inside it.

```python
def my_func():
    x = 10
    print(x)

my_func()      # 10
# print(x)     # NameError: name 'x' is not defined
```

### Global scope

A variable created outside all functions is global.

```python
x = 50

def show():
    print(x)    # reads the global x

show()          # 50
print(x)        # 50
```

### Same name in both scopes

```python
x = "global"

def func():
    x = "local"
    print(x)

func()          # local
print(x)        # global (unchanged)
```

### The `global` keyword

Lets a function change a global variable.

```python
count = 0

def increment():
    global count
    count += 1

increment()
increment()
print(count)    # 2
```

Without `global`, this fails:

```python
count = 0

def increment():
    count += 1    # UnboundLocalError

# increment()
```

Create a global from inside a function:

```python
def create():
    global name
    name = "Ali"

create()
print(name)    # Ali
```

> Prefer parameters and `return` over globals. Global state makes code harder to test and debug.

```python
def increment(n):
    return n + 1

count = 0
count = increment(count)
```

### Enclosing scope and `nonlocal`

```python
def outer():
    x = "outer"

    def inner():
        print(x)    # reads outer's x

    inner()

outer()    # outer
```

To **change** the enclosing variable, use `nonlocal`:

```python
def outer():
    count = 0

    def inner():
        nonlocal count
        count += 1

    inner()
    inner()
    return count

print(outer())    # 2
```

### Counter using a closure

```python
def make_counter():
    count = 0

    def counter():
        nonlocal count
        count += 1
        return count

    return counter

c = make_counter()
print(c())    # 1
print(c())    # 2
print(c())    # 3
```

### Built-in scope

```python
print(len("hello"))    # len comes from the built-in scope
```

Do not shadow built-in names:

```python
# list = [1, 2, 3]
# list("abc")    # TypeError: 'list' object is not callable
```

### Loop variables

Loop variables stay after the loop ends.

```python
for i in range(3):
    pass

print(i)    # 2
```

`if`, `for`, and `while` blocks do **not** create a new scope. Only functions, classes, and modules do.

```python
if True:
    y = 5

print(y)    # 5
```

### Comprehension scope

Variables in a comprehension do not leak out.

```python
squares = [n ** 2 for n in range(3)]
# print(n)    # NameError
```

### Inspecting scope

```python
x = 5

def f():
    y = 10
    print(locals())

f()                          # {'y': 10}
print("x" in globals())      # True
```

### Scope summary

| Keyword | Purpose |
|---|---|
| (none) | Assignment creates a local variable |
| `global x` | Use and modify the global `x` |
| `nonlocal x` | Modify `x` from the nearest enclosing function |

---

## 4.5 Python Lambda

A **lambda** is a small anonymous function written in one line. It can take any number of arguments but has only **one expression**.

### Syntax

```python
lambda arguments: expression
```

### Basic examples

```python
add_ten = lambda x: x + 10
print(add_ten(5))    # 15

multiply = lambda a, b: a * b
print(multiply(4, 5))    # 20

greet = lambda name="friend": f"Hello, {name}"
print(greet())           # Hello, friend
```

The same as a regular function:

```python
def add_ten(x):
    return x + 10
```

### Calling a lambda immediately

```python
print((lambda x, y: x + y)(3, 4))    # 7
```

### Lambda with `*args`

```python
total = lambda *nums: sum(nums)
print(total(1, 2, 3))    # 6
```

### Lambda with conditions

```python
sign = lambda n: "positive" if n > 0 else "negative" if n < 0 else "zero"
print(sign(5))     # positive
print(sign(-2))    # negative
print(sign(0))     # zero
```

### Where lambdas shine: short throwaway functions

#### 1. With `sorted()`, `sort()`, `min()`, `max()`

```python
words = ["banana", "kiwi", "apple", "fig"]

print(sorted(words, key=lambda w: len(w)))
# ['fig', 'kiwi', 'apple', 'banana']

students = [("Ali", 80), ("Sara", 95), ("Bilal", 70)]
students.sort(key=lambda s: s[1], reverse=True)
print(students)
# [('Sara', 95), ('Ali', 80), ('Bilal', 70)]

print(max(students, key=lambda s: s[1]))    # ('Sara', 95)
```

#### 2. With `map()`

Applies a function to every item.

```python
nums = [1, 2, 3, 4]
squares = list(map(lambda x: x ** 2, nums))
print(squares)    # [1, 4, 9, 16]
```

#### 3. With `filter()`

Keeps items where the function returns `True`.

```python
nums = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, nums))
print(evens)    # [2, 4, 6]
```

#### 4. With `functools.reduce()`

Combines all items into one value.

```python
from functools import reduce

nums = [1, 2, 3, 4]
product = reduce(lambda a, b: a * b, nums)
print(product)    # 24
```

### Lambda inside a function

```python
def make_multiplier(n):
    return lambda x: x * n

double = make_multiplier(2)
triple = make_multiplier(3)

print(double(7))    # 14
print(triple(7))    # 21
```

### Sorting a dictionary with lambda

```python
scores = {"Ali": 70, "Sara": 95, "Bilal": 82}

by_score = sorted(scores.items(), key=lambda kv: kv[1], reverse=True)
print(by_score)
# [('Sara', 95), ('Bilal', 82), ('Ali', 70)]
```

### Lambda vs list comprehension

Often a comprehension is clearer than `map` or `filter` with a lambda.

```python
nums = [1, 2, 3, 4, 5, 6]

# map + lambda
print(list(map(lambda x: x ** 2, nums)))

# comprehension (preferred)
print([x ** 2 for x in nums])

# filter + lambda
print(list(filter(lambda x: x % 2 == 0, nums)))

# comprehension (preferred)
print([x for x in nums if x % 2 == 0])
```

### Limits of lambda

| Limit | Detail |
|---|---|
| One expression only | No multiple statements, no assignments |
| No `return` keyword | The expression value is returned |
| Harder to debug | Shows as `<lambda>` in errors |
| No docstrings | Cannot be documented directly |

If the logic grows beyond one short line, use `def`.

### Lambda late binding trap

```python
funcs = [lambda: i for i in range(3)]
print([f() for f in funcs])      # [2, 2, 2]  (all see the final i)

funcs = [lambda i=i: i for i in range(3)]
print([f() for f in funcs])      # [0, 1, 2]  (fixed with a default value)
```

### `def` vs `lambda`

| Feature | `def` | `lambda` |
|---|---|---|
| Has a name | Yes | No (anonymous) |
| Number of statements | Many | One expression |
| Docstring | Yes | No |
| Best for | Reusable logic | Short, one-off functions |

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Functions | `def`, parameters, `return`; defaults, `*args`, `**kwargs`; recursion; avoid mutable defaults |
| Iterators | `iter()` and `next()`; `__iter__` and `__next__`; generators with `yield` |
| Modules | `import`, `from ... import`, aliases; standard library; `__name__ == "__main__"`; packages |
| Scope | LEGB rule; `global` and `nonlocal`; prefer parameters and `return` |
| Lambda | `lambda args: expression`; used with `sorted`, `map`, `filter`, `reduce` |

### Quick syntax cheat sheet

```python
# Function
def add(a, b=0, *args, **kwargs):
    """Add numbers."""
    return a + b

# Lambda
square = lambda x: x ** 2

# Iterator
it = iter([1, 2, 3])
next(it)

# Generator
def count_up(n):
    for i in range(n):
        yield i

# Modules
import math
from math import sqrt
import numpy as np

# Scope
def f():
    global counter
    counter += 1

def outer():
    n = 0
    def inner():
        nonlocal n
        n += 1
```

---

## Practice Exercises

### Beginner

1. Write a function `greet(name)` that prints a greeting, and call it with two different names.
2. Write a function `square(n)` that returns the square of a number.
3. Write a function `is_even(n)` that returns `True` or `False`.
4. Import the `math` module and print the square root of 144 and the value of pi.
5. Write a lambda that adds 5 to a number.

### Intermediate

6. Write a function `average(*nums)` that accepts any number of values and returns their average.
7. Write a function that takes a list and returns both the smallest and the largest values.
8. Sort a list of words by length using `sorted()` and a lambda.
9. Use `map()` and a lambda to convert `[1, 2, 3, 4]` to a list of cubes.
10. Create your own module `mathtools.py` with `add`, `subtract`, and `multiply` functions, then import and use it from another file.

### Challenge

11. Write a recursive function that returns the sum of digits of a number.
12. Write a custom iterator class `EvenNumbers(limit)` that gives even numbers up to `limit`.
13. Write a generator `fibonacci(n)` that yields the first `n` Fibonacci numbers.
14. Write a function `make_counter()` that returns a function. Each call to the returned function gives the next number (1, 2, 3, ...).
15. Given a list of dictionaries like `{"name": "Ali", "age": 25}`, sort them by age using a lambda.

### Sample solution (Exercise 6)

```python
def average(*nums):
    if not nums:
        return 0
    return sum(nums) / len(nums)

print(average(4, 8, 6))    # 6.0
```

### Sample solution (Exercise 11)

```python
def digit_sum(n):
    if n < 10:
        return n
    return n % 10 + digit_sum(n // 10)

print(digit_sum(1234))    # 10
```

### Sample solution (Exercise 12)

```python
class EvenNumbers:
    def __init__(self, limit):
        self.limit = limit

    def __iter__(self):
        self.n = 0
        return self

    def __next__(self):
        if self.n > self.limit:
            raise StopIteration
        value = self.n
        self.n += 2
        return value

print(list(EvenNumbers(10)))    # [0, 2, 4, 6, 8, 10]
```

### Sample solution (Exercise 13)

```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

print(list(fibonacci(8)))    # [0, 1, 1, 2, 3, 5, 8, 13]
```

### Sample solution (Exercise 15)

```python
people = [
    {"name": "Ali", "age": 25},
    {"name": "Sara", "age": 22},
    {"name": "Bilal", "age": 30},
]

people.sort(key=lambda p: p["age"])
print(people)
```

---

**Previous:** [Chapter 3: Python Control Flow](chapter-03-python-control-flow.md)
**Next:** [Chapter 5: Python Built-in Tools and Formatting](#)
