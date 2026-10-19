# Chapter 8: Python Object-Oriented Programming (OOP)

> OOP organizes code around **objects**: things that bundle data (attributes) and behavior (methods) together. This chapter takes you from your first class to inheritance, polymorphism, encapsulation, and magic methods.

## Table of Contents

- [8.1 Python OOP](#81-python-oop)
- [8.2 Python Classes/Objects](#82-python-classesobjects)
- [8.3 Python `__init__()` Method](#83-python-__init__-method)
- [8.4 Python `self` Parameter](#84-python-self-parameter)
- [8.5 Python Class Properties](#85-python-class-properties)
- [8.6 Python Class Methods](#86-python-class-methods)
- [8.7 Python Magic Methods](#87-python-magic-methods)
- [8.8 Python Inheritance](#88-python-inheritance)
- [8.9 Python Polymorphism](#89-python-polymorphism)
- [8.10 Python Encapsulation](#810-python-encapsulation)
- [8.11 Python Inner Classes](#811-python-inner-classes)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 8.1 Python OOP

### Programming styles

| Style | Idea | Example |
|---|---|---|
| **Procedural** | Functions operate on separate data | `calculate_area(width, height)` |
| **Object-oriented** | Objects hold their own data and behavior | `rectangle.area()` |

### The core idea

Think of a **blueprint** and the things built from it.

```
Class (blueprint)  →  Object (instance)
Car                →  my_car, your_car, taxi
Student            →  ali, sara, bilal
```

| Term | Meaning |
|---|---|
| **Class** | A blueprint that defines attributes and methods |
| **Object / instance** | A specific thing created from a class |
| **Attribute** | A variable that belongs to an object or class |
| **Method** | A function that belongs to a class |

### The four pillars of OOP

| Pillar | Meaning | Section |
|---|---|---|
| **Encapsulation** | Bundle data and methods; control access | 8.10 |
| **Abstraction** | Hide complex details behind a simple interface | 8.8, 8.9 |
| **Inheritance** | A class reuses another class's features | 8.8 |
| **Polymorphism** | Different classes respond to the same call in their own way | 8.9 |

### Why use OOP?

- Models real-world things naturally.
- Code is reusable and easier to maintain.
- Large programs stay organized.
- Changes in one class affect fewer other places.

### Everything in Python is an object

```python
print(type(5))          # <class 'int'>
print(type("hi"))       # <class 'str'>
print(type([1, 2]))     # <class 'list'>
print(type(print))      # <class 'builtin_function_or_method'>

print("hello".upper())  # methods belong to the str class
print([3, 1, 2].sort)   # so do list methods
```

### A first taste

```python
class Dog:
    def bark(self):
        print("Woof!")

my_dog = Dog()
my_dog.bark()    # Woof!
```

---

## 8.2 Python Classes/Objects

### Creating a class

**Syntax**

```python
class ClassName:
    # attributes and methods
```

```python
class Person:
    species = "Human"       # class attribute
```

Class names use **PascalCase** (`BankAccount`, not `bank_account`).

### Creating objects (instances)

```python
class Person:
    species = "Human"

p1 = Person()
p2 = Person()

print(p1.species)         # Human
print(type(p1))           # <class '__main__.Person'>
print(p1 is p2)           # False (two separate objects)
```

### Adding attributes to an object

```python
class Person:
    pass

p = Person()
p.name = "Ali"
p.age = 25

print(p.name, p.age)    # Ali 25
```

Python allows this, but defining attributes in `__init__` (next section) is cleaner and safer.

### A class with methods

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print(f"Hi, I'm {self.name} and I'm {self.age}.")

    def birthday(self):
        self.age += 1

p = Person("Ali", 25)
p.greet()           # Hi, I'm Ali and I'm 25.
p.birthday()
p.greet()           # Hi, I'm Ali and I'm 26.
```

### Accessing and changing attributes

```python
p = Person("Sara", 22)

print(p.name)        # read
p.name = "Sarah"     # change
p.city = "Lahore"    # add a new attribute
del p.city           # delete an attribute
```

### Attribute functions

```python
p = Person("Ali", 25)

print(hasattr(p, "name"))         # True
print(getattr(p, "name"))         # Ali
print(getattr(p, "email", "n/a")) # n/a  (default)
setattr(p, "email", "ali@example.com")
delattr(p, "email")
print(p.__dict__)                 # {'name': 'Ali', 'age': 25}
```

### Deleting objects

```python
p = Person("Ali", 25)
del p
# print(p)    # NameError
```

Python frees memory automatically through garbage collection.

### The `pass` statement

An empty class needs `pass`.

```python
class Placeholder:
    pass
```

### Objects are independent

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1

a = Counter()
b = Counter()

a.increment()
a.increment()
b.increment()

print(a.count)    # 2
print(b.count)    # 1
```

### Objects are passed by reference

```python
def birthday(person):
    person.age += 1

p = Person("Ali", 25)
birthday(p)
print(p.age)    # 26
```

### Comparing objects

```python
a = Person("Ali", 25)
b = Person("Ali", 25)
c = a

print(a == b)    # False (different objects, unless __eq__ is defined)
print(a is b)    # False
print(a is c)    # True
```

### Class vs instance

| | Class | Instance |
|---|---|---|
| What it is | The blueprint | One object made from it |
| How many | One | As many as you like |
| Example | `Person` | `Person("Ali", 25)` |

### Real-world example: Rectangle

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

r = Rectangle(5, 3)
print(r.area())         # 15
print(r.perimeter())    # 16
```

### Real-world example: BankAccount

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount > self.balance:
            print("Insufficient funds")
        else:
            self.balance -= amount

acc = BankAccount("Ali", 1000)
acc.deposit(500)
acc.withdraw(200)
print(acc.balance)    # 1300
```

---

## 8.3 Python `__init__()` Method

`__init__()` is the **initializer** (often called the constructor). Python runs it automatically **every time you create an object**.

### Syntax

```python
class ClassName:
    def __init__(self, param1, param2):
        self.attr1 = param1
        self.attr2 = param2
```

### Example

```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade
        print(f"Created {name}")

s = Student("Ali", "A")    # Created Ali
print(s.name, s.grade)     # Ali A
```

When you write `Student("Ali", "A")`:

```
1. Python creates a new empty object          (__new__)
2. Python calls __init__(new_object, "Ali", "A")
3. The finished object is returned
```

### Default values

```python
class Student:
    def __init__(self, name, grade="Unknown", age=18):
        self.name = name
        self.grade = grade
        self.age = age

print(Student("Ali").grade)              # Unknown
print(Student("Sara", "A").grade)        # A
print(Student("Bilal", age=20).age)      # 20
```

### Required vs optional arguments

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Point()        # TypeError: missing 2 required positional arguments
p = Point(3, 4)
```

### Keyword arguments

```python
p = Point(y=4, x=3)
```

### Validation inside `__init__`

```python
class Person:
    def __init__(self, name, age):
        if not name:
            raise ValueError("Name cannot be empty")
        if age < 0:
            raise ValueError("Age cannot be negative")
        self.name = name
        self.age = age

try:
    Person("Ali", -5)
except ValueError as e:
    print(e)    # Age cannot be negative
```

### Computed attributes

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius
        self.diameter = radius * 2

c = Circle(5)
print(c.diameter)    # 10
```

If `radius` changes later, `diameter` becomes stale. Use a property (see 8.5) when values depend on each other.

### Mutable defaults: a trap

```python
# Wrong: all objects share one list
class Team:
    def __init__(self, members=[]):
        self.members = members

# Right
class Team:
    def __init__(self, members=None):
        self.members = members if members is not None else []

a = Team()
b = Team()
a.members.append("Ali")
print(b.members)    # []
```

### `__init__` returns nothing

```python
class Bad:
    def __init__(self):
        return 5    # TypeError: __init__() should return None
```

### Calling other methods from `__init__`

```python
class Person:
    def __init__(self, full_name):
        self.first, self.last = self._split(full_name)

    def _split(self, full_name):
        first, last = full_name.split(" ", 1)
        return first, last

p = Person("Ali Khan")
print(p.first, p.last)    # Ali Khan
```

### Multiple ways to construct: alternative constructors

Python allows one `__init__`. Use class methods (8.6) for alternatives.

```python
class Date:
    def __init__(self, year, month, day):
        self.year, self.month, self.day = year, month, day

    @classmethod
    def from_string(cls, text):
        y, m, d = map(int, text.split("-"))
        return cls(y, m, d)

d = Date.from_string("2026-10-18")
print(d.year, d.month, d.day)    # 2026 10 18
```

### `__init__` in inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)      # initialize the parent part
        self.breed = breed

d = Dog("Rex", "Labrador")
print(d.name, d.breed)    # Rex Labrador
```

### `__init__` vs `__new__`

| | `__new__` | `__init__` |
|---|---|---|
| Job | Creates the object | Sets up the new object |
| Receives | `cls` | `self` |
| Returns | The new instance | `None` |
| Used for | Singletons, immutable types | Almost every class |

### Dataclasses: less boilerplate

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: float
    y: float = 0.0

p = Point(3, 4)
print(p)             # Point(x=3, y=4)
print(p == Point(3, 4))    # True
```

`@dataclass` writes `__init__`, `__repr__`, and `__eq__` for you.

---

## 8.4 Python `self` Parameter

`self` is the **first parameter of every instance method**. It refers to **the object the method is called on**.

### Why `self` exists

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        print(f"{self.name} says Woof!")

rex = Dog("Rex")
fido = Dog("Fido")

rex.bark()     # Rex says Woof!
fido.bark()    # Fido says Woof!
```

`self` tells each method **which object's data** to use.

### What Python does behind the scenes

```python
rex.bark()
# is the same as
Dog.bark(rex)
```

Python passes the object as the first argument automatically.

### `self` is a convention, not a keyword

```python
class Dog:
    def __init__(this, name):    # works, but do NOT do this
        this.name = name
```

Always call it `self`. Other programmers (and tools) expect it.

### `self.x` vs a plain `x`

```python
class Example:
    def __init__(self, value):
        self.value = value      # attribute: stored on the object
        temp = value * 2        # local variable: disappears after the method

    def show(self):
        print(self.value)       # works
        # print(temp)           # NameError
```

| | Lives | Visible in |
|---|---|---|
| `self.x` | As long as the object | Every method of the object |
| `x` (local) | Until the method ends | That method only |

### Common mistake: forgetting `self`

```python
class Greeter:
    def hello():             # missing self
        print("Hello")

# Greeter().hello()
# TypeError: hello() takes 0 positional arguments but 1 was given
```

Another common mistake:

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        count += 1           # UnboundLocalError: forgot self.
        # self.count += 1    # correct
```

### Calling one method from another

```python
class Calculator:
    def add(self, a, b):
        return a + b

    def add_three(self, a, b, c):
        return self.add(self.add(a, b), c)

print(Calculator().add_three(1, 2, 3))    # 6
```

### Method chaining by returning `self`

```python
class QueryBuilder:
    def __init__(self):
        self.parts = []

    def select(self, columns):
        self.parts.append(f"SELECT {columns}")
        return self

    def from_table(self, table):
        self.parts.append(f"FROM {table}")
        return self

    def where(self, condition):
        self.parts.append(f"WHERE {condition}")
        return self

    def build(self):
        return " ".join(self.parts)

query = QueryBuilder().select("*").from_table("users").where("age > 18").build()
print(query)    # SELECT * FROM users WHERE age > 18
```

### Passing `self` to other objects

```python
class Engine:
    def __init__(self, car):
        self.car = car

class Car:
    def __init__(self):
        self.engine = Engine(self)    # gives the engine a reference to this car
```

### `self` in the class body vs methods

```python
class Demo:
    x = 10                    # class body: no self

    def show(self):
        print(self.x)         # reads class attribute through the object
```

### `self` and the three kinds of methods

| Method type | First parameter | Decorator |
|---|---|---|
| Instance method | `self` (the object) | none |
| Class method | `cls` (the class) | `@classmethod` |
| Static method | none | `@staticmethod` |

### Real-world example: Counter

```python
class Counter:
    def __init__(self, start=0):
        self.value = start

    def increment(self, by=1):
        self.value += by
        return self

    def reset(self):
        self.value = 0
        return self

c = Counter()
c.increment().increment(5).increment()
print(c.value)    # 7
```

---

## 8.5 Python Class Properties

"Properties" covers three things: **instance attributes**, **class attributes**, and the `@property` decorator.

### Instance attributes

Belong to **one object**. Defined with `self.` inside methods.

```python
class Person:
    def __init__(self, name):
        self.name = name

a = Person("Ali")
b = Person("Sara")
print(a.name, b.name)    # Ali Sara
```

### Class attributes

Belong to the **class** and are shared by all instances. Defined directly in the class body.

```python
class Person:
    species = "Human"         # class attribute

    def __init__(self, name):
        self.name = name      # instance attribute

a = Person("Ali")
b = Person("Sara")

print(a.species)         # Human
print(b.species)         # Human
print(Person.species)    # Human
```

### Changing class attributes

```python
Person.species = "Homo sapiens"
print(a.species)    # Homo sapiens (all instances see it)
```

**Pitfall:** setting through an instance creates a **new instance attribute** that hides the class one.

```python
a.species = "Alien"
print(a.species)         # Alien   (instance attribute)
print(b.species)         # Homo sapiens
print(Person.species)    # Homo sapiens
```

### Counting instances with a class attribute

```python
class Student:
    count = 0

    def __init__(self, name):
        self.name = name
        Student.count += 1

Student("Ali")
Student("Sara")
print(Student.count)    # 2
```

### Mutable class attributes are shared

```python
class Team:
    members = []                 # shared by ALL teams (usually a bug)

a = Team()
b = Team()
a.members.append("Ali")
print(b.members)                 # ['Ali']
```

Use an instance attribute instead:

```python
class Team:
    def __init__(self):
        self.members = []
```

### Instance vs class attribute

| | Instance attribute | Class attribute |
|---|---|---|
| Defined | `self.x = ...` in a method | In the class body |
| Belongs to | One object | The class (shared) |
| Access | `obj.x` | `Class.x` or `obj.x` |
| Use for | Per-object data (name, age) | Shared constants, counters, defaults |

### Lookup order

Python looks for `obj.x` in this order: **the instance → the class → parent classes**.

```python
class A:
    x = "class"

a = A()
print(a.x)         # class
a.x = "instance"
print(a.x)         # instance
del a.x
print(a.x)         # class again
```

### Constants

```python
class Circle:
    PI = 3.14159

    def __init__(self, r):
        self.r = r

    def area(self):
        return Circle.PI * self.r ** 2

print(Circle(2).area())    # 12.56636
```

### The `@property` decorator

Lets a method act like an attribute, so you can compute, validate, or protect values.

#### Read-only computed property

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    @property
    def area(self):
        return self.width * self.height

r = Rectangle(4, 5)
print(r.area)      # 20  (no parentheses)
r.width = 10
print(r.area)      # 50  (always up to date)
# r.area = 99      # AttributeError: can't set attribute
```

#### Getter and setter

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age          # goes through the setter

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value

p = Person("Ali", 25)
p.age = 26
print(p.age)       # 26
# p.age = -1       # ValueError
```

#### Deleter

```python
class Person:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.deleter
    def name(self):
        print("Deleting name")
        del self._name
```

#### Computed property from other attributes

```python
class Person:
    def __init__(self, first, last):
        self.first = first
        self.last = last

    @property
    def full_name(self):
        return f"{self.first} {self.last}"

p = Person("Ali", "Khan")
print(p.full_name)    # Ali Khan
```

#### Temperature converter

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    @property
    def fahrenheit(self):
        return self.celsius * 9 / 5 + 32

    @fahrenheit.setter
    def fahrenheit(self, f):
        self.celsius = (f - 32) * 5 / 9

t = Temperature(100)
print(t.fahrenheit)    # 212.0
t.fahrenheit = 32
print(t.celsius)       # 0.0
```

#### Cached property

```python
from functools import cached_property

class Report:
    def __init__(self, data):
        self.data = data

    @cached_property
    def total(self):
        print("Calculating...")
        return sum(self.data)

r = Report([1, 2, 3])
print(r.total)    # Calculating... then 6
print(r.total)    # 6  (no recalculation)
```

### `__slots__`: fixed attributes, less memory

```python
class Point:
    __slots__ = ("x", "y")

    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(1, 2)
# p.z = 3    # AttributeError
```

### Inspecting properties

```python
class Person:
    species = "Human"
    def __init__(self, name):
        self.name = name

p = Person("Ali")
print(p.__dict__)         # {'name': 'Ali'}   (instance)
print(Person.__dict__)    # includes species and methods
print(vars(p))            # same as p.__dict__
```

---

## 8.6 Python Class Methods

Python has three kinds of methods.

| Kind | Decorator | First parameter | Can access |
|---|---|---|---|
| Instance method | none | `self` | Instance and class data |
| Class method | `@classmethod` | `cls` | Class data |
| Static method | `@staticmethod` | none | Neither (just a utility) |

### Instance methods (recap)

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):             # instance method
        print(f"{self.name} says Woof!")
```

### Class methods

**Syntax**

```python
class ClassName:
    @classmethod
    def method_name(cls, params):
        ...
```

`cls` is the class itself (not an instance).

```python
class Student:
    school = "City High"

    def __init__(self, name):
        self.name = name

    @classmethod
    def change_school(cls, new_name):
        cls.school = new_name

Student.change_school("Green Valley")
print(Student.school)           # Green Valley
print(Student("Ali").school)    # Green Valley
```

### Alternative constructors (the most common use)

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_string(cls, text):
        name, age = text.split("-")
        return cls(name, int(age))

    @classmethod
    def from_dict(cls, data):
        return cls(data["name"], data["age"])

p1 = Person("Ali", 25)
p2 = Person.from_string("Sara-22")
p3 = Person.from_dict({"name": "Bilal", "age": 30})

print(p2.name, p2.age)    # Sara 22
```

`cls(...)` (not `Person(...)`) makes this work correctly for **subclasses** too.

```python
class Employee(Person):
    pass

e = Employee.from_string("Hina-28")
print(type(e))    # <class '__main__.Employee'>
```

### Tracking instances

```python
class Product:
    all_products = []

    def __init__(self, name):
        self.name = name
        Product.all_products.append(self)

    @classmethod
    def count(cls):
        return len(cls.all_products)

Product("Pen")
Product("Book")
print(Product.count())    # 2
```

### Static methods

A static method has no `self` or `cls`. It is a plain function placed inside a class because it belongs logically with it.

```python
class MathTools:
    @staticmethod
    def add(a, b):
        return a + b

    @staticmethod
    def is_even(n):
        return n % 2 == 0

print(MathTools.add(2, 3))        # 5
print(MathTools.is_even(8))       # True
```

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @staticmethod
    def is_adult(age):
        return age >= 18

print(Person.is_adult(20))    # True
```

### Calling methods

```python
class Demo:
    def instance(self):
        return "instance"

    @classmethod
    def klass(cls):
        return "class"

    @staticmethod
    def static():
        return "static"

d = Demo()

print(d.instance())       # instance
print(d.klass())          # class   (works on an instance)
print(d.static())         # static  (works on an instance)

print(Demo.klass())       # class   (works on the class)
print(Demo.static())      # static
# Demo.instance()         # TypeError: needs an instance
```

### Choosing the right kind

```
Need the object's data (self.name)?            → instance method
Need the class or to create instances?         → class method
Just a helper that needs neither?              → static method
```

### Validation helper pattern

```python
class User:
    def __init__(self, email):
        if not User.is_valid_email(email):
            raise ValueError("Invalid email")
        self.email = email

    @staticmethod
    def is_valid_email(email):
        return "@" in email and "." in email.split("@")[-1]

print(User.is_valid_email("a@b.com"))    # True
```

### Class method for configuration

```python
class Config:
    debug = False

    @classmethod
    def enable_debug(cls):
        cls.debug = True

Config.enable_debug()
print(Config.debug)    # True
```

### Singleton-style counter

```python
class IDGenerator:
    _next = 1

    @classmethod
    def new_id(cls):
        value = cls._next
        cls._next += 1
        return value

print(IDGenerator.new_id())    # 1
print(IDGenerator.new_id())    # 2
```

---

## 8.7 Python Magic Methods

**Magic methods** (also called **dunder methods**, for "double underscore") let your objects work with Python's built-in features such as `print()`, `+`, `len()`, `==`, and `for` loops.

They look like `__name__` and Python calls them automatically. You rarely call them directly.

### String representation

| Method | Called by | Purpose |
|---|---|---|
| `__str__` | `print(obj)`, `str(obj)` | Friendly text for users |
| `__repr__` | `repr(obj)`, the console, debuggers | Unambiguous text for developers |
| `__format__` | `format(obj, spec)` | Custom format specs |

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"

    def __repr__(self):
        return f"Point({self.x!r}, {self.y!r})"

p = Point(3, 4)
print(p)           # (3, 4)       uses __str__
print(repr(p))     # Point(3, 4)  uses __repr__
print([p])         # [Point(3, 4)]  lists show __repr__
```

Without these, you get an unhelpful result:

```python
class Plain:
    pass

print(Plain())    # <__main__.Plain object at 0x7f...>
```

> Always define `__repr__`. If only `__repr__` exists, `print()` uses it too.

### Arithmetic operators

| Operator | Method | Reflected (`other op self`) |
|---|---|---|
| `+` | `__add__` | `__radd__` |
| `-` | `__sub__` | `__rsub__` |
| `*` | `__mul__` | `__rmul__` |
| `/` | `__truediv__` | `__rtruediv__` |
| `//` | `__floordiv__` | `__rfloordiv__` |
| `%` | `__mod__` | `__rmod__` |
| `**` | `__pow__` | `__rpow__` |
| `@` | `__matmul__` | `__rmatmul__` |
| `-x` | `__neg__` | |
| `+x` | `__pos__` | |
| `abs(x)` | `__abs__` | |

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)

    def __rmul__(self, scalar):
        return self * scalar

    def __neg__(self):
        return Vector(-self.x, -self.y)

    def __abs__(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

a = Vector(1, 2)
b = Vector(3, 4)

print(a + b)      # Vector(4, 6)
print(b - a)      # Vector(2, 2)
print(a * 3)      # Vector(3, 6)
print(3 * a)      # Vector(3, 6)  (uses __rmul__)
print(-a)         # Vector(-1, -2)
print(abs(b))     # 5.0
```

### In-place operators

`+=`, `-=`, and so on. If missing, Python falls back to `+` and reassigns.

```python
class Bag:
    def __init__(self):
        self.items = []

    def __iadd__(self, item):
        self.items.append(item)
        return self           # must return self

bag = Bag()
bag += "apple"
bag += "pear"
print(bag.items)    # ['apple', 'pear']
```

### Comparison operators

| Operator | Method |
|---|---|
| `==` | `__eq__` |
| `!=` | `__ne__` |
| `<` | `__lt__` |
| `<=` | `__le__` |
| `>` | `__gt__` |
| `>=` | `__ge__` |

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def __eq__(self, other):
        if not isinstance(other, Student):
            return NotImplemented
        return self.marks == other.marks

    def __lt__(self, other):
        return self.marks < other.marks

a = Student("Ali", 80)
b = Student("Sara", 90)

print(a == b)    # False
print(a < b)     # True
print(a > b)     # False  (Python reverses __lt__)
```

**`functools.total_ordering`** fills in the rest from `__eq__` and one ordering method:

```python
from functools import total_ordering

@total_ordering
class Student:
    def __init__(self, marks):
        self.marks = marks

    def __eq__(self, other):
        return self.marks == other.marks

    def __lt__(self, other):
        return self.marks < other.marks

print(Student(80) <= Student(90))    # True
```

**Sorting uses `__lt__`:**

```python
students = [Student(90), Student(70), Student(80)]
print([s.marks for s in sorted(students)])    # [70, 80, 90]
```

### Hashing

If you define `__eq__`, Python removes `__hash__` unless you add it. Objects must be hashable to be dictionary keys or set members.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return (self.x, self.y) == (other.x, other.y)

    def __hash__(self):
        return hash((self.x, self.y))

points = {Point(1, 2), Point(1, 2), Point(3, 4)}
print(len(points))    # 2
```

### Container methods

| Function / syntax | Method |
|---|---|
| `len(obj)` | `__len__` |
| `obj[key]` | `__getitem__` |
| `obj[key] = v` | `__setitem__` |
| `del obj[key]` | `__delitem__` |
| `x in obj` | `__contains__` |
| `for x in obj` | `__iter__` |
| `reversed(obj)` | `__reversed__` |

```python
class Playlist:
    def __init__(self, songs):
        self.songs = list(songs)

    def __len__(self):
        return len(self.songs)

    def __getitem__(self, index):
        return self.songs[index]

    def __setitem__(self, index, value):
        self.songs[index] = value

    def __delitem__(self, index):
        del self.songs[index]

    def __contains__(self, song):
        return song in self.songs

    def __iter__(self):
        return iter(self.songs)

p = Playlist(["A", "B", "C"])

print(len(p))        # 3
print(p[0])          # A
print(p[-1])         # C
print(p[0:2])        # ['A', 'B']
p[1] = "Z"
del p[0]
print("Z" in p)      # True

for song in p:
    print(song)
```

### Callable objects: `__call__`

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, x):
        return x * self.factor

double = Multiplier(2)
print(double(10))              # 20
print(callable(double))        # True
```

### Truthiness: `__bool__`

```python
class Basket:
    def __init__(self, items):
        self.items = items

    def __bool__(self):
        return len(self.items) > 0

if Basket([]):
    print("has items")
else:
    print("empty")    # empty
```

If `__bool__` is missing, Python uses `__len__`.

### Context managers: `__enter__` and `__exit__`

```python
class Timer:
    def __enter__(self):
        import time
        self.start = time.perf_counter()
        return self

    def __exit__(self, exc_type, exc, tb):
        import time
        print(f"Took {time.perf_counter() - self.start:.4f}s")
        return False      # do not hide exceptions

with Timer():
    sum(range(1_000_000))
```

### Attribute access methods

| Method | Called when |
|---|---|
| `__getattr__(self, name)` | An attribute is **not found** normally |
| `__getattribute__(self, name)` | **Every** attribute access |
| `__setattr__(self, name, value)` | Any attribute is set |
| `__delattr__(self, name)` | An attribute is deleted |

```python
class Flexible:
    def __getattr__(self, name):
        return f"No attribute '{name}', returning a default"

f = Flexible()
print(f.color)    # No attribute 'color', returning a default
```

```python
class Locked:
    def __setattr__(self, name, value):
        if name.startswith("_"):
            raise AttributeError("Private names not allowed")
        super().__setattr__(name, value)
```

### Lifecycle methods

| Method | When |
|---|---|
| `__new__` | Object is created |
| `__init__` | Object is initialized |
| `__del__` | Object is about to be destroyed (avoid relying on it) |

### Other useful dunders

| Method | Used by |
|---|---|
| `__int__`, `__float__`, `__str__` | `int(obj)`, `float(obj)`, `str(obj)` |
| `__index__` | Using an object as a list index |
| `__round__` | `round(obj)` |
| `__copy__`, `__deepcopy__` | `copy.copy`, `copy.deepcopy` |
| `__getstate__`, `__setstate__` | `pickle` |
| `__class_getitem__` | `MyClass[int]` generics |
| `__init_subclass__` | When a class is subclassed |
| `__dir__` | `dir(obj)` |
| `__sizeof__` | `sys.getsizeof(obj)` |

### Real-world example: Money class

```python
from functools import total_ordering

@total_ordering
class Money:
    def __init__(self, amount, currency="PKR"):
        self.amount = amount
        self.currency = currency

    def __repr__(self):
        return f"Money({self.amount}, {self.currency!r})"

    def __str__(self):
        return f"{self.currency} {self.amount:,.2f}"

    def _check(self, other):
        if not isinstance(other, Money):
            return NotImplemented
        if self.currency != other.currency:
            raise ValueError("Currency mismatch")

    def __add__(self, other):
        self._check(other)
        return Money(self.amount + other.amount, self.currency)

    def __eq__(self, other):
        self._check(other)
        return self.amount == other.amount

    def __lt__(self, other):
        self._check(other)
        return self.amount < other.amount

    def __bool__(self):
        return self.amount != 0

a = Money(1500)
b = Money(2500.5)

print(a + b)         # PKR 4,000.50
print(a < b)         # True
print(repr(a))       # Money(1500, 'PKR')
print(max(a, b))     # PKR 2,500.50
```

### Real-world example: Deck of cards

```python
import random

class Deck:
    ranks = [str(n) for n in range(2, 11)] + list("JQKA")
    suits = ["♠", "♥", "♦", "♣"]

    def __init__(self):
        self.cards = [f"{r}{s}" for s in self.suits for r in self.ranks]

    def __len__(self):
        return len(self.cards)

    def __getitem__(self, i):
        return self.cards[i]

deck = Deck()
print(len(deck))              # 52
print(deck[0], deck[-1])      # 2♠ A♣
print(random.choice(deck))
```

---

## 8.8 Python Inheritance

**Inheritance** lets a class (the **child** or **subclass**) reuse and extend another class (the **parent**, **base**, or **superclass**).

```
        Animal           ← parent
       /      \
     Dog      Cat        ← children
```

### Syntax

```python
class Parent:
    ...

class Child(Parent):
    ...
```

### Basic example

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")

    def speak(self):
        print("Some sound")

class Dog(Animal):
    pass

d = Dog("Rex")
d.eat()      # Rex is eating   (inherited)
d.speak()    # Some sound      (inherited)
```

### Overriding methods

A child can replace a parent's method.

```python
class Dog(Animal):
    def speak(self):
        print("Woof!")

class Cat(Animal):
    def speak(self):
        print("Meow!")

Dog("Rex").speak()    # Woof!
Cat("Tom").speak()    # Meow!
```

### The `super()` function

Calls the parent's version of a method.

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)       # run the parent's __init__
        self.breed = breed

    def speak(self):
        super().speak()              # extend, don't just replace
        print("Woof!")

d = Dog("Rex", "Labrador")
print(d.name, d.breed)    # Rex Labrador
d.speak()
# Some sound
# Woof!
```

> If a child defines `__init__`, it must call `super().__init__(...)` so the parent's setup still runs.

### Adding new attributes and methods

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def details(self):
        return f"{self.name}: {self.salary}"

class Manager(Employee):
    def __init__(self, name, salary, team):
        super().__init__(name, salary)
        self.team = team

    def add_member(self, name):
        self.team.append(name)

    def details(self):
        return super().details() + f", team size {len(self.team)}"

m = Manager("Ali", 100_000, ["Sara", "Bilal"])
m.add_member("Hina")
print(m.details())    # Ali: 100000, team size 3
```

### Checking relationships

```python
d = Dog("Rex", "Lab")

print(isinstance(d, Dog))       # True
print(isinstance(d, Animal))    # True  (a Dog is an Animal)
print(issubclass(Dog, Animal))  # True
print(issubclass(Animal, Dog))  # False
print(type(d) == Animal)        # False (type() ignores inheritance)
```

### Multi-level inheritance

```python
class Animal:
    def breathe(self):
        print("Breathing")

class Mammal(Animal):
    def feed_milk(self):
        print("Feeding milk")

class Dog(Mammal):
    def bark(self):
        print("Woof")

d = Dog()
d.breathe()      # from Animal
d.feed_milk()    # from Mammal
d.bark()         # own
```

### Multiple inheritance

A class can have more than one parent.

```python
class Flyer:
    def fly(self):
        print("Flying")

class Swimmer:
    def swim(self):
        print("Swimming")

class Duck(Flyer, Swimmer):
    pass

d = Duck()
d.fly()
d.swim()
```

### Method Resolution Order (MRO)

Python decides which parent to use by searching in a set order.

```python
class A:
    def hello(self): print("A")

class B(A):
    def hello(self): print("B")

class C(A):
    def hello(self): print("C")

class D(B, C):
    pass

D().hello()                 # B
print(D.__mro__)
# (D, B, C, A, object)
print([c.__name__ for c in D.mro()])
```

Rule (C3 linearization): children before parents, and the order you list the parents is kept.

### `super()` in multiple inheritance

`super()` follows the MRO, so every class in the chain gets a turn.

```python
class A:
    def __init__(self):
        print("A init")
        super().__init__()

class B(A):
    def __init__(self):
        print("B init")
        super().__init__()

class C(A):
    def __init__(self):
        print("C init")
        super().__init__()

class D(B, C):
    def __init__(self):
        print("D init")
        super().__init__()

D()
# D init
# B init
# C init
# A init
```

### Mixins

Small classes that add one ability to other classes.

```python
import json

class JSONMixin:
    def to_json(self):
        return json.dumps(self.__dict__)

class LogMixin:
    def log(self, msg):
        print(f"[{type(self).__name__}] {msg}")

class User(JSONMixin, LogMixin):
    def __init__(self, name):
        self.name = name

u = User("Ali")
print(u.to_json())    # {"name": "Ali"}
u.log("created")      # [User] created
```

### Abstract base classes

Force children to implement certain methods.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        ...

    @abstractmethod
    def perimeter(self):
        ...

    def describe(self):                      # normal method, shared
        return f"Area {self.area():.2f}, perimeter {self.perimeter():.2f}"

# Shape()    # TypeError: Can't instantiate abstract class

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w, self.h = w, h

    def area(self):
        return self.w * self.h

    def perimeter(self):
        return 2 * (self.w + self.h)

print(Rectangle(3, 4).describe())    # Area 12.00, perimeter 14.00
```

### "Is-a" vs "has-a"

| Relationship | Meaning | Use |
|---|---|---|
| **Is-a** | A Dog **is an** Animal | Inheritance |
| **Has-a** | A Car **has an** Engine | Composition (an attribute holds another object) |

```python
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self):
        self.engine = Engine()    # composition

    def start(self):
        self.engine.start()

Car().start()
```

> **Prefer composition over inheritance** when the relationship is not truly "is-a". Deep inheritance trees become hard to change.

### Accessing parent attributes

```python
class Parent:
    value = "parent"

class Child(Parent):
    pass

print(Child.value)               # parent
print(Child.__bases__)           # (<class '__main__.Parent'>,)
print(Parent.__subclasses__())   # [<class '__main__.Child'>]
```

### Real-world example: Vehicles

```python
class Vehicle:
    def __init__(self, brand, speed=0):
        self.brand = brand
        self.speed = speed

    def accelerate(self, amount):
        self.speed += amount

    def __str__(self):
        return f"{self.brand} at {self.speed} km/h"

class Car(Vehicle):
    def __init__(self, brand, doors=4):
        super().__init__(brand)
        self.doors = doors

class Bike(Vehicle):
    def accelerate(self, amount):
        super().accelerate(min(amount, 30))    # bikes are slower

c = Car("Toyota")
b = Bike("Honda")
c.accelerate(100)
b.accelerate(100)
print(c)    # Toyota at 100 km/h
print(b)    # Honda at 30 km/h
```

---

## 8.9 Python Polymorphism

**Polymorphism** means "many forms": the **same call** can behave differently depending on the object.

### Built-in polymorphism

```python
print(len("hello"))          # 5
print(len([1, 2, 3]))        # 3
print(len({"a": 1}))         # 1

print(1 + 2)                 # 3
print("a" + "b")             # ab
print([1] + [2])             # [1, 2]
```

### Polymorphism with inheritance (method overriding)

```python
class Shape:
    def area(self):
        raise NotImplementedError("Subclasses must implement area()")

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w, self.h = w, h

    def area(self):
        return self.w * self.h

class Circle(Shape):
    def __init__(self, r):
        self.r = r

    def area(self):
        return 3.14159 * self.r ** 2

class Triangle(Shape):
    def __init__(self, b, h):
        self.b, self.h = b, h

    def area(self):
        return 0.5 * self.b * self.h

shapes = [Rectangle(3, 4), Circle(2), Triangle(6, 5)]

for s in shapes:
    print(f"{type(s).__name__}: {s.area():.2f}")
```

```
Rectangle: 12.00
Circle: 12.57
Triangle: 15.00
```

The loop does not care about the exact type. It only needs `area()`.

### Duck typing

> "If it walks like a duck and quacks like a duck, it's a duck."

Python checks **what an object can do**, not what class it belongs to. No shared parent is required.

```python
class Duck:
    def speak(self):
        print("Quack!")

class Dog:
    def speak(self):
        print("Woof!")

class Robot:
    def speak(self):
        print("Beep boop")

def make_it_speak(thing):
    thing.speak()

for t in [Duck(), Dog(), Robot()]:
    make_it_speak(t)
```

### Duck typing with files

```python
import io

def write_report(out):
    out.write("Report\n")

with open("r.txt", "w") as f:
    write_report(f)

buffer = io.StringIO()
write_report(buffer)
print(buffer.getvalue())
```

Anything with `write()` works.

### Operator polymorphism

Magic methods give your classes natural operators (see 8.7).

```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

print(Vector(1, 2) + Vector(3, 4))    # Vector(4, 6)
```

### Polymorphic functions

```python
def total_length(items):
    return sum(len(i) for i in items)

print(total_length(["ab", "cde"]))            # 5
print(total_length([[1, 2], (3, 4, 5)]))      # 5
```

### Method overloading in Python

Python does not support multiple methods with the same name and different parameters. The last one wins. Use these patterns instead:

**Default arguments**

```python
class Greeter:
    def greet(self, name=None):
        print(f"Hello, {name or 'friend'}!")
```

**`*args`**

```python
class Calculator:
    def add(self, *numbers):
        return sum(numbers)

print(Calculator().add(1, 2))        # 3
print(Calculator().add(1, 2, 3, 4))  # 10
```

**Type checks**

```python
class Printer:
    def show(self, value):
        if isinstance(value, int):
            print(f"Integer: {value}")
        elif isinstance(value, str):
            print(f"String: {value}")
        else:
            print(f"Other: {value}")
```

**`functools.singledispatchmethod`**

```python
from functools import singledispatchmethod

class Formatter:
    @singledispatchmethod
    def format(self, value):
        return f"Other: {value}"

    @format.register
    def _(self, value: int):
        return f"Integer: {value:,}"

    @format.register
    def _(self, value: str):
        return f"Text: {value.title()}"

f = Formatter()
print(f.format(1500))        # Integer: 1,500
print(f.format("hello"))     # Text: Hello
print(f.format(3.5))         # Other: 3.5
```

### Polymorphism with `super()`

```python
class Employee:
    def pay(self):
        return 1000

class Manager(Employee):
    def pay(self):
        return super().pay() + 500

class Director(Manager):
    def pay(self):
        return super().pay() + 2000

for e in [Employee(), Manager(), Director()]:
    print(type(e).__name__, e.pay())
```

```
Employee 1000
Manager 1500
Director 3500
```

### Protocols (structural typing)

Describe "anything with these methods" for type checkers.

```python
from typing import Protocol

class Speaker(Protocol):
    def speak(self) -> None: ...

def announce(s: Speaker) -> None:
    s.speak()
```

### Real-world example: payment processing

```python
class Payment:
    def pay(self, amount):
        raise NotImplementedError

class CreditCard(Payment):
    def pay(self, amount):
        return f"Charged {amount} to credit card"

class PayPal(Payment):
    def pay(self, amount):
        return f"Sent {amount} via PayPal"

class Cash(Payment):
    def pay(self, amount):
        return f"Received {amount} in cash"

def checkout(method, amount):
    print(method.pay(amount))

for m in [CreditCard(), PayPal(), Cash()]:
    checkout(m, 2500)
```

Adding a new payment type means adding one class. `checkout()` never changes.

---

## 8.10 Python Encapsulation

**Encapsulation** means bundling data with the methods that work on it, and **controlling access** so outside code cannot easily put an object into an invalid state.

### Python's access conventions

Python has no strict `private` keyword. It uses **naming conventions**.

| Syntax | Name | Meaning |
|---|---|---|
| `name` | Public | Anyone may use it |
| `_name` | Protected (convention) | "Internal use. Please don't touch from outside." |
| `__name` | Private (name mangling) | Hidden behind a changed name |

### Public

```python
class Person:
    def __init__(self, name):
        self.name = name

p = Person("Ali")
p.name = "Sara"    # freely changeable
```

### Protected (single underscore)

```python
class Account:
    def __init__(self, balance):
        self._balance = balance      # a signal to other programmers

a = Account(100)
print(a._balance)    # works, but you are bypassing the intended interface
```

Nothing stops access. It is a **polite warning**.

### Private (double underscore)

```python
class Account:
    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance

a = Account(100)
print(a.get_balance())    # 100
# print(a.__balance)      # AttributeError
```

### Name mangling

Python renames `__balance` to `_Account__balance`.

```python
print(a._Account__balance)    # 100   (still reachable if you really try)
print(a.__dict__)             # {'_Account__balance': 100}
```

Name mangling mainly prevents **accidental clashes** between parent and child classes. It is not a security feature.

### Private methods

```python
class Report:
    def __clean(self, text):         # private helper
        return text.strip().lower()

    def _validate(self, text):       # protected helper
        return bool(text)

    def process(self, text):         # public interface
        cleaned = self.__clean(text)
        if self._validate(cleaned):
            return cleaned
```

### Getters and setters: the Pythonic way

Use `@property` (see 8.5) to control access without changing how callers use the attribute.

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance

    @property
    def balance(self):
        return self._balance            # read-only from outside

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self._balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount

acc = BankAccount("Ali", 1000)
acc.deposit(500)
acc.withdraw(200)
print(acc.balance)       # 1300
# acc.balance = 999999   # AttributeError: can't set attribute
```

The balance can only change through methods that **enforce rules**.

### Validation in a setter

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price         # uses the setter

    @property
    def price(self):
        return self._price

    @price.setter
    def price(self, value):
        if not isinstance(value, (int, float)):
            raise TypeError("Price must be a number")
        if value < 0:
            raise ValueError("Price cannot be negative")
        self._price = value

p = Product("Pen", 50)
p.price = 60
# p.price = -5     # ValueError
```

### Read-only attributes

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @property
    def area(self):
        return 3.14159 * self._radius ** 2

c = Circle(3)
print(c.radius, c.area)
# c.radius = 5   # AttributeError
```

### Immutable objects

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Point:
    x: int
    y: int

p = Point(1, 2)
# p.x = 5      # FrozenInstanceError
```

### Information hiding with a class method API

```python
class Counter:
    def __init__(self):
        self.__count = 0

    def increment(self):
        self.__count += 1

    def value(self):
        return self.__count

c = Counter()
c.increment()
c.increment()
print(c.value())    # 2
```

### Why encapsulation matters

| Benefit | Example |
|---|---|
| **Validation** | Reject a negative age |
| **Safety** | Balance changes only through `deposit` and `withdraw` |
| **Flexibility** | Change internals later without breaking users |
| **Simpler use** | Callers use a small public interface |
| **Debugging** | All changes go through a few places |

### Encapsulation guidelines

- Start with **public** attributes. Add `@property` later if you need control. Callers do not have to change.
- Use `_name` for "internal" items.
- Use `__name` only to avoid subclass clashes, not for "security".
- Expose behavior (methods), not raw data.
- Do not write Java-style `get_x()` and `set_x()` everywhere. Use properties when needed.

### Real-world example: Student with validation

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    @property
    def marks(self):
        return self._marks

    @marks.setter
    def marks(self, value):
        if not 0 <= value <= 100:
            raise ValueError("Marks must be between 0 and 100")
        self._marks = value

    @property
    def grade(self):
        if self._marks >= 90: return "A+"
        if self._marks >= 80: return "A"
        if self._marks >= 70: return "B"
        if self._marks >= 60: return "C"
        return "F"

s = Student("Ali", 85)
print(s.grade)    # A
s.marks = 92
print(s.grade)    # A+
```

---

## 8.11 Python Inner Classes

An **inner class** (nested class) is a class defined **inside another class**.

### Syntax

```python
class Outer:
    class Inner:
        ...
```

### Basic example

```python
class Computer:
    def __init__(self):
        self.cpu = self.CPU("Intel i7", 8)

    class CPU:
        def __init__(self, model, cores):
            self.model = model
            self.cores = cores

        def info(self):
            return f"{self.model} with {self.cores} cores"

pc = Computer()
print(pc.cpu.info())    # Intel i7 with 8 cores
```

### Creating inner objects from outside

```python
inner = Computer.CPU("AMD Ryzen", 12)
print(inner.info())    # AMD Ryzen with 12 cores
```

### Multiple inner classes

```python
class Computer:
    def __init__(self):
        self.cpu = self.CPU("i7")
        self.ram = self.RAM(16)

    class CPU:
        def __init__(self, model):
            self.model = model

    class RAM:
        def __init__(self, size):
            self.size = size

pc = Computer()
print(pc.cpu.model, pc.ram.size)    # i7 16
```

### An inner class does not automatically know the outer object

```python
class Outer:
    def __init__(self):
        self.name = "outer"

    class Inner:
        def show(self):
            # print(self.name)    # AttributeError: no access to Outer's data
            pass
```

Pass the outer object in explicitly:

```python
class Outer:
    def __init__(self):
        self.name = "outer"
        self.inner = self.Inner(self)

    class Inner:
        def __init__(self, outer):
            self.outer = outer

        def show(self):
            print(f"Inner sees: {self.outer.name}")

Outer().inner.show()    # Inner sees: outer
```

### Why use inner classes?

| Use | Example |
|---|---|
| **Grouping** closely related helper classes | `LinkedList.Node` |
| **Namespace** organization | `Config.Settings`, `Order.Status` |
| **Hiding** helper classes from the module level | A parser's internal `Token` class |
| **Enums and constants** tied to a class | `Task.Priority` |

### Linked list with an inner `Node`

```python
class LinkedList:
    class Node:
        def __init__(self, value):
            self.value = value
            self.next = None

    def __init__(self):
        self.head = None

    def append(self, value):
        node = self.Node(value)
        if self.head is None:
            self.head = node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = node

    def __iter__(self):
        current = self.head
        while current:
            yield current.value
            current = current.next

ll = LinkedList()
for n in [10, 20, 30]:
    ll.append(n)

print(list(ll))    # [10, 20, 30]
```

### Inner Enum

```python
from enum import Enum

class Task:
    class Priority(Enum):
        LOW = 1
        MEDIUM = 2
        HIGH = 3

    def __init__(self, title, priority=Priority.MEDIUM):
        self.title = title
        self.priority = priority

t = Task("Write report", Task.Priority.HIGH)
print(t.priority)          # Priority.HIGH
print(t.priority.name)     # HIGH
print(t.priority.value)    # 3
```

### Inner exception class

```python
class Database:
    class ConnectionError(Exception):
        pass

    def connect(self):
        raise self.ConnectionError("Cannot reach server")

try:
    Database().connect()
except Database.ConnectionError as e:
    print("Error:", e)
```

### Inner class as a builder or iterator

```python
class Playlist:
    def __init__(self, songs):
        self.songs = songs

    def __iter__(self):
        return self.Iterator(self.songs)

    class Iterator:
        def __init__(self, songs):
            self.songs = songs
            self.i = 0

        def __iter__(self):
            return self

        def __next__(self):
            if self.i >= len(self.songs):
                raise StopIteration
            song = self.songs[self.i]
            self.i += 1
            return song

for s in Playlist(["A", "B", "C"]):
    print(s)
```

### Accessing through the class

```python
print(Computer.CPU)               # <class '__main__.Computer.CPU'>
print(Computer.CPU.__qualname__)  # Computer.CPU
```

### Multiple levels

```python
class A:
    class B:
        class C:
            def hello(self):
                return "deep"

print(A.B.C().hello())    # deep
```

Avoid deep nesting. It is hard to read.

### Inner class vs two separate classes

| Inner class | Separate classes |
|---|---|
| Strongly tied to the outer class | Useful on their own |
| Hidden by namespace | Easy to reuse and test |
| Good for small helpers | Better for big, independent concepts |

> In Python, many developers define helper classes at the module level instead. Use inner classes when the grouping really helps readability.

### Real-world example: Pizza order

```python
class Pizza:
    class Topping:
        def __init__(self, name, price):
            self.name = name
            self.price = price

        def __repr__(self):
            return f"{self.name} (+{self.price})"

    def __init__(self, size, base_price):
        self.size = size
        self.base_price = base_price
        self.toppings = []

    def add_topping(self, name, price):
        self.toppings.append(self.Topping(name, price))

    def total(self):
        return self.base_price + sum(t.price for t in self.toppings)

p = Pizza("Large", 1200)
p.add_topping("Olives", 100)
p.add_topping("Cheese", 150)

print(p.toppings)       # [Olives (+100), Cheese (+150)]
print(p.total())        # 1450
```

---

## Chapter Summary

| Topic | Key points |
|---|---|
| OOP basics | Classes are blueprints, objects are instances; four pillars: encapsulation, abstraction, inheritance, polymorphism |
| Classes/objects | `class Name:`; create with `Name()`; attributes and methods |
| `__init__()` | Runs when an object is created; sets up instance attributes |
| `self` | The current object; first parameter of instance methods |
| Class properties | Instance attributes vs shared class attributes; `@property` for computed or validated values |
| Class methods | Instance (`self`), `@classmethod` (`cls`), `@staticmethod` (neither) |
| Magic methods | `__str__`, `__repr__`, operators, `__len__`, `__getitem__`, `__eq__`, `__call__`, context managers |
| Inheritance | `class Child(Parent)`; `super()`; MRO; multiple inheritance; mixins; abstract classes; prefer composition when not "is-a" |
| Polymorphism | Same call, different behavior; overriding; duck typing; operator overloading |
| Encapsulation | `_protected`, `__private`; control access with `@property` and methods |
| Inner classes | A class inside a class; groups helpers; needs an explicit link to the outer object |

### Quick syntax cheat sheet

```python
class Animal:
    kind = "animal"                          # class attribute

    def __init__(self, name):                # initializer
        self.name = name                     # instance attribute
        self._secret = 1                     # protected (convention)
        self.__hidden = 2                    # private (name mangled)

    @property
    def upper_name(self):                    # computed property
        return self.name.upper()

    @upper_name.setter
    def upper_name(self, value):
        self.name = value.lower()

    def speak(self):                         # instance method
        return "..."

    @classmethod
    def create(cls, name):                   # class method
        return cls(name)

    @staticmethod
    def helper(x):                           # static method
        return x * 2

    def __str__(self):                       # magic method
        return f"Animal({self.name})"

    def __repr__(self):
        return f"Animal({self.name!r})"


class Dog(Animal):                           # inheritance
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

    def speak(self):                         # overriding
        return "Woof!"

    class Collar:                            # inner class
        pass


d = Dog("Rex", "Lab")
isinstance(d, Animal)
issubclass(Dog, Animal)
```

---

## Practice Exercises

### Beginner

1. Create a class `Car` with attributes `brand` and `year`, and a method `info()` that prints them.
2. Create three `Car` objects and call `info()` on each.
3. Add a class attribute `wheels = 4` to `Car` and print it from an object and from the class.
4. Create a class `Circle` with a `radius` and methods `area()` and `circumference()`.
5. Add `__str__` to `Car` so that `print(my_car)` shows a friendly message.

### Intermediate

6. Create a `BankAccount` class with `deposit()`, `withdraw()`, and a read-only `balance` property. Reject negative amounts and overdrafts.
7. Create a `Student` class that counts how many students have been created using a class attribute.
8. Add a class method `Person.from_birth_year(name, year)` that calculates the age.
9. Create a `Shape` parent class with `area()`, and subclasses `Square` and `Circle`. Loop through a list of shapes and print each area.
10. Create a `Vector` class that supports `+`, `-`, `*` (by a number), `==`, and `len()` (number of components).
11. Create a class `Temperature` with a `celsius` attribute and a `fahrenheit` property that has a getter and setter.

### Challenge

12. Build an `Employee` class and subclasses `Manager` and `Developer`. Each has a different `calculate_salary()`. Print a payroll report for a mixed list.
13. Create an abstract class `Payment` with an abstract method `pay(amount)`, and implement `CreditCard`, `PayPal`, and `Cash`.
14. Write a `Library` class that holds `Book` objects. Support `add_book()`, `remove_book()`, `search()`, `len(library)`, iteration, and `in`.
15. Write a `Stack` class (with `push`, `pop`, `peek`, `__len__`, `__bool__`, `__repr__`) using a list inside.
16. Build a `LinkedList` class with an inner `Node` class. Add `append()`, `prepend()`, `__iter__()`, and `__len__()`.
17. Design a small **inventory system**: `Product` (with validation), `Inventory` (with add, remove, total value, low-stock report), and an inner `Category` enum.

### Sample solution (Exercise 6)

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self._balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount

acc = BankAccount("Ali", 1000)
acc.deposit(500)
acc.withdraw(300)
print(acc.balance)    # 1200
```

### Sample solution (Exercise 10)

```python
class Vector:
    def __init__(self, *components):
        self.components = tuple(components)

    def __add__(self, other):
        return Vector(*(a + b for a, b in zip(self.components, other.components)))

    def __sub__(self, other):
        return Vector(*(a - b for a, b in zip(self.components, other.components)))

    def __mul__(self, k):
        return Vector(*(a * k for a in self.components))

    __rmul__ = __mul__

    def __eq__(self, other):
        return self.components == other.components

    def __len__(self):
        return len(self.components)

    def __repr__(self):
        return f"Vector{self.components}"

a = Vector(1, 2, 3)
b = Vector(4, 5, 6)

print(a + b)      # Vector(5, 7, 9)
print(b - a)      # Vector(3, 3, 3)
print(a * 2)      # Vector(2, 4, 6)
print(len(a))     # 3
print(a == Vector(1, 2, 3))    # True
```

### Sample solution (Exercise 12)

```python
class Employee:
    def __init__(self, name, base):
        self.name = name
        self.base = base

    def calculate_salary(self):
        return self.base

class Manager(Employee):
    def __init__(self, name, base, bonus):
        super().__init__(name, base)
        self.bonus = bonus

    def calculate_salary(self):
        return self.base + self.bonus

class Developer(Employee):
    def __init__(self, name, base, overtime_hours, rate=500):
        super().__init__(name, base)
        self.overtime_hours = overtime_hours
        self.rate = rate

    def calculate_salary(self):
        return self.base + self.overtime_hours * self.rate

staff = [
    Employee("Hina", 60_000),
    Manager("Ali", 100_000, 25_000),
    Developer("Sara", 80_000, 10),
]

for e in staff:
    print(f"{e.name:<6} {type(e).__name__:<10} {e.calculate_salary():>10,}")
```

### Sample solution (Exercise 13)

```python
from abc import ABC, abstractmethod

class Payment(ABC):
    @abstractmethod
    def pay(self, amount):
        ...

class CreditCard(Payment):
    def pay(self, amount):
        return f"Charged {amount} to credit card"

class PayPal(Payment):
    def pay(self, amount):
        return f"Sent {amount} via PayPal"

class Cash(Payment):
    def pay(self, amount):
        return f"Received {amount} in cash"

for method in (CreditCard(), PayPal(), Cash()):
    print(method.pay(1500))

# Payment()    # TypeError: Can't instantiate abstract class
```

### Sample solution (Exercise 15)

```python
class Stack:
    def __init__(self):
        self._items = []

    def push(self, item):
        self._items.append(item)

    def pop(self):
        if not self._items:
            raise IndexError("pop from empty stack")
        return self._items.pop()

    def peek(self):
        if not self._items:
            raise IndexError("peek from empty stack")
        return self._items[-1]

    def __len__(self):
        return len(self._items)

    def __bool__(self):
        return bool(self._items)

    def __repr__(self):
        return f"Stack({self._items})"

s = Stack()
s.push(1)
s.push(2)
s.push(3)
print(s)              # Stack([1, 2, 3])
print(s.pop())        # 3
print(s.peek())       # 2
print(len(s))         # 2
```

### Sample solution (Exercise 17)

```python
from enum import Enum

class Inventory:
    class Category(Enum):
        FOOD = "food"
        ELECTRONICS = "electronics"
        OTHER = "other"

    class Product:
        def __init__(self, name, price, quantity, category):
            self.name = name
            self.price = price
            self.quantity = quantity
            self.category = category

        @property
        def price(self):
            return self._price

        @price.setter
        def price(self, value):
            if value < 0:
                raise ValueError("Price cannot be negative")
            self._price = value

        @property
        def quantity(self):
            return self._quantity

        @quantity.setter
        def quantity(self, value):
            if value < 0:
                raise ValueError("Quantity cannot be negative")
            self._quantity = value

        def __repr__(self):
            return f"{self.name} x{self.quantity}"

    def __init__(self):
        self._products = {}

    def add(self, name, price, quantity, category=Category.OTHER):
        self._products[name] = self.Product(name, price, quantity, category)

    def remove(self, name):
        del self._products[name]

    def total_value(self):
        return sum(p.price * p.quantity for p in self._products.values())

    def low_stock(self, threshold=5):
        return [p for p in self._products.values() if p.quantity <= threshold]

    def __len__(self):
        return len(self._products)

inv = Inventory()
inv.add("Rice", 250, 40, Inventory.Category.FOOD)
inv.add("Phone", 45_000, 3, Inventory.Category.ELECTRONICS)

print(len(inv))              # 2
print(inv.total_value())     # 145000
print(inv.low_stock())       # [Phone x3]
```

---

**Previous:** [Chapter 7: Python File Handling](chapter-07-python-file-handling.md)
**Next:** [Chapter 9: Advanced Python](#)
