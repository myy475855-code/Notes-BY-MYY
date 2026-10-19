# Chapter 2: Python Data Structures

> Data structures store collections of values. Python's four built-in collection types are **lists**, **tuples**, **sets**, and **dictionaries**. This chapter also covers **arrays** for storing large amounts of numeric data efficiently.

## Table of Contents

- [2.1 Python Lists](#21-python-lists)
- [2.2 Python Tuples](#22-python-tuples)
- [2.3 Python Sets](#23-python-sets)
- [2.4 Python Dictionaries](#24-python-dictionaries)
- [2.5 Python Arrays](#25-python-arrays)
- [Comparison of Data Structures](#comparison-of-data-structures)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 2.1 Python Lists

A list is an **ordered**, **changeable** collection that **allows duplicates**. Lists are written with square brackets.

### Creating lists

**Syntax**

```python
list_name = [item1, item2, item3]
```

```python
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]
mixed = ["Ali", 25, True, 3.5]
empty = []

print(fruits)
print(type(fruits))    # <class 'list'>
```

Using the `list()` constructor:

```python
letters = list(("a", "b", "c"))
print(letters)    # ['a', 'b', 'c']
```

### Key properties

| Property | Meaning |
|---|---|
| Ordered | Items keep their position |
| Changeable | Items can be added, removed, or modified |
| Allows duplicates | `[1, 1, 2]` is valid |
| Indexed | First item is index `0` |

### Length

```python
fruits = ["apple", "banana", "cherry"]
print(len(fruits))    # 3
```

### Accessing items (indexing)

```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])     # apple
print(fruits[2])     # cherry
print(fruits[-1])    # cherry (last item)
print(fruits[-2])    # banana
```

### Slicing

**Syntax**

```python
list[start:stop:step]
```

```python
nums = [10, 20, 30, 40, 50, 60]
print(nums[1:4])     # [20, 30, 40]
print(nums[:3])      # [10, 20, 30]
print(nums[3:])      # [40, 50, 60]
print(nums[-3:])     # [40, 50, 60]
print(nums[::2])     # [10, 30, 50]
print(nums[::-1])    # [60, 50, 40, 30, 20, 10]
```

### Changing items

```python
fruits = ["apple", "banana", "cherry"]
fruits[1] = "mango"
print(fruits)    # ['apple', 'mango', 'cherry']

# Change a range of items
fruits[0:2] = ["kiwi", "grape"]
print(fruits)    # ['kiwi', 'grape', 'cherry']
```

### Adding items

| Method | Purpose |
|---|---|
| `append(x)` | Add to the end |
| `insert(i, x)` | Add at position `i` |
| `extend(iterable)` | Add all items of another collection |

```python
fruits = ["apple", "banana"]

fruits.append("cherry")
print(fruits)    # ['apple', 'banana', 'cherry']

fruits.insert(1, "mango")
print(fruits)    # ['apple', 'mango', 'banana', 'cherry']

fruits.extend(["kiwi", "grape"])
print(fruits)    # ['apple', 'mango', 'banana', 'cherry', 'kiwi', 'grape']
```

### Removing items

| Method | Purpose |
|---|---|
| `remove(x)` | Remove first item with value `x` |
| `pop(i)` | Remove and return item at index `i` (last if no index) |
| `del list[i]` | Delete item at index `i` |
| `clear()` | Empty the list |

```python
fruits = ["apple", "banana", "cherry", "banana"]

fruits.remove("banana")
print(fruits)          # ['apple', 'cherry', 'banana']

last = fruits.pop()
print(last)            # banana
print(fruits)          # ['apple', 'cherry']

del fruits[0]
print(fruits)          # ['cherry']

fruits.clear()
print(fruits)          # []
```

### Looping through a list

```python
fruits = ["apple", "banana", "cherry"]

# Loop through items
for fruit in fruits:
    print(fruit)

# Loop with index
for i in range(len(fruits)):
    print(i, fruits[i])

# Loop with enumerate (cleaner)
for i, fruit in enumerate(fruits):
    print(i, fruit)
```

### Checking if an item exists

```python
fruits = ["apple", "banana", "cherry"]
if "banana" in fruits:
    print("Yes, banana is in the list")
```

### Sorting

```python
nums = [5, 2, 9, 1, 7]

nums.sort()
print(nums)    # [1, 2, 5, 7, 9]

nums.sort(reverse=True)
print(nums)    # [9, 7, 5, 2, 1]

names = ["Zara", "ali", "Bilal"]
names.sort()
print(names)   # ['Bilal', 'Zara', 'ali']  (uppercase sorts first)

names.sort(key=str.lower)
print(names)   # ['ali', 'Bilal', 'Zara']
```

`sorted()` returns a new list and leaves the original unchanged:

```python
nums = [5, 2, 9]
new = sorted(nums)
print(nums)    # [5, 2, 9]
print(new)     # [2, 5, 9]
```

### Reversing

```python
nums = [1, 2, 3]
nums.reverse()
print(nums)    # [3, 2, 1]
```

### Copying a list

Assigning with `=` does **not** copy. Both names point to the same list.

```python
a = [1, 2, 3]
b = a
b.append(4)
print(a)    # [1, 2, 3, 4]  (a changed too!)
```

Make a real copy:

```python
a = [1, 2, 3]
b = a.copy()       # or list(a) or a[:]
b.append(4)
print(a)    # [1, 2, 3]
print(b)    # [1, 2, 3, 4]
```

### Joining lists

```python
a = [1, 2]
b = [3, 4]
print(a + b)       # [1, 2, 3, 4]

a.extend(b)
print(a)           # [1, 2, 3, 4]
```

### List comprehension (preview)

A short way to build a list.

**Syntax**

```python
[expression for item in iterable if condition]
```

```python
squares = [x ** 2 for x in range(1, 6)]
print(squares)    # [1, 4, 9, 16, 25]

evens = [x for x in range(10) if x % 2 == 0]
print(evens)      # [0, 2, 4, 6, 8]
```

### Nested lists

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]
print(matrix[1])       # [4, 5, 6]
print(matrix[1][2])    # 6
```

### List methods reference

| Method | Description |
|---|---|
| `append(x)` | Add item at end |
| `insert(i, x)` | Insert item at index |
| `extend(it)` | Add items from iterable |
| `remove(x)` | Remove first matching value |
| `pop([i])` | Remove and return item |
| `clear()` | Remove all items |
| `index(x)` | Index of first match |
| `count(x)` | Number of times `x` appears |
| `sort()` | Sort in place |
| `reverse()` | Reverse in place |
| `copy()` | Shallow copy |

### Useful built-in functions with lists

```python
nums = [4, 8, 1, 9]
print(len(nums))    # 4
print(sum(nums))    # 22
print(min(nums))    # 1
print(max(nums))    # 9
```

---

## 2.2 Python Tuples

A tuple is an **ordered**, **unchangeable** (immutable) collection that **allows duplicates**. Tuples are written with round brackets.

### Creating tuples

**Syntax**

```python
tuple_name = (item1, item2, item3)
```

```python
colors = ("red", "green", "blue")
point = (10, 20)
mixed = ("Ali", 25, True)
empty = ()

print(colors)
print(type(colors))    # <class 'tuple'>
```

### One-item tuple

A trailing comma is required.

```python
a = ("apple",)
b = ("apple")

print(type(a))    # <class 'tuple'>
print(type(b))    # <class 'str'>
```

### Tuple without brackets (packing)

```python
t = 1, 2, 3
print(t)    # (1, 2, 3)
```

### Using the `tuple()` constructor

```python
t = tuple(["a", "b", "c"])
print(t)    # ('a', 'b', 'c')
```

### Accessing items

```python
colors = ("red", "green", "blue", "yellow")
print(colors[0])      # red
print(colors[-1])     # yellow
print(colors[1:3])    # ('green', 'blue')
```

### Tuples cannot be changed

```python
t = (1, 2, 3)
# t[0] = 10    # TypeError: 'tuple' object does not support item assignment
```

**Workaround:** convert to a list, change it, then convert back.

```python
t = ("apple", "banana", "cherry")
temp = list(t)
temp[1] = "mango"
temp.append("kiwi")
t = tuple(temp)
print(t)    # ('apple', 'mango', 'cherry', 'kiwi')
```

### Unpacking

```python
fruits = ("apple", "banana", "cherry")
a, b, c = fruits
print(a, b, c)    # apple banana cherry
```

**Using `*` to collect the rest**

```python
nums = (1, 2, 3, 4, 5)
first, *middle, last = nums
print(first)     # 1
print(middle)    # [2, 3, 4]
print(last)      # 5
```

### Looping through a tuple

```python
for color in ("red", "green", "blue"):
    print(color)
```

### Joining and repeating

```python
a = (1, 2)
b = (3, 4)
print(a + b)    # (1, 2, 3, 4)
print(a * 3)    # (1, 2, 1, 2, 1, 2)
```

### Tuple methods

Tuples have only two methods:

| Method | Description |
|---|---|
| `count(x)` | Number of times `x` appears |
| `index(x)` | Index of first occurrence of `x` |

```python
t = (1, 2, 2, 3, 2)
print(t.count(2))     # 3
print(t.index(3))     # 3
```

### Why use tuples?

- Protect data that should not change (coordinates, RGB colors, dates).
- Slightly faster and use less memory than lists.
- Can be used as dictionary keys (lists cannot).
- Return multiple values from a function.

```python
def min_max(nums):
    return min(nums), max(nums)

low, high = min_max([4, 8, 1, 9])
print(low, high)    # 1 9
```

### Tuple as a dictionary key

```python
locations = {
    (31.5, 74.3): "Lahore",
    (24.8, 67.0): "Karachi",
}
print(locations[(31.5, 74.3)])    # Lahore
```

---

## 2.3 Python Sets

A set is an **unordered**, **unindexed** collection of **unique** items. Sets are written with curly brackets.

### Creating sets

**Syntax**

```python
set_name = {item1, item2, item3}
```

```python
fruits = {"apple", "banana", "cherry"}
print(fruits)
print(type(fruits))    # <class 'set'>
```

### Empty set

`{}` creates an empty **dictionary**, not a set. Use `set()`.

```python
empty = set()
print(type(empty))    # <class 'set'>
print(type({}))       # <class 'dict'>
```

### Key properties

| Property | Meaning |
|---|---|
| Unordered | No fixed position; order may differ each run |
| Unique | Duplicates are removed automatically |
| No indexing | `my_set[0]` raises an error |
| Items are immutable | Items must be hashable (numbers, strings, tuples) |
| Set itself is changeable | You can add and remove items |

### Duplicates are ignored

```python
s = {1, 2, 2, 3, 3, 3}
print(s)    # {1, 2, 3}
```

**Remove duplicates from a list**

```python
nums = [1, 2, 2, 3, 3, 3, 4]
unique = list(set(nums))
print(unique)    # [1, 2, 3, 4]
```

### Length and membership

```python
fruits = {"apple", "banana", "cherry"}
print(len(fruits))            # 3
print("apple" in fruits)      # True
```

### Looping through a set

```python
for fruit in {"apple", "banana", "cherry"}:
    print(fruit)
```

### Adding items

```python
fruits = {"apple", "banana"}

fruits.add("cherry")
print(fruits)

fruits.update(["mango", "kiwi"])
print(fruits)
```

### Removing items

| Method | Behavior if item is missing |
|---|---|
| `remove(x)` | Raises `KeyError` |
| `discard(x)` | Does nothing |
| `pop()` | Removes a random item |
| `clear()` | Empties the set |

```python
fruits = {"apple", "banana", "cherry"}

fruits.remove("banana")
fruits.discard("mango")     # no error
print(fruits)

fruits.clear()
print(fruits)    # set()
```

### Set operations

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
```

| Operation | Operator | Method | Result |
|---|---|---|---|
| Union (all items) | `a \| b` | `a.union(b)` | `{1, 2, 3, 4, 5, 6}` |
| Intersection (common) | `a & b` | `a.intersection(b)` | `{3, 4}` |
| Difference (in a, not b) | `a - b` | `a.difference(b)` | `{1, 2}` |
| Symmetric difference | `a ^ b` | `a.symmetric_difference(b)` | `{1, 2, 5, 6}` |

```python
print(a | b)    # {1, 2, 3, 4, 5, 6}
print(a & b)    # {3, 4}
print(a - b)    # {1, 2}
print(a ^ b)    # {1, 2, 5, 6}
```

### Subset and superset

```python
x = {1, 2}
y = {1, 2, 3, 4}

print(x.issubset(y))       # True
print(y.issuperset(x))     # True
print(x.isdisjoint({7}))   # True (nothing in common)
```

### Set comprehension

```python
squares = {n ** 2 for n in range(1, 6)}
print(squares)    # {1, 4, 9, 16, 25}
```

### Frozen sets

An unchangeable set.

```python
fs = frozenset([1, 2, 3])
# fs.add(4)    # AttributeError
print(fs)
```

### Set methods reference

| Method | Description |
|---|---|
| `add(x)` | Add one item |
| `update(it)` | Add many items |
| `remove(x)` | Remove, error if missing |
| `discard(x)` | Remove, no error |
| `pop()` | Remove a random item |
| `clear()` | Remove all items |
| `copy()` | Copy the set |
| `union()` | Items in either set |
| `intersection()` | Items in both sets |
| `difference()` | Items only in the first |
| `symmetric_difference()` | Items in only one set |
| `issubset()` / `issuperset()` / `isdisjoint()` | Relationship tests |

### Real-world example

```python
python_students = {"Ali", "Sara", "Bilal", "Hina"}
java_students = {"Sara", "Hina", "Omar"}

print("Both courses:", python_students & java_students)
print("Any course:  ", python_students | java_students)
print("Python only: ", python_students - java_students)
```

---

## 2.4 Python Dictionaries

A dictionary stores data as **key: value** pairs. It is **ordered** (as of Python 3.7), **changeable**, and does **not allow duplicate keys**.

### Creating dictionaries

**Syntax**

```python
dict_name = {key1: value1, key2: value2}
```

```python
person = {
    "name": "Ali",
    "age": 25,
    "city": "Lahore",
}

print(person)
print(type(person))    # <class 'dict'>
```

Using the `dict()` constructor:

```python
person = dict(name="Ali", age=25, city="Lahore")
print(person)
```

### Key properties

| Property | Meaning |
|---|---|
| Key-value pairs | Each item has a key and a value |
| Ordered | Keeps insertion order (Python 3.7+) |
| Changeable | Add, edit, or delete items |
| Unique keys | A repeated key overwrites the earlier value |
| Keys must be immutable | Strings, numbers, and tuples work; lists do not |

```python
d = {"a": 1, "a": 2}
print(d)    # {'a': 2}
```

### Accessing values

```python
person = {"name": "Ali", "age": 25}

print(person["name"])        # Ali
print(person.get("age"))     # 25
```

**Difference between `[]` and `get()`**

```python
# print(person["salary"])     # KeyError
print(person.get("salary"))         # None
print(person.get("salary", 0))      # 0 (default value)
```

### Keys, values, and items

```python
person = {"name": "Ali", "age": 25, "city": "Lahore"}

print(person.keys())      # dict_keys(['name', 'age', 'city'])
print(person.values())    # dict_values(['Ali', 25, 'Lahore'])
print(person.items())     # dict_items([('name', 'Ali'), ('age', 25), ('city', 'Lahore')])
```

### Checking if a key exists

```python
if "name" in person:
    print("Key exists")
```

### Changing and adding items

```python
person = {"name": "Ali", "age": 25}

person["age"] = 26            # change
person["city"] = "Lahore"     # add new key
print(person)    # {'name': 'Ali', 'age': 26, 'city': 'Lahore'}

person.update({"age": 27, "job": "Developer"})
print(person)
```

### Removing items

| Method | Purpose |
|---|---|
| `pop(key)` | Remove key and return its value |
| `popitem()` | Remove the last inserted pair |
| `del d[key]` | Delete a key |
| `clear()` | Empty the dictionary |

```python
person = {"name": "Ali", "age": 25, "city": "Lahore"}

age = person.pop("age")
print(age)       # 25

del person["city"]
print(person)    # {'name': 'Ali'}

person.clear()
print(person)    # {}
```

### Looping through a dictionary

```python
person = {"name": "Ali", "age": 25, "city": "Lahore"}

# Keys (default)
for key in person:
    print(key)

# Values
for value in person.values():
    print(value)

# Keys and values together
for key, value in person.items():
    print(f"{key}: {value}")
```

```
name: Ali
age: 25
city: Lahore
```

### Copying a dictionary

```python
a = {"x": 1, "y": 2}
b = a.copy()       # or dict(a)
b["x"] = 100
print(a)    # {'x': 1, 'y': 2}
print(b)    # {'x': 100, 'y': 2}
```

### Nested dictionaries

```python
students = {
    "s1": {"name": "Ali", "grade": "A"},
    "s2": {"name": "Sara", "grade": "B"},
}

print(students["s1"]["name"])    # Ali
students["s2"]["grade"] = "A"

for sid, info in students.items():
    print(sid, info["name"], info["grade"])
```

### Dictionary comprehension

**Syntax**

```python
{key_expr: value_expr for item in iterable}
```

```python
squares = {n: n ** 2 for n in range(1, 6)}
print(squares)    # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

### `setdefault()` and `fromkeys()`

```python
d = {"a": 1}
d.setdefault("b", 2)      # adds b only if missing
d.setdefault("a", 99)     # a already exists, unchanged
print(d)    # {'a': 1, 'b': 2}

keys = ["x", "y", "z"]
d2 = dict.fromkeys(keys, 0)
print(d2)    # {'x': 0, 'y': 0, 'z': 0}
```

### Merging dictionaries

```python
a = {"x": 1, "y": 2}
b = {"y": 99, "z": 3}

print(a | b)     # {'x': 1, 'y': 99, 'z': 3}   (Python 3.9+)
print({**a, **b})                              # works in older versions too
```

### Sorting a dictionary

```python
scores = {"Ali": 70, "Sara": 95, "Bilal": 82}

# Sort by key
print(dict(sorted(scores.items())))

# Sort by value, highest first
print(dict(sorted(scores.items(), key=lambda kv: kv[1], reverse=True)))
```

### Dictionary methods reference

| Method | Description |
|---|---|
| `get(k, default)` | Value for key, or default |
| `keys()` | View of all keys |
| `values()` | View of all values |
| `items()` | View of key-value pairs |
| `update(d)` | Merge another dictionary |
| `pop(k)` | Remove key and return value |
| `popitem()` | Remove last inserted pair |
| `setdefault(k, v)` | Get key, or set it if missing |
| `clear()` | Remove everything |
| `copy()` | Shallow copy |
| `fromkeys(keys, v)` | New dict from keys |

### Real-world example: word counter

```python
text = "apple banana apple cherry banana apple"
counts = {}

for word in text.split():
    counts[word] = counts.get(word, 0) + 1

print(counts)    # {'apple': 3, 'banana': 2, 'cherry': 1}
```

---

## 2.5 Python Arrays

Python has no built-in array type like some other languages. There are two common options:

| Option | Use it when |
|---|---|
| **Lists** | General purpose; store mixed types. Most people use these as "arrays". |
| **`array` module** | You need a compact array of one numeric type |
| **NumPy arrays** | Scientific computing and large numeric data (third-party package) |

### Using a list as an array

```python
cars = ["Ford", "Volvo", "BMW"]
print(cars[0])        # Ford
cars.append("Honda")
print(len(cars))      # 4
```

### The `array` module

Stores items of **one fixed type**, using less memory than a list.

**Syntax**

```python
from array import array
array(typecode, [values])
```

**Common type codes**

| Code | C type | Python type | Bytes |
|---|---|---|---|
| `'b'` | signed char | int | 1 |
| `'B'` | unsigned char | int | 1 |
| `'h'` | signed short | int | 2 |
| `'i'` | signed int | int | 2 or 4 |
| `'l'` | signed long | int | 4 or 8 |
| `'f'` | float | float | 4 |
| `'d'` | double | float | 8 |
| `'u'` | Unicode char | str | 2 or 4 |

```python
from array import array

nums = array("i", [10, 20, 30, 40])
print(nums)            # array('i', [10, 20, 30, 40])
print(nums[0])         # 10
print(len(nums))       # 4
```

### Type is enforced

```python
from array import array

nums = array("i", [1, 2, 3])
# nums.append(2.5)     # TypeError: 'float' object cannot be interpreted as an integer
```

### Array operations

```python
from array import array

a = array("i", [3, 1, 4])

a.append(5)
print(a)                 # array('i', [3, 1, 4, 5])

a.insert(1, 9)
print(a)                 # array('i', [3, 9, 1, 4, 5])

a.extend([7, 8])
print(a)

a.remove(9)
print(a)

print(a.pop())           # 8   (last item)
print(a.index(4))        # position of value 4
print(a.count(1))        # 1

a.reverse()
print(a)

print(a.tolist())        # convert to a regular list
```

### Looping through an array

```python
from array import array

for x in array("d", [1.5, 2.5, 3.5]):
    print(x)
```

### Sorting an array

The `array` type has no `sort()` method. Use `sorted()`:

```python
from array import array

a = array("i", [5, 2, 8, 1])
a = array("i", sorted(a))
print(a)    # array('i', [1, 2, 5, 8])
```

### List vs `array`

| Feature | `list` | `array` |
|---|---|---|
| Item types | Mixed | One type only |
| Memory use | Higher | Lower |
| Speed for numeric work | Slower | Faster |
| Built in | Yes | Needs `import` |
| Flexibility | High | Limited |

### NumPy arrays (preview)

For serious numeric work, use NumPy (install with `pip install numpy`).

```python
import numpy as np

a = np.array([1, 2, 3, 4])
print(a * 2)          # [2 4 6 8]  (works on every item at once)
print(a.mean())       # 2.5

m = np.array([[1, 2], [3, 4]])
print(m.shape)        # (2, 2)
```

With a plain list, `[1, 2, 3] * 2` repeats the list instead of doubling each value. NumPy does element-wise math.

---

## Comparison of Data Structures

| Feature | List | Tuple | Set | Dictionary |
|---|---|---|---|---|
| Syntax | `[1, 2]` | `(1, 2)` | `{1, 2}` | `{"a": 1}` |
| Ordered | Yes | Yes | No | Yes (3.7+) |
| Changeable | Yes | No | Yes | Yes |
| Duplicates | Allowed | Allowed | Not allowed | Keys: not allowed |
| Indexed | Yes | Yes | No | By key |
| Best for | General sequences | Fixed data | Unique items, set math | Labeled data, lookups |

### Which one should I use?

```
Need labeled data (name → value)?        → Dictionary
Need unique items or set math?           → Set
Data should never change?                → Tuple
Need an ordered, changeable collection?  → List
```

### Converting between types

```python
data = [1, 2, 2, 3]

print(tuple(data))    # (1, 2, 2, 3)
print(set(data))      # {1, 2, 3}
print(list(set(data)))

pairs = [("a", 1), ("b", 2)]
print(dict(pairs))    # {'a': 1, 'b': 2}
```

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Lists | Ordered, changeable, allow duplicates; `append`, `insert`, `remove`, `pop`, `sort`, slicing |
| Tuples | Ordered, unchangeable; use `(x,)` for one item; unpacking; can be dictionary keys |
| Sets | Unordered, unique items; union `\|`, intersection `&`, difference `-`; use `set()` for empty |
| Dictionaries | Key-value pairs; `get()`, `items()`, `update()`, nested dicts, comprehensions |
| Arrays | Use lists for general work, the `array` module for compact typed data, NumPy for math |

### Quick syntax cheat sheet

```python
# List
lst = [1, 2, 3]
lst.append(4); lst[0]; lst[1:3]; lst.sort()

# Tuple
tup = (1, 2, 3)
a, b, c = tup

# Set
s = {1, 2, 3}
s.add(4); s1 | s2; s1 & s2; s1 - s2

# Dictionary
d = {"name": "Ali", "age": 25}
d["name"]; d.get("x", 0); d["city"] = "Lahore"
for k, v in d.items():
    print(k, v)

# Array
from array import array
arr = array("i", [1, 2, 3])
```

---

## Practice Exercises

### Beginner

1. Create a list of 5 favorite foods. Print the first and last items.
2. Add a new item to the end of your list and remove the second item.
3. Create a tuple of three numbers and unpack it into three variables.
4. Create a set from `[1, 2, 2, 3, 3, 3]` and print its length.
5. Create a dictionary with your name, age, and city. Print each key and value.

### Intermediate

6. Given `nums = [5, 3, 8, 1, 9, 2]`, print the largest, smallest, sum, and the list sorted in descending order.
7. Remove duplicates from `[1, 2, 2, 3, 4, 4, 5]` while keeping the result as a list.
8. Given two sets `{1, 2, 3, 4}` and `{3, 4, 5, 6}`, print their union, intersection, and both differences.
9. Count how often each letter appears in the word `"mississippi"` using a dictionary.
10. Use a list comprehension to create a list of the squares of all odd numbers from 1 to 15.

### Challenge

11. Build a student dictionary where each key is a student name and each value is a list of marks. Print each student's average.
12. Given a list of names, group them by their first letter into a dictionary, for example `{"A": ["Ali", "Amna"], "B": ["Bilal"]}`.
13. Write a program that finds the common items between two lists without duplicates.

### Sample solution (Exercise 9)

```python
word = "mississippi"
counts = {}

for letter in word:
    counts[letter] = counts.get(letter, 0) + 1

print(counts)    # {'m': 1, 'i': 4, 's': 4, 'p': 2}
```

### Sample solution (Exercise 11)

```python
students = {
    "Ali": [80, 90, 70],
    "Sara": [95, 85, 100],
}

for name, marks in students.items():
    average = sum(marks) / len(marks)
    print(f"{name}: {average:.1f}")
```

### Sample solution (Exercise 12)

```python
names = ["Ali", "Amna", "Bilal", "Bushra", "Sara"]
groups = {}

for name in names:
    groups.setdefault(name[0], []).append(name)

print(groups)
# {'A': ['Ali', 'Amna'], 'B': ['Bilal', 'Bushra'], 'S': ['Sara']}
```

---

**Previous:** [Chapter 1: Python Fundamentals](chapter-01-python-fundamentals.md)
**Next:** [Chapter 3: Python Control Flow](#)
