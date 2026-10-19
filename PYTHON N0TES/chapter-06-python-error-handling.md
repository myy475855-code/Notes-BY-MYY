# Chapter 6: Python Error Handling

> Every program meets errors: bad user input, missing files, network failures, typos. This chapter shows how to **handle** errors gracefully with `try...except`, and how to **find and fix** them with debugging tools.

## Table of Contents

- [6.1 Python Try...Except](#61-python-tryexcept)
- [6.2 Python Debugging](#62-python-debugging)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 6.1 Python Try...Except

### Types of errors

| Type | When it happens | Example |
|---|---|---|
| **Syntax error** | Python cannot read your code; program does not start | `print("Hi"` |
| **Runtime error (exception)** | Code is valid but fails while running | `10 / 0` |
| **Logic error** | Code runs but gives the wrong result | Using `+` instead of `*` |

`try...except` handles **runtime errors** (exceptions). Syntax errors must be fixed in the code.

### What happens without handling

```python
print(10 / 0)
print("This line never runs")
```

```
Traceback (most recent call last):
  File "main.py", line 1, in <module>
    print(10 / 0)
ZeroDivisionError: division by zero
```

The program stops at the first unhandled error.

### Basic `try...except`

**Syntax**

```python
try:
    # code that might fail
except:
    # runs if an error occurs
```

```python
try:
    print(10 / 0)
except:
    print("Something went wrong")

print("Program continues")
```

```
Something went wrong
Program continues
```

> A bare `except:` catches everything, including `Ctrl+C`. It hides bugs. Prefer naming the exception type.

### Catching a specific exception

**Syntax**

```python
try:
    ...
except ExceptionType:
    ...
```

```python
try:
    number = int("abc")
except ValueError:
    print("That is not a valid number")
```

### Getting the error details

```python
try:
    x = 10 / 0
except ZeroDivisionError as e:
    print("Error:", e)
    print(type(e))
```

```
Error: division by zero
<class 'ZeroDivisionError'>
```

### Multiple `except` blocks

```python
try:
    a = int(input("Number: "))
    b = int(input("Divide by: "))
    print(a / b)
except ValueError:
    print("Please enter whole numbers only")
except ZeroDivisionError:
    print("You cannot divide by zero")
```

Python checks the blocks **from top to bottom** and runs the first match.

### Catching several types in one block

```python
try:
    value = int("abc")
except (ValueError, TypeError) as e:
    print("Bad value:", e)
```

### `else` block

Runs only if **no exception** happened.

```python
try:
    n = int(input("Number: "))
except ValueError:
    print("Invalid input")
else:
    print(f"You entered {n}")
```

Keep the `try` block small. Put code that should not be guarded by the `except` in `else`.

### `finally` block

Runs **always**, whether or not an error happened. Use it for cleanup.

```python
try:
    f = open("data.txt")
    print(f.read())
except FileNotFoundError:
    print("File not found")
finally:
    print("This always runs")
```

### Full structure

```python
try:
    # risky code
except SomeError as e:
    # handle that error
except OtherError:
    # handle another error
else:
    # runs if no error
finally:
    # always runs
```

| Block | Runs when |
|---|---|
| `try` | Always first |
| `except` | An exception matching the type occurs |
| `else` | The `try` block finished with no exception |
| `finally` | Always, even after `return`, `break`, or an unhandled error |

### Cleanup with `with` (better than `finally`)

```python
try:
    with open("data.txt") as f:
        print(f.read())
except FileNotFoundError:
    print("File not found")
```

The file is closed automatically, even if an error occurs.

### Common built-in exceptions

| Exception | Cause | Example |
|---|---|---|
| `ValueError` | Right type, wrong value | `int("abc")` |
| `TypeError` | Wrong type of operation | `"a" + 1` |
| `ZeroDivisionError` | Dividing by zero | `5 / 0` |
| `IndexError` | List index out of range | `[1, 2][5]` |
| `KeyError` | Missing dictionary key | `{"a": 1}["b"]` |
| `NameError` | Variable not defined | `print(x)` |
| `AttributeError` | Object has no such attribute | `"hi".push()` |
| `FileNotFoundError` | File does not exist | `open("nofile.txt")` |
| `PermissionError` | No permission for the file | Writing to a protected folder |
| `ImportError` / `ModuleNotFoundError` | Import failed | `import nomodule` |
| `OverflowError` | Number too large | `math.exp(1000)` |
| `RecursionError` | Too many nested calls | Infinite recursion |
| `StopIteration` | Iterator has no more items | `next(iter([]))` |
| `KeyboardInterrupt` | User pressed `Ctrl+C` | Stopping a program |
| `IndentationError` | Wrong indentation | Missing block indent |

```python
# Examples
# int("abc")             # ValueError
# "5" + 5                # TypeError
# [1, 2, 3][10]          # IndexError
# {"a": 1}["z"]          # KeyError
# open("missing.txt")    # FileNotFoundError
# undefined_name         # NameError
```

### Exception hierarchy

Exceptions form a family tree. Catching a parent also catches its children.

```
BaseException
├── KeyboardInterrupt
├── SystemExit
└── Exception
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── OverflowError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── OSError
    │   ├── FileNotFoundError
    │   └── PermissionError
    ├── ValueError
    ├── TypeError
    ├── NameError
    └── AttributeError
```

```python
try:
    [1, 2, 3][10]
except LookupError:
    print("Caught an IndexError via its parent LookupError")
```

> Use `except Exception:` as the broadest reasonable catch. It does not catch `KeyboardInterrupt` or `SystemExit`.

### Order matters: specific first

```python
try:
    int("abc")
except ValueError:
    print("Value error")
except Exception:
    print("Some other error")
```

If `Exception` came first, `ValueError` would never be reached.

### Raising exceptions with `raise`

Create your own error when something is wrong.

**Syntax**

```python
raise ExceptionType("message")
```

```python
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative")
    return age

try:
    set_age(-5)
except ValueError as e:
    print("Error:", e)    # Error: Age cannot be negative
```

```python
x = "hello"

if not isinstance(x, int):
    raise TypeError("Only integers are allowed")
```

### Re-raising an exception

Handle part of it, then pass it on.

```python
try:
    int("abc")
except ValueError:
    print("Logging the error...")
    raise    # raises the same exception again
```

### Exception chaining with `from`

```python
try:
    int("abc")
except ValueError as e:
    raise RuntimeError("Could not read the config") from e
```

The traceback shows both errors, which makes the root cause easy to find.

### Custom exceptions

Create a class that inherits from `Exception`.

```python
class InsufficientFundsError(Exception):
    """Raised when a withdrawal is larger than the balance."""
    pass

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError(
            f"Balance {balance}, requested {amount}"
        )
    return balance - amount

try:
    withdraw(100, 500)
except InsufficientFundsError as e:
    print("Failed:", e)
```

**With extra data**

```python
class ValidationError(Exception):
    def __init__(self, field, message):
        super().__init__(f"{field}: {message}")
        self.field = field
        self.message = message

try:
    raise ValidationError("email", "Invalid format")
except ValidationError as e:
    print(e.field)      # email
    print(e.message)    # Invalid format
```

**A hierarchy of your own errors**

```python
class AppError(Exception):
    pass

class DatabaseError(AppError):
    pass

class NetworkError(AppError):
    pass

try:
    raise NetworkError("Timeout")
except AppError as e:
    print("App problem:", e)    # catches any child too
```

### `assert`

Checks a condition during development. If it is false, `AssertionError` is raised.

```python
def average(nums):
    assert len(nums) > 0, "List must not be empty"
    return sum(nums) / len(nums)

# average([])    # AssertionError: List must not be empty
```

> Do not use `assert` to validate user input. Assertions can be turned off with `python -O`. Use `if` and `raise` for real validation.

### Exception groups (Python 3.11+)

Handle several unrelated errors raised together (common in async and concurrent code).

```python
try:
    raise ExceptionGroup("Multiple problems", [
        ValueError("bad value"),
        TypeError("bad type"),
    ])
except* ValueError as eg:
    print("Value errors:", eg.exceptions)
except* TypeError as eg:
    print("Type errors:", eg.exceptions)
```

### Adding notes to exceptions (Python 3.11+)

```python
try:
    int("abc")
except ValueError as e:
    e.add_note("while reading the 'age' field")
    raise
```

### Handling input errors in a loop

```python
while True:
    try:
        age = int(input("Enter your age: "))
        if age < 0:
            raise ValueError("Age cannot be negative")
        break
    except ValueError as e:
        print("Invalid input:", e)

print("Your age is", age)
```

### Handling missing keys and indexes

```python
data = {"name": "Ali"}

try:
    print(data["age"])
except KeyError:
    print("No age on file")

# Often simpler without try:
print(data.get("age", "No age on file"))
```

### EAFP vs LBYL

| Style | Meaning | Example |
|---|---|---|
| **LBYL**: Look Before You Leap | Check first | `if key in d: value = d[key]` |
| **EAFP**: Easier to Ask Forgiveness than Permission | Try, then handle failure | `try: value = d[key] except KeyError: ...` |

Python code often prefers EAFP, particularly where checks can go stale (files, networks).

```python
# LBYL
import os
if os.path.exists("data.txt"):
    f = open("data.txt")    # file could vanish between the check and the open

# EAFP
try:
    f = open("data.txt")
except FileNotFoundError:
    f = None
```

### Retry pattern

```python
import time

def fetch_with_retry(action, retries=3, delay=1):
    for attempt in range(1, retries + 1):
        try:
            return action()
        except ConnectionError as e:
            print(f"Attempt {attempt} failed: {e}")
            if attempt == retries:
                raise
            time.sleep(delay)
```

### Return values and `finally`

```python
def test():
    try:
        return "from try"
    finally:
        print("finally runs before the function returns")

print(test())
```

```
finally runs before the function returns
from try
```

### Logging errors

```python
import logging

logging.basicConfig(level=logging.ERROR)

try:
    1 / 0
except ZeroDivisionError:
    logging.exception("Calculation failed")    # logs the full traceback
```

### Context managers for cleanup

```python
from contextlib import suppress

with suppress(FileNotFoundError):
    import os
    os.remove("maybe_missing.txt")    # ignore if the file does not exist
```

### Good practices

| Do | Avoid |
|---|---|
| Catch specific exceptions | Bare `except:` |
| Keep `try` blocks short | Wrapping a whole program in one `try` |
| Handle errors where you can fix them | Catching an error just to ignore it |
| Add context to error messages | Vague messages like `"Error"` |
| Use `with` for files and connections | Forgetting to close resources |
| Log unexpected errors | `except Exception: pass` |

### Anti-pattern: swallowing errors

```python
# Bad: the problem is hidden
try:
    process(data)
except Exception:
    pass

# Better: at least log it
import logging
try:
    process(data)
except Exception:
    logging.exception("process() failed")
```

### Real-world example: safe calculator

```python
def calculate(a, op, b):
    try:
        a, b = float(a), float(b)
        match op:
            case "+": return a + b
            case "-": return a - b
            case "*": return a * b
            case "/": return a / b
            case _:   raise ValueError(f"Unknown operator '{op}'")
    except ZeroDivisionError:
        return "Cannot divide by zero"
    except ValueError as e:
        return f"Invalid input: {e}"

print(calculate("10", "/", "2"))    # 5.0
print(calculate("10", "/", "0"))    # Cannot divide by zero
print(calculate("ten", "+", "5"))   # Invalid input: could not convert...
```

### Real-world example: safe file reader

```python
def read_file(path):
    try:
        with open(path, encoding="utf-8") as f:
            return f.read()
    except FileNotFoundError:
        print(f"'{path}' does not exist")
    except PermissionError:
        print(f"No permission to read '{path}'")
    except UnicodeDecodeError:
        print(f"'{path}' is not a valid text file")
    return None

content = read_file("notes.txt")
```

---

## 6.2 Python Debugging

**Debugging** means finding and fixing the cause of a problem in your code. A good process matters more than any single tool.

### The debugging mindset

```
1. Reproduce  →  Make the bug happen reliably
2. Isolate    →  Find the smallest code that shows it
3. Understand →  Ask why, not just where
4. Fix        →  Change one thing at a time
5. Verify     →  Confirm it's fixed; add a test
```

### Reading a traceback

A traceback lists the calls leading to the error. **Read it from the bottom up.**

```python
def divide(a, b):
    return a / b

def calculate():
    return divide(10, 0)

calculate()
```

```
Traceback (most recent call last):
  File "main.py", line 7, in <module>
    calculate()
  File "main.py", line 5, in calculate
    return divide(10, 0)
  File "main.py", line 2, in divide
    return a / b
ZeroDivisionError: division by zero
```

| Part | Meaning |
|---|---|
| Last line | The error type and message |
| Line above it | The exact line that failed |
| Lines above | The chain of calls that led there |

### Common errors and what they usually mean

| Error message | Typical cause | Fix |
|---|---|---|
| `SyntaxError: invalid syntax` | Typo, missing colon or bracket | Check the line shown and the line before it |
| `IndentationError` | Inconsistent spaces or tabs | Use 4 spaces everywhere |
| `NameError: name 'x' is not defined` | Typo, or variable used before creation | Check spelling and order |
| `TypeError: can only concatenate str` | Mixing `str` and `int` | Convert with `str()` or `int()` |
| `IndexError: list index out of range` | Index too large | Check `len()`; remember indexes start at 0 |
| `KeyError: 'name'` | Missing dictionary key | Use `.get()` or check with `in` |
| `AttributeError: 'NoneType' object has no attribute ...` | A value is `None` unexpectedly | Find where `None` came from |
| `UnboundLocalError` | Assigning to a global inside a function | Use `global` or pass a parameter |
| `ModuleNotFoundError` | Package not installed | `pip install ...` in the right environment |
| `RecursionError` | Missing base case | Add or fix the stopping condition |

### Print debugging

The simplest tool: print values to see what is happening.

```python
def average(nums):
    total = sum(nums)
    print("DEBUG total:", total)
    print("DEBUG count:", len(nums))
    return total / len(nums)

print(average([4, 8, 6]))
```

**f-string debug shortcut**

```python
x = 10
items = [1, 2, 3]
print(f"{x = }, {len(items) = }")    # x = 10, len(items) = 3
```

**Show types too**

```python
value = "42"
print(f"{value!r} is {type(value).__name__}")    # '42' is str
```

**`pprint` for nested data**

```python
from pprint import pprint

data = {"users": [{"name": "Ali", "roles": ["admin", "dev"]}]}
pprint(data)
```

**Print to stderr**

```python
import sys
print("Debug info", file=sys.stderr)
```

Remove debug prints before finishing, or use logging (below).

### The `breakpoint()` function and `pdb`

Python's built-in debugger lets you pause a program and inspect it.

```python
def calculate(a, b):
    result = a * b
    breakpoint()          # program pauses here
    return result + 1

calculate(3, 4)
```

You get a `(Pdb)` prompt. Common commands:

| Command | Short | Action |
|---|---|---|
| `help` | `h` | Show commands |
| `next` | `n` | Run the next line (step over calls) |
| `step` | `s` | Step **into** a function call |
| `continue` | `c` | Run until the next breakpoint |
| `print x` | `p x` | Show the value of `x` |
| `pp x` | | Pretty-print `x` |
| `list` | `l` | Show code around the current line |
| `where` | `w` | Show the call stack |
| `up` / `down` | `u` / `d` | Move through the call stack |
| `return` | `r` | Run until the current function returns |
| `break 25` | `b 25` | Set a breakpoint on line 25 |
| `clear` | `cl` | Remove breakpoints |
| `quit` | `q` | Exit the debugger |

Sample session:

```
> main.py(4)calculate()
-> return result + 1
(Pdb) p result
12
(Pdb) p a, b
(3, 4)
(Pdb) n
(Pdb) c
```

**Start the debugger on a whole script**

```bash
python -m pdb main.py
```

**Debug after a crash (post-mortem)**

```python
import pdb

try:
    1 / 0
except Exception:
    pdb.post_mortem()
```

Or run `python -i main.py` and then call `import pdb; pdb.pm()` after a failure.

**Disable all breakpoints without editing code**

```bash
PYTHONBREAKPOINT=0 python main.py
```

### Debugging in VS Code

1. Click in the left margin to set a **breakpoint** (a red dot).
2. Press `F5` (or choose **Run and Debug**).
3. Use the toolbar:

| Key | Action |
|---|---|
| `F5` | Continue |
| `F10` | Step over |
| `F11` | Step into |
| `Shift+F11` | Step out |
| `Shift+F5` | Stop |

4. Use the **Variables**, **Watch**, and **Call Stack** panels to inspect values.
5. Right-click a breakpoint to add a **condition** (for example `i == 50`) so it only pauses when needed.

PyCharm and other IDEs offer similar tools.

### Logging (better than print)

Logging can be turned up or down, written to files, and carries timestamps and levels.

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s | %(levelname)s | %(message)s",
)

logging.debug("Detailed info for debugging")
logging.info("Normal progress message")
logging.warning("Something unexpected, but not fatal")
logging.error("An operation failed")
logging.critical("The program may not continue")
```

```
2026-10-18 19:43:10,123 | DEBUG | Detailed info for debugging
...
```

| Level | Use for |
|---|---|
| `DEBUG` | Detailed diagnostic data |
| `INFO` | Confirmation things work as expected |
| `WARNING` | Something unexpected, but running |
| `ERROR` | A function failed |
| `CRITICAL` | A serious failure |

**Log to a file**

```python
import logging

logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s",
)

logging.info("Program started")
```

**A logger per module**

```python
import logging

logger = logging.getLogger(__name__)
logger.info("Loaded %d records", 250)    # lazy formatting
```

**Log exceptions with the traceback**

```python
try:
    1 / 0
except ZeroDivisionError:
    logging.exception("Division failed")
```

(Logging is covered in more depth in Chapter 10.)

### The `traceback` module

```python
import traceback

try:
    int("abc")
except ValueError:
    traceback.print_exc()                  # prints the full traceback
    text = traceback.format_exc()          # get it as a string
    print(len(text))
```

### Inspecting objects

```python
x = [1, 2, 3]

print(type(x))                 # <class 'list'>
print(dir(x))                  # all attributes and methods
print(isinstance(x, list))     # True
help(x.append)                 # documentation

import inspect
print(inspect.getsource(inspect.getdoc))   # source of a function
```

```python
def f(a, b=2):
    pass

print(f.__name__)                           # f
print(f.__defaults__)                       # (2,)
print(f.__code__.co_varnames)               # ('a', 'b')
```

### Using `assert` to catch bugs early

```python
def apply_discount(price, percent):
    assert 0 <= percent <= 100, f"Bad percent: {percent}"
    return price * (1 - percent / 100)
```

### Type hints and static checks

Tools like `mypy` find many bugs before you run the code.

```python
def add(a: int, b: int) -> int:
    return a + b

add("1", "2")    # mypy reports an error
```

```bash
pip install mypy
mypy main.py
```

### Linters and formatters

| Tool | What it catches |
|---|---|
| `pyflakes` / `flake8` | Unused imports, undefined names, style problems |
| `pylint` | Wider code quality checks |
| `ruff` | Very fast linter and formatter |
| `black` | Automatic formatting |
| `mypy` / `pyright` | Type errors |

```bash
pip install ruff
ruff check .
```

### Unit tests as debugging aids

```python
# test_calc.py
def add(a, b):
    return a + b

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
```

```bash
pip install pytest
pytest
```

A failing test pinpoints a bug and stops it returning. (See Chapter 10.)

### Common debugging strategies

| Strategy | How |
|---|---|
| **Rubber duck** | Explain the code line by line, out loud, to someone (or a rubber duck) |
| **Binary search** | Comment out or test half the code to see which half has the bug |
| **Minimal example** | Strip the problem down to the smallest code that still fails |
| **Check assumptions** | Print types and values you "know" are correct |
| **Change one thing** | Make one change at a time, then re-test |
| **Read the error** | Read the whole message, not just the first line |
| **Take a break** | Fresh eyes often spot the typo |
| **Use version control** | `git diff` and `git bisect` show what changed |

### Typical bugs and fixes

**1. Off-by-one error**

```python
nums = [10, 20, 30]

# Bug: IndexError
# for i in range(len(nums) + 1):
#     print(nums[i])

for i in range(len(nums)):
    print(nums[i])
```

**2. Modifying a list while looping**

```python
nums = [1, 2, 3, 4, 5, 6]

# Bug: skips items
# for n in nums:
#     if n % 2 == 0:
#         nums.remove(n)

nums = [n for n in nums if n % 2 != 0]
print(nums)    # [1, 3, 5]
```

**3. Mutable default argument**

```python
# Bug
def add(item, items=[]):
    items.append(item)
    return items

# Fix
def add(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

**4. `=` instead of `==`**

```python
x = 5
# if x = 5:       # SyntaxError
if x == 5:
    print("equal")
```

**5. Using the result of an in-place method**

```python
nums = [3, 1, 2]
# nums = nums.sort()      # nums becomes None
nums.sort()
```

**6. Integer vs string input**

```python
a = input("A: ")        # "5"
b = input("B: ")        # "3"
# print(a + b)          # "53"
print(int(a) + int(b))  # 8
```

**7. Float comparison**

```python
import math

print(0.1 + 0.2 == 0.3)               # False
print(math.isclose(0.1 + 0.2, 0.3))   # True
```

**8. Shadowing built-ins**

```python
# list = [1, 2, 3]
# list("abc")          # TypeError: 'list' object is not callable
numbers = [1, 2, 3]    # use a different name
```

**9. Wrong scope**

```python
count = 0

def add():
    global count      # needed to change the global
    count += 1
```

**10. Late binding in lambdas**

```python
funcs = [lambda i=i: i for i in range(3)]    # fix with a default value
print([f() for f in funcs])                  # [0, 1, 2]
```

### Measuring performance problems

```python
import time

start = time.perf_counter()
sum(range(5_000_000))
print(f"{time.perf_counter() - start:.3f}s")
```

```python
import timeit
print(timeit.timeit("sum(range(1000))", number=10_000))
```

**Profile with `cProfile`**

```bash
python -m cProfile -s cumtime main.py
```

```python
import cProfile
cProfile.run("sum(range(1_000_000))")
```

### Memory problems

```python
import sys

print(sys.getsizeof([1, 2, 3]))           # bytes used by the list object

import tracemalloc
tracemalloc.start()
data = [i for i in range(100_000)]
current, peak = tracemalloc.get_traced_memory()
print(f"Current: {current / 1024:.0f} KB, peak: {peak / 1024:.0f} KB")
```

### A debugging checklist

- [ ] Did I read the full error message and traceback?
- [ ] Can I reproduce the bug every time?
- [ ] Did I check the exact line shown, and the line before it?
- [ ] Are variable types and values what I think they are?
- [ ] Is it a typo or wrong name?
- [ ] Is the right Python or virtual environment active?
- [ ] Did something change recently (`git diff`)?
- [ ] Have I written a test that fails, then passes after the fix?

### Real-world example: finding a bug step by step

**Buggy code**

```python
def average_of_positives(nums):
    positives = [n for n in nums if n > 0]
    return sum(positives) / len(positives)

print(average_of_positives([3, -1, 5, 7]))    # 5.0
print(average_of_positives([-2, -4]))         # ZeroDivisionError
```

**Step 1: Read the error**

```
ZeroDivisionError: division by zero
  File "main.py", line 3, in average_of_positives
```

**Step 2: Inspect values**

```python
positives = [n for n in nums if n > 0]
print(f"{positives = }")    # positives = []
```

**Step 3: Understand.** When there are no positive numbers, `len(positives)` is `0`.

**Step 4: Fix and decide the behavior**

```python
def average_of_positives(nums):
    positives = [n for n in nums if n > 0]
    if not positives:
        return None            # or raise ValueError("no positive numbers")
    return sum(positives) / len(positives)
```

**Step 5: Add tests**

```python
assert average_of_positives([3, -1, 5, 7]) == 5.0
assert average_of_positives([-2, -4]) is None
```

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Try...Except | `try`, `except`, `else`, `finally`; catch specific exceptions; `raise` your own; custom exception classes; avoid bare `except` |
| Debugging | Read tracebacks bottom-up; `print`/f-string debugging; `breakpoint()` and `pdb`; logging; linters, type checkers, tests |

### Quick syntax cheat sheet

```python
# Handling errors
try:
    risky()
except (ValueError, TypeError) as e:
    print("Bad input:", e)
except FileNotFoundError:
    print("Missing file")
else:
    print("No errors")
finally:
    print("Always runs")

# Raising errors
raise ValueError("message")

# Custom error
class MyError(Exception):
    pass

# Re-raise / chain
raise
raise NewError("...") from original

# Debugging
print(f"{x = }")
breakpoint()
import logging
logging.basicConfig(level=logging.DEBUG)
logging.exception("failed")
```

---

## Practice Exercises

### Beginner

1. Write code that asks for a number and catches `ValueError` if the user types text.
2. Divide two numbers entered by the user and handle `ZeroDivisionError`.
3. Try to open a file that does not exist and handle `FileNotFoundError`.
4. Access index `10` of a 3-item list and catch the `IndexError`.
5. Use `finally` to print `"Done"` after a `try...except` block.

### Intermediate

6. Write a loop that keeps asking for an integer until the user enters a valid one.
7. Write a function `safe_divide(a, b)` that returns `None` on division by zero and `"invalid"` on a `TypeError`.
8. Write a function `get_age(data)` that reads `data["age"]` and handles a missing key using `try...except KeyError`.
9. Raise a `ValueError` from a function when a password is shorter than 8 characters, and catch it outside.
10. Add `logging` to a small program so errors are written to `app.log` with a timestamp.

### Challenge

11. Create a custom exception `InvalidAgeError` and a function that raises it for ages below 0 or above 120.
12. Write a bank account class whose `withdraw()` raises `InsufficientFundsError`. Catch it and print the shortfall.
13. Write a retry function that calls a given function up to 3 times, waiting 1 second between failures.
14. Debug this code: find and fix **all** bugs.

```python
def get_average(numbers):
    total = 0
    for i in range(1, len(numbers) + 1):
        total += numbers[i]
    return total / len(numbers)

result = get_average([])
print("Average is " + result)
```

15. Use `pdb` or your IDE's debugger to step through a loop that sums the numbers 1 to 10 and watch the value of `total` change.

### Sample solution (Exercise 6)

```python
while True:
    try:
        n = int(input("Enter an integer: "))
        break
    except ValueError:
        print("That is not an integer. Try again.")

print("You entered", n)
```

### Sample solution (Exercise 7)

```python
def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None
    except TypeError:
        return "invalid"

print(safe_divide(10, 2))      # 5.0
print(safe_divide(10, 0))      # None
print(safe_divide(10, "a"))    # invalid
```

### Sample solution (Exercise 11)

```python
class InvalidAgeError(Exception):
    pass

def set_age(age):
    if age < 0 or age > 120:
        raise InvalidAgeError(f"Age {age} is not realistic")
    return age

try:
    set_age(150)
except InvalidAgeError as e:
    print("Error:", e)
```

### Sample solution (Exercise 12)

```python
class InsufficientFundsError(Exception):
    def __init__(self, shortfall):
        super().__init__(f"Short by {shortfall}")
        self.shortfall = shortfall

class Account:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError(amount - self.balance)
        self.balance -= amount

acc = Account(100)
try:
    acc.withdraw(250)
except InsufficientFundsError as e:
    print(f"Cannot withdraw. You need {e.shortfall} more.")
```

### Sample solution (Exercise 13)

```python
import time

def retry(func, attempts=3, delay=1):
    for i in range(1, attempts + 1):
        try:
            return func()
        except Exception as e:
            print(f"Attempt {i} failed: {e}")
            if i == attempts:
                raise
            time.sleep(delay)
```

### Sample solution (Exercise 14)

Bugs found:

1. `range(1, len(numbers) + 1)` skips index `0` and runs past the end (`IndexError`).
2. An empty list causes division by zero.
3. `"Average is " + result` mixes `str` and `float` (`TypeError`).

```python
def get_average(numbers):
    if not numbers:
        return None
    total = 0
    for n in numbers:
        total += n
    return total / len(numbers)

result = get_average([4, 8, 6])
if result is None:
    print("No numbers given")
else:
    print(f"Average is {result}")
```

---

**Previous:** [Chapter 5: Python Built-in Tools and Formatting](chapter-05-python-built-in-tools-and-formatting.md)
**Next:** [Chapter 7: Python File Handling](#)
