# Unit 2: Object-Oriented Programming (OOP) in Python

> **Notes roadmap:** Learn object-oriented programming from the basics to advanced Python OOP features.  
> **Prerequisite:** Complete Unit 1 — Python Basics.  
> **Scope:** Classes, objects, encapsulation, inheritance, polymorphism, abstraction, special methods, and OOP design.

## Table of Contents

1. [Introduction to OOP](#1-introduction-to-oop)
2. [Procedural Programming vs OOP](#2-procedural-programming-vs-oop)
3. [Classes and Objects](#3-classes-and-objects)
4. [Attributes and Methods](#4-attributes-and-methods)
5. [The `self` Parameter](#5-the-self-parameter)
6. [Constructors and `__init__()`](#6-constructors-and-__init__)
7. [Instance Variables and Class Variables](#7-instance-variables-and-class-variables)
8. [Types of Methods](#8-types-of-methods)
9. [Encapsulation](#9-encapsulation)
10. [Public, Protected, and Private Attributes](#10-public-protected-and-private-attributes)
11. [Getters, Setters, and Properties](#11-getters-setters-and-properties)
12. [Inheritance](#12-inheritance)
13. [Method Overriding and `super()`](#13-method-overriding-and-super)
14. [Polymorphism](#14-polymorphism)
15. [Method Overloading Concepts](#15-method-overloading-concepts)
16. [Duck Typing](#16-duck-typing)
17. [Abstraction](#17-abstraction)
18. [Abstract Classes and Methods](#18-abstract-classes-and-methods)
19. [Operator Overloading](#19-operator-overloading)
20. [Special (Dunder) Methods](#20-special-dunder-methods)
21. [Composition, Aggregation, and Association](#21-composition-aggregation-and-association)
22. [Multiple Inheritance and MRO](#22-multiple-inheritance-and-mro)
23. [Dataclasses](#23-dataclasses)
24. [OOP Design Principles](#24-oop-design-principles)
25. [Advantages, Limitations, and Best Practices](#25-advantages-limitations-and-best-practices)
26. [OOP Practice Programs and Projects](#26-oop-practice-programs-and-projects)

---

## 1. Introduction to OOP

- Meaning of object-oriented programming
- Why OOP is used
- Objects as data plus behavior
- Real-world examples: students, bank accounts, employees, and vehicles
- Main concepts of OOP:
  - Classes and objects
  - Encapsulation
  - Inheritance
  - Polymorphism
  - Abstraction
- When OOP is useful
- When simple functions and data structures may be enough

## 2. Procedural Programming vs OOP

- Procedural programming overview
- Organizing code around functions
- Organizing code around objects
- Differences in data handling and code reuse
- Advantages and disadvantages of each approach
- Converting a procedural example into a class-based design
- Choosing the right approach for a problem

## 3. Classes and Objects

- What a class is
- What an object is
- Defining a class with `class`
- Creating objects (instantiation)
- Accessing attributes and calling methods
- Multiple objects from one class
- Object identity
- Class namespace and instance namespace
- Example:

```python
class Student:
    def introduce(self):
        print("I am a student.")

student1 = Student()
student2 = Student()

student1.introduce()
student2.introduce()
```

## 4. Attributes and Methods

- What attributes are
- What methods are
- Instance attributes
- Class attributes
- Defining methods inside a class
- Accessing and updating attributes
- Adding attributes dynamically and why it can be risky
- Attribute lookup basics
- Example with a `Car` class

## 5. The `self` Parameter

- Meaning of `self`
- Why instance methods receive `self`
- Accessing instance attributes with `self`
- Passing arguments to methods
- `self` is a convention, not a reserved keyword
- Difference between `self` and a class
- How Python binds an instance method
- Common mistakes when omitting `self`

```python
class Student:
    def set_name(self, name):
        self.name = name

    def show_name(self):
        print(self.name)

student = Student()
student.set_name("Ali")
student.show_name()
```

## 6. Constructors and `__init__()`

- Object creation and initialization
- The `__new__()` method: introductory overview
- The `__init__()` method
- Default and parameterized initialization
- Setting initial attributes
- Default parameter values
- Validating constructor arguments
- Constructor mistakes
- `__init__()` is an initializer, not technically the object creator

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

student = Student("Sara", 20)
print(student.name)
print(student.age)
```

## 7. Instance Variables and Class Variables

- Instance variables: separate data for each object
- Class variables: data shared through the class
- Defining class attributes
- Accessing class attributes through the class and instances
- Attribute shadowing
- Mutable class-attribute pitfalls
- When to use instance vs class variables
- Example: counting created objects

## 8. Types of Methods

### 8.1 Instance Methods
- Working with instance state
- The `self` parameter

### 8.2 Class Methods
- The `@classmethod` decorator
- The `cls` parameter
- Alternative constructors
- Working with class-level state

### 8.3 Static Methods
- The `@staticmethod` decorator
- Utility behavior related to a class
- Static methods vs module-level functions

### 8.4 Choosing a Method Type
- Instance method vs class method vs static method
- Common design mistakes

## 9. Encapsulation

- Meaning of encapsulation
- Combining data and behavior
- Controlling access through methods and properties
- Protecting object invariants
- Validation before changing state
- Encapsulation vs data hiding
- Benefits and limitations in Python

## 10. Public, Protected, and Private Attributes

- Public attributes: `name`
- Protected-by-convention attributes: `_name`
- Name-mangled attributes: `__name`
- Python's naming conventions and what they do (and do not) guarantee
- Name mangling with `_ClassName__attribute`
- Why double underscores are not strict security
- Choosing clear attribute names
- Avoiding unnecessary restrictions

## 11. Getters, Setters, and Properties

- Traditional getter and setter methods
- Why direct attribute access is often fine in Python
- The `@property` decorator
- Defining a setter with `@name.setter`
- Defining a deleter with `@name.deleter`
- Validation in property setters
- Read-only properties
- Keeping an interface stable while implementation changes

```python
class Temperature:
    def __init__(self, celsius):
        self.celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero.")
        self._celsius = value

temperature = Temperature(25)
print(temperature.celsius)
temperature.celsius = 30
```

## 12. Inheritance

- Meaning of inheritance
- Base (parent) and derived (child) classes
- Reusing and extending behavior
- Single inheritance
- Multilevel inheritance
- Hierarchical inheritance
- Multiple inheritance overview
- Hybrid inheritance as a combination of inheritance patterns
- The `isinstance()` and `issubclass()` functions
- “Is-a” relationships
- When inheritance is appropriate
- Avoiding overly deep inheritance hierarchies

```python
class Animal:
    def speak(self):
        print("An animal makes a sound.")

class Dog(Animal):
    def bark(self):
        print("Woof!")

dog = Dog()
dog.speak()
dog.bark()
```

## 13. Method Overriding and `super()`

- What method overriding means
- Replacing inherited behavior in a subclass
- Extending rather than completely replacing a parent method
- Calling parent behavior with `super()`
- `super()` and the method resolution order
- Overriding constructors
- Keeping method signatures compatible
- Common overriding mistakes

## 14. Polymorphism

- Meaning of polymorphism
- One interface, multiple implementations
- Polymorphism through inheritance
- Method overriding
- Polymorphism with unrelated objects
- Polymorphism in loops and function arguments
- Built-in polymorphism examples
- Benefits of programming to an interface

## 15. Method Overloading Concepts

- Meaning of method overloading in languages such as Java and C++
- Python's approach to function and method calls
- Why defining the same method name twice replaces the earlier definition
- Default arguments
- `*args` and `**kwargs`
- Type checks and dispatching alternatives
- `functools.singledispatch` for functions
- Multiple dispatch libraries: overview
- Avoiding overly complicated overload-like logic

## 16. Duck Typing

- Meaning of “if it behaves like a duck”
- Behavior over declared type
- Objects with compatible methods
- Writing flexible functions
- Duck typing vs inheritance
- Duck typing and Python protocols
- Examples using file-like or iterable objects
- Benefits and potential risks

## 17. Abstraction

- Meaning of abstraction
- Hiding implementation details
- Defining what an object must do
- Abstraction vs encapsulation
- Interfaces and contracts
- Practical examples: payment methods, notifications, and storage systems
- Benefits of abstraction in larger programs

## 18. Abstract Classes and Methods

- Abstract base classes (ABCs)
- The `abc` module
- `ABC` base class
- `@abstractmethod`
- Abstract methods and concrete methods
- Preventing incomplete subclasses from being instantiated
- Abstract properties
- Implementing required methods in subclasses
- ABCs vs duck typing and `Protocol`

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

shape = Rectangle(4, 5)
print(shape.area())
```

## 19. Operator Overloading

- Meaning of operator overloading
- Using special methods to define operator behavior
- Arithmetic operators: `__add__()`, `__sub__()`, `__mul__()`
- Comparison operators: `__eq__()`, `__lt__()`
- String representation methods
- Returning `NotImplemented` when an operation is unsupported
- Equality and hashing rules
- Designing intuitive custom behavior
- Avoiding surprising operator meanings

## 20. Special (Dunder) Methods

- What double-underscore methods are
- Object representation:
  - `__str__()`
  - `__repr__()`
- Comparison:
  - `__eq__()`
  - `__lt__()`
- Arithmetic:
  - `__add__()`
  - `__sub__()`
  - `__mul__()`
- Container behavior:
  - `__len__()`
  - `__getitem__()`
  - `__contains__()`
- Iteration:
  - `__iter__()`
  - `__next__()`
- Calling objects with `__call__()`
- Context managers:
  - `__enter__()`
  - `__exit__()`
- Why dunder methods should follow Python conventions

## 21. Composition, Aggregation, and Association

### 21.1 Association
- General relationship between objects
- One-to-one and one-to-many examples

### 21.2 Aggregation
- A “has-a” relationship where parts can exist independently
- Example: a department and its teachers

### 21.3 Composition
- Strong ownership between an object and its parts
- Example: a house and its rooms as a modeling choice
- Object lifecycle considerations

### 21.4 Choosing a Relationship
- Inheritance (“is-a”) vs composition (“has-a”)
- Reducing tight coupling
- Dependency injection basics
- Modeling real-world relationships carefully

## 22. Multiple Inheritance and MRO

- What multiple inheritance is
- Potential benefits and risks
- Method Resolution Order (MRO)
- C3 linearization overview
- Inspecting MRO with `ClassName.mro()`
- How `super()` follows the MRO
- Cooperative multiple inheritance
- Mixins
- Diamond inheritance problem
- When to prefer composition

## 23. Dataclasses

- Purpose of the `dataclasses` module
- The `@dataclass` decorator
- Automatically generated `__init__()` and `__repr__()`
- Equality comparisons
- Default field values
- Mutable defaults and `default_factory`
- Frozen dataclasses
- Ordering options
- Type annotations in dataclasses
- When a dataclass is appropriate vs a regular class

```python
from dataclasses import dataclass

@dataclass
class Student:
    name: str
    age: int

student = Student("Ayesha", 19)
print(student)
```

## 24. OOP Design Principles

- Single Responsibility Principle (SRP)
- Open/Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)
- High cohesion and low coupling
- “Favor composition over inheritance”
- Separation of concerns
- Encapsulating changing behavior
- Refactoring large classes
- Avoiding unnecessary abstraction
- Applying principles to small Python projects

## 25. Advantages, Limitations, and Best Practices

### Advantages
- Code organization
- Reuse and extensibility
- Encapsulation of state
- Easier modeling of complex domains
- Improved maintainability when designed well

### Limitations
- More structure than small scripts need
- Poorly designed class hierarchies can become complex
- Overuse of inheritance can create tight coupling
- Objects and abstractions can add overhead

### Best Practices
- Use meaningful class and method names
- Keep classes focused
- Prefer composition when it fits better
- Avoid mutable class attributes unless sharing is intended
- Validate important state changes
- Use properties when they improve an interface
- Document public behavior
- Write tests for class behavior
- Follow PEP 8 naming conventions

## 26. OOP Practice Programs and Projects

### Beginner Exercises
1. Create a `Student` class with name, age, and a display method.
2. Create a `Rectangle` class that calculates area and perimeter.
3. Create a `Circle` class that calculates area and circumference.
4. Create a `Book` class with title, author, and publication year.
5. Create an `Employee` class with a method to calculate annual salary.

### Intermediate Exercises
6. Create a `BankAccount` class with deposit and withdrawal validation.
7. Create a `Car` class with instance attributes and class-level counters.
8. Demonstrate inheritance using `Person`, `Student`, and `Teacher`.
9. Demonstrate polymorphism with different payment method classes.
10. Create an abstract `Shape` class with `Circle` and `Rectangle` subclasses.
11. Create a class with validated properties.
12. Overload `+` for a suitable custom type.
13. Use a dataclass to represent a product or student record.
14. Demonstrate composition with an `Order` and its `OrderItem` objects.
15. Demonstrate multiple inheritance and inspect the MRO.

### Mini Projects
- Student management system using classes
- Bank account simulation
- Library book management model
- Employee payroll model
- Shopping cart with products and orders
- Vehicle rental model
- Shape calculator using polymorphism
- School management model using inheritance and composition

---

## Suggested Study Method

For every OOP topic, write notes in this order:

1. **Definition** — explain the concept in simple words.
2. **Purpose** — describe the problem it solves.
3. **Syntax** — show the basic structure.
4. **Code example** — write a small, runnable program.
5. **Output** — show what the program produces.
6. **Explanation** — describe the code step by step.
7. **Comparison** — compare it with related concepts.
8. **Common mistakes** — list pitfalls and fixes.
9. **Practice** — solve exercises without copying the solution.

## Unit 2 Completion Checklist

- [ ] I can create classes and instantiate objects.
- [ ] I understand `self`, `__init__()`, and class vs instance attributes.
- [ ] I can choose between instance, class, and static methods.
- [ ] I understand encapsulation and properties.
- [ ] I can use inheritance and `super()` appropriately.
- [ ] I can explain polymorphism, abstraction, and duck typing.
- [ ] I can use abstract base classes.
- [ ] I understand operator overloading and common dunder methods.
- [ ] I can distinguish inheritance, composition, aggregation, and association.
- [ ] I understand multiple inheritance and MRO.
- [ ] I can design and complete a small OOP project.

**Previous unit:** Unit 1 — Python Basics.  
**Next unit:** Unit 3 — File Handling in Python.
