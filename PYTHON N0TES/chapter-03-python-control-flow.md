# Chapter 3: Python Control Flow

> Control flow decides **which code runs** and **how many times**. In this chapter you will learn to make decisions with `if`, `elif`, `else` and `match`, and to repeat work with `while` and `for` loops.

## Table of Contents

- [3.1 Python If...Else](#31-python-ifelse)
- [3.2 Python Match](#32-python-match)
- [3.3 Python While Loops](#33-python-while-loops)
- [3.4 Python For Loops](#34-python-for-loops)
- [3.5 Python Range](#35-python-range)
- [Choosing the Right Tool](#choosing-the-right-tool)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 3.1 Python If...Else

Conditional statements run code only when a condition is true.

### Comparison operators (recap)

| Operator | Meaning | Example |
|---|---|---|
| `==` | Equal | `a == b` |
| `!=` | Not equal | `a != b` |
| `>` | Greater than | `a > b` |
| `<` | Less than | `a < b` |
| `>=` | Greater or equal | `a >= b` |
| `<=` | Less or equal | `a <= b` |

### The `if` statement

**Syntax**

```python
if condition:
    # code runs when condition is True
```

The code inside must be **indented** (4 spaces is standard), and the line with `if` ends with a colon `:`.

```python
age = 20

if age >= 18:
    print("You are an adult.")
```

```
You are an adult.
```

### The `else` statement

Runs when the `if` condition is false.

**Syntax**

```python
if condition:
    # runs if True
else:
    # runs if False
```

```python
age = 15

if age >= 18:
    print("You can vote.")
else:
    print("You cannot vote yet.")
```

```
You cannot vote yet.
```

### The `elif` statement

Checks another condition when the previous ones were false. Short for "else if".

**Syntax**

```python
if condition1:
    # ...
elif condition2:
    # ...
elif condition3:
    # ...
else:
    # ...
```

```python
marks = 82

if marks >= 90:
    print("Grade A+")
elif marks >= 80:
    print("Grade A")
elif marks >= 70:
    print("Grade B")
elif marks >= 60:
    print("Grade C")
else:
    print("Fail")
```

```
Grade A
```

Python checks conditions **top to bottom** and runs only the **first** one that is true.

### Logical operators in conditions

```python
age = 25
has_license = True

if age >= 18 and has_license:
    print("You can drive.")

day = "Sunday"
if day == "Saturday" or day == "Sunday":
    print("Weekend!")

is_raining = False
if not is_raining:
    print("No umbrella needed.")
```

### Checking membership

```python
vowels = "aeiou"
letter = "e"

if letter in vowels:
    print("It's a vowel.")

fruits = ["apple", "banana"]
if "mango" not in fruits:
    print("No mango.")
```

### Truthy and falsy checks

```python
name = ""

if name:
    print("Name is set")
else:
    print("Name is empty")    # runs

items = [1, 2, 3]
if items:
    print("List has items")   # runs
```

Check `None` with `is`:

```python
result = None
if result is None:
    print("No result yet")
```

### Nested if statements

```python
age = 25
has_ticket = True

if age >= 18:
    if has_ticket:
        print("Welcome to the show!")
    else:
        print("You need a ticket.")
else:
    print("Adults only.")
```

Often clearer when combined:

```python
if age >= 18 and has_ticket:
    print("Welcome to the show!")
```

### Short-hand if (one line)

```python
a = 10
if a > 5: print("Greater than 5")
```

### Ternary operator (conditional expression)

**Syntax**

```python
value_if_true if condition else value_if_false
```

```python
age = 20
status = "Adult" if age >= 18 else "Minor"
print(status)    # Adult

print("Even" if 8 % 2 == 0 else "Odd")    # Even
```

Chained:

```python
n = 0
label = "Positive" if n > 0 else "Negative" if n < 0 else "Zero"
print(label)    # Zero
```

### The `pass` statement

An `if` block cannot be empty. Use `pass` as a placeholder.

```python
x = 5
if x > 3:
    pass    # TODO: add code later
```

### Chained comparisons

```python
x = 15
if 10 < x < 20:
    print("x is between 10 and 20")
```

### Common mistakes

| Mistake | Wrong | Correct |
|---|---|---|
| Using `=` instead of `==` | `if x = 5:` | `if x == 5:` |
| Missing colon | `if x > 5` | `if x > 5:` |
| Wrong indentation | no indent after `if` | indent the block |
| Comparing to a bool | `if flag == True:` | `if flag:` |

### Real-world example: login check

```python
username = "admin"
password = "1234"

if username == "admin" and password == "1234":
    print("Login successful")
elif username == "admin":
    print("Wrong password")
else:
    print("Unknown user")
```

### Real-world example: leap year

```python
year = 2028

if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
    print(f"{year} is a leap year")
else:
    print(f"{year} is not a leap year")
```

---

## 3.2 Python Match

The `match` statement compares a value against several **patterns**. It was added in **Python 3.10** and works like a more powerful `switch` from other languages.

> Requires Python 3.10 or newer. Check with `python --version`.

### Basic syntax

```python
match value:
    case pattern1:
        # code
    case pattern2:
        # code
    case _:
        # default (runs if nothing else matched)
```

### Simple example

```python
day = 3

match day:
    case 1:
        print("Monday")
    case 2:
        print("Tuesday")
    case 3:
        print("Wednesday")
    case _:
        print("Another day")
```

```
Wednesday
```

The underscore `_` is the **wildcard** that matches anything. It acts like `else`.

### Matching strings

```python
command = "start"

match command:
    case "start":
        print("Starting...")
    case "stop":
        print("Stopping...")
    case "pause":
        print("Pausing...")
    case _:
        print("Unknown command")
```

### Combining patterns with `|` (OR)

```python
day = "Sat"

match day:
    case "Sat" | "Sun":
        print("Weekend")
    case "Mon" | "Tue" | "Wed" | "Thu" | "Fri":
        print("Weekday")
    case _:
        print("Invalid day")
```

### Using guards (`if` inside a case)

```python
n = 15

match n:
    case x if x < 0:
        print("Negative")
    case 0:
        print("Zero")
    case x if x % 2 == 0:
        print("Positive even")
    case _:
        print("Positive odd")
```

### Capturing values

A name in a pattern captures the matched value.

```python
point = (3, 0)

match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print(f"On the x-axis at {x}")
    case (0, y):
        print(f"On the y-axis at {y}")
    case (x, y):
        print(f"Point at {x}, {y}")
```

```
On the x-axis at 3
```

### Matching lists

```python
data = [1, 2, 3]

match data:
    case []:
        print("Empty")
    case [x]:
        print(f"One item: {x}")
    case [x, y]:
        print(f"Two items: {x}, {y}")
    case [first, *rest]:
        print(f"First: {first}, rest: {rest}")
```

```
First: 1, rest: [2, 3]
```

### Matching dictionaries

```python
user = {"role": "admin", "name": "Ali"}

match user:
    case {"role": "admin", "name": name}:
        print(f"Welcome admin {name}")
    case {"role": "guest"}:
        print("Welcome guest")
    case _:
        print("Unknown role")
```

### Matching class instances

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(0, 5)

match p:
    case Point(x=0, y=y):
        print(f"On the y-axis at {y}")
    case Point(x=x, y=0):
        print(f"On the x-axis at {x}")
    case _:
        print("Somewhere else")
```

### Matching by type

```python
value = 3.5

match value:
    case int():
        print("Integer")
    case float():
        print("Float")
    case str():
        print("String")
    case _:
        print("Something else")
```

### `match` vs `if...elif`

| Feature | `if...elif` | `match` |
|---|---|---|
| Works in all Python 3 versions | Yes | 3.10+ only |
| Compare ranges and complex logic | Easy | Needs guards |
| Compare one value to many options | Repetitive | Clean |
| Unpack lists, tuples, dicts | Manual | Built in |

### Real-world example: simple HTTP status handler

```python
status = 404

match status:
    case 200:
        print("OK")
    case 301 | 302:
        print("Redirect")
    case 404:
        print("Not Found")
    case 500:
        print("Server Error")
    case _:
        print("Other status")
```

### Real-world example: mini menu

```python
choice = input("Choose (add / list / quit): ").strip().lower()

match choice:
    case "add":
        print("Adding an item...")
    case "list":
        print("Listing items...")
    case "quit":
        print("Goodbye!")
    case _:
        print("Invalid choice")
```

---

## 3.3 Python While Loops

A `while` loop repeats as long as a condition is true.

### Syntax

```python
while condition:
    # code to repeat
```

### Basic example

```python
i = 1

while i <= 5:
    print(i)
    i += 1
```

```
1
2
3
4
5
```

> Always change something inside the loop (like `i += 1`). Otherwise the condition stays true forever and you get an **infinite loop**.

### Infinite loop

```python
# while True:
#     print("This never stops!")    # press Ctrl + C to stop
```

`while True` is useful when you exit with `break` (see below).

### The `break` statement

Stops the loop immediately.

```python
i = 1

while i <= 10:
    if i == 4:
        break
    print(i)
    i += 1
```

```
1
2
3
```

### The `continue` statement

Skips the rest of the current pass and starts the next one.

```python
i = 0

while i < 6:
    i += 1
    if i == 3:
        continue
    print(i)
```

```
1
2
4
5
6
```

### The `else` clause

The `else` block runs when the loop finishes normally (the condition became false). It does **not** run if the loop was stopped by `break`.

```python
i = 1

while i <= 3:
    print(i)
    i += 1
else:
    print("Loop finished")
```

```
1
2
3
Loop finished
```

```python
i = 1

while i <= 3:
    if i == 2:
        break
    print(i)
    i += 1
else:
    print("Loop finished")    # does NOT run
```

### Menu loop with `while True`

```python
while True:
    choice = input("Type 'quit' to exit: ")
    if choice == "quit":
        print("Goodbye!")
        break
    print(f"You typed: {choice}")
```

### Input validation loop

```python
while True:
    age = input("Enter your age: ")
    if age.isdigit():
        age = int(age)
        break
    print("Please enter numbers only.")

print("Your age is", age)
```

### Counting down

```python
n = 5

while n > 0:
    print(n)
    n -= 1

print("Liftoff!")
```

### Sum of numbers

```python
total = 0
n = 1

while n <= 100:
    total += n
    n += 1

print("Sum 1 to 100:", total)    # 5050
```

### Number guessing game

```python
import random

secret = random.randint(1, 10)
attempts = 0

while True:
    guess = int(input("Guess a number (1-10): "))
    attempts += 1

    if guess < secret:
        print("Too low")
    elif guess > secret:
        print("Too high")
    else:
        print(f"Correct! You took {attempts} attempts.")
        break
```

### Loop until a condition on a list

```python
stack = [1, 2, 3]

while stack:
    print(stack.pop())
```

```
3
2
1
```

### Nested while loops

```python
i = 1

while i <= 3:
    j = 1
    while j <= 3:
        print(i * j, end=" ")
        j += 1
    print()
    i += 1
```

```
1 2 3 
2 4 6 
3 6 9 
```

---

## 3.4 Python For Loops

A `for` loop goes through each item of a sequence (list, tuple, string, dictionary, set, range, and more).

### Syntax

```python
for item in sequence:
    # code using item
```

### Looping through a list

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)
```

```
apple
banana
cherry
```

### Looping through a string

```python
for letter in "Python":
    print(letter)
```

### Looping through a tuple and a set

```python
for n in (1, 2, 3):
    print(n)

for color in {"red", "green"}:
    print(color)
```

### Looping through a dictionary

```python
person = {"name": "Ali", "age": 25}

for key in person:
    print(key)

for value in person.values():
    print(value)

for key, value in person.items():
    print(key, "=", value)
```

### `break` in a for loop

```python
for fruit in ["apple", "banana", "cherry"]:
    if fruit == "banana":
        break
    print(fruit)
```

```
apple
```

### `continue` in a for loop

```python
for fruit in ["apple", "banana", "cherry"]:
    if fruit == "banana":
        continue
    print(fruit)
```

```
apple
cherry
```

### The `else` clause

Runs after the loop finishes without a `break`.

```python
for n in range(3):
    print(n)
else:
    print("Done!")
```

```
0
1
2
Done!
```

**Practical use: searching**

```python
numbers = [3, 7, 9, 12]

for n in numbers:
    if n == 5:
        print("Found 5")
        break
else:
    print("5 is not in the list")
```

### `enumerate()`: get index and value

```python
fruits = ["apple", "banana", "cherry"]

for i, fruit in enumerate(fruits):
    print(i, fruit)

for i, fruit in enumerate(fruits, start=1):
    print(f"{i}. {fruit}")
```

```
1. apple
2. banana
3. cherry
```

### `zip()`: loop over several sequences together

```python
names = ["Ali", "Sara", "Bilal"]
scores = [80, 95, 70]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
```

```
Ali: 80
Sara: 95
Bilal: 70
```

### Nested for loops

```python
colors = ["red", "blue"]
items = ["pen", "book"]

for color in colors:
    for item in items:
        print(color, item)
```

```
red pen
red book
blue pen
blue book
```

**Multiplication table**

```python
for i in range(1, 4):
    for j in range(1, 4):
        print(i * j, end="\t")
    print()
```

```
1	2	3	
2	4	6	
3	6	9	
```

**Star pattern**

```python
for i in range(1, 6):
    print("*" * i)
```

```
*
**
***
****
*****
```

### Modifying a list while looping

Do not add or remove items from a list while looping over it. Loop over a copy, or build a new list.

```python
nums = [1, 2, 3, 4, 5, 6]

# Wrong: removing during the loop skips items
# Right: build a new list
evens = [n for n in nums if n % 2 == 0]
print(evens)    # [2, 4, 6]
```

### Looping in reverse

```python
for n in reversed([1, 2, 3]):
    print(n)

for n in range(5, 0, -1):
    print(n)
```

### Loop with a counter and total

```python
prices = [10.5, 20, 5.25]
total = 0

for p in prices:
    total += p

print("Total:", total)    # 35.75
```

### Finding the maximum manually

```python
nums = [4, 9, 2, 7]
biggest = nums[0]

for n in nums:
    if n > biggest:
        biggest = n

print(biggest)    # 9
```

### List comprehension (compact for loop)

```python
squares = [n ** 2 for n in range(1, 6)]
print(squares)    # [1, 4, 9, 16, 25]
```

### `for` vs `while`

| Use `for` when | Use `while` when |
|---|---|
| You know what to loop over | You don't know how many repeats are needed |
| Going through a collection | Waiting for a condition to change |
| Fixed number of repeats | Reading input until valid |

---

## 3.5 Python Range

`range()` produces a sequence of numbers. It is most often used with `for` loops.

### Syntax

```python
range(stop)
range(start, stop)
range(start, stop, step)
```

| Parameter | Meaning | Default |
|---|---|---|
| `start` | First number (included) | `0` |
| `stop` | End number (**not** included) | required |
| `step` | Gap between numbers | `1` |

### `range(stop)`

```python
for i in range(5):
    print(i)
```

```
0
1
2
3
4
```

### `range(start, stop)`

```python
for i in range(2, 6):
    print(i)    # 2 3 4 5
```

### `range(start, stop, step)`

```python
for i in range(0, 10, 2):
    print(i)    # 0 2 4 6 8
```

### Counting backwards

Use a **negative** step, with `start` larger than `stop`.

```python
for i in range(5, 0, -1):
    print(i)    # 5 4 3 2 1

for i in range(10, 0, -3):
    print(i)    # 10 7 4 1
```

### Range is lazy

`range()` does not build the whole list in memory. Convert it to see the values.

```python
print(range(5))          # range(0, 5)
print(list(range(5)))    # [0, 1, 2, 3, 4]
```

### Range properties

```python
r = range(0, 10, 2)

print(len(r))       # 5
print(r[2])         # 4
print(6 in r)       # True
print(r[-1])        # 8
print(r[1:3])       # range(2, 6, 2)
```

### Only integers

```python
# range(0, 1, 0.1)    # TypeError: 'float' object cannot be interpreted as an integer
```

For decimal steps, use a comprehension:

```python
values = [i / 10 for i in range(0, 11, 2)]
print(values)    # [0.0, 0.2, 0.4, 0.6, 0.8, 1.0]
```

### Common uses

**Repeat something N times**

```python
for _ in range(3):
    print("Hello!")
```

The underscore `_` is a convention for "I don't need this variable".

**Loop through indexes**

```python
fruits = ["apple", "banana", "cherry"]

for i in range(len(fruits)):
    print(i, fruits[i])
```

**Sum a range**

```python
print(sum(range(1, 101)))    # 5050
```

**Multiplication table**

```python
n = 7

for i in range(1, 11):
    print(f"{n} x {i} = {n * i}")
```

**Even and odd numbers**

```python
evens = list(range(0, 21, 2))
odds = list(range(1, 21, 2))
print(evens)
print(odds)
```

**Factorial**

```python
n = 5
result = 1

for i in range(1, n + 1):
    result *= i

print(result)    # 120
```

**Fibonacci series**

```python
a, b = 0, 1

for _ in range(10):
    print(a, end=" ")
    a, b = b, a + b
# 0 1 1 2 3 5 8 13 21 34
```

**Prime numbers up to 30**

```python
for n in range(2, 31):
    for d in range(2, int(n ** 0.5) + 1):
        if n % d == 0:
            break
    else:
        print(n, end=" ")
# 2 3 5 7 11 13 17 19 23 29
```

### Quick reference

| Code | Result |
|---|---|
| `range(5)` | 0, 1, 2, 3, 4 |
| `range(1, 6)` | 1, 2, 3, 4, 5 |
| `range(0, 10, 3)` | 0, 3, 6, 9 |
| `range(5, 0, -1)` | 5, 4, 3, 2, 1 |
| `range(10, 0, -2)` | 10, 8, 6, 4, 2 |

---

## Choosing the Right Tool

```
Make a decision between options?         → if / elif / else
Compare one value against many patterns? → match
Loop while a condition is true?          → while
Loop through items or a fixed count?     → for (with range if needed)
Stop a loop early?                       → break
Skip to the next pass?                   → continue
```

### Loop control summary

| Keyword | Effect |
|---|---|
| `break` | Exit the loop completely |
| `continue` | Skip to the next pass |
| `pass` | Do nothing (placeholder) |
| `else` (on a loop) | Runs if the loop ended without `break` |

---

## Chapter Summary

| Topic | Key points |
|---|---|
| If...Else | `if`, `elif`, `else`; indentation matters; ternary `a if cond else b` |
| Match | Python 3.10+; patterns, `\|` for OR, `_` wildcard, guards, unpacking |
| While loops | Repeat while true; avoid infinite loops; `break`, `continue`, `else` |
| For loops | Iterate over any sequence; `enumerate()`, `zip()`, nested loops |
| Range | `range(start, stop, step)`; stop is excluded; negative step counts down |

### Quick syntax cheat sheet

```python
# if / elif / else
if x > 0:
    print("positive")
elif x == 0:
    print("zero")
else:
    print("negative")

# ternary
label = "even" if x % 2 == 0 else "odd"

# match (3.10+)
match command:
    case "go" | "start":
        ...
    case _:
        ...

# while
while condition:
    ...
    if done:
        break

# for + range
for i in range(1, 6):
    print(i)

# for + enumerate / zip
for i, item in enumerate(items):
    ...
for a, b in zip(list1, list2):
    ...
```

---

## Practice Exercises

### Beginner

1. Ask the user for a number and print whether it is positive, negative, or zero.
2. Ask for a person's age and print "Child" (under 13), "Teen" (13 to 19), or "Adult" (20 or more).
3. Print the numbers 1 to 10 using a `for` loop.
4. Print all even numbers from 2 to 20 using `range()`.
5. Use a `while` loop to print the numbers 10 down to 1.

### Intermediate

6. Write a program that prints the multiplication table of a number entered by the user.
7. Use `match` to build a calculator that takes `+`, `-`, `*`, or `/` and two numbers.
8. Loop through a list of numbers and print only those divisible by 3.
9. Keep asking the user for a password until they type the correct one, using `while`.
10. Find the sum of all digits of a number, for example `1234` gives `10`.

### Challenge

11. Print this pyramid for `n = 5`:
    ```
        *
       ***
      *****
     *******
    *********
    ```
12. Write a FizzBuzz program: for 1 to 50, print "Fizz" for multiples of 3, "Buzz" for multiples of 5, "FizzBuzz" for multiples of both, otherwise the number.
13. Write a program that checks whether a number is prime.
14. Write a guessing game where the computer picks a number from 1 to 100 and gives "higher" or "lower" hints. Limit the player to 7 attempts.

### Sample solution (Exercise 7)

```python
a = float(input("First number: "))
op = input("Operator (+ - * /): ")
b = float(input("Second number: "))

match op:
    case "+":
        print(a + b)
    case "-":
        print(a - b)
    case "*":
        print(a * b)
    case "/":
        if b == 0:
            print("Cannot divide by zero")
        else:
            print(a / b)
    case _:
        print("Invalid operator")
```

### Sample solution (Exercise 11)

```python
n = 5

for i in range(1, n + 1):
    spaces = " " * (n - i)
    stars = "*" * (2 * i - 1)
    print(spaces + stars)
```

### Sample solution (Exercise 12)

```python
for i in range(1, 51):
    if i % 15 == 0:
        print("FizzBuzz")
    elif i % 3 == 0:
        print("Fizz")
    elif i % 5 == 0:
        print("Buzz")
    else:
        print(i)
```

### Sample solution (Exercise 13)

```python
n = int(input("Enter a number: "))

if n < 2:
    print("Not prime")
else:
    for d in range(2, int(n ** 0.5) + 1):
        if n % d == 0:
            print("Not prime")
            break
    else:
        print("Prime")
```

---

**Previous:** [Chapter 2: Python Data Structures](chapter-02-python-data-structures.md)
**Next:** [Chapter 4: Python Functions and Iteration](#)
