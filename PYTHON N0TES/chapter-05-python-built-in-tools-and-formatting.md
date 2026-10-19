# Chapter 5: Python Built-in Tools and Formatting

> Python's standard library gives you ready-made tools for dates, math, JSON data, and text patterns. This chapter also covers string formatting, the special `None` value, and how to install packages and keep projects separate with `pip` and virtual environments.

## Table of Contents

- [5.1 Python Dates](#51-python-dates)
- [5.2 Python Math](#52-python-math)
- [5.3 Python JSON](#53-python-json)
- [5.4 Python RegEx](#54-python-regex)
- [5.5 Python String Formatting](#55-python-string-formatting)
- [5.6 Python None](#56-python-none)
- [5.7 Python PIP](#57-python-pip)
- [5.8 Python Virtual Environments](#58-python-virtual-environments)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 5.1 Python Dates

Python has no date type of its own. Use the built-in `datetime` module.

```python
import datetime
```

### Current date and time

```python
import datetime

now = datetime.datetime.now()
print(now)             # 2026-10-18 19:41:05.123456
print(now.year)        # 2026
print(now.month)       # 10
print(now.day)         # 18
print(now.hour)        # 19
print(now.minute)      # 41
print(now.second)      # 5
```

### Main classes in the module

| Class | Holds |
|---|---|
| `datetime.date` | Year, month, day |
| `datetime.time` | Hour, minute, second, microsecond |
| `datetime.datetime` | Date and time together |
| `datetime.timedelta` | A duration (difference between two moments) |
| `datetime.timezone` | A fixed UTC offset |

### Today's date only

```python
from datetime import date

today = date.today()
print(today)             # 2026-10-18
print(today.weekday())   # 6  (Monday = 0 ... Sunday = 6)
print(today.isoweekday())# 7  (Monday = 1 ... Sunday = 7)
```

### Creating a specific date

**Syntax**

```python
datetime.datetime(year, month, day, hour, minute, second)
```

```python
import datetime

d = datetime.date(2026, 12, 25)
print(d)    # 2026-12-25

dt = datetime.datetime(2026, 12, 25, 9, 30, 0)
print(dt)   # 2026-12-25 09:30:00
```

Only `year`, `month`, and `day` are required for `datetime`. The rest default to `0`.

### Formatting dates with `strftime()`

**Syntax**

```python
date_object.strftime(format_code)
```

```python
import datetime

x = datetime.datetime(2026, 10, 18, 19, 41)

print(x.strftime("%Y-%m-%d"))          # 2026-10-18
print(x.strftime("%d/%m/%Y"))          # 18/10/2026
print(x.strftime("%B %d, %Y"))         # October 18, 2026
print(x.strftime("%A"))                # Sunday
print(x.strftime("%I:%M %p"))          # 07:41 PM
print(x.strftime("%H:%M:%S"))          # 19:41:00
```

### Common format codes

| Code | Meaning | Example |
|---|---|---|
| `%Y` | Year, 4 digits | `2026` |
| `%y` | Year, 2 digits | `26` |
| `%m` | Month number | `10` |
| `%B` | Month name | `October` |
| `%b` | Month name, short | `Oct` |
| `%d` | Day of month | `18` |
| `%A` | Weekday name | `Sunday` |
| `%a` | Weekday name, short | `Sun` |
| `%H` | Hour, 24-hour | `19` |
| `%I` | Hour, 12-hour | `07` |
| `%M` | Minute | `41` |
| `%S` | Second | `05` |
| `%p` | AM / PM | `PM` |
| `%j` | Day of the year | `291` |
| `%U` | Week number of the year | `41` |
| `%f` | Microsecond | `123456` |
| `%z` | UTC offset | `+0500` |

### Parsing text into dates with `strptime()`

**Syntax**

```python
datetime.datetime.strptime(text, format_code)
```

```python
from datetime import datetime

d = datetime.strptime("25/12/2026", "%d/%m/%Y")
print(d)             # 2026-12-25 00:00:00
print(d.year)        # 2026

t = datetime.strptime("March 5, 2026 14:30", "%B %d, %Y %H:%M")
print(t)             # 2026-03-05 14:30:00
```

> `strftime` turns a date **into** text. `strptime` turns text **into** a date. (Memory aid: **f**ormat, **p**arse.)

### ISO format

```python
from datetime import datetime, date

print(datetime(2026, 10, 18, 9, 30).isoformat())    # 2026-10-18T09:30:00

d = date.fromisoformat("2026-10-18")
print(d)    # 2026-10-18

dt = datetime.fromisoformat("2026-10-18T09:30:00")
print(dt)   # 2026-10-18 09:30:00
```

### Date arithmetic with `timedelta`

```python
from datetime import datetime, timedelta

now = datetime(2026, 10, 18)

print(now + timedelta(days=7))              # 2026-10-25 00:00:00
print(now - timedelta(days=30))             # 2026-09-18 00:00:00
print(now + timedelta(hours=5, minutes=30)) # 2026-10-18 05:30:00
print(now + timedelta(weeks=2))             # 2026-11-01 00:00:00
```

**Difference between two dates**

```python
from datetime import date

start = date(2026, 1, 1)
end = date(2026, 12, 25)

diff = end - start
print(diff)             # 358 days, 0:00:00
print(diff.days)        # 358
```

### Comparing dates

```python
from datetime import date

a = date(2026, 5, 1)
b = date(2026, 8, 1)

print(a < b)     # True
print(a == b)    # False
print(max(a, b)) # 2026-08-01
```

### Calculate age

```python
from datetime import date

def age(born):
    today = date.today()
    years = today.year - born.year
    if (today.month, today.day) < (born.month, born.day):
        years -= 1
    return years

print(age(date(2000, 5, 15)))
```

### Days until an event

```python
from datetime import date

event = date(2026, 12, 25)
left = (event - date.today()).days
print(f"{left} days left")
```

### Replacing parts of a date

```python
from datetime import datetime

d = datetime(2026, 10, 18, 9, 30)
print(d.replace(year=2030, hour=0))    # 2030-10-18 00:30:00
```

### Time zones

```python
from datetime import datetime, timezone, timedelta

utc_now = datetime.now(timezone.utc)
print(utc_now)

pk = timezone(timedelta(hours=5))
print(utc_now.astimezone(pk))
```

For named zones such as `"Asia/Karachi"`, use `zoneinfo` (Python 3.9+):

```python
from datetime import datetime
from zoneinfo import ZoneInfo

now = datetime.now(ZoneInfo("Asia/Karachi"))
print(now)

ny = now.astimezone(ZoneInfo("America/New_York"))
print(ny)
```

### Timestamps

```python
import time
from datetime import datetime

print(time.time())                               # seconds since 1970-01-01
print(datetime.fromtimestamp(1_700_000_000))
print(datetime(2026, 1, 1).timestamp())
```

### Measuring how long code takes

```python
import time

start = time.perf_counter()
total = sum(range(1_000_000))
elapsed = time.perf_counter() - start
print(f"Took {elapsed:.4f} seconds")
```

### Pausing a program

```python
import time

print("Wait...")
time.sleep(2)    # pause 2 seconds
print("Done")
```

### Calendar module

```python
import calendar

print(calendar.month(2026, 10))
print(calendar.isleap(2028))             # True
print(calendar.monthrange(2026, 2))      # (6, 28) -> starts Sunday, 28 days
```

---

## 5.2 Python Math

### Built-in math functions

| Function | Purpose | Example | Result |
|---|---|---|---|
| `min()` | Smallest value | `min(3, 9, 1)` | `1` |
| `max()` | Largest value | `max(3, 9, 1)` | `9` |
| `abs()` | Absolute value | `abs(-7.5)` | `7.5` |
| `pow(x, y)` | x to the power y | `pow(2, 5)` | `32` |
| `round(x, n)` | Round to n digits | `round(3.14159, 2)` | `3.14` |
| `sum()` | Total of an iterable | `sum([1, 2, 3])` | `6` |
| `divmod(a, b)` | Quotient and remainder | `divmod(17, 5)` | `(3, 2)` |

```python
print(min(5, 10, 25))       # 5
print(max(5, 10, 25))       # 25
print(abs(-7.25))           # 7.25
print(pow(4, 3))            # 64
print(round(2.675, 2))      # 2.67 (float precision)
print(divmod(17, 5))        # (3, 2)
```

> `round()` uses "banker's rounding" for halves: `round(2.5)` is `2` and `round(3.5)` is `4`.

### The `math` module

```python
import math
```

#### Roots, powers, and logarithms

```python
import math

print(math.sqrt(64))        # 8.0
print(math.pow(2, 3))       # 8.0
print(math.exp(1))          # 2.718281828459045
print(math.log(100, 10))    # 2.0
print(math.log10(1000))     # 3.0
print(math.log2(8))         # 3.0
print(math.log(math.e))     # 1.0 (natural log)
print(math.isqrt(17))       # 4 (integer square root)
```

#### Rounding

```python
import math

print(math.ceil(1.2))     # 2  (round up)
print(math.floor(1.8))    # 1  (round down)
print(math.trunc(-2.7))   # -2 (drop the decimal part)
```

| Value | `round()` | `math.ceil()` | `math.floor()` | `math.trunc()` |
|---|---|---|---|---|
| `2.7` | `3` | `3` | `2` | `2` |
| `-2.7` | `-3` | `-2` | `-3` | `-2` |

#### Constants

```python
import math

print(math.pi)     # 3.141592653589793
print(math.e)      # 2.718281828459045
print(math.tau)    # 6.283185307179586
print(math.inf)    # inf
print(math.nan)    # nan
```

#### Trigonometry (angles are in radians)

```python
import math

print(math.sin(math.pi / 2))      # 1.0
print(math.cos(0))                # 1.0
print(math.tan(math.pi / 4))      # 0.9999999999999999

print(math.radians(180))          # 3.141592653589793
print(math.degrees(math.pi))      # 180.0
```

#### Factorial, GCD, and combinations

```python
import math

print(math.factorial(5))      # 120
print(math.gcd(12, 18))       # 6
print(math.lcm(4, 6))         # 12      (Python 3.9+)
print(math.comb(5, 2))        # 10      (5 choose 2)
print(math.perm(5, 2))        # 20
```

#### Distance and hypotenuse

```python
import math

print(math.hypot(3, 4))                      # 5.0
print(math.dist((0, 0), (3, 4)))             # 5.0
```

#### Checking special values

```python
import math

print(math.isnan(float("nan")))     # True
print(math.isinf(float("inf")))     # True
print(math.isclose(0.1 + 0.2, 0.3)) # True
```

Never compare floats with `==` when they come from calculations. Use `math.isclose()`.

### Random numbers

```python
import random

print(random.random())                   # float in [0.0, 1.0)
print(random.randint(1, 6))              # int from 1 to 6, inclusive
print(random.randrange(0, 10, 2))        # 0, 2, 4, 6, or 8
print(random.uniform(1.5, 5.5))          # float in range
print(random.choice(["a", "b", "c"]))    # one random item
print(random.choices(["a", "b", "c"], k=5))   # 5 items, with repeats
print(random.sample(range(1, 50), 6))    # 6 unique items
```

**Shuffle a list**

```python
import random

cards = [1, 2, 3, 4, 5]
random.shuffle(cards)
print(cards)
```

**Reproducible results with a seed**

```python
import random

random.seed(42)
print(random.randint(1, 100))    # same value every run
```

> `random` is not secure. For passwords and tokens, use the `secrets` module.

```python
import secrets

print(secrets.token_hex(8))
print(secrets.randbelow(100))
```

### Statistics

```python
import statistics

data = [2, 4, 4, 4, 5, 5, 7, 9]

print(statistics.mean(data))       # 5
print(statistics.median(data))     # 4.5
print(statistics.mode(data))       # 4
print(statistics.stdev(data))      # sample standard deviation
print(statistics.pstdev(data))     # 2.0 (population)
print(statistics.variance(data))
```

### Exact decimals and fractions

```python
from decimal import Decimal, getcontext
from fractions import Fraction

print(0.1 + 0.2)                            # 0.30000000000000004
print(Decimal("0.1") + Decimal("0.2"))      # 0.3

getcontext().prec = 10
print(Decimal(1) / Decimal(3))              # 0.3333333333

print(Fraction(1, 3) + Fraction(1, 6))      # 1/2
```

### Complex numbers: `cmath`

```python
import cmath

print(cmath.sqrt(-4))     # 2j
```

### Real-world example: compound interest

```python
principal = 10_000
rate = 0.07
years = 5

amount = principal * (1 + rate) ** years
print(f"After {years} years: {amount:,.2f}")    # 14,025.52
```

### Real-world example: circle calculator

```python
import math

r = 7
print(f"Area:          {math.pi * r ** 2:.2f}")       # 153.94
print(f"Circumference: {2 * math.pi * r:.2f}")        # 43.98
```

### Real-world example: quadratic equation

```python
import math

a, b, c = 1, -3, 2
d = b ** 2 - 4 * a * c

if d < 0:
    print("No real roots")
else:
    x1 = (-b + math.sqrt(d)) / (2 * a)
    x2 = (-b - math.sqrt(d)) / (2 * a)
    print(x1, x2)    # 2.0 1.0
```

---

## 5.3 Python JSON

**JSON** (JavaScript Object Notation) is a lightweight text format for exchanging data. It is used by web APIs, config files, and databases.

```python
import json
```

### JSON syntax

```json
{
  "name": "Ali",
  "age": 25,
  "is_student": false,
  "courses": ["Python", "SQL"],
  "address": { "city": "Lahore", "zip": null }
}
```

### Python ↔ JSON type mapping

| Python | JSON |
|---|---|
| `dict` | object `{}` |
| `list`, `tuple` | array `[]` |
| `str` | string |
| `int`, `float` | number |
| `True` | `true` |
| `False` | `false` |
| `None` | `null` |

### JSON string → Python: `json.loads()`

**Syntax**

```python
json.loads(json_string)
```

```python
import json

text = '{"name": "Ali", "age": 25, "courses": ["Python", "SQL"]}'
data = json.loads(text)

print(type(data))           # <class 'dict'>
print(data["name"])         # Ali
print(data["courses"][0])   # Python
```

### Python → JSON string: `json.dumps()`

**Syntax**

```python
json.dumps(object, indent=None, sort_keys=False)
```

```python
import json

person = {
    "name": "Ali",
    "age": 25,
    "is_student": False,
    "courses": ["Python", "SQL"],
    "address": None,
}

print(json.dumps(person))
# {"name": "Ali", "age": 25, "is_student": false, "courses": ["Python", "SQL"], "address": null}
```

### Pretty printing

```python
print(json.dumps(person, indent=4))
```

```json
{
    "name": "Ali",
    "age": 25,
    "is_student": false,
    "courses": [
        "Python",
        "SQL"
    ],
    "address": null
}
```

```python
print(json.dumps(person, indent=2, sort_keys=True))
print(json.dumps(person, separators=(",", ":")))    # compact
```

### Non-ASCII text

```python
import json

data = {"city": "لاہور"}
print(json.dumps(data))                         # escapes as \u....
print(json.dumps(data, ensure_ascii=False))     # keeps the real characters
```

### Reading a JSON file: `json.load()`

```python
import json

with open("data.json", "r", encoding="utf-8") as f:
    data = json.load(f)

print(data)
```

### Writing a JSON file: `json.dump()`

```python
import json

data = {"name": "Ali", "scores": [90, 85]}

with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=4)
```

### `load` vs `loads`, `dump` vs `dumps`

| Function | Works with | Direction |
|---|---|---|
| `json.loads(s)` | **s**tring | JSON → Python |
| `json.load(f)` | **f**ile | JSON → Python |
| `json.dumps(obj)` | **s**tring | Python → JSON |
| `json.dump(obj, f)` | **f**ile | Python → JSON |

### Handling invalid JSON

```python
import json

try:
    data = json.loads("{name: 'Ali'}")    # invalid JSON
except json.JSONDecodeError as e:
    print("Invalid JSON:", e)
```

JSON requires **double quotes** around strings and keys.

### Objects JSON cannot store by default

```python
import json
from datetime import datetime

data = {"time": datetime(2026, 10, 18, 9, 30)}
# json.dumps(data)    # TypeError: Object of type datetime is not JSON serializable

print(json.dumps(data, default=str))    # {"time": "2026-10-18 09:30:00"}
```

**Custom encoder function**

```python
import json
from datetime import datetime

def encode(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()
    if isinstance(obj, set):
        return list(obj)
    raise TypeError(f"Cannot serialize {type(obj)}")

data = {"time": datetime(2026, 10, 18), "tags": {"a", "b"}}
print(json.dumps(data, default=encode))
```

### Tuples become lists

```python
import json

print(json.dumps((1, 2, 3)))            # [1, 2, 3]
print(json.loads("[1, 2, 3]"))          # [1, 2, 3]  (a list, not a tuple)
```

### Dictionary keys become strings

```python
import json

print(json.dumps({1: "a", 2: "b"}))     # {"1": "a", "2": "b"}
```

### Working with nested JSON

```python
import json

text = """
{
  "students": [
    {"name": "Ali", "marks": [80, 90]},
    {"name": "Sara", "marks": [95, 85]}
  ]
}
"""

data = json.loads(text)

for s in data["students"]:
    avg = sum(s["marks"]) / len(s["marks"])
    print(f"{s['name']}: {avg}")
```

```
Ali: 85.0
Sara: 90.0
```

### Safe access to missing keys

```python
city = data.get("address", {}).get("city", "Unknown")
```

### Real-world example: a simple JSON "database"

```python
import json
import os

FILE = "contacts.json"

def load():
    if not os.path.exists(FILE):
        return []
    with open(FILE, encoding="utf-8") as f:
        return json.load(f)

def save(contacts):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(contacts, f, indent=2)

contacts = load()
contacts.append({"name": "Ali", "phone": "0300-1234567"})
save(contacts)
print(load())
```

### Command line pretty-printer

```bash
python -m json.tool data.json
```

---

## 5.4 Python RegEx

A **regular expression** (RegEx) is a pattern used to search, match, and edit text. Python provides the `re` module.

```python
import re
```

### First example

```python
import re

text = "My phone number is 0300-1234567"
match = re.search(r"\d{4}-\d{7}", text)

if match:
    print(match.group())    # 0300-1234567
```

### Raw strings

Always write patterns as **raw strings** (`r"..."`) so backslashes are not treated as escape codes.

```python
pattern = r"\d+"    # good
```

### Core `re` functions

| Function | Purpose | Returns |
|---|---|---|
| `re.search(p, s)` | Find the first match anywhere | Match object or `None` |
| `re.match(p, s)` | Match only at the **start** | Match object or `None` |
| `re.fullmatch(p, s)` | Match the **whole** string | Match object or `None` |
| `re.findall(p, s)` | Find all matches | List of strings |
| `re.finditer(p, s)` | Find all matches | Iterator of match objects |
| `re.sub(p, repl, s)` | Replace matches | New string |
| `re.split(p, s)` | Split at matches | List |
| `re.compile(p)` | Pre-build a pattern for reuse | Pattern object |

```python
import re

text = "The rain in Spain"

print(re.search(r"ain", text).group())        # ain
print(re.match(r"The", text) is not None)     # True
print(re.findall(r"ain", text))               # ['ain', 'ain']
print(re.sub(r"ain", "AIN", text))            # The rAIN in SpAIN
print(re.split(r"\s", text))                  # ['The', 'rain', 'in', 'Spain']
```

### Match object methods

```python
import re

m = re.search(r"(\d{4})-(\d{2})-(\d{2})", "Date: 2026-10-18")

print(m.group())     # 2026-10-18  (whole match)
print(m.group(1))    # 2026
print(m.group(2))    # 10
print(m.groups())    # ('2026', '10', '18')
print(m.start())     # 6
print(m.end())       # 16
print(m.span())      # (6, 16)
```

### Metacharacters

| Character | Meaning | Example |
|---|---|---|
| `.` | Any character except newline | `a.c` matches `abc`, `a1c` |
| `^` | Start of string | `^Hello` |
| `$` | End of string | `world$` |
| `*` | 0 or more | `ab*` matches `a`, `abbb` |
| `+` | 1 or more | `ab+` matches `ab`, `abbb` |
| `?` | 0 or 1 | `colou?r` |
| `{n}` | Exactly n | `\d{4}` |
| `{n,m}` | n to m | `\d{2,4}` |
| `[]` | A set of characters | `[aeiou]` |
| `\|` | Either / or | `cat\|dog` |
| `()` | Group | `(ab)+` |
| `\` | Escape a special character | `\.` matches a dot |

### Special sequences

| Code | Matches |
|---|---|
| `\d` | A digit (0-9) |
| `\D` | A non-digit |
| `\w` | A letter, digit, or underscore |
| `\W` | Anything else |
| `\s` | Whitespace (space, tab, newline) |
| `\S` | Non-whitespace |
| `\b` | Word boundary |
| `\B` | Not a word boundary |
| `\A` | Start of the string |
| `\Z` | End of the string |

### Sets

| Pattern | Meaning |
|---|---|
| `[abc]` | `a`, `b`, or `c` |
| `[a-z]` | Any lowercase letter |
| `[A-Za-z0-9]` | Any letter or digit |
| `[^abc]` | Anything **except** `a`, `b`, `c` |
| `[0-9]` | Any digit |
| `[+*.]` | Literal `+`, `*`, `.` (special chars lose meaning inside `[]`) |

```python
import re

print(re.findall(r"[aeiou]", "education"))     # ['e', 'u', 'a', 'i', 'o']
print(re.findall(r"[^aeiou\s]", "hi you"))     # ['h', 'y']
print(re.findall(r"[A-Z][a-z]+", "Ali and Sara met Bilal"))
# ['Ali', 'Sara', 'Bilal']
```

### Quantifiers in action

```python
import re

print(re.findall(r"\d+", "a1 b22 c333"))         # ['1', '22', '333']
print(re.findall(r"\d{2}", "a1 b22 c333"))       # ['22', '33']
print(re.findall(r"colou?r", "color colour"))    # ['color', 'colour']
```

### Greedy vs lazy matching

Quantifiers are **greedy** by default (match as much as possible). Add `?` to make them **lazy**.

```python
import re

html = "<b>bold</b> and <i>italic</i>"

print(re.findall(r"<.*>", html))     # ['<b>bold</b> and <i>italic</i>']
print(re.findall(r"<.*?>", html))    # ['<b>', '</b>', '<i>', '</i>']
```

### Anchors

```python
import re

print(bool(re.search(r"^Hello", "Hello world")))     # True
print(bool(re.search(r"world$", "Hello world")))     # True
print(bool(re.fullmatch(r"\d+", "12345")))           # True
print(bool(re.fullmatch(r"\d+", "123a")))            # False
```

### Word boundaries

```python
import re

text = "cat concatenate cat."
print(re.findall(r"\bcat\b", text))    # ['cat', 'cat']
```

### Groups

```python
import re

m = re.search(r"(\w+)@(\w+)\.com", "mail ali@example.com now")
print(m.group(1))    # ali
print(m.group(2))    # example
```

**Named groups**

```python
import re

m = re.search(r"(?P<year>\d{4})-(?P<month>\d{2})", "2026-10")
print(m.group("year"))     # 2026
print(m.group("month"))    # 10
print(m.groupdict())       # {'year': '2026', 'month': '10'}
```

**Non-capturing group** `(?:...)`

```python
import re

print(re.findall(r"(?:Mr|Ms)\. (\w+)", "Mr. Ali and Ms. Sara"))
# ['Ali', 'Sara']
```

### Alternation

```python
import re

print(re.findall(r"cat|dog", "I have a cat and a dog"))    # ['cat', 'dog']
```

### `findall()` with groups

When the pattern has groups, `findall` returns the groups.

```python
import re

print(re.findall(r"(\w+)=(\d+)", "a=1, b=22"))
# [('a', '1'), ('b', '22')]
```

### Replacing with `sub()`

```python
import re

print(re.sub(r"\s+", " ", "too    many     spaces"))    # too many spaces
print(re.sub(r"\d", "#", "PIN: 1234"))                   # PIN: ####
print(re.sub(r"(\w+) (\w+)", r"\2 \1", "hello world"))   # world hello
print(re.sub(r"\d", "X", "a1b2c3", count=2))             # aXbXc3
```

**Replace using a function**

```python
import re

def double(m):
    return str(int(m.group()) * 2)

print(re.sub(r"\d+", double, "5 apples and 12 pears"))
# 10 apples and 24 pears
```

### Splitting

```python
import re

print(re.split(r"[,;\s]+", "a, b;c   d"))    # ['a', 'b', 'c', 'd']
print(re.split(r"\d", "a1b2c3d", maxsplit=2))  # ['a', 'b', 'c3d']
```

### Flags

| Flag | Short | Meaning |
|---|---|---|
| `re.IGNORECASE` | `re.I` | Ignore case |
| `re.MULTILINE` | `re.M` | `^` and `$` match at each line |
| `re.DOTALL` | `re.S` | `.` also matches newline |
| `re.VERBOSE` | `re.X` | Allow whitespace and comments in pattern |

```python
import re

print(re.findall(r"python", "Python PYTHON python", re.I))
# ['Python', 'PYTHON', 'python']

text = "line1\nline2\nline3"
print(re.findall(r"^line\d", text, re.M))    # ['line1', 'line2', 'line3']
```

### Compiling patterns

Good when you reuse the same pattern many times.

```python
import re

pattern = re.compile(r"\d{3}-\d{4}")

print(pattern.search("Call 555-1234"))
print(pattern.findall("555-1234 and 555-9876"))
```

### Lookahead and lookbehind

```python
import re

# Followed by
print(re.findall(r"\d+(?= dollars)", "50 dollars and 70 euros"))    # ['50']

# Not followed by
print(re.findall(r"\d+(?! dollars)\b", "50 dollars and 70 euros"))  # ['70']

# Preceded by
print(re.findall(r"(?<=\$)\d+", "cost $45 and 60"))                 # ['45']
```

### Real-world patterns

**Email (basic check)**

```python
import re

email = r"^[\w.+-]+@[\w-]+\.[\w.-]+$"

for e in ["ali@example.com", "bad@", "sara.k@mail.co.uk"]:
    print(e, bool(re.match(email, e)))
```

**Pakistani mobile number**

```python
import re

phone = r"^(?:\+92|0)3\d{2}-?\d{7}$"

for p in ["0300-1234567", "03001234567", "+923001234567", "0400-123"]:
    print(p, bool(re.match(phone, p)))
```

**Extract all numbers**

```python
import re

print(re.findall(r"\d+\.?\d*", "Price 12.5 and 7 and 100.25"))
# ['12.5', '7', '100.25']
```

**Strong password check**

```python
import re

def strong(pw):
    return bool(re.fullmatch(
        r"(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^\w\s]).{8,}", pw
    ))

print(strong("Abc@1234"))    # True
print(strong("abc123"))      # False
```

**Extract hashtags**

```python
import re

print(re.findall(r"#\w+", "Learning #Python and #RegEx today"))
# ['#Python', '#RegEx']
```

**Clean text**

```python
import re

text = "  Hello,   World!!  "
clean = re.sub(r"[^\w\s]", "", text)
clean = re.sub(r"\s+", " ", clean).strip()
print(clean)    # Hello World
```

### Tips

- Use raw strings: `r"..."`.
- Test patterns on a site such as regex101.com (choose the Python flavor).
- Do not use RegEx for HTML or nested data. Use a proper parser.
- For simple checks, string methods (`startswith`, `in`, `replace`) are often clearer.

---

## 5.5 Python String Formatting

Formatting inserts values into strings in a readable way.

### Four ways

| Method | Example | Python version |
|---|---|---|
| f-strings (recommended) | `f"Hi {name}"` | 3.6+ |
| `str.format()` | `"Hi {}".format(name)` | 2.7+ |
| `%` operator (old style) | `"Hi %s" % name` | all |
| `string.Template` | `Template("Hi $name")` | all |

### f-strings

**Syntax**

```python
f"text {expression} text"
```

```python
name = "Ali"
age = 25

print(f"My name is {name} and I am {age}.")
print(f"Next year: {age + 1}")
print(f"Upper: {name.upper()}")
print(f"Sum: {2 + 3}")
print(f"{'quoted'} text")
```

**Debug shortcut** (Python 3.8+)

```python
x = 10
y = 20
print(f"{x = }, {y = }, {x + y = }")
# x = 10, y = 20, x + y = 30
```

### Format specifiers

**Syntax**

```python
{value:format_spec}
```

#### Decimal places

```python
pi = 3.14159265

print(f"{pi:.2f}")      # 3.14
print(f"{pi:.0f}")      # 3
print(f"{pi:.4f}")      # 3.1416
```

#### Thousands separators

```python
n = 1234567.891

print(f"{n:,}")        # 1,234,567.891
print(f"{n:,.2f}")     # 1,234,567.89
print(f"{n:_}")        # 1_234_567.891
```

#### Percent

```python
print(f"{0.256:.1%}")    # 25.6%
```

#### Scientific notation

```python
print(f"{123456:e}")     # 1.234560e+05
print(f"{123456:.2e}")   # 1.23e+05
```

#### Number bases

```python
n = 255

print(f"{n:b}")     # 11111111   (binary)
print(f"{n:o}")     # 377        (octal)
print(f"{n:x}")     # ff         (hex, lowercase)
print(f"{n:X}")     # FF         (hex, uppercase)
print(f"{n:#x}")    # 0xff
print(f"{n:08b}")   # 11111111   (padded to 8)
```

#### Padding with zeros

```python
print(f"{7:03}")      # 007
print(f"{42:05}")     # 00042
print(f"{3.5:06.2f}") # 003.50
```

#### Width and alignment

| Symbol | Alignment |
|---|---|
| `<` | Left |
| `>` | Right |
| `^` | Center |

```python
print(f"|{'Ali':<10}|")     # |Ali       |
print(f"|{'Ali':>10}|")     # |       Ali|
print(f"|{'Ali':^10}|")     # |   Ali    |
print(f"|{'Ali':*^10}|")    # |***Ali****|
print(f"|{'Ali':-<10}|")    # |Ali-------|
```

Numbers align right by default, and text aligns left by default.

#### Sign

```python
print(f"{5:+}")      # +5
print(f"{-5:+}")     # -5
print(f"{5: }")      # " 5"  (space for positive numbers)
```

### Aligned table

```python
items = [("Pen", 10, 2.5), ("Notebook", 5, 120.0), ("Bag", 1, 1500.75)]

print(f"{'Item':<12}{'Qty':>5}{'Price':>10}")
print("-" * 27)

for name, qty, price in items:
    print(f"{name:<12}{qty:>5}{price:>10,.2f}")
```

```
Item          Qty     Price
---------------------------
Pen            10      2.50
Notebook        5    120.00
Bag             1  1,500.75
```

### Dynamic width

```python
width = 12
precision = 3

print(f"{3.14159265:{width}.{precision}f}")    # "       3.142"
```

### Formatting dates in f-strings

```python
from datetime import datetime

now = datetime(2026, 10, 18, 19, 41)
print(f"{now:%Y-%m-%d %H:%M}")     # 2026-10-18 19:41
print(f"{now:%A, %B %d}")          # Sunday, October 18
```

### Dictionaries and lists in f-strings

Use different quotes inside the braces.

```python
person = {"name": "Ali", "age": 25}
print(f"{person['name']} is {person['age']}")

fruits = ["apple", "banana"]
print(f"First: {fruits[0]}")
```

### Conversion flags

```python
text = "Café"

print(f"{text!s}")    # str()   -> Café
print(f"{text!r}")    # repr()  -> 'Café'
print(f"{text!a}")    # ascii() -> 'Caf\xe9'
```

### Multi-line f-strings

```python
name = "Ali"
total = 1500

msg = f"""
Dear {name},
Your total is {total:,}.
Thank you!
"""
print(msg)
```

### Braces in f-strings

Double them to print a literal brace.

```python
print(f"{{literal braces}} and {1 + 1}")    # {literal braces} and 2
```

### `str.format()`

**Syntax**

```python
"text {} text {}".format(value1, value2)
```

```python
print("Hello, {}!".format("Ali"))
print("{} is {} years old".format("Ali", 25))
print("{0} {1} {0}".format("a", "b"))              # a b a
print("{name} lives in {city}".format(name="Ali", city="Lahore"))
print("{:.2f}".format(3.14159))                    # 3.14
print("{:>8}".format("hi"))                        #       hi
print("{:,}".format(1234567))                      # 1,234,567
```

**With a dictionary**

```python
person = {"name": "Ali", "age": 25}
print("{name} is {age}".format(**person))
```

### `%` formatting (older style)

```python
name = "Ali"
age = 25

print("Hello, %s!" % name)
print("%s is %d years old" % (name, age))
print("Pi is %.2f" % 3.14159)
print("%5d|%-5d|" % (42, 42))
```

| Code | Type |
|---|---|
| `%s` | String |
| `%d` | Integer |
| `%f` | Float |
| `%x` | Hex |
| `%%` | A literal `%` |

### `string.Template`

Useful when templates come from users or files, because it does not run code.

```python
from string import Template

t = Template("Hello, $name! You owe $$${amount}.")
print(t.substitute(name="Ali", amount=50))
# Hello, Ali! You owe $50.

print(t.safe_substitute(name="Ali"))    # leaves missing keys unchanged
```

### Useful string alignment methods

```python
print("Ali".ljust(10, "."))      # Ali.......
print("Ali".rjust(10, "."))      # .......Ali
print("Ali".center(10, "*"))     # ***Ali****
print("42".zfill(5))             # 00042
```

### Which method should I use?

| Situation | Choice |
|---|---|
| Most code | f-strings |
| Template stored in a variable or file | `str.format()` |
| Logging (lazy formatting) | `%` style with the `logging` module |
| User-supplied templates | `string.Template` |

---

## 5.6 Python None

`None` is Python's special value that means **"no value"** or **"nothing here"**. It is the only value of type `NoneType`.

### Basics

```python
x = None

print(x)          # None
print(type(x))    # <class 'NoneType'>
```

### `None` is not zero, empty, or false

```python
print(None == 0)       # False
print(None == "")      # False
print(None == False)   # False
print(None == [])      # False
```

But it **is falsy**:

```python
if not None:
    print("None counts as False in conditions")

print(bool(None))    # False
```

### Check with `is`, not `==`

```python
x = None

if x is None:
    print("x has no value")

if x is not None:
    print("x has a value")
```

`None` is a single object (a **singleton**), so `is` is the correct and fastest check. It is also safe when a class overrides `==`.

### Why `is None` matters

```python
data = 0

if data:                # wrong test for "has a value"
    print("has data")
else:
    print("no data")    # prints, even though data is 0

if data is not None:
    print("has data")   # correct
```

### Functions that return `None`

A function without a `return` returns `None`.

```python
def say_hi():
    print("Hi")

result = say_hi()
print(result)    # None
```

Many methods that change an object in place return `None`:

```python
nums = [3, 1, 2]
result = nums.sort()
print(result)    # None
print(nums)      # [1, 2, 3]

# Common bug:
nums = nums.sort()    # nums is now None!
```

### `None` as a default parameter

The safe way to give a function a default list or dict.

```python
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

print(add_item("a"))    # ['a']
print(add_item("b"))    # ['b']
```

### `None` as a placeholder

```python
result = None

for n in [3, 8, 5]:
    if n > 6:
        result = n
        break

if result is None:
    print("No number above 6")
else:
    print("Found", result)    # Found 8
```

### Returning `None` to signal "not found"

```python
def find_user(users, name):
    for u in users:
        if u["name"] == name:
            return u
    return None

users = [{"name": "Ali"}, {"name": "Sara"}]
user = find_user(users, "Bilal")

if user is None:
    print("User not found")
```

### `None` in data structures

```python
profile = {"name": "Ali", "middle_name": None}

print(profile["middle_name"])         # None
print(profile.get("nickname"))        # None (key missing)
print([1, None, 3].count(None))       # 1
```

`dict.get()` returns `None` when the key is missing.

### Providing a fallback value

```python
name = None
display = name or "Guest"
print(display)    # Guest
```

`or` replaces **any** falsy value (`0`, `""`, `[]`). To replace only `None`:

```python
count = 0
shown = count if count is not None else 10
print(shown)    # 0
```

### Sorting and arithmetic with `None`

```python
# None + 1        # TypeError
# sorted([3, None, 1])   # TypeError: '<' not supported
```

Filter out `None` first:

```python
data = [3, None, 1, None, 2]
clean = [x for x in data if x is not None]
print(sorted(clean))    # [1, 2, 3]
```

### Type hints with `None`

```python
def find(name: str) -> str | None:       # Python 3.10+
    ...

from typing import Optional

def find_old(name: str) -> Optional[str]:
    ...
```

### `None` vs related ideas

| Value | Meaning |
|---|---|
| `None` | No value at all |
| `0` | The number zero |
| `""` | An empty string |
| `[]` | An empty list |
| `False` | A boolean false |
| `float("nan")` | "Not a number" (a float) |

### Common mistakes

| Mistake | Fix |
|---|---|
| `if x == None` | `if x is None` |
| `lst = lst.sort()` | `lst.sort()` or `lst = sorted(lst)` |
| `def f(items=[])` | `def f(items=None)` |
| Doing math on `None` | Check `is not None` first |

---

## 5.7 Python PIP

**pip** is Python's package manager. It installs packages from **PyPI** (the Python Package Index, pypi.org), which hosts hundreds of thousands of packages.

### Check that pip is installed

```bash
pip --version
```

If `pip` is not found, try:

```bash
python -m pip --version
python3 -m pip --version
```

> Using `python -m pip` guarantees pip matches the Python you are running.

### Install a package

```bash
pip install requests
```

```python
import requests

r = requests.get("https://example.com")
print(r.status_code)
```

### Install a specific version

```bash
pip install requests==2.31.0         # exact version
pip install "requests>=2.28"         # minimum version
pip install "requests>=2.28,<3.0"    # range
pip install "requests~=2.31"         # compatible release (>=2.31, <3.0)
```

### Install several packages

```bash
pip install numpy pandas matplotlib
```

### Upgrade a package

```bash
pip install --upgrade requests
pip install -U requests
```

Upgrade pip itself:

```bash
python -m pip install --upgrade pip
```

### Uninstall a package

```bash
pip uninstall requests
pip uninstall -y requests       # skip confirmation
```

### List and inspect packages

```bash
pip list                        # all installed packages
pip list --outdated             # packages with newer versions
pip show requests               # details about one package
pip freeze                      # installed packages in requirements format
```

Example of `pip show`:

```
Name: requests
Version: 2.31.0
Summary: Python HTTP for Humans.
Location: ...
Requires: certifi, charset-normalizer, idna, urllib3
```

### Search for packages

The `pip search` command is disabled. Search on [pypi.org](https://pypi.org) instead.

### `requirements.txt`

A file that lists the packages a project needs, so others can recreate the setup.

**Create it from your current environment**

```bash
pip freeze > requirements.txt
```

**Example `requirements.txt`**

```
requests==2.31.0
numpy>=1.26
pandas
```

**Install everything listed**

```bash
pip install -r requirements.txt
```

### Install from other sources

```bash
pip install ./my_package                           # local folder
pip install my_package-1.0-py3-none-any.whl        # wheel file
pip install git+https://github.com/user/repo.git   # Git repository
pip install -e .                                   # editable (development) install
```

### Extras

Some packages offer optional feature sets.

```bash
pip install "requests[socks]"
```

### User install

```bash
pip install --user requests
```

Installs into your user folder and does not need admin rights. Virtual environments (next section) are usually better.

### Check for dependency problems

```bash
pip check
```

### Download without installing

```bash
pip download requests -d ./packages
```

### Common packages to know

| Package | Use |
|---|---|
| `requests` | HTTP requests and APIs |
| `numpy` | Fast numeric arrays |
| `pandas` | Data tables and analysis |
| `matplotlib` | Charts and plots |
| `flask`, `django`, `fastapi` | Web development |
| `beautifulsoup4` | Parse HTML |
| `pytest` | Testing |
| `pillow` | Image processing |
| `python-dotenv` | Load `.env` files |
| `scikit-learn` | Machine learning |

### Common problems

| Problem | Likely fix |
|---|---|
| `'pip' is not recognized` | Use `python -m pip ...`, or add Python to PATH |
| `externally-managed-environment` error (Linux/macOS) | Use a virtual environment |
| `Permission denied` | Use a virtual environment or `--user` |
| Installed, but `import` fails | pip and Python belong to different installations; use `python -m pip` |
| Version conflicts | Use a separate virtual environment per project |

### Security tips

- Install only packages you trust. Check the name carefully (typosquatting is real).
- Pin versions in `requirements.txt` for repeatable builds.
- Never run `pip install` with `sudo`.

---

## 5.8 Python Virtual Environments

A **virtual environment** (venv) is an isolated folder that holds its own Python packages for one project.

### Why use virtual environments?

- Project A needs `requests 2.25`, project B needs `requests 2.31`. Each gets its own copy.
- The global Python installation stays clean.
- `requirements.txt` stays accurate, listing only what the project uses.
- Easy to delete and recreate.

```
Global Python
├── project_a/.venv  → requests 2.25, flask 2.0
└── project_b/.venv  → requests 2.31, django 5.0
```

### Create a virtual environment

**Syntax**

```bash
python -m venv <folder_name>
```

```bash
cd my_project
python -m venv .venv
```

On macOS and Linux use `python3 -m venv .venv` if `python` points to Python 2 or is missing.

The `.venv` folder now contains a private Python and a private `site-packages`.

### Activate it

| System | Command |
|---|---|
| Windows (Command Prompt) | `.venv\Scripts\activate.bat` |
| Windows (PowerShell) | `.venv\Scripts\Activate.ps1` |
| macOS / Linux (bash, zsh) | `source .venv/bin/activate` |
| Fish shell | `source .venv/bin/activate.fish` |

After activation the prompt shows the environment name:

```
(.venv) C:\my_project>
```

**PowerShell note:** if scripts are blocked, run once:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

### Install packages inside it

```bash
(.venv) pip install requests
(.venv) pip list
(.venv) pip freeze > requirements.txt
```

### Check which Python is active

```bash
which python        # macOS / Linux
where python        # Windows
```

```python
import sys
print(sys.prefix)        # path of the active environment
print(sys.executable)
```

### Deactivate

```bash
deactivate
```

### Delete an environment

Just delete the folder.

```bash
rm -rf .venv                  # macOS / Linux
rmdir /s /q .venv             # Windows (cmd)
```

### Recreate an environment on another computer

```bash
git clone <your-repo>
cd your-repo

python -m venv .venv
source .venv/bin/activate          # or the Windows command
pip install -r requirements.txt
```

### Typical project layout

```
my_project/
├── .venv/              ← do NOT commit this
├── .gitignore
├── requirements.txt
├── main.py
└── src/
```

**`.gitignore`**

```
.venv/
__pycache__/
*.pyc
.env
```

### Choosing a Python version

```bash
python3.12 -m venv .venv            # use a specific installed version
py -3.12 -m venv .venv              # Windows launcher
```

### Useful options

```bash
python -m venv .venv --system-site-packages   # can also see global packages
python -m venv .venv --clear                  # reset an existing environment
python -m venv .venv --upgrade-deps           # update pip in the new environment
```

### Running without activating

```bash
.venv/bin/python main.py             # macOS / Linux
.venv\Scripts\python main.py         # Windows
```

### Using it in VS Code

1. Open the project folder.
2. Press `Ctrl+Shift+P` and choose **Python: Select Interpreter**.
3. Pick the interpreter inside `.venv`.

### Other tools

| Tool | What it adds |
|---|---|
| `venv` | Built in, simple, enough for most projects |
| `virtualenv` | Faster, more options |
| `pipenv` | Combines pip and virtual environments |
| `poetry` | Dependency management and packaging |
| `uv` | Very fast installer and environment manager |
| `conda` | Environments for Python and non-Python packages (data science) |

```bash
# uv example
pip install uv
uv venv
uv pip install requests
```

### Common problems

| Problem | Fix |
|---|---|
| `venv` module missing (Debian/Ubuntu) | `sudo apt install python3-venv` |
| Packages not found after activating | Check the prompt shows `(.venv)`; reinstall inside the environment |
| Moved the project folder and it broke | Recreate the environment; venvs are not portable |
| Wrong environment active | Run `deactivate`, then activate the right one |

### Recommended workflow

```
1. mkdir my_project && cd my_project
2. python -m venv .venv
3. activate it
4. pip install what you need
5. pip freeze > requirements.txt
6. write code
7. deactivate when finished
```

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Dates | `datetime`, `date`, `timedelta`; `strftime` formats, `strptime` parses; `zoneinfo` for time zones |
| Math | Built-ins (`min`, `max`, `round`), `math` module, `random`, `statistics`, `Decimal` for exact values |
| JSON | `loads`/`dumps` for strings, `load`/`dump` for files; `indent=4` for pretty output |
| RegEx | `re.search`, `findall`, `sub`, `split`; use raw strings; groups, flags, lookaheads |
| String formatting | f-strings first; specifiers like `:.2f`, `:,`, `:>10`, `:08b` |
| None | Means "no value"; test with `is None`; use as a safe default parameter |
| PIP | `pip install`, `uninstall`, `freeze`, `-r requirements.txt` |
| Virtual environments | `python -m venv .venv`, activate, install, `deactivate`; do not commit `.venv` |

### Quick syntax cheat sheet

```python
# Dates
from datetime import datetime, timedelta
now = datetime.now()
now.strftime("%Y-%m-%d")
datetime.strptime("2026-10-18", "%Y-%m-%d")
now + timedelta(days=7)

# Math
import math, random
math.sqrt(16); math.ceil(2.1); math.pi
random.randint(1, 6); random.choice(items)

# JSON
import json
data = json.loads(text)
text = json.dumps(data, indent=4)

# RegEx
import re
re.search(r"\d+", s); re.findall(r"\w+", s); re.sub(r"\s+", " ", s)

# Formatting
f"{x:.2f} {n:,} {s:>10} {n:05}"

# None
if value is None: ...
```

```bash
# pip and venv
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install requests
pip freeze > requirements.txt
pip install -r requirements.txt
deactivate
```

---

## Practice Exercises

### Beginner

1. Print today's date in the format `18 October 2026`.
2. Use `math.sqrt()` and `math.ceil()` to find the square root of 50, rounded up.
3. Convert the dictionary `{"name": "Ali", "age": 25}` to a JSON string, then back to a dictionary.
4. Format the number `1234567.891` as `1,234,567.89` using an f-string.
5. Create a variable set to `None` and print whether it is `None`.

### Intermediate

6. Ask the user for a birth date (`DD/MM/YYYY`) and print their age in years.
7. Count the days from today until the next 1 January.
8. Generate 6 unique random numbers from 1 to 49 and print them sorted.
9. Extract all email addresses from a paragraph of text using `re.findall()`.
10. Save a list of three dictionaries to `students.json`, then read it back and print each name.
11. Print a neat table of three products with aligned columns using f-string width and alignment.

### Challenge

12. Write a function that validates a password: at least 8 characters, one uppercase letter, one lowercase letter, one digit, one special character. Use `re`.
13. Write a program that reads a text file, finds all dates written as `DD-MM-YYYY`, and prints them in `YYYY-MM-DD` format using `re.sub()`.
14. Build a simple contact book that stores contacts in a JSON file, with add, list, and search options.
15. Create a virtual environment, install `requests`, write a script that fetches a web page and prints its status code, then generate a `requirements.txt`.

### Sample solution (Exercise 6)

```python
from datetime import datetime, date

text = input("Birth date (DD/MM/YYYY): ")
born = datetime.strptime(text, "%d/%m/%Y").date()
today = date.today()

age = today.year - born.year
if (today.month, today.day) < (born.month, born.day):
    age -= 1

print(f"You are {age} years old.")
```

### Sample solution (Exercise 7)

```python
from datetime import date

today = date.today()
next_year = date(today.year + 1, 1, 1)
print(f"{(next_year - today).days} days until New Year")
```

### Sample solution (Exercise 12)

```python
import re

def is_strong(pw):
    checks = [
        r".{8,}",
        r"[A-Z]",
        r"[a-z]",
        r"\d",
        r"[^\w\s]",
    ]
    return all(re.search(c, pw) for c in checks)

print(is_strong("Abc@1234"))    # True
print(is_strong("password"))    # False
```

### Sample solution (Exercise 13)

```python
import re

text = "Born on 05-03-1999, joined on 17-11-2021."
result = re.sub(r"(\d{2})-(\d{2})-(\d{4})", r"\3-\2-\1", text)
print(result)
# Born on 1999-03-05, joined on 2021-11-17.
```

### Sample solution (Exercise 14)

```python
import json
import os

FILE = "contacts.json"

def load():
    if os.path.exists(FILE):
        with open(FILE, encoding="utf-8") as f:
            return json.load(f)
    return []

def save(contacts):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(contacts, f, indent=2)

contacts = load()

while True:
    choice = input("add / list / search / quit: ").strip().lower()

    if choice == "add":
        name = input("Name: ")
        phone = input("Phone: ")
        contacts.append({"name": name, "phone": phone})
        save(contacts)
    elif choice == "list":
        for c in contacts:
            print(f"{c['name']:<15}{c['phone']}")
    elif choice == "search":
        term = input("Search: ").lower()
        for c in contacts:
            if term in c["name"].lower():
                print(f"{c['name']:<15}{c['phone']}")
    elif choice == "quit":
        break
    else:
        print("Invalid choice")
```

---

**Previous:** [Chapter 4: Python Functions and Iteration](chapter-04-python-functions-and-iteration.md)
**Next:** [Chapter 6: Python Error Handling](#)
