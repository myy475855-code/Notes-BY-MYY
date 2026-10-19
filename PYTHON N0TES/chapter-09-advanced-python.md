# Chapter 9: Advanced Python

> These features separate beginners from confident Python developers: compact comprehensions, lazy generators, decorators, context managers, type hints, advanced regular expressions, and concurrent programming with async, threads, and processes.

## Table of Contents

- [9.1 Python List/Dictionary Comprehensions](#91-python-listdictionary-comprehensions)
- [9.2 Python Generators](#92-python-generators)
- [9.3 Python Decorators](#93-python-decorators)
- [9.4 Python Context Managers](#94-python-context-managers)
- [9.5 Python Type Hints](#95-python-type-hints)
- [9.6 Python Regular Expressions: Advanced](#96-python-regular-expressions-advanced)
- [9.7 Python Async Programming](#97-python-async-programming)
- [9.8 Python Multithreading and Multiprocessing](#98-python-multithreading-and-multiprocessing)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 9.1 Python List/Dictionary Comprehensions

A **comprehension** builds a collection from an iterable in a single, readable expression.

### List comprehension

**Syntax**

```python
[expression for item in iterable]
[expression for item in iterable if condition]
```

**Loop vs comprehension**

```python
# Loop
squares = []
for n in range(1, 6):
    squares.append(n ** 2)

# Comprehension
squares = [n ** 2 for n in range(1, 6)]
print(squares)    # [1, 4, 9, 16, 25]
```

### With a condition (filter)

```python
nums = range(1, 11)

evens = [n for n in nums if n % 2 == 0]
print(evens)    # [2, 4, 6, 8, 10]

long_words = [w for w in ["a", "apple", "kiwi", "banana"] if len(w) > 4]
print(long_words)    # ['apple', 'banana']
```

### With `if...else` (transform)

The `if...else` goes **before** the `for`.

```python
nums = [1, 2, 3, 4, 5]

labels = ["even" if n % 2 == 0 else "odd" for n in nums]
print(labels)    # ['odd', 'even', 'odd', 'even', 'odd']

clamped = [n if n <= 3 else 3 for n in nums]
print(clamped)   # [1, 2, 3, 3, 3]
```

| Goal | Pattern |
|---|---|
| Filter items | `[x for x in items if condition]` |
| Transform items | `[f(x) for x in items]` |
| Choose between two values | `[a if condition else b for x in items]` |

### Common patterns

```python
words = ["apple", "banana", "cherry"]

print([w.upper() for w in words])         # ['APPLE', 'BANANA', 'CHERRY']
print([len(w) for w in words])            # [5, 6, 6]
print([w[0] for w in words])              # ['a', 'b', 'c']
print([int(x) for x in "1 2 3".split()])  # [1, 2, 3]
print([c for c in "hello" if c in "aeiou"])    # ['e', 'o']
```

### Nested loops in a comprehension

Read it in the same order as nested `for` loops.

```python
pairs = [(x, y) for x in [1, 2, 3] for y in ["a", "b"]]
print(pairs)
# [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b'), (3, 'a'), (3, 'b')]
```

**Flatten a nested list**

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

flat = [n for row in matrix for n in row]
print(flat)    # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**Build a matrix**

```python
grid = [[r * c for c in range(1, 4)] for r in range(1, 4)]
print(grid)    # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

**Transpose a matrix**

```python
matrix = [[1, 2, 3], [4, 5, 6]]
transposed = [[row[i] for row in matrix] for i in range(3)]
print(transposed)    # [[1, 4], [2, 5], [3, 6]]
```

### Multiple conditions

```python
nums = range(1, 31)
result = [n for n in nums if n % 2 == 0 if n % 3 == 0]
print(result)    # [6, 12, 18, 24, 30]

same = [n for n in nums if n % 2 == 0 and n % 3 == 0]
```

### Dictionary comprehension

**Syntax**

```python
{key_expr: value_expr for item in iterable}
{key_expr: value_expr for item in iterable if condition}
```

```python
squares = {n: n ** 2 for n in range(1, 6)}
print(squares)    # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

lengths = {w: len(w) for w in ["apple", "kiwi", "banana"]}
print(lengths)    # {'apple': 5, 'kiwi': 4, 'banana': 6}
```

**Swap keys and values**

```python
d = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in d.items()}
print(swapped)    # {1: 'a', 2: 'b', 3: 'c'}
```

**Filter a dictionary**

```python
scores = {"Ali": 70, "Sara": 95, "Bilal": 82, "Hina": 55}

passed = {name: s for name, s in scores.items() if s >= 60}
print(passed)    # {'Ali': 70, 'Sara': 95, 'Bilal': 82}
```

**Transform values**

```python
prices = {"pen": 10, "book": 250}
with_tax = {item: round(p * 1.17, 2) for item, p in prices.items()}
print(with_tax)    # {'pen': 11.7, 'book': 292.5}
```

**Build from two lists**

```python
keys = ["name", "age", "city"]
values = ["Ali", 25, "Lahore"]

d = {k: v for k, v in zip(keys, values)}
# or simply: dict(zip(keys, values))
print(d)
```

**Count characters**

```python
text = "banana"
counts = {c: text.count(c) for c in set(text)}
print(counts)
```

### Set comprehension

```python
nums = [1, 2, 2, 3, 3, 3, 4]

unique_squares = {n ** 2 for n in nums}
print(unique_squares)    # {1, 4, 9, 16}

first_letters = {w[0].lower() for w in ["Apple", "avocado", "Banana"]}
print(first_letters)     # {'a', 'b'}
```

### Generator expression (lazy)

Round brackets give a **generator** that produces items on demand.

```python
squares = (n ** 2 for n in range(1_000_000))
print(next(squares))    # 0
print(next(squares))    # 1

print(sum(n ** 2 for n in range(1000)))     # no extra brackets needed
print(any(n > 5 for n in [1, 3, 7]))        # True
print(max(len(w) for w in ["a", "bbb", "cc"]))    # 3
```

| Syntax | Result | Memory |
|---|---|---|
| `[x for ...]` | list | All items stored |
| `{x for ...}` | set | All items stored |
| `{k: v for ...}` | dict | All items stored |
| `(x for ...)` | generator | One item at a time |

### The walrus operator in comprehensions

```python
data = ["5", "x", "12", "y", "7"]

numbers = [int(s) for s in data if s.isdigit()]
print(numbers)    # [5, 12, 7]

def slow_square(n):
    return n * n

results = [y for n in range(6) if (y := slow_square(n)) > 5]
print(results)    # [9, 16, 25]  (slow_square called once per item)
```

### Comprehension scope

The loop variable does not leak out.

```python
squares = [n ** 2 for n in range(3)]
# print(n)    # NameError
```

### When NOT to use a comprehension

```python
# Too complex: hard to read
result = [x * y for x in range(10) if x % 2 for y in range(10) if y % 3 if x != y]

# Better: a normal loop
result = []
for x in range(10):
    if x % 2 == 0:
        continue
    for y in range(10):
        if y % 3 and x != y:
            result.append(x * y)
```

| Use a comprehension when | Use a loop when |
|---|---|
| Building a collection from another | You need `break`, or complicated logic |
| One clear transformation or filter | You perform side effects (printing, writing) |
| It fits comfortably on 1 to 2 lines | There are multiple nested conditions |

> **Do not** use comprehensions just for side effects: `[print(x) for x in items]` creates a useless list. Use a `for` loop.

### Performance

Comprehensions are usually faster than the equivalent `for` + `append` loop.

```python
import timeit

loop = """
r = []
for i in range(1000):
    r.append(i * 2)
"""
comp = "r = [i * 2 for i in range(1000)]"

print(timeit.timeit(loop, number=5000))
print(timeit.timeit(comp, number=5000))
```

### Real-world example: parse a CSV-like text

```python
text = """name,age,city
Ali,25,Lahore
Sara,22,Karachi
Bilal,30,Islamabad"""

lines = text.splitlines()
header = lines[0].split(",")
rows = [dict(zip(header, line.split(","))) for line in lines[1:]]

print(rows[0])    # {'name': 'Ali', 'age': '25', 'city': 'Lahore'}

adults = [r["name"] for r in rows if int(r["age"]) > 24]
print(adults)     # ['Ali', 'Bilal']
```

### Real-world example: group data

```python
words = ["apple", "avocado", "banana", "blueberry", "cherry"]

groups = {
    letter: [w for w in words if w[0] == letter]
    for letter in {w[0] for w in words}
}
print(groups)
```

---

## 9.2 Python Generators

A **generator** is a function that produces values **one at a time** and remembers where it left off. It saves memory and can represent infinite sequences.

### Regular function vs generator

```python
# Regular function: builds the whole list in memory
def squares_list(n):
    result = []
    for i in range(n):
        result.append(i ** 2)
    return result

# Generator: yields one value at a time
def squares_gen(n):
    for i in range(n):
        yield i ** 2
```

### The `yield` keyword

**Syntax**

```python
def generator_name():
    yield value
```

```python
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1

for num in count_up_to(3):
    print(num)
```

```
1
2
3
```

### How a generator runs

```python
def demo():
    print("Start")
    yield 1
    print("Middle")
    yield 2
    print("End")

g = demo()          # nothing runs yet
print(next(g))      # prints "Start", then 1
print(next(g))      # prints "Middle", then 2
# next(g)           # prints "End", then raises StopIteration
```

Each `next()` runs the function until the next `yield`, then **pauses**, keeping all its local variables.

### Using generators

```python
def numbers():
    yield 10
    yield 20
    yield 30

print(list(numbers()))     # [10, 20, 30]
print(sum(numbers()))      # 60
print(max(numbers()))      # 30

for n in numbers():
    print(n)
```

### Generators are single-use

```python
g = numbers()
print(list(g))    # [10, 20, 30]
print(list(g))    # []  (already exhausted)
```

Create a new generator to start again.

### Memory comparison

```python
import sys

as_list = [n for n in range(1_000_000)]
as_gen = (n for n in range(1_000_000))

print(sys.getsizeof(as_list))    # about 8,000,000 bytes
print(sys.getsizeof(as_gen))     # about 200 bytes
```

### Infinite generators

```python
def naturals():
    n = 1
    while True:
        yield n
        n += 1

g = naturals()
print(next(g), next(g), next(g))    # 1 2 3

from itertools import islice
print(list(islice(naturals(), 5)))  # [1, 2, 3, 4, 5]
```

### Fibonacci generator

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

from itertools import islice
print(list(islice(fibonacci(), 10)))
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

### Reading large files lazily

```python
def read_lines(path):
    with open(path, encoding="utf-8") as f:
        for line in f:
            yield line.rstrip("\n")

def grep(lines, word):
    for line in lines:
        if word in line:
            yield line

for match in grep(read_lines("big.log"), "ERROR"):
    print(match)
```

Only one line is in memory at a time, even for a multi-gigabyte file.

### Pipelines

Chain generators like an assembly line.

```python
def numbers():
    for i in range(1, 11):
        yield i

def square(nums):
    for n in nums:
        yield n ** 2

def only_even(nums):
    for n in nums:
        if n % 2 == 0:
            yield n

pipeline = only_even(square(numbers()))
print(list(pipeline))    # [4, 16, 36, 64, 100]
```

### `yield from`: delegate to another iterable

```python
def letters():
    yield from "abc"
    yield from [1, 2, 3]

print(list(letters()))    # ['a', 'b', 'c', 1, 2, 3]
```

**Flatten nested lists**

```python
def flatten(items):
    for item in items:
        if isinstance(item, list):
            yield from flatten(item)
        else:
            yield item

print(list(flatten([1, [2, [3, 4]], 5])))    # [1, 2, 3, 4, 5]
```

### `send()`: two-way communication

```python
def accumulator():
    total = 0
    while True:
        value = yield total
        if value is None:
            break
        total += value

g = accumulator()
next(g)                 # start the generator
print(g.send(10))       # 10
print(g.send(5))        # 15
print(g.send(20))       # 35
```

### `return` and `StopIteration`

```python
def counter():
    yield 1
    yield 2
    return "done"

g = counter()
print(next(g))    # 1
print(next(g))    # 2
try:
    next(g)
except StopIteration as e:
    print(e.value)    # done
```

### `close()` and `throw()`

```python
def worker():
    try:
        while True:
            yield "working"
    except GeneratorExit:
        print("Cleaning up")

g = worker()
next(g)
g.close()    # Cleaning up
```

```python
def safe():
    while True:
        try:
            yield
        except ValueError:
            print("Handled inside generator")

g = safe()
next(g)
g.throw(ValueError)
```

### Generator-based class iteration

```python
class Countdown:
    def __init__(self, start):
        self.start = start

    def __iter__(self):
        n = self.start
        while n > 0:
            yield n
            n -= 1

print(list(Countdown(5)))    # [5, 4, 3, 2, 1]
```

This is far simpler than writing `__next__` by hand.

### Useful `itertools` with generators

```python
from itertools import islice, chain, count, cycle, takewhile, groupby

print(list(islice(count(10, 5), 4)))              # [10, 15, 20, 25]
print(list(chain([1, 2], [3, 4])))                # [1, 2, 3, 4]
print(list(islice(cycle("AB"), 5)))               # ['A', 'B', 'A', 'B', 'A']
print(list(takewhile(lambda x: x < 5, count())))  # [0, 1, 2, 3, 4]

data = sorted(["apple", "avocado", "banana", "blueberry"])
for key, group in groupby(data, key=lambda w: w[0]):
    print(key, list(group))
```

### Generator expression vs generator function

| | Generator expression | Generator function |
|---|---|---|
| Syntax | `(x * 2 for x in items)` | `def f(): yield ...` |
| Best for | Simple one-liners | Multi-step logic and state |

### Common mistakes

| Mistake | Explanation |
|---|---|
| Reusing an exhausted generator | Create a new one |
| Calling `len()` on a generator | Not supported; convert to a list first |
| Indexing `g[0]` | Not supported |
| Using `return value` expecting it in a loop | `return` ends the generator; its value is in `StopIteration.value` |

### Real-world example: batch processing

```python
def batches(items, size):
    batch = []
    for item in items:
        batch.append(item)
        if len(batch) == size:
            yield batch
            batch = []
    if batch:
        yield batch

for b in batches(range(1, 11), 4):
    print(b)
```

```
[1, 2, 3, 4]
[5, 6, 7, 8]
[9, 10]
```

### Real-world example: paginated API

```python
def fetch_all(get_page):
    page = 1
    while True:
        items = get_page(page)
        if not items:
            return
        yield from items
        page += 1
```

---

## 9.3 Python Decorators

A **decorator** is a function that **wraps another function** to add behavior, without changing the original code.

### Prerequisite ideas

```python
# 1. Functions are objects
def shout(text):
    return text.upper()

yell = shout
print(yell("hi"))    # HI

# 2. Functions can be passed as arguments
def apply(func, value):
    return func(value)

print(apply(shout, "hello"))    # HELLO

# 3. Functions can return functions (closures)
def make_adder(n):
    def add(x):
        return x + n
    return add

print(make_adder(5)(10))    # 15
```

### A simple decorator

```python
def my_decorator(func):
    def wrapper():
        print("Before the function")
        func()
        print("After the function")
    return wrapper

def say_hello():
    print("Hello!")

say_hello = my_decorator(say_hello)
say_hello()
```

```
Before the function
Hello!
After the function
```

### The `@` syntax

`@my_decorator` is shorthand for `say_hello = my_decorator(say_hello)`.

```python
@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```

### Decorating functions with arguments

Use `*args` and `**kwargs` so the wrapper accepts anything.

```python
def log_calls(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with {args} {kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_calls
def add(a, b):
    return a + b

add(3, 4)
```

```
Calling add with (3, 4) {}
add returned 7
```

### Always use `functools.wraps`

Without it, the decorated function loses its name and docstring.

```python
from functools import wraps

def log_calls(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@log_calls
def add(a, b):
    """Add two numbers."""
    return a + b

print(add.__name__)    # add         (not "wrapper")
print(add.__doc__)     # Add two numbers.
```

### Standard decorator template

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        # code before
        result = func(*args, **kwargs)
        # code after
        return result
    return wrapper
```

### Timer decorator

```python
import time
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow_sum(n):
    return sum(range(n))

slow_sum(5_000_000)    # slow_sum took 0.0xxxs
```

### Decorators with arguments

Add one more layer: a function that **returns** the decorator.

```python
from functools import wraps

def repeat(times):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            result = None
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

greet("Ali")
```

```
Hello, Ali!
Hello, Ali!
Hello, Ali!
```

`@repeat(3)` first calls `repeat(3)` to get the decorator, then applies it.

### Retry decorator

```python
import time
from functools import wraps

def retry(attempts=3, delay=1, exceptions=(Exception,)):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for i in range(1, attempts + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    print(f"Attempt {i} failed: {e}")
                    if i == attempts:
                        raise
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(attempts=3, delay=0.5, exceptions=(ConnectionError,))
def fetch():
    raise ConnectionError("Server down")
```

### Access control / validation

```python
from functools import wraps

def require_login(func):
    @wraps(func)
    def wrapper(user, *args, **kwargs):
        if not user.get("logged_in"):
            raise PermissionError("Please log in first")
        return func(user, *args, **kwargs)
    return wrapper

@require_login
def view_profile(user):
    return f"Profile of {user['name']}"

print(view_profile({"name": "Ali", "logged_in": True}))
# view_profile({"name": "Sara", "logged_in": False})    # PermissionError
```

```python
def validate_positive(func):
    @wraps(func)
    def wrapper(*args):
        if any(a < 0 for a in args):
            raise ValueError("All arguments must be non-negative")
        return func(*args)
    return wrapper
```

### Stacking decorators

Applied from the **bottom up**.

```python
def bold(func):
    @wraps(func)
    def wrapper(*a, **k):
        return f"<b>{func(*a, **k)}</b>"
    return wrapper

def italic(func):
    @wraps(func)
    def wrapper(*a, **k):
        return f"<i>{func(*a, **k)}</i>"
    return wrapper

@bold
@italic
def text():
    return "Hello"

print(text())    # <b><i>Hello</i></b>
```

`@bold @italic def text()` equals `text = bold(italic(text))`.

### Class-based decorators

A class with `__call__` can be a decorator and can **hold state**.

```python
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0

    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call #{self.count}")
        return self.func(*args, **kwargs)

@CountCalls
def hello():
    print("Hello")

hello()    # Call #1
hello()    # Call #2
print(hello.count)    # 2
```

### Decorating classes

```python
def add_repr(cls):
    def __repr__(self):
        attrs = ", ".join(f"{k}={v!r}" for k, v in vars(self).items())
        return f"{cls.__name__}({attrs})"
    cls.__repr__ = __repr__
    return cls

@add_repr
class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y

print(Point(1, 2))    # Point(x=1, y=2)
```

### Built-in decorators

| Decorator | Purpose |
|---|---|
| `@property` | Method acts like an attribute |
| `@classmethod` | Method receives the class |
| `@staticmethod` | Method receives neither |
| `@functools.wraps` | Preserve wrapped function metadata |
| `@functools.lru_cache` | Cache results |
| `@functools.cache` | Unbounded cache (3.9+) |
| `@functools.cached_property` | Compute once per instance |
| `@functools.singledispatch` | Overload by argument type |
| `@functools.total_ordering` | Fill in comparison methods |
| `@dataclasses.dataclass` | Auto-generate `__init__`, etc. |
| `@contextlib.contextmanager` | Build context managers from generators |
| `@abc.abstractmethod` | Require subclasses to implement |

### Caching with `lru_cache`

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

print(fib(80))             # 23416728348467685  (instant)
print(fib.cache_info())    # hits, misses, size
fib.cache_clear()
```

Without caching, `fib(80)` would take longer than a lifetime.

### Writing your own cache

```python
from functools import wraps

def memoize(func):
    cache = {}

    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]

    return wrapper
```

### Debug decorator

```python
def debug(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        arg_text = ", ".join([repr(a) for a in args] +
                             [f"{k}={v!r}" for k, v in kwargs.items()])
        print(f"-> {func.__name__}({arg_text})")
        result = func(*args, **kwargs)
        print(f"<- {func.__name__} returned {result!r}")
        return result
    return wrapper
```

### Registering functions

```python
commands = {}

def command(name):
    def decorator(func):
        commands[name] = func
        return func
    return decorator

@command("hello")
def hello():
    print("Hello!")

@command("bye")
def bye():
    print("Goodbye!")

commands["hello"]()    # Hello!
```

This pattern is how web frameworks like Flask register routes (`@app.route("/")`).

### Rate limiting / once-only

```python
def once(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        if not wrapper.called:
            wrapper.called = True
            wrapper.result = func(*args, **kwargs)
        return wrapper.result
    wrapper.called = False
    return wrapper

@once
def initialize():
    print("Initializing...")
    return 42

initialize()    # Initializing...
initialize()    # (silent)
```

### Optional-argument decorators

Work both as `@deco` and `@deco(arg=...)`.

```python
from functools import wraps

def smart(func=None, *, prefix=">>"):
    def decorator(f):
        @wraps(f)
        def wrapper(*args, **kwargs):
            print(prefix, f.__name__)
            return f(*args, **kwargs)
        return wrapper

    if func is not None:       # used as @smart
        return decorator(func)
    return decorator           # used as @smart(prefix="##")

@smart
def a(): pass

@smart(prefix="##")
def b(): pass
```

### Decorator checklist

- Use `@wraps(func)`.
- Accept `*args, **kwargs` and return the function's result.
- Keep decorators small and focused.
- Name them clearly (`timer`, `retry`, `require_login`).

---

## 9.4 Python Context Managers

A **context manager** sets something up, lets you use it, and **guarantees cleanup**, even if an error occurs. You use them with the `with` statement.

### The familiar example

```python
with open("data.txt", "w", encoding="utf-8") as f:
    f.write("Hello")
# file is closed here, even if an error occurred
```

Equivalent to:

```python
f = open("data.txt", "w", encoding="utf-8")
try:
    f.write("Hello")
finally:
    f.close()
```

### Built-in context managers

```python
# Files
with open("a.txt") as f: ...

# Locks (threading)
import threading
lock = threading.Lock()
with lock:
    ...

# Temporary directories
import tempfile
with tempfile.TemporaryDirectory() as folder:
    print(folder)

# Suppress exceptions
from contextlib import suppress
with suppress(FileNotFoundError):
    import os
    os.remove("missing.txt")

# Redirect output
from contextlib import redirect_stdout
import io
buf = io.StringIO()
with redirect_stdout(buf):
    print("captured")
print(buf.getvalue())     # captured

# Decimal precision
from decimal import localcontext
with localcontext() as ctx:
    ctx.prec = 5
```

### Multiple context managers

```python
with open("in.txt", encoding="utf-8") as src, open("out.txt", "w", encoding="utf-8") as dst:
    dst.write(src.read().upper())
```

Python 3.10+ allows parentheses for readability:

```python
with (
    open("in.txt", encoding="utf-8") as src,
    open("out.txt", "w", encoding="utf-8") as dst,
):
    dst.write(src.read().upper())
```

### Creating one with a class

A context manager needs `__enter__` and `__exit__`.

```python
class Timer:
    def __enter__(self):
        import time
        self.start = time.perf_counter()
        print("Timer started")
        return self                    # value for "as"

    def __exit__(self, exc_type, exc_value, traceback):
        import time
        self.elapsed = time.perf_counter() - self.start
        print(f"Elapsed: {self.elapsed:.4f}s")
        return False                   # do not suppress exceptions

with Timer() as t:
    sum(range(1_000_000))

print(t.elapsed)
```

### `__exit__` parameters

| Parameter | Meaning |
|---|---|
| `exc_type` | Exception class, or `None` |
| `exc_value` | Exception instance, or `None` |
| `traceback` | Traceback object, or `None` |
| **Return value** | `True` = suppress the exception; `False`/`None` = let it propagate |

```python
class Ignore:
    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc, tb):
        if exc_type is ZeroDivisionError:
            print("Ignored a division error")
            return True        # suppress
        return False

with Ignore():
    1 / 0
print("Program continues")
```

### Generator-based context managers

`@contextmanager` turns a generator into a context manager: shorter and easier.

```python
from contextlib import contextmanager

@contextmanager
def my_context():
    print("Setup")
    try:
        yield "resource"        # value given to "as"
    finally:
        print("Cleanup")

with my_context() as r:
    print("Using", r)
```

```
Setup
Using resource
Cleanup
```

Always use `try...finally` around `yield` so cleanup runs even after errors.

### Practical examples

**Timer**

```python
import time
from contextlib import contextmanager

@contextmanager
def timer(label="Block"):
    start = time.perf_counter()
    try:
        yield
    finally:
        print(f"{label}: {time.perf_counter() - start:.4f}s")

with timer("Sum"):
    sum(range(2_000_000))
```

**Change directory temporarily**

```python
import os
from contextlib import contextmanager

@contextmanager
def working_directory(path):
    old = os.getcwd()
    os.chdir(path)
    try:
        yield
    finally:
        os.chdir(old)

with working_directory("/tmp"):
    print(os.getcwd())
print(os.getcwd())    # back to the original
```

**Temporary environment variable**

```python
import os
from contextlib import contextmanager

@contextmanager
def env_var(name, value):
    old = os.environ.get(name)
    os.environ[name] = value
    try:
        yield
    finally:
        if old is None:
            del os.environ[name]
        else:
            os.environ[name] = old
```

**Database transaction**

```python
from contextlib import contextmanager

@contextmanager
def transaction(conn):
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
```

**Temporary change to an object's attribute**

```python
from contextlib import contextmanager

@contextmanager
def patched(obj, name, value):
    old = getattr(obj, name)
    setattr(obj, name, value)
    try:
        yield
    finally:
        setattr(obj, name, old)
```

### `ContextDecorator`: use as a decorator too

```python
from contextlib import ContextDecorator
import time

class Timed(ContextDecorator):
    def __enter__(self):
        self.start = time.perf_counter()
        return self

    def __exit__(self, *exc):
        print(f"Took {time.perf_counter() - self.start:.4f}s")
        return False

@Timed()
def work():
    sum(range(1_000_000))

work()
```

### `ExitStack`: dynamic number of context managers

```python
from contextlib import ExitStack

filenames = ["a.txt", "b.txt", "c.txt"]

with ExitStack() as stack:
    files = [stack.enter_context(open(name, encoding="utf-8")) for name in filenames]
    for f in files:
        print(f.readline())
# all files are closed here
```

### `closing()`: for objects that have `close()`

```python
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen("https://example.com")) as page:
    html = page.read()
```

### `nullcontext()`: optional context managers

```python
from contextlib import nullcontext

def process(lock=None):
    with lock or nullcontext():
        print("working")
```

### Async context managers

```python
import asyncio

class AsyncResource:
    async def __aenter__(self):
        print("Opening")
        return self

    async def __aexit__(self, exc_type, exc, tb):
        print("Closing")

async def main():
    async with AsyncResource() as r:
        print("Using")

asyncio.run(main())
```

### When to use context managers

- Open and close (files, sockets, database connections)
- Acquire and release (locks, semaphores)
- Start and stop (timers, transactions, mock patches)
- Temporarily change and restore (directory, settings, environment)

---

## 9.5 Python Type Hints

**Type hints** (annotations) describe the types a function expects and returns. Python **does not enforce** them at runtime. They help editors, linters, and type checkers find bugs early, and they document your code.

### Variables

```python
name: str = "Ali"
age: int = 25
price: float = 19.99
active: bool = True
nothing: None = None

count: int            # annotation without a value
```

### Functions

**Syntax**

```python
def function(param: type, param2: type = default) -> return_type:
```

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"

def add(a: int, b: int = 0) -> int:
    return a + b

def log(message: str) -> None:
    print(message)
```

### Hints are not enforced

```python
def double(x: int) -> int:
    return x * 2

print(double("ab"))    # "abab"   (no error at runtime)
```

A checker such as `mypy` would flag this call.

### Collections (Python 3.9+)

```python
names: list[str] = ["Ali", "Sara"]
point: tuple[float, float] = (1.5, 2.5)
scores: dict[str, int] = {"Ali": 90}
unique: set[int] = {1, 2, 3}

mixed_tuple: tuple[int, str, bool] = (1, "a", True)
any_length: tuple[int, ...] = (1, 2, 3, 4)
```

For Python 3.8 and earlier, import from `typing`:

```python
from typing import List, Dict, Tuple, Set

names: List[str] = []
scores: Dict[str, int] = {}
```

### Optional and union types

```python
# Python 3.10+
def find(name: str) -> str | None:
    ...

def parse(value: int | str) -> int:
    return int(value)

# Older style
from typing import Optional, Union

def find_old(name: str) -> Optional[str]:     # same as Union[str, None]
    ...

def parse_old(value: Union[int, str]) -> int:
    ...
```

### `Any`

Opt out of type checking for one value.

```python
from typing import Any

def show(value: Any) -> None:
    print(value)
```

### Callable

```python
from typing import Callable

def apply(func: Callable[[int, int], int], a: int, b: int) -> int:
    return func(a, b)

print(apply(lambda x, y: x + y, 2, 3))    # 5

Handler = Callable[[str], None]
```

### Iterables, sequences, mappings

Prefer abstract types for **parameters**, and concrete types for **return values**.

```python
from collections.abc import Iterable, Sequence, Mapping, Iterator, Generator

def total(numbers: Iterable[int]) -> int:
    return sum(numbers)

def first(items: Sequence[str]) -> str:
    return items[0]

def show(data: Mapping[str, int]) -> None:
    for k, v in data.items():
        print(k, v)

def countdown(n: int) -> Iterator[int]:
    while n > 0:
        yield n
        n -= 1

def gen() -> Generator[int, None, str]:   # yields int, sends None, returns str
    yield 1
    return "done"
```

### Type aliases

```python
Point = tuple[float, float]
Matrix = list[list[float]]
UserId = int

def distance(a: Point, b: Point) -> float:
    return ((a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2) ** 0.5
```

**Explicit alias (3.12+)**

```python
type Vector = list[float]
```

### `Literal`

Restrict a value to specific choices.

```python
from typing import Literal

def set_mode(mode: Literal["read", "write", "append"]) -> None:
    ...

set_mode("read")
# set_mode("delete")    # type checker error
```

### `Final`

```python
from typing import Final

MAX_USERS: Final = 100
# MAX_USERS = 200    # type checker error
```

### `TypedDict`

Describe the shape of a dictionary.

```python
from typing import TypedDict

class User(TypedDict):
    name: str
    age: int

class Profile(TypedDict, total=False):    # keys are optional
    bio: str
    website: str

user: User = {"name": "Ali", "age": 25}
```

### `NamedTuple` and dataclasses

```python
from typing import NamedTuple
from dataclasses import dataclass, field

class Point(NamedTuple):
    x: float
    y: float

@dataclass
class Student:
    name: str
    marks: list[int] = field(default_factory=list)
    active: bool = True

s = Student("Ali", [80, 90])
print(s)    # Student(name='Ali', marks=[80, 90], active=True)
```

### Classes as types

```python
class Animal: ...
class Dog(Animal): ...

def adopt(pet: Animal) -> None:    # accepts Animal or Dog
    ...
```

**Return the class itself**

```python
from typing import Self        # Python 3.11+

class Builder:
    def add(self, x: int) -> Self:
        ...
        return self
```

Older versions can use a string:

```python
class Node:
    def next(self) -> "Node":
        ...
```

or `from __future__ import annotations` at the top of the file.

### Generics with `TypeVar`

A function that works with **any** type while keeping relationships.

```python
from typing import TypeVar

T = TypeVar("T")

def first(items: list[T]) -> T:
    return items[0]

reveal_type(first([1, 2, 3]))       # int
reveal_type(first(["a", "b"]))      # str
```

**Python 3.12+ syntax**

```python
def first[T](items: list[T]) -> T:
    return items[0]

class Stack[T]:
    def __init__(self) -> None:
        self._items: list[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()
```

**Older generic class**

```python
from typing import Generic, TypeVar

T = TypeVar("T")

class Box(Generic[T]):
    def __init__(self, item: T) -> None:
        self.item = item

b: Box[int] = Box(5)
```

### Constrained and bounded TypeVars

```python
from typing import TypeVar

Number = TypeVar("Number", int, float)

def add(a: Number, b: Number) -> Number:
    return a + b

from typing import Hashable
H = TypeVar("H", bound=Hashable)
```

### Protocols (structural typing)

"Anything with these methods is acceptable."

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...

class Circle:
    def draw(self) -> None:
        print("Drawing circle")

def render(item: Drawable) -> None:
    item.draw()

render(Circle())    # fine; Circle never inherited from Drawable
```

### Annotating `*args` and `**kwargs`

```python
def total(*numbers: int) -> int:
    return sum(numbers)            # numbers is tuple[int, ...]

def options(**kwargs: str) -> None:
    ...                            # kwargs is dict[str, str]
```

### Decorators and `ParamSpec`

```python
from typing import Callable, ParamSpec, TypeVar
from functools import wraps

P = ParamSpec("P")
R = TypeVar("R")

def logged(func: Callable[P, R]) -> Callable[P, R]:
    @wraps(func)
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
        print("calling", func.__name__)
        return func(*args, **kwargs)
    return wrapper
```

### `Annotated`

Attach extra information to a type.

```python
from typing import Annotated

Age = Annotated[int, "years, 0 to 120"]
```

### `NewType`

```python
from typing import NewType

UserId = NewType("UserId", int)
OrderId = NewType("OrderId", int)

def get_user(uid: UserId) -> str: ...

get_user(UserId(5))
# get_user(OrderId(5))    # type checker error
```

### `TYPE_CHECKING`: avoid circular imports

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from other_module import BigClass

def process(item: "BigClass") -> None:
    ...
```

### Reading hints at runtime

```python
def f(a: int, b: str = "x") -> bool:
    ...

print(f.__annotations__)
# {'a': <class 'int'>, 'b': <class 'str'>, 'return': <class 'bool'>}

from typing import get_type_hints
print(get_type_hints(f))
```

### Static type checkers

```bash
pip install mypy
mypy script.py
mypy --strict script.py
```

Alternatives: **pyright** (used by Pylance in VS Code), **pyre**, **pytype**.

**Example mypy output**

```python
def add(a: int, b: int) -> int:
    return a + b

add("1", "2")
```

```
error: Argument 1 to "add" has incompatible type "str"; expected "int"
```

### Runtime validation with Pydantic

Type hints are not enforced by Python, but libraries can use them:

```python
# pip install pydantic
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

u = User(name="Ali", age="25")    # "25" is converted to 25
print(u.age + 1)                  # 26
# User(name="Ali", age="abc")     # ValidationError
```

### Benefits and tips

| Benefit | Detail |
|---|---|
| Better editor help | Autocomplete and inline errors |
| Fewer bugs | Catch mismatches before running |
| Living documentation | Signatures explain themselves |
| Safer refactoring | Tools find every affected place |

**Tips**

- Add hints to public functions first.
- Use built-in generics (`list[int]`) on 3.9+ and `X | None` on 3.10+.
- Use `Iterable`/`Sequence`/`Mapping` for parameters.
- Do not over-annotate trivial local variables.
- Introduce hints gradually and run `mypy` or `pyright` in your editor or CI.

### Real-world example: typed inventory

```python
from dataclasses import dataclass

@dataclass
class Item:
    name: str
    price: float
    quantity: int = 0

def total_value(items: list[Item]) -> float:
    return sum(i.price * i.quantity for i in items)

def find_item(items: list[Item], name: str) -> Item | None:
    for item in items:
        if item.name == name:
            return item
    return None

stock = [Item("Pen", 10.0, 50), Item("Book", 250.0, 5)]
print(total_value(stock))        # 1750.0
print(find_item(stock, "Pen"))   # Item(name='Pen', price=10.0, quantity=50)
```

---

## 9.6 Python Regular Expressions: Advanced

This section builds on the basics from Chapter 5. It covers precise control over matching: groups, lookarounds, backreferences, flags, performance, and complete real-world patterns.

```python
import re
```

### Quick recap of functions

| Function | Use |
|---|---|
| `re.search` | First match anywhere |
| `re.match` | Match at the start |
| `re.fullmatch` | Match the whole string |
| `re.findall` | All matches as a list |
| `re.finditer` | All matches as match objects |
| `re.sub` / `re.subn` | Replace |
| `re.split` | Split |
| `re.compile` | Pre-build a pattern |
| `re.escape` | Escape special characters in text |

### `re.escape()`: matching literal text

```python
import re

user_text = "price (USD) + tax?"
pattern = re.escape(user_text)
print(pattern)    # price\ \(USD\)\ \+\ tax\?

print(bool(re.search(pattern, "The price (USD) + tax? is high")))    # True
```

Always escape user-provided text before putting it inside a pattern.

### Named groups

```python
import re

pattern = r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"
m = re.search(pattern, "Order placed on 2026-10-18.")

print(m.group("year"))     # 2026
print(m["month"])          # 10
print(m.groupdict())       # {'year': '2026', 'month': '10', 'day': '18'}
print(m.start("day"), m.end("day"))
```

### Backreferences

Refer back to what a group matched.

```python
import re

# Repeated words
text = "This is is a test test of the the system"
print(re.findall(r"\b(\w+)\s+\1\b", text))    # ['is', 'test', 'the']

# Remove duplicate words
print(re.sub(r"\b(\w+)(\s+\1\b)+", r"\1", text))
# This is a test of the system

# Matching quotes
print(re.findall(r"""(['"])(.*?)\1""", """say 'hi' and "bye" """))
# [("'", 'hi'), ('"', 'bye')]

# Named backreference
print(re.search(r"(?P<q>['\"]).*?(?P=q)", "He said 'hello'").group())
```

### Non-capturing groups

`(?:...)` groups without creating a numbered group.

```python
import re

print(re.findall(r"(?:Mr|Ms|Dr)\. (\w+)", "Dr. Khan met Ms. Ali"))
# ['Khan', 'Ali']
```

### Lookahead and lookbehind (lookarounds)

They match a **position**, not text: they check what comes before or after without consuming it.

| Syntax | Name | Meaning |
|---|---|---|
| `(?=...)` | Positive lookahead | Followed by ... |
| `(?!...)` | Negative lookahead | **Not** followed by ... |
| `(?<=...)` | Positive lookbehind | Preceded by ... |
| `(?<!...)` | Negative lookbehind | **Not** preceded by ... |

```python
import re

text = "cost: $45, tax: 7, total: $52"

print(re.findall(r"(?<=\$)\d+", text))        # ['45', '52']   (preceded by $)
print(re.findall(r"\b\d+\b(?<!\$)", text))    # numbers (see note)
print(re.findall(r"\d+(?= dollars)", "5 dollars, 7 euros"))   # ['5']
print(re.findall(r"\d+(?! dollars)\b", "5 dollars, 7 euros")) # ['7']
print(re.findall(r"(?<!\d)\d{3}(?!\d)", "12 345 6789"))        # ['345']
```

> Lookbehind patterns must have a **fixed width** in the standard `re` module. Use the third-party `regex` module for variable-width lookbehind.

**Add commas to numbers**

```python
import re

def add_commas(n):
    return re.sub(r"(?<=\d)(?=(\d{3})+$)", ",", str(n))

print(add_commas(1234567))    # 1,234,567
```

**Password rules with lookaheads**

```python
import re

strong = re.compile(r"""
    ^
    (?=.*[a-z])          # a lowercase letter
    (?=.*[A-Z])          # an uppercase letter
    (?=.*\d)             # a digit
    (?=.*[^\w\s])        # a special character
    .{8,}                # at least 8 characters
    $
""", re.VERBOSE)

print(bool(strong.match("Abc@1234")))    # True
print(bool(strong.match("abcdefgh")))    # False
```

### Greedy, lazy, and possessive quantifiers

| Type | Syntax | Behavior |
|---|---|---|
| Greedy | `*`, `+`, `?`, `{n,m}` | As much as possible |
| Lazy | `*?`, `+?`, `??`, `{n,m}?` | As little as possible |
| Possessive (3.11+) | `*+`, `++`, `?+` | Greedy with no backtracking |

```python
import re

html = "<b>one</b> <b>two</b>"

print(re.findall(r"<b>.*</b>", html))     # ['<b>one</b> <b>two</b>']
print(re.findall(r"<b>.*?</b>", html))    # ['<b>one</b>', '<b>two</b>']
print(re.findall(r"<b>(.*?)</b>", html))  # ['one', 'two']
```

### Atomic groups (3.11+)

```python
import re

# (?>...) matches once and never backtracks into the group
print(bool(re.match(r"(?>a+)b", "aaab")))    # True
```

### Alternation tips

Order matters: the first alternative that works wins.

```python
import re

print(re.findall(r"cat|category", "category"))      # ['cat']
print(re.findall(r"category|cat", "category"))      # ['category']
print(re.findall(r"cat(?:egory)?", "category cat")) # ['category', 'cat']
```

### Flags

| Flag | Short | Inline | Effect |
|---|---|---|---|
| `re.IGNORECASE` | `re.I` | `(?i)` | Ignore case |
| `re.MULTILINE` | `re.M` | `(?m)` | `^`/`$` match at every line |
| `re.DOTALL` | `re.S` | `(?s)` | `.` matches newline too |
| `re.VERBOSE` | `re.X` | `(?x)` | Whitespace and comments allowed |
| `re.ASCII` | `re.A` | `(?a)` | `\w`, `\d`, `\s` match ASCII only |
| `re.UNICODE` | `re.U` | `(?u)` | Unicode matching (default for `str`) |

Combine with `|`:

```python
import re

text = "Hello\nWORLD"
print(re.findall(r"^w\w+", text, re.I | re.M))    # ['WORLD']
```

**Inline and scoped flags**

```python
import re

print(re.findall(r"(?i)hello", "Hello HELLO"))        # ['Hello', 'HELLO']
print(re.findall(r"(?i:he)llo", "HEllo HELLO"))       # ['HEllo']
```

### Verbose patterns: readable regex

```python
import re

phone = re.compile(r"""
    ^
    (?:\+92|0)           # country code or leading zero
    3\d{2}               # operator code, e.g. 300, 321
    [-\s]?               # optional separator
    \d{7}                # subscriber number
    $
""", re.VERBOSE)

for n in ["0300-1234567", "+92 3211234567", "0400-123"]:
    print(n, bool(phone.match(n)))
```

### Conditional patterns

```python
import re

# (?(1)...|...) : if group 1 matched, use the first, otherwise the second
pattern = r"^(\()?\d{3}(?(1)\)|-)\d{4}$"

print(bool(re.match(pattern, "(555)1234")))    # True
print(bool(re.match(pattern, "555-1234")))     # True
print(bool(re.match(pattern, "(555-1234")))    # False
```

### `finditer()` for positions

```python
import re

text = "cat bat rat"
for m in re.finditer(r"\b\w+at\b", text):
    print(m.group(), m.span())
```

```
cat (0, 3)
bat (4, 7)
rat (8, 11)
```

### `sub()` with a function

```python
import re

def censor(m):
    word = m.group()
    return word[0] + "*" * (len(word) - 1)

print(re.sub(r"\b(?:darn|heck)\b", censor, "Oh darn, what the heck"))
# Oh d***, what the h***
```

```python
text = "Prices: 10, 25, 100"
print(re.sub(r"\d+", lambda m: str(int(m.group()) * 2), text))
# Prices: 20, 50, 200
```

### Named group replacement

```python
import re

text = "2026-10-18"
print(re.sub(r"(?P<y>\d{4})-(?P<m>\d{2})-(?P<d>\d{2})",
             r"\g<d>/\g<m>/\g<y>", text))
# 18/10/2026
```

`\g<name>` or `\g<1>` avoids ambiguity such as `\10`.

### `subn()`: replace and count

```python
import re

new_text, count = re.subn(r"\s+", " ", "a   b    c")
print(new_text, count)    # a b c 2
```

### `split()` that keeps the separators

```python
import re

print(re.split(r"([,;])", "a,b;c"))    # ['a', ',', 'b', ';', 'c']
```

### Matching across lines

```python
import re

text = """START
first line
second line
END"""

print(re.search(r"START.*END", text, re.S).group())

m = re.search(r"^START\n(.*?)\nEND$", text, re.M | re.S)
print(m.group(1))
```

### Tokenizer using named groups

```python
import re

TOKEN = re.compile(r"""
    (?P<NUMBER>\d+(?:\.\d+)?)
  | (?P<NAME>[A-Za-z_]\w*)
  | (?P<OP>[+\-*/=()])
  | (?P<SPACE>\s+)
""", re.VERBOSE)

def tokenize(code):
    for m in TOKEN.finditer(code):
        if m.lastgroup != "SPACE":
            yield m.lastgroup, m.group()

print(list(tokenize("price = 12.5 * (qty + 3)")))
```

```
[('NAME', 'price'), ('OP', '='), ('NUMBER', '12.5'), ('OP', '*'),
 ('OP', '('), ('NAME', 'qty'), ('OP', '+'), ('NUMBER', '3'), ('OP', ')')]
```

### Compile and reuse

```python
import re

EMAIL = re.compile(r"^[\w.+-]+@[\w-]+(?:\.[\w-]+)+$")

def is_email(text):
    return bool(EMAIL.match(text))

print(is_email("ali@example.com"))    # True
```

Python caches recently used patterns, but compiling at module level is cleaner and slightly faster in hot paths.

### Catastrophic backtracking (ReDoS)

Some patterns can take exponential time on certain inputs.

```python
import re

# Dangerous: nested quantifiers
bad = r"^(a+)+$"
# re.match(bad, "a" * 30 + "b")    # may hang for a very long time
```

| Risky pattern | Safer alternative |
|---|---|
| `(a+)+` | `a+` |
| `(.*)*` | `.*` |
| `(a\|aa)+` | `a+` |
| Overlapping alternatives inside a repeat | Make alternatives mutually exclusive |

Use atomic groups or possessive quantifiers (3.11+), and never run untrusted patterns on untrusted input without limits.

### Unicode and non-English text

```python
import re

urdu = "میرا نام علی ہے"
print(re.findall(r"\w+", urdu))    # ['میرا', 'نام', 'علی', 'ہے']

# Unicode ranges
print(re.findall(r"[\u0600-\u06FF]+", "Hello سلام World"))    # ['سلام']
```

The third-party `regex` module adds `\p{L}` (Unicode properties) and more:

```bash
pip install regex
```

```python
import regex

print(regex.findall(r"\p{Lu}\p{Ll}+", "Ali and Sara"))    # ['Ali', 'Sara']
```

### Debugging patterns

```python
import re

re.compile(r"(\d+)-(\w+)", re.DEBUG)    # prints the parsed structure
```

Test interactively at **regex101.com** (choose the Python flavor).

### Real-world patterns

**Extract data from log lines**

```python
import re

LOG = re.compile(
    r"^(?P<ip>\d{1,3}(?:\.\d{1,3}){3}) - - "
    r"\[(?P<time>[^\]]+)\] "
    r'"(?P<method>[A-Z]+) (?P<path>\S+) [^"]*" '
    r"(?P<status>\d{3}) (?P<size>\d+)$"
)

line = '192.168.1.10 - - [18/Oct/2026:19:53:00 +0500] "GET /index.html HTTP/1.1" 200 1534'
m = LOG.match(line)
print(m.groupdict())
```

**URLs**

```python
import re

URL = re.compile(r"https?://[^\s<>\"']+")
text = "Visit https://example.com/page?x=1 or http://test.org."
print([u.rstrip(".,)") for u in URL.findall(text)])
```

**Hex colors**

```python
import re

print(re.findall(r"#(?:[0-9a-fA-F]{3}){1,2}\b", "colors #fff #1a2B3c #12"))
# ['#fff', '#1a2B3c']
```

**Case conversion**

```python
import re

def snake_to_camel(s):
    return re.sub(r"_([a-z])", lambda m: m.group(1).upper(), s)

def camel_to_snake(s):
    return re.sub(r"(?<!^)(?=[A-Z])", "_", s).lower()

print(snake_to_camel("user_first_name"))    # userFirstName
print(camel_to_snake("userFirstName"))      # user_first_name
```

**Template substitution**

```python
import re

template = "Hello {name}, you owe {amount}."
values = {"name": "Ali", "amount": "Rs. 500"}

print(re.sub(r"\{(\w+)\}", lambda m: values[m.group(1)], template))
# Hello Ali, you owe Rs. 500.
```

**Extract key=value pairs**

```python
import re

text = 'name="Ali Khan" age=25 city=Lahore'
pairs = re.findall(r'(\w+)=(?:"([^"]*)"|(\S+))', text)
print({k: q or u for k, q, u in pairs})
# {'name': 'Ali Khan', 'age': '25', 'city': 'Lahore'}
```

### When NOT to use regex

| Task | Better tool |
|---|---|
| Parsing HTML/XML | `BeautifulSoup`, `lxml` |
| Parsing JSON | `json` |
| Parsing CSV | `csv` |
| Parsing dates | `datetime.strptime`, `dateutil` |
| Simple "contains" or "starts with" | `in`, `startswith`, `endswith` |
| Validating full emails and URLs | `email-validator`, `urllib.parse` |

---

## 9.7 Python Async Programming

**Asynchronous programming** lets one program handle many **waiting** tasks (network calls, file or database I/O) at the same time, without needing multiple threads. It uses `async` and `await`.

### The problem it solves

```python
import time

def fetch(n):
    time.sleep(1)       # simulates waiting for a server
    return f"result {n}"

start = time.perf_counter()
results = [fetch(i) for i in range(3)]
print(f"{time.perf_counter() - start:.1f}s")    # about 3.0s
```

Each call waits for the previous one. With async, the three waits **overlap** (about 1 second total).

### Core vocabulary

| Term | Meaning |
|---|---|
| **Coroutine** | The object created by calling an `async def` function |
| **Event loop** | The scheduler that runs coroutines and switches between them |
| **`await`** | "Pause here until this finishes, and let others run meanwhile" |
| **Task** | A coroutine scheduled to run on the event loop |

### Your first coroutine

```python
import asyncio

async def hello():
    print("Hello")
    await asyncio.sleep(1)
    print("World")

asyncio.run(hello())
```

- `async def` defines a coroutine function.
- Calling `hello()` does **not** run it. It returns a coroutine object.
- `asyncio.run()` starts the event loop and runs it.

```python
coro = hello()
print(type(coro))    # <class 'coroutine'>
coro.close()
```

### `await`

`await` can only be used inside an `async def` function.

```python
import asyncio

async def get_number():
    await asyncio.sleep(1)
    return 42

async def main():
    value = await get_number()
    print(value)    # 42

asyncio.run(main())
```

### Sequential vs concurrent

```python
import asyncio, time

async def fetch(n):
    await asyncio.sleep(1)
    return f"result {n}"

async def sequential():
    return [await fetch(i) for i in range(3)]       # 3 seconds

async def concurrent():
    return await asyncio.gather(fetch(0), fetch(1), fetch(2))    # 1 second

for runner in (sequential, concurrent):
    start = time.perf_counter()
    print(asyncio.run(runner()), f"{time.perf_counter() - start:.1f}s")
```

```
['result 0', 'result 1', 'result 2'] 3.0s
['result 0', 'result 1', 'result 2'] 1.0s
```

> Writing `await` one after another runs things **in sequence**. To run them **together**, use `gather()` or tasks.

### `asyncio.gather()`

Runs many awaitables together and returns results **in order**.

```python
import asyncio

async def square(n):
    await asyncio.sleep(0.5)
    return n * n

async def main():
    results = await asyncio.gather(*(square(n) for n in range(5)))
    print(results)    # [0, 1, 4, 9, 16]

asyncio.run(main())
```

**Handle exceptions**

```python
async def might_fail(n):
    if n == 2:
        raise ValueError("bad item")
    return n

async def main():
    results = await asyncio.gather(
        might_fail(1), might_fail(2), might_fail(3),
        return_exceptions=True,
    )
    print(results)    # [1, ValueError('bad item'), 3]
```

### Tasks: `create_task()`

Starts a coroutine **immediately in the background**.

```python
import asyncio

async def worker(name, delay):
    await asyncio.sleep(delay)
    print(f"{name} done")
    return name

async def main():
    t1 = asyncio.create_task(worker("A", 2))
    t2 = asyncio.create_task(worker("B", 1))

    print("Tasks started")
    r1 = await t1
    r2 = await t2
    print(r1, r2)

asyncio.run(main())
```

```
Tasks started
B done
A done
A B
```

### `TaskGroup` (Python 3.11+): structured concurrency

Safer than loose tasks: if one fails, the rest are cancelled, and all are awaited.

```python
import asyncio

async def work(n):
    await asyncio.sleep(n)
    return n * 10

async def main():
    async with asyncio.TaskGroup() as tg:
        t1 = tg.create_task(work(1))
        t2 = tg.create_task(work(2))
    print(t1.result(), t2.result())    # 10 20

asyncio.run(main())
```

### Timeouts

```python
import asyncio

async def slow():
    await asyncio.sleep(10)

async def main():
    try:
        await asyncio.wait_for(slow(), timeout=2)
    except asyncio.TimeoutError:
        print("Took too long")

asyncio.run(main())
```

Python 3.11+ also offers:

```python
async def main():
    try:
        async with asyncio.timeout(2):
            await slow()
    except TimeoutError:
        print("Timed out")
```

### Cancellation

```python
import asyncio

async def forever():
    try:
        while True:
            await asyncio.sleep(1)
    except asyncio.CancelledError:
        print("Cleaning up")
        raise

async def main():
    task = asyncio.create_task(forever())
    await asyncio.sleep(2)
    task.cancel()
    try:
        await task
    except asyncio.CancelledError:
        print("Task cancelled")

asyncio.run(main())
```

### `asyncio.wait()` and `as_completed()`

```python
import asyncio

async def job(n):
    await asyncio.sleep(n)
    return n

async def main():
    tasks = [asyncio.create_task(job(n)) for n in (3, 1, 2)]

    for finished in asyncio.as_completed(tasks):
        print("Finished:", await finished)     # 1, then 2, then 3

asyncio.run(main())
```

### Limiting concurrency: `Semaphore`

```python
import asyncio

sem = asyncio.Semaphore(3)       # at most 3 at the same time

async def limited(n):
    async with sem:
        print(f"start {n}")
        await asyncio.sleep(1)
        print(f"end {n}")

async def main():
    await asyncio.gather(*(limited(i) for i in range(10)))

asyncio.run(main())
```

### Locks and events

```python
import asyncio

lock = asyncio.Lock()
counter = 0

async def increment():
    global counter
    async with lock:
        value = counter
        await asyncio.sleep(0)
        counter = value + 1

async def main():
    await asyncio.gather(*(increment() for _ in range(100)))
    print(counter)    # 100

asyncio.run(main())
```

```python
event = asyncio.Event()

async def waiter():
    await event.wait()
    print("Event received")

async def setter():
    await asyncio.sleep(1)
    event.set()
```

### Queues: producer and consumer

```python
import asyncio

async def producer(queue):
    for i in range(5):
        await queue.put(i)
        await asyncio.sleep(0.2)
    await queue.put(None)             # signal "no more items"

async def consumer(queue):
    while True:
        item = await queue.get()
        if item is None:
            break
        print("Processing", item)
        queue.task_done()

async def main():
    queue = asyncio.Queue(maxsize=3)
    await asyncio.gather(producer(queue), consumer(queue))

asyncio.run(main())
```

### Async iterators and generators

```python
import asyncio

async def countdown(n):
    while n > 0:
        yield n
        await asyncio.sleep(0.5)
        n -= 1

async def main():
    async for i in countdown(3):
        print(i)

    squares = [x * x async for x in countdown(4)]
    print(squares)

asyncio.run(main())
```

### Async context managers

```python
import asyncio
from contextlib import asynccontextmanager

@asynccontextmanager
async def connection(name):
    print(f"Connecting to {name}")
    try:
        yield name
    finally:
        print(f"Closing {name}")

async def main():
    async with connection("db") as c:
        print("Using", c)

asyncio.run(main())
```

### Blocking code inside async programs

Blocking calls (`time.sleep`, `requests.get`, heavy CPU work) **freeze the whole event loop**.

```python
import asyncio, time

async def bad():
    time.sleep(2)              # blocks everything
    return "done"

async def good():
    await asyncio.sleep(2)     # lets others run
    return "done"
```

**Run blocking code in a thread**

```python
import asyncio, time

def blocking_task(n):
    time.sleep(n)
    return f"slept {n}"

async def main():
    result = await asyncio.to_thread(blocking_task, 2)     # Python 3.9+
    print(result)

    results = await asyncio.gather(
        asyncio.to_thread(blocking_task, 1),
        asyncio.to_thread(blocking_task, 1),
    )
    print(results)

asyncio.run(main())
```

**Run CPU-heavy work in a process pool**

```python
import asyncio
from concurrent.futures import ProcessPoolExecutor

def heavy(n):
    return sum(i * i for i in range(n))

async def main():
    loop = asyncio.get_running_loop()
    with ProcessPoolExecutor() as pool:
        result = await loop.run_in_executor(pool, heavy, 5_000_000)
    print(result)

if __name__ == "__main__":
    asyncio.run(main())
```

### Async HTTP requests

The `requests` library is blocking. Use `aiohttp` or `httpx`.

```bash
pip install aiohttp
```

```python
import asyncio
import aiohttp

URLS = [
    "https://example.com",
    "https://www.python.org",
    "https://httpbin.org/get",
]

async def fetch(session, url):
    async with session.get(url) as response:
        text = await response.text()
        return url, response.status, len(text)

async def main():
    async with aiohttp.ClientSession() as session:
        results = await asyncio.gather(*(fetch(session, u) for u in URLS))
    for url, status, size in results:
        print(status, size, url)

asyncio.run(main())
```

**With `httpx`**

```python
import asyncio
import httpx

async def main():
    async with httpx.AsyncClient() as client:
        responses = await asyncio.gather(
            client.get("https://example.com"),
            client.get("https://www.python.org"),
        )
    print([r.status_code for r in responses])

asyncio.run(main())
```

### Async file I/O

The built-in `open()` blocks. Use `asyncio.to_thread` or the `aiofiles` package.

```python
import asyncio

def read_file(path):
    with open(path, encoding="utf-8") as f:
        return f.read()

async def main():
    text = await asyncio.to_thread(read_file, "notes.txt")
    print(len(text))
```

### Common mistakes

| Mistake | Fix |
|---|---|
| Forgetting `await` | `fetch()` returns a coroutine; write `await fetch()` |
| Using `time.sleep()` | Use `await asyncio.sleep()` |
| Calling `asyncio.run()` inside a running loop (Jupyter) | `await main()` directly in the notebook |
| Awaiting coroutines one by one | Use `gather()` or `TaskGroup` for concurrency |
| Using blocking libraries | Choose async libraries or `asyncio.to_thread` |
| Creating a task and dropping the reference | Keep a reference, or tasks may be garbage-collected |
| Mixing sync and async carelessly | Keep a clear boundary |

> Forgetting `await` produces: `RuntimeWarning: coroutine 'fetch' was never awaited`.

### Debug mode

```python
asyncio.run(main(), debug=True)
```

or run with `python -X dev script.py`.

### When to use async

| Good fit | Poor fit |
|---|---|
| Many network requests | Heavy CPU number-crunching |
| Web servers (FastAPI, aiohttp) | Simple scripts with a single task |
| Chat bots, websockets | Code that relies on blocking libraries |
| Scrapers hitting many pages | |

### Real-world example: concurrent downloader with a limit

```python
import asyncio
import aiohttp

async def download(session, sem, url):
    async with sem:
        async with session.get(url) as resp:
            data = await resp.read()
            print(f"{url}: {len(data)} bytes")
            return len(data)

async def main(urls):
    sem = asyncio.Semaphore(5)
    async with aiohttp.ClientSession() as session:
        sizes = await asyncio.gather(*(download(session, sem, u) for u in urls))
    print("Total:", sum(sizes))

asyncio.run(main(["https://example.com"] * 10))
```

### Real-world example: periodic task

```python
import asyncio

async def heartbeat(interval):
    while True:
        print("tick")
        await asyncio.sleep(interval)

async def main():
    task = asyncio.create_task(heartbeat(1))
    await asyncio.sleep(3.5)
    task.cancel()

asyncio.run(main())
```

---

## 9.8 Python Multithreading and Multiprocessing

Both let a program do several things **at the same time**, but in different ways.

| | Threads | Processes |
|---|---|---|
| What | Several flows of execution inside **one** process | Several **separate** Python processes |
| Memory | Shared | Separate (data must be sent between them) |
| Start-up cost | Low | Higher |
| Best for | **I/O-bound** work (network, disk, waiting) | **CPU-bound** work (math, image processing) |
| Affected by the GIL | Yes | No (each process has its own) |

### The GIL (Global Interpreter Lock)

In standard CPython, only **one thread runs Python bytecode at a time**.

- Threads still help when the program is **waiting** (network, disk, `sleep`), because the lock is released while waiting.
- Threads do **not** speed up pure-Python CPU work.
- Processes avoid the limit by using separate interpreters.
- Python 3.13+ offers an experimental free-threaded build (no GIL), but it is not the default.

### Choosing the right tool

```
Waiting on network/disk/database?      → threads (or asyncio)
Heavy calculation using many CPU cores? → multiprocessing
Thousands of connections?               → asyncio
Simple, few tasks?                      → threads / ThreadPoolExecutor
```

### Multithreading basics

```python
import threading
import time

def worker(name, delay):
    print(f"{name} starting")
    time.sleep(delay)
    print(f"{name} done")

t1 = threading.Thread(target=worker, args=("A", 2))
t2 = threading.Thread(target=worker, args=("B", 1))

t1.start()
t2.start()

t1.join()      # wait for t1 to finish
t2.join()
print("All done")
```

```
A starting
B starting
B done
A done
All done
```

### Thread options

```python
t = threading.Thread(
    target=worker,
    args=("A", 1),
    kwargs={},
    name="worker-A",
    daemon=True,       # dies automatically when the main program exits
)
```

```python
print(threading.current_thread().name)
print(threading.active_count())
print(t.is_alive())
```

### Sequential vs threaded

```python
import threading, time

def download(i):
    time.sleep(1)          # simulates waiting for I/O

start = time.perf_counter()
for i in range(5):
    download(i)
print(f"Sequential: {time.perf_counter() - start:.1f}s")    # about 5s

start = time.perf_counter()
threads = [threading.Thread(target=download, args=(i,)) for i in range(5)]
for t in threads: t.start()
for t in threads: t.join()
print(f"Threaded:   {time.perf_counter() - start:.1f}s")    # about 1s
```

### Race conditions and locks

Two threads changing shared data can corrupt it.

```python
import threading

counter = 0

def increment():
    global counter
    for _ in range(100_000):
        counter += 1           # read, add, write: not atomic

threads = [threading.Thread(target=increment) for _ in range(4)]
for t in threads: t.start()
for t in threads: t.join()

print(counter)    # may be less than 400000
```

**Fix with a `Lock`**

```python
import threading

counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(100_000):
        with lock:
            counter += 1

threads = [threading.Thread(target=increment) for _ in range(4)]
for t in threads: t.start()
for t in threads: t.join()

print(counter)    # 400000
```

### Other synchronization tools

| Tool | Purpose |
|---|---|
| `Lock` | Only one thread at a time |
| `RLock` | A lock the same thread can acquire repeatedly |
| `Semaphore(n)` | At most `n` threads at a time |
| `Event` | One thread signals others |
| `Condition` | Wait for a condition to become true |
| `Barrier(n)` | Wait until `n` threads reach a point |
| `Queue` | Thread-safe data passing |

```python
import threading

sem = threading.Semaphore(2)

def limited(i):
    with sem:
        print("running", i)

event = threading.Event()

def waiter():
    event.wait()
    print("Got the signal")

threading.Thread(target=waiter).start()
event.set()
```

**Deadlock warning:** if thread A holds lock 1 and waits for lock 2 while thread B holds lock 2 and waits for lock 1, both freeze. Always acquire locks in the same order.

### Thread-safe communication with `Queue`

```python
import threading
import queue

q = queue.Queue()

def producer():
    for i in range(5):
        q.put(i)
    q.put(None)

def consumer():
    while True:
        item = q.get()
        if item is None:
            break
        print("Got", item)

p = threading.Thread(target=producer)
c = threading.Thread(target=consumer)
p.start(); c.start()
p.join(); c.join()
```

### `ThreadPoolExecutor` (recommended)

A simpler, higher-level interface.

```python
from concurrent.futures import ThreadPoolExecutor
import time

def fetch(n):
    time.sleep(1)
    return f"result {n}"

with ThreadPoolExecutor(max_workers=5) as pool:
    results = list(pool.map(fetch, range(5)))

print(results)    # about 1 second in total
```

**`submit()` and `as_completed()`**

```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def task(n):
    time.sleep(n)
    return n

with ThreadPoolExecutor() as pool:
    futures = {pool.submit(task, n): n for n in (3, 1, 2)}

    for future in as_completed(futures):
        print("Finished", future.result())     # 1, 2, 3
```

**Handling errors**

```python
def risky(n):
    if n == 2:
        raise ValueError("bad")
    return n

with ThreadPoolExecutor() as pool:
    futures = [pool.submit(risky, n) for n in range(4)]
    for f in futures:
        try:
            print(f.result())
        except ValueError as e:
            print("Error:", e)
```

### Thread-local data

```python
import threading

local = threading.local()

def work(name):
    local.name = name
    print(local.name)
```

### Multiprocessing basics

```python
import multiprocessing
import time

def worker(n):
    print(f"Process {n} starting")
    time.sleep(1)
    print(f"Process {n} done")

if __name__ == "__main__":
    processes = [multiprocessing.Process(target=worker, args=(i,)) for i in range(3)]
    for p in processes:
        p.start()
    for p in processes:
        p.join()
```

> The `if __name__ == "__main__":` guard is **required** on Windows and macOS. Without it, each new process re-runs your script and can create an endless chain of processes.

### CPU-bound speedup

```python
import time
from multiprocessing import Pool

def heavy(n):
    return sum(i * i for i in range(n))

if __name__ == "__main__":
    numbers = [5_000_000] * 8

    start = time.perf_counter()
    results = [heavy(n) for n in numbers]
    print(f"Sequential: {time.perf_counter() - start:.2f}s")

    start = time.perf_counter()
    with Pool() as pool:                    # one worker per CPU core
        results = pool.map(heavy, numbers)
    print(f"Pool:       {time.perf_counter() - start:.2f}s")
```

### `Pool` methods

| Method | Use |
|---|---|
| `pool.map(f, items)` | Apply `f` to every item, return a list in order |
| `pool.imap(f, items)` | Lazy version of `map` |
| `pool.imap_unordered(f, items)` | Results as they finish |
| `pool.starmap(f, [(a, b), ...])` | Several arguments per call |
| `pool.apply_async(f, args)` | Run one call without blocking |
| `pool.close()` / `pool.join()` | Finish up (done automatically by `with`) |

```python
from multiprocessing import Pool

def power(a, b):
    return a ** b

if __name__ == "__main__":
    with Pool(4) as pool:
        print(pool.starmap(power, [(2, 3), (3, 2), (4, 4)]))    # [8, 9, 256]
```

### `ProcessPoolExecutor`

Same interface as the thread pool.

```python
from concurrent.futures import ProcessPoolExecutor

def heavy(n):
    return sum(i * i for i in range(n))

if __name__ == "__main__":
    with ProcessPoolExecutor() as pool:
        results = list(pool.map(heavy, [3_000_000] * 4))
    print(results)
```

### Sharing data between processes

Processes have separate memory, so use special tools.

**Queue**

```python
from multiprocessing import Process, Queue

def producer(q):
    for i in range(3):
        q.put(i)
    q.put(None)

if __name__ == "__main__":
    q = Queue()
    p = Process(target=producer, args=(q,))
    p.start()

    while (item := q.get()) is not None:
        print("Got", item)
    p.join()
```

**Pipe**

```python
from multiprocessing import Process, Pipe

def child(conn):
    conn.send("hello from child")
    conn.close()

if __name__ == "__main__":
    parent_conn, child_conn = Pipe()
    p = Process(target=child, args=(child_conn,))
    p.start()
    print(parent_conn.recv())
    p.join()
```

**Shared value and lock**

```python
from multiprocessing import Process, Value, Lock

def add(counter, lock):
    for _ in range(10_000):
        with lock:
            counter.value += 1

if __name__ == "__main__":
    counter = Value("i", 0)
    lock = Lock()

    procs = [Process(target=add, args=(counter, lock)) for _ in range(4)]
    for p in procs: p.start()
    for p in procs: p.join()

    print(counter.value)    # 40000
```

**Manager (shared lists and dicts)**

```python
from multiprocessing import Manager, Process

def work(shared, key):
    shared[key] = key * 2

if __name__ == "__main__":
    with Manager() as manager:
        shared = manager.dict()
        procs = [Process(target=work, args=(shared, i)) for i in range(4)]
        for p in procs: p.start()
        for p in procs: p.join()
        print(dict(shared))
```

**Shared memory (Python 3.8+)**

```python
from multiprocessing import shared_memory

shm = shared_memory.SharedMemory(create=True, size=10)
shm.buf[0] = 42
print(shm.buf[0])
shm.close()
shm.unlink()
```

### Start methods

| Method | Platforms | Notes |
|---|---|---|
| `spawn` | Windows, macOS (default) | Starts a fresh interpreter. Safest. Requires the `__main__` guard. |
| `fork` | Linux (default before 3.14) | Fast copy of the parent. Can misbehave with threads. |
| `forkserver` | Unix | Compromise |

```python
import multiprocessing
multiprocessing.set_start_method("spawn")
```

### What can be passed to a process

Arguments and return values are **pickled** (serialized).

- Works: numbers, strings, lists, dicts, most classes, top-level functions.
- Fails: lambdas, locally defined functions, open files, sockets, locks.

```python
# Fails with a lambda:
# pool.map(lambda x: x * 2, range(5))     # PicklingError

# Works with a named, top-level function:
def double(x):
    return x * 2
```

### Threads vs processes vs async compared

| Feature | `threading` | `multiprocessing` | `asyncio` |
|---|---|---|---|
| Concurrency model | Preemptive threads | Separate processes | Cooperative tasks |
| Uses multiple CPU cores | No (GIL) | **Yes** | No |
| Memory sharing | Shared | Separate | Shared |
| Overhead | Low | High | Very low |
| Scales to thousands | Hundreds | Tens | **Thousands** |
| Best for | Blocking I/O | CPU-heavy work | Many network connections |
| Needs async libraries | No | No | Yes |
| Risk | Race conditions, deadlocks | Pickling limits, memory | Blocking the loop |

### Real-world example: threaded URL checker

```python
from concurrent.futures import ThreadPoolExecutor
from urllib.request import urlopen
from urllib.error import URLError

URLS = [
    "https://www.python.org",
    "https://example.com",
    "https://httpbin.org/status/404",
]

def check(url):
    try:
        with urlopen(url, timeout=5) as r:
            return url, r.status
    except URLError as e:
        return url, str(e)

with ThreadPoolExecutor(max_workers=10) as pool:
    for url, status in pool.map(check, URLS):
        print(status, url)
```

### Real-world example: parallel image or data processing

```python
from concurrent.futures import ProcessPoolExecutor
from pathlib import Path

def process_file(path):
    text = Path(path).read_text(encoding="utf-8")
    return path, len(text.split())

if __name__ == "__main__":
    files = list(Path("documents").glob("*.txt"))
    with ProcessPoolExecutor() as pool:
        for path, words in pool.map(process_file, files):
            print(f"{words:>8} words  {path}")
```

### Best practices

- Prefer `ThreadPoolExecutor` / `ProcessPoolExecutor` over managing threads and processes manually.
- Always guard multiprocessing code with `if __name__ == "__main__":`.
- Avoid shared mutable state. Pass messages through queues.
- Keep critical sections (locked code) as short as possible.
- Measure first. Concurrency adds complexity and does not always add speed.
- Set `daemon=True` carefully. Daemon threads can be killed mid-operation.
- Use timeouts when waiting (`join(timeout=...)`, `result(timeout=...)`).

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Comprehensions | `[x for x in it if cond]`, dict and set versions, generator expressions; keep them readable |
| Generators | `yield` produces values lazily; memory efficient; `yield from`; pipelines; infinite sequences |
| Decorators | Functions that wrap functions; use `@wraps`; with arguments need an extra layer; `lru_cache`, `timer`, `retry` |
| Context managers | `with` guarantees cleanup; `__enter__`/`__exit__` or `@contextmanager`; `ExitStack`, `suppress` |
| Type hints | Annotate variables and functions; `list[int]`, `X \| None`, `TypeVar`, `Protocol`; check with `mypy` |
| Advanced RegEx | Named groups, backreferences, lookarounds, `re.VERBOSE`, `sub` with functions, avoid catastrophic backtracking |
| Async | `async def`, `await`, `asyncio.run`, `gather`, `TaskGroup`; never block the event loop |
| Threads and processes | Threads for I/O (GIL limits CPU work); processes for CPU work; use executors; guard with `__main__`; use locks and queues |

### Quick syntax cheat sheet

```python
# Comprehensions
[x ** 2 for x in range(10) if x % 2 == 0]
{k: v for k, v in pairs}
{x for x in items}
(x for x in items)                         # generator expression

# Generator
def gen():
    yield 1
    yield from other()

# Decorator
from functools import wraps
def deco(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

# Context manager
from contextlib import contextmanager
@contextmanager
def ctx():
    try:
        yield
    finally:
        ...

# Type hints
def f(a: int, b: list[str] | None = None) -> dict[str, int]: ...

# Regex (advanced)
re.search(r"(?P<name>\w+)(?=\s)", text)
re.sub(r"\d+", lambda m: str(int(m[0]) * 2), text)

# Async
import asyncio
async def main():
    results = await asyncio.gather(task1(), task2())
asyncio.run(main())

# Threads and processes
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
with ThreadPoolExecutor() as pool:
    results = list(pool.map(func, items))
```

---

## Practice Exercises

### Beginner

1. Use a list comprehension to create the cubes of the numbers 1 to 10.
2. Use a list comprehension to get only the words longer than 4 letters from a list.
3. Create a dictionary comprehension that maps each number from 1 to 5 to its square.
4. Write a generator `countdown(n)` that yields `n, n-1, ..., 1`.
5. Write a decorator `@shout` that converts a function's returned string to uppercase.

### Intermediate

6. Flatten `[[1, 2], [3, 4], [5, 6]]` using a comprehension.
7. Given a dictionary of names to scores, build a new dictionary containing only the passing scores (60 or more).
8. Write a generator that reads a file line by line and yields only the lines containing a keyword.
9. Write a `@timer` decorator that prints how long a function takes, preserving the function's name with `functools.wraps`.
10. Write a context manager (class-based and with `@contextmanager`) that prints how long its block took.
11. Add type hints to a function that takes a list of integers and returns the average, or `None` for an empty list.
12. Use a regular expression with named groups to parse the log line `2026-10-18 19:53:00 ERROR Disk full` into date, time, level, and message.

### Challenge

13. Write a `@retry(attempts=3, delay=1)` decorator that retries a function when it raises an exception.
14. Write a generator pipeline: read numbers → keep primes → square them → take the first 10.
15. Use `asyncio.gather` to simulate fetching 5 URLs (with `asyncio.sleep`) and show that the total time is about the slowest one, not the sum.
16. Use `ThreadPoolExecutor` to check the status of 10 URLs at once.
17. Use `multiprocessing.Pool` to compute the sum of squares for 8 large ranges and compare the time with a plain loop.
18. Build a `@cache_with_ttl(seconds)` decorator that caches function results and expires them after a given time.
19. Write a regular expression that validates a password (at least 8 characters, an uppercase letter, a lowercase letter, a digit, and a special character) using lookaheads and `re.VERBOSE`.

### Sample solution (Exercise 8)

```python
def grep(path, keyword):
    with open(path, encoding="utf-8") as f:
        for number, line in enumerate(f, start=1):
            if keyword in line:
                yield number, line.rstrip()

for number, line in grep("app.log", "ERROR"):
    print(f"{number}: {line}")
```

### Sample solution (Exercise 10)

```python
import time
from contextlib import contextmanager

class Timer:
    def __enter__(self):
        self.start = time.perf_counter()
        return self

    def __exit__(self, *exc):
        print(f"Elapsed: {time.perf_counter() - self.start:.4f}s")
        return False

@contextmanager
def timer():
    start = time.perf_counter()
    try:
        yield
    finally:
        print(f"Elapsed: {time.perf_counter() - start:.4f}s")

with Timer():
    sum(range(1_000_000))

with timer():
    sum(range(1_000_000))
```

### Sample solution (Exercise 12)

```python
import re

LINE = re.compile(
    r"(?P<date>\d{4}-\d{2}-\d{2}) "
    r"(?P<time>\d{2}:\d{2}:\d{2}) "
    r"(?P<level>[A-Z]+) "
    r"(?P<message>.*)"
)

m = LINE.match("2026-10-18 19:53:00 ERROR Disk full")
print(m.groupdict())
# {'date': '2026-10-18', 'time': '19:53:00', 'level': 'ERROR', 'message': 'Disk full'}
```

### Sample solution (Exercise 13)

```python
import time
from functools import wraps

def retry(attempts=3, delay=1):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(1, attempts + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {attempt} failed: {e}")
                    if attempt == attempts:
                        raise
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(attempts=3, delay=0.5)
def flaky():
    raise ConnectionError("Server down")

# flaky()
```

### Sample solution (Exercise 14)

```python
from itertools import islice

def numbers():
    n = 2
    while True:
        yield n
        n += 1

def primes(nums):
    for n in nums:
        if all(n % d for d in range(2, int(n ** 0.5) + 1)):
            yield n

def squares(nums):
    for n in nums:
        yield n ** 2

print(list(islice(squares(primes(numbers())), 10)))
# [4, 9, 25, 49, 121, 169, 289, 361, 529, 841]
```

### Sample solution (Exercise 15)

```python
import asyncio
import random
import time

async def fetch(i):
    delay = random.uniform(0.5, 2)
    await asyncio.sleep(delay)
    return i, round(delay, 2)

async def main():
    start = time.perf_counter()
    results = await asyncio.gather(*(fetch(i) for i in range(5)))
    total = time.perf_counter() - start

    print(results)
    print(f"Total {total:.2f}s (slowest {max(d for _, d in results):.2f}s, sum {sum(d for _, d in results):.2f}s)")

asyncio.run(main())
```

### Sample solution (Exercise 17)

```python
import time
from multiprocessing import Pool

def sum_squares(n):
    return sum(i * i for i in range(n))

if __name__ == "__main__":
    work = [4_000_000] * 8

    start = time.perf_counter()
    plain = [sum_squares(n) for n in work]
    print(f"Plain loop: {time.perf_counter() - start:.2f}s")

    start = time.perf_counter()
    with Pool() as pool:
        parallel = pool.map(sum_squares, work)
    print(f"Pool:       {time.perf_counter() - start:.2f}s")

    assert plain == parallel
```

### Sample solution (Exercise 18)

```python
import time
from functools import wraps

def cache_with_ttl(seconds):
    def decorator(func):
        cache = {}

        @wraps(func)
        def wrapper(*args):
            now = time.monotonic()
            if args in cache:
                value, stored_at = cache[args]
                if now - stored_at < seconds:
                    return value
            value = func(*args)
            cache[args] = (value, now)
            return value

        wrapper.clear = cache.clear
        return wrapper
    return decorator

@cache_with_ttl(seconds=5)
def slow_square(n):
    print("Computing...")
    time.sleep(1)
    return n * n

print(slow_square(4))    # Computing... 16
print(slow_square(4))    # 16 (cached)
time.sleep(5)
print(slow_square(4))    # Computing... 16 (expired)
```

### Sample solution (Exercise 19)

```python
import re

PASSWORD = re.compile(r"""
    ^
    (?=.*[a-z])          # lowercase letter
    (?=.*[A-Z])          # uppercase letter
    (?=.*\d)             # digit
    (?=.*[^\w\s])        # special character
    .{8,}                # at least 8 characters
    $
""", re.VERBOSE)

for pw in ["Abc@1234", "abc12345", "ABC@1234", "Ab@1"]:
    print(f"{pw:<12} {bool(PASSWORD.match(pw))}")
```

---

**Previous:** [Chapter 8: Python Object-Oriented Programming (OOP)](chapter-08-python-oop.md)
**Next:** [Chapter 10: Professional Development Skills](#)
