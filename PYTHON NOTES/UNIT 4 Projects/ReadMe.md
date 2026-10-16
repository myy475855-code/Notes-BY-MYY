# Python Notes — Unit 4: Practice Projects

> **Purpose:** Apply Python basics, object-oriented programming (OOP), and file handling by building small, practical projects.  
> **How to use this unit:** Build each project step by step. Start with a working version, test it, then add the optional improvements.

## Table of Contents

1. [How to Approach a Python Project](#1-how-to-approach-a-python-project)
2. [Project Setup and Good Habits](#2-project-setup-and-good-habits)
3. [Basics Project 1: Simple Calculator](#3-basics-project-1-simple-calculator)
4. [Basics Project 2: Number Guessing Game](#4-basics-project-2-number-guessing-game)
5. [Basics Project 3: Multiplication Table Generator](#5-basics-project-3-multiplication-table-generator)
6. [Basics Project 4: Student Grading System](#6-basics-project-4-student-grading-system)
7. [OOP Project 1: Student Management System](#7-oop-project-1-student-management-system)
8. [OOP Project 2: Bank Account Simulator](#8-oop-project-2-bank-account-simulator)
9. [OOP Project 3: Employee Management System](#9-oop-project-3-employee-management-system)
10. [OOP Project 4: Library Management System](#10-oop-project-4-library-management-system)
11. [File Handling Project 1: Notes App](#11-file-handling-project-1-notes-app)
12. [File Handling Project 2: Contact Book](#12-file-handling-project-2-contact-book)
13. [File Handling Project 3: Student Records](#13-file-handling-project-3-student-records)
14. [File Handling Project 4: CSV/JSON Expense Tracker](#14-file-handling-project-4-csvjson-expense-tracker)
15. [Testing and Debugging Projects](#15-testing-and-debugging-projects)
16. [Improving Project Quality](#16-improving-project-quality)
17. [Extra Practice Challenges](#17-extra-practice-challenges)
18. [Suggested Project Folder Structure](#18-suggested-project-folder-structure)
19. [Unit 4 Completion Checklist](#19-unit-4-completion-checklist)

---

## 1. How to Approach a Python Project

Use this repeatable process for every project.

1. **Understand the problem:** write down what the program must do.
2. **List the inputs:** decide what information the user must provide.
3. **Plan the outputs:** decide what the program will display or save.
4. **Break the task into functions:** give each function one clear responsibility.
5. **Choose data structures:** use strings, numbers, lists, dictionaries, or classes as needed.
6. **Build a minimum working version:** implement the essential features first.
7. **Test normal and invalid inputs:** do not test only the perfect case.
8. **Improve the design:** remove repeated code and use meaningful names.
9. **Document the project:** explain its features and how to run it.

### Suggested project levels

- **Level 1 — Basic:** make the main feature work.
- **Level 2 — Improved:** validate inputs and handle errors.
- **Level 3 — Extended:** add file storage, reports, or extra features.

---

## 2. Project Setup and Good Habits

### Create a project folder

```text
python-projects/
└── calculator/
    └── main.py
```

Run the program from a terminal:

```bash
python main.py
```

Depending on your setup, you may need to use `python3 main.py`.

### Good habits

- Use clear names such as `total_marks` rather than vague names such as `x`.
- Keep functions short and focused.
- Validate user input before performing calculations.
- Avoid duplicating the same logic in several places.
- Add comments when they explain *why* something is done.
- Do not store passwords or other secrets in plain-text project files.
- Test edge cases, such as zero, negative values, empty input, and missing files.

---

## 3. Basics Project 1: Simple Calculator

### Objective

Build a command-line calculator that performs addition, subtraction, multiplication, and division.

### Features

- Ask the user for two numbers.
- Let the user choose an operation.
- Display the result.
- Reject invalid operations.
- Prevent division by zero.

### Example implementation

```python
def calculate(first, operator, second):
    if operator == "+":
        return first + second
    elif operator == "-":
        return first - second
    elif operator == "*":
        return first * second
    elif operator == "/":
        if second == 0:
            raise ZeroDivisionError("Cannot divide by zero.")
        return first / second
    else:
        raise ValueError("Unsupported operator.")


try:
    first = float(input("Enter the first number: "))
    operator = input("Choose +, -, *, or /: ").strip()
    second = float(input("Enter the second number: "))

    result = calculate(first, operator, second)
    print("Result:", result)

except ValueError as error:
    print("Input error:", error)
except ZeroDivisionError as error:
    print("Math error:", error)
```

### Concepts practiced

- Variables and data types
- `input()` and `print()`
- Conditional statements
- Functions and return values
- Exceptions

### Improvements

- Add exponentiation (`**`) and remainder (`%`).
- Keep calculating until the user chooses to quit.
- Store calculation history in a text file.
- Add automated tests for each operation.

---

## 4. Basics Project 2: Number Guessing Game

### Objective

The computer chooses a secret number, and the user tries to guess it.

### Example implementation

```python
import random

secret_number = random.randint(1, 100)
attempts = 0

print("Guess a number from 1 to 100.")

while True:
    try:
        guess = int(input("Your guess: "))
    except ValueError:
        print("Please enter a whole number.")
        continue

    if not 1 <= guess <= 100:
        print("Choose a number between 1 and 100.")
        continue

    attempts += 1

    if guess < secret_number:
        print("Too low.")
    elif guess > secret_number:
        print("Too high.")
    else:
        print(f"Correct! You needed {attempts} valid guess(es).")
        break
```

### Concepts practiced

- `random.randint()`
- `while` loops
- Conditional statements
- Input validation
- Counters and `break`

### Improvements

- Add difficulty levels with different number ranges.
- Limit the number of attempts.
- Ask whether the player wants another round.
- Save the best score to a file.

---

## 5. Basics Project 3: Multiplication Table Generator

### Objective

Ask for a number and print its multiplication table.

### Example implementation

```python
def print_table(number, limit=10):
    for multiplier in range(1, limit + 1):
        print(f"{number} x {multiplier} = {number * multiplier}")


try:
    number = int(input("Enter a number: "))
    limit = int(input("How many rows should be shown? "))
    if limit < 1:
        print("The number of rows must be positive.")
    else:
        print_table(number, limit)
except ValueError:
    print("Please enter valid whole numbers.")
```

### Concepts practiced

- `for` loops and `range()`
- Functions and default arguments
- Integer arithmetic
- Formatted strings
- Input validation

### Improvements

- Generate tables for a range of numbers.
- Save the table to a `.txt` file.
- Display tables in aligned columns.

---

## 6. Basics Project 4: Student Grading System

### Objective

Calculate a student's total, percentage, and grade from subject marks.

### Example implementation

```python
def get_grade(percentage):
    if percentage >= 90:
        return "A"
    elif percentage >= 80:
        return "B"
    elif percentage >= 70:
        return "C"
    elif percentage >= 60:
        return "D"
    else:
        return "F"


try:
    subject_count = int(input("Number of subjects: "))
    if subject_count <= 0:
        raise ValueError("Subject count must be positive.")

    marks = []
    for index in range(subject_count):
        score = float(input(f"Marks for subject {index + 1} (0-100): "))
        if not 0 <= score <= 100:
            raise ValueError("Each score must be between 0 and 100.")
        marks.append(score)

    total = sum(marks)
    maximum = subject_count * 100
    percentage = total / maximum * 100

    print("Total:", total, "/", maximum)
    print(f"Percentage: {percentage:.2f}%")
    print("Grade:", get_grade(percentage))

except ValueError as error:
    print("Input error:", error)
```

The grade thresholds here are examples. Adjust them to match the grading rules required by your school or assignment.

### Improvements

- Support multiple students.
- Show each subject's marks.
- Save results to CSV.
- Rank students by percentage.
- Reject empty student names and invalid scores.

---

## 7. OOP Project 1: Student Management System

### Objective

Use classes and objects to represent students and manage their records.

### Requirements

- Create a `Student` class.
- Store a student's name, roll number, and marks.
- Calculate the average mark.
- Display a formatted student summary.
- Create several student objects.

### Example implementation

```python
class Student:
    def __init__(self, name, roll_number, marks):
        self.name = name
        self.roll_number = roll_number
        self.marks = marks

    def average(self):
        if not self.marks:
            return 0
        return sum(self.marks) / len(self.marks)

    def display(self):
        print(f"Name: {self.name}")
        print(f"Roll number: {self.roll_number}")
        print(f"Average: {self.average():.2f}")


student = Student("Ayesha", "ST-101", [85, 90, 78])
student.display()
```

### Concepts practiced

- Classes and objects
- `__init__()` and `self`
- Instance attributes
- Methods
- Lists and calculations

### Improvements

- Add methods to update marks.
- Validate each mark.
- Store students in a dictionary using roll number as the key.
- Save and load student records using JSON.

---

## 8. OOP Project 2: Bank Account Simulator

### Objective

Model a bank account with safe deposit and withdrawal operations.

### Requirements

- Store the account holder and balance.
- Deposit a positive amount.
- Withdraw only when the amount is valid and funds are sufficient.
- Display the balance.
- Keep the balance controlled through methods rather than changing it arbitrarily.

### Example implementation

```python
class BankAccount:
    def __init__(self, holder, opening_balance=0):
        if opening_balance < 0:
            raise ValueError("Opening balance cannot be negative.")
        self.holder = holder
        self._balance = opening_balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive.")
        self._balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive.")
        if amount > self._balance:
            raise ValueError("Insufficient funds.")
        self._balance -= amount

    @property
    def balance(self):
        return self._balance


account = BankAccount("Ali", 1000)
account.deposit(500)
account.withdraw(200)
print("Balance:", account.balance)
```

The underscore in `_balance` signals that it is intended for internal use. Python does not enforce strict private access through a single underscore; the class methods provide the intended interface.

### Improvements

- Add transaction history.
- Store each transaction's type, amount, and time.
- Save account data to JSON.
- Add unit tests for deposits and withdrawals.

**Learning note:** This is a learning simulation, not a real banking system. Real financial software requires stronger security, persistence, audit controls, and transaction guarantees.

---

## 9. OOP Project 3: Employee Management System

### Objective

Represent employees as objects and calculate their monthly pay.

### Requirements

- Store employee ID, name, and monthly salary.
- Display employee details.
- Calculate a bonus or annual salary.
- Reject invalid salary values.
- Manage several employee objects.

### Starter implementation

```python
class Employee:
    def __init__(self, employee_id, name, monthly_salary):
        if monthly_salary < 0:
            raise ValueError("Salary cannot be negative.")
        self.employee_id = employee_id
        self.name = name
        self.monthly_salary = monthly_salary

    def annual_salary(self):
        return self.monthly_salary * 12

    def bonus(self, rate=0.10):
        if rate < 0:
            raise ValueError("Bonus rate cannot be negative.")
        return self.monthly_salary * rate

    def display(self):
        print(f"{self.employee_id}: {self.name}")
        print(f"Monthly salary: {self.monthly_salary:.2f}")
        print(f"Annual salary: {self.annual_salary():.2f}")


employee = Employee("E-101", "Sara", 50000)
employee.display()
print("Bonus:", employee.bonus())
```

### Improvements

- Add subclasses for different employee types.
- Override a method to calculate different bonus rules.
- Sort employees by salary.
- Save records to CSV or JSON.

---

## 10. OOP Project 4: Library Management System

### Objective

Use classes to manage books and borrowing status.

### Requirements

- Create a `Book` class with title, author, and availability.
- Create a `Library` class that stores books.
- Add books to the library.
- Search for books by title.
- Borrow an available book and return a borrowed book.
- Prevent a book from being borrowed twice at the same time.

### Starter implementation

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author
        self.is_available = True


class Library:
    def __init__(self):
        self.books = []

    def add_book(self, book):
        self.books.append(book)

    def search(self, title):
        return [
            book for book in self.books
            if title.lower() in book.title.lower()
        ]

    def borrow(self, title):
        for book in self.search(title):
            if book.is_available:
                book.is_available = False
                return book
        return None

    def return_book(self, book):
        if book in self.books:
            book.is_available = True


library = Library()
library.add_book(Book("Learning Python", "Example Author"))

borrowed = library.borrow("Learning Python")
if borrowed:
    print("Borrowed:", borrowed.title)
else:
    print("No matching available book.")
```

### Improvements

- Use unique book IDs.
- Track who borrowed each book.
- Add due dates and late-return rules.
- Save and reload library data.
- Write tests for borrowing and returning.

---

## 11. File Handling Project 1: Notes App

### Objective

Build a small command-line application that saves notes to a text file.

### Requirements

- Add a note.
- View all notes.
- Search for a keyword.
- Keep previous notes when adding new ones.
- Handle a missing notes file.

### Starter implementation

```python
from pathlib import Path

NOTES_FILE = Path("notes.txt")


def add_note(note):
    with NOTES_FILE.open("a", encoding="utf-8") as file:
        file.write(note.strip() + "\n")


def show_notes():
    if not NOTES_FILE.is_file():
        print("No notes have been saved yet.")
        return

    with NOTES_FILE.open("r", encoding="utf-8") as file:
        for number, line in enumerate(file, start=1):
            print(f"{number}. {line.rstrip()}")


def search_notes(keyword):
    if not NOTES_FILE.is_file():
        print("No notes have been saved yet.")
        return

    with NOTES_FILE.open("r", encoding="utf-8") as file:
        for number, line in enumerate(file, start=1):
            if keyword.casefold() in line.casefold():
                print(f"{number}. {line.rstrip()}")


add_note("Review Python file handling.")
show_notes()
```

### Improvements

- Add a menu with add, view, search, and quit options.
- Reject empty notes.
- Add note dates.
- Store notes as JSON for more structured records.

---

## 12. File Handling Project 2: Contact Book

### Objective

Save contacts so they remain available after the program closes.

### Suggested data structure

```python
contacts = [
    {"name": "Ayesha", "phone": "03000000000"},
    {"name": "Bilal", "phone": "03111111111"},
]
```

### Requirements

- Add a contact.
- Display all contacts.
- Search by name.
- Update a contact's phone number.
- Delete a contact after confirmation.
- Save and load contacts from JSON.

### Starter save/load functions

```python
import json
from pathlib import Path

CONTACTS_FILE = Path("contacts.json")


def load_contacts():
    if not CONTACTS_FILE.exists():
        return []

    with CONTACTS_FILE.open("r", encoding="utf-8") as file:
        return json.load(file)


def save_contacts(contacts):
    with CONTACTS_FILE.open("w", encoding="utf-8") as file:
        json.dump(contacts, file, indent=4, ensure_ascii=False)
```

Add validation and handle malformed JSON before treating this as a finished application. Do not store sensitive information unless the storage and access controls are appropriate.

---

## 13. File Handling Project 3: Student Records

### Objective

Create a program that saves student information to a CSV file.

### Example CSV structure

```csv
roll_number,name,marks
ST-101,Ayesha,85
ST-102,Bilal,91
```

### Requirements

- Add a student record.
- List saved records.
- Search by roll number.
- Calculate average marks.
- Reject duplicate roll numbers.
- Handle missing files and invalid data.

### Starter implementation

```python
import csv
from pathlib import Path

STUDENTS_FILE = Path("students.csv")
FIELDS = ["roll_number", "name", "marks"]


def add_student(roll_number, name, marks):
    file_exists = STUDENTS_FILE.exists()

    with STUDENTS_FILE.open(
        "a", newline="", encoding="utf-8"
    ) as file:
        writer = csv.DictWriter(file, fieldnames=FIELDS)
        if not file_exists or STUDENTS_FILE.stat().st_size == 0:
            writer.writeheader()

        writer.writerow({
            "roll_number": roll_number,
            "name": name,
            "marks": marks,
        })


def list_students():
    if not STUDENTS_FILE.exists():
        print("No student records found.")
        return

    with STUDENTS_FILE.open(
        "r", newline="", encoding="utf-8"
    ) as file:
        for student in csv.DictReader(file):
            print(student)


add_student("ST-101", "Ayesha", 85)
list_students()
```

This starter example does not check for duplicate IDs. Add that validation by reading existing records before inserting a new one.

---

## 14. File Handling Project 4: CSV/JSON Expense Tracker

### Objective

Record expenses, save them, and calculate totals.

### Example record

```python
expense = {
    "date": "2026-10-16",
    "category": "Transport",
    "description": "Bus fare",
    "amount": 250.0,
}
```

### Features

- Add an expense with date, category, description, and amount.
- List expenses.
- Calculate the total spent.
- Summarize spending by category.
- Save data to CSV or JSON.
- Validate that the amount is positive.
- Handle missing or invalid data files.

### Starter total calculation

```python
expenses = [
    {"category": "Food", "amount": 500.0},
    {"category": "Transport", "amount": 250.0},
    {"category": "Food", "amount": 300.0},
]

total = sum(item["amount"] for item in expenses)
print(f"Total expenses: {total:.2f}")

totals_by_category = {}
for item in expenses:
    category = item["category"]
    totals_by_category[category] = (
        totals_by_category.get(category, 0) + item["amount"]
    )

for category, amount in totals_by_category.items():
    print(category, f"{amount:.2f}")
```

### Improvements

- Add a date range filter.
- Display monthly totals.
- Export a summary report.
- Allow CSV and JSON as storage options.
- Use `decimal.Decimal` rather than binary floating-point if precise decimal currency arithmetic is important.

---

## 15. Testing and Debugging Projects

Test more than the happy path. For each project, check:

- Normal, valid input.
- Empty input.
- Invalid numbers or formats.
- Missing files.
- Files with no content.
- Duplicate records.
- Unexpected permissions or invalid paths.
- Existing data that must not be overwritten.
- Very large input files, where relevant.

### Example: test a calculation function

```python
def calculate_total(values):
    return sum(values)


assert calculate_total([10, 20, 30]) == 60
assert calculate_total([]) == 0
```

An `assert` is useful for simple checks. For larger projects, learn Python's `unittest` or a testing framework such as `pytest`.

### Debugging process

1. Read the complete error message and traceback.
2. Identify the line where the exception occurred.
3. Check the input values and path.
4. Reproduce the issue with a small example.
5. Fix the cause rather than only hiding the error.
6. Run the tests again.

---

## 16. Improving Project Quality

After the core program works, improve it gradually.

### Code quality

- Split large programs into small functions.
- Use classes when they represent meaningful objects or responsibilities.
- Keep input/output code separate from core logic when practical.
- Use descriptive variable and function names.
- Remove repeated code.
- Add docstrings to explain functions and classes.

### Reliability

- Validate input at the point where it enters the program.
- Catch specific exceptions.
- Avoid silently ignoring errors.
- Do not overwrite important files without a deliberate decision.
- Make file formats and storage locations clear.

### Usability

- Display clear instructions.
- Show helpful error messages.
- Provide a quit option for menu-driven applications.
- Confirm destructive actions such as deletion.
- Format output so users can read it easily.

### Portfolio readiness

A finished project should include:

- A clear project name.
- A `README.md` describing its purpose and features.
- Instructions to run it.
- Example input and output.
- A list of concepts demonstrated.
- A few tests.
- No personal data, passwords, or API keys committed to the repository.

---

## 17. Extra Practice Challenges

Try these after completing the main projects.

1. **Password-strength checker:** assess length and character variety without saving passwords.
2. **Quiz application:** ask questions, calculate the score, and save results.
3. **Inventory tracker:** add items, update quantities, and save inventory data.
4. **To-do list:** add, complete, list, and remove tasks; save them as JSON.
5. **Text statistics tool:** count lines, words, characters, and frequent words.
6. **Folder report:** list files by extension and report their sizes.
7. **Backup utility:** copy selected files to a backup folder with a clear naming strategy.
8. **Book catalog:** use OOP classes and save records to JSON.
9. **Personal budget:** import expenses from CSV and generate category summaries.
10. **Log monitor:** scan logs for warning and error messages.
11. **File search tool:** search text files for a phrase.
12. **Student result system:** combine a `Student` class with CSV persistence.

For every challenge, write a short plan before coding and test at least three edge cases.

---

## 18. Suggested Project Folder Structure

Keep each project in its own folder so files from different exercises do not get mixed together.

```text
python-projects/
├── calculator/
│   ├── main.py
│   └── README.md
├── student_manager/
│   ├── main.py
│   ├── models.py
│   ├── data/
│   ├── tests/
│   └── README.md
├── notes_app/
│   ├── main.py
│   ├── data/
│   └── README.md
└── expense_tracker/
    ├── main.py
    ├── storage.py
    ├── data/
    ├── tests/
    └── README.md
```

For small beginner projects, one Python file is enough. Add more modules and folders when the project becomes easier to understand that way. Avoid committing private or sensitive data files.

---

## 19. Unit 4 Completion Checklist

Mark each item when you can do it independently.

- [ ] Plan a project by identifying its inputs, outputs, and required features.
- [ ] Build a calculator with functions and exception handling.
- [ ] Build a number guessing game with input validation.
- [ ] Generate multiplication tables using loops and functions.
- [ ] Calculate student totals, percentages, and grades.
- [ ] Create classes and objects for students or employees.
- [ ] Implement methods that validate and update object state.
- [ ] Model a bank account or library using OOP.
- [ ] Save and retrieve notes using text files.
- [ ] Save structured records using CSV or JSON.
- [ ] Handle missing files and malformed input.
- [ ] Use `pathlib` to manage paths and folders.
- [ ] Test normal cases and edge cases.
- [ ] Write a README explaining how to run a project.
- [ ] Complete at least two mini projects from start to finish.

**Unit 4 is complete when you can combine Python basics, OOP, and file handling into small, tested programs that solve practical problems.**
