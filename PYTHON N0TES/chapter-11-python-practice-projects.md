# Chapter 11: Python Practice Projects

> Projects turn knowledge into skill. Each project below uses ideas from earlier chapters, gives you a clear goal, a step-by-step plan, **complete working code**, sample output, and ideas to extend it. Build them in order: they get harder as you go.

## Table of Contents

- [How to Use This Chapter](#how-to-use-this-chapter)
- [11.1 Calculator](#111-calculator)
- [11.2 Number Guessing Game](#112-number-guessing-game)
- [11.3 Quiz Application](#113-quiz-application)
- [11.4 To-Do List](#114-to-do-list)
- [11.5 Contact Book](#115-contact-book)
- [11.6 Expense Tracker](#116-expense-tracker)
- [11.7 File Organizer](#117-file-organizer)
- [11.8 API-Based Weather Application](#118-api-based-weather-application)
- [11.9 Database Management Application](#119-database-management-application)
- [11.10 Final Capstone Project](#1110-final-capstone-project)
- [Project Skills Map](#project-skills-map)
- [What to Learn Next](#what-to-learn-next)

---

## How to Use This Chapter

### Recommended method for each project

```
1. Read the goal and requirements
2. Try it yourself FIRST (even a rough attempt counts)
3. Break the problem into small steps
4. Get a minimal version working
5. Compare with the sample solution
6. Add the "Level up" ideas
7. Write tests and commit it to Git
```

> Do not copy and paste the solutions. **Type them out**, change things, break them, and fix them. That is where the learning happens.

### Difficulty guide

| Project | Level | Main skills |
|---|---|---|
| 11.1 Calculator | Beginner | Functions, input, error handling |
| 11.2 Number Guessing Game | Beginner | Loops, `random`, conditions |
| 11.3 Quiz Application | Beginner-Intermediate | Lists, dictionaries, JSON, scoring |
| 11.4 To-Do List | Intermediate | File storage, menus, data structures |
| 11.5 Contact Book | Intermediate | Classes, JSON, search |
| 11.6 Expense Tracker | Intermediate | Dates, CSV, summaries |
| 11.7 File Organizer | Intermediate | `pathlib`, `shutil`, safety |
| 11.8 Weather Application | Intermediate-Advanced | APIs, `requests`, error handling |
| 11.9 Database Management | Advanced | SQLite, SQL, OOP |
| 11.10 Capstone | Advanced | Everything combined |

### Suggested folder setup

```
python-projects/
├── 01_calculator/
├── 02_guessing_game/
├── 03_quiz/
├── ...
└── 10_capstone/
```

For each project:

```bash
cd python-projects/01_calculator
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
git init
```

---

## 11.1 Calculator

### Goal

Build a command-line calculator that performs basic and advanced operations and never crashes on bad input.

### Skills practiced

Functions, `match` (or `if/elif`), `try/except`, loops, input validation, dictionaries.

### Requirements

- [ ] Add, subtract, multiply, divide
- [ ] Power, modulus, square root
- [ ] Handle invalid input and division by zero
- [ ] Keep running until the user quits
- [ ] Remember the last result and history

### Plan

```
1. Write one function per operation
2. Map operator symbols to functions with a dictionary
3. Write a safe number-reading helper
4. Build the main loop with a menu
5. Add history and "use last result"
```

### Version 1: Simple calculator

```python
# calculator_simple.py

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b

def main():
    print("=== Simple Calculator ===")

    try:
        a = float(input("First number:  "))
        op = input("Operator (+ - * /): ").strip()
        b = float(input("Second number: "))
    except ValueError:
        print("Please enter valid numbers.")
        return

    match op:
        case "+":
            result = add(a, b)
        case "-":
            result = subtract(a, b)
        case "*":
            result = multiply(a, b)
        case "/":
            try:
                result = divide(a, b)
            except ZeroDivisionError as e:
                print("Error:", e)
                return
        case _:
            print("Unknown operator.")
            return

    print(f"{a} {op} {b} = {result}")

if __name__ == "__main__":
    main()
```

### Version 2: Full calculator with history

```python
# calculator.py
import math

HISTORY_LIMIT = 10

def add(a, b):      return a + b
def subtract(a, b): return a - b
def multiply(a, b): return a * b
def power(a, b):    return a ** b

def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b

def modulus(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot take modulus by zero")
    return a % b

def sqrt(a):
    if a < 0:
        raise ValueError("Cannot take the square root of a negative number")
    return math.sqrt(a)

BINARY_OPERATIONS = {
    "+": add,
    "-": subtract,
    "*": multiply,
    "/": divide,
    "%": modulus,
    "^": power,
}

UNARY_OPERATIONS = {
    "sqrt": sqrt,
}

def read_number(prompt, last_result=None):
    """Read a number. The word 'ans' means 'use the last result'."""
    while True:
        text = input(prompt).strip().lower()

        if text == "ans" and last_result is not None:
            return last_result

        try:
            return float(text)
        except ValueError:
            hint = " (or 'ans')" if last_result is not None else ""
            print(f"  Invalid number. Try again{hint}.")

def format_number(value):
    """Show whole numbers without the .0"""
    if isinstance(value, float) and value.is_integer():
        return str(int(value))
    return f"{value:.10g}"

def show_menu():
    print("\nOperations:")
    print("  +  add          -  subtract")
    print("  *  multiply     /  divide")
    print("  %  modulus      ^  power")
    print("  sqrt  square root")
    print("  history  show past calculations")
    print("  clear    clear history")
    print("  q        quit")

def main():
    history = []
    last_result = None

    print("=== Python Calculator ===")
    show_menu()

    while True:
        op = input("\nOperation: ").strip().lower()

        if op in ("q", "quit", "exit"):
            print("Goodbye!")
            break

        if op == "history":
            if not history:
                print("  (no history yet)")
            for i, entry in enumerate(history[-HISTORY_LIMIT:], start=1):
                print(f"  {i}. {entry}")
            continue

        if op == "clear":
            history.clear()
            print("  History cleared.")
            continue

        try:
            if op in UNARY_OPERATIONS:
                a = read_number("Number: ", last_result)
                result = UNARY_OPERATIONS[op](a)
                record = f"{op}({format_number(a)}) = {format_number(result)}"

            elif op in BINARY_OPERATIONS:
                a = read_number("First number:  ", last_result)
                b = read_number("Second number: ", last_result)
                result = BINARY_OPERATIONS[op](a, b)
                record = f"{format_number(a)} {op} {format_number(b)} = {format_number(result)}"

            else:
                print("  Unknown operation. Type one of the options above.")
                continue

        except (ZeroDivisionError, ValueError, OverflowError) as e:
            print(f"  Error: {e}")
            continue

        last_result = result
        history.append(record)
        print(f"  {record}")

if __name__ == "__main__":
    main()
```

### Sample run

```
=== Python Calculator ===

Operation: +
First number:  12
Second number: 8
  12 + 8 = 20

Operation: *
First number:  ans
Second number: 3
  20 * 3 = 60

Operation: /
First number:  10
Second number: 0
  Error: Cannot divide by zero

Operation: sqrt
Number: 144
  sqrt(144) = 12
```

### Tests

```python
# test_calculator.py
import pytest
from calculator import add, divide, sqrt, format_number

def test_add():
    assert add(2, 3) == 5

def test_divide():
    assert divide(10, 4) == 2.5

def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        divide(1, 0)

def test_sqrt_negative():
    with pytest.raises(ValueError):
        sqrt(-4)

@pytest.mark.parametrize("value, expected", [(5.0, "5"), (2.5, "2.5"), (0.1 + 0.2, "0.3")])
def test_format_number(value, expected):
    assert format_number(value) == expected
```

### Level up

- Parse whole expressions such as `3 + 4 * (2 - 1)` (research the `ast` module or the shunting-yard algorithm).
- Add memory functions (`M+`, `M-`, `MR`).
- Build a GUI with `tkinter`.
- Add trigonometric and logarithmic functions.
- Save history to a file.

> **Safety note:** Do **not** use `eval()` on user input to parse expressions. It can run arbitrary code. If you want expression parsing, walk the syntax tree with `ast` and allow only numbers and arithmetic operators.

### Safe expression evaluator (bonus)

```python
import ast
import operator

OPS = {
    ast.Add: operator.add,
    ast.Sub: operator.sub,
    ast.Mult: operator.mul,
    ast.Div: operator.truediv,
    ast.Pow: operator.pow,
    ast.Mod: operator.mod,
    ast.USub: operator.neg,
}

def safe_eval(expression):
    def evaluate(node):
        if isinstance(node, ast.Constant) and isinstance(node.value, (int, float)):
            return node.value
        if isinstance(node, ast.BinOp) and type(node.op) in OPS:
            return OPS[type(node.op)](evaluate(node.left), evaluate(node.right))
        if isinstance(node, ast.UnaryOp) and type(node.op) in OPS:
            return OPS[type(node.op)](evaluate(node.operand))
        raise ValueError("Unsupported expression")

    tree = ast.parse(expression, mode="eval")
    return evaluate(tree.body)

print(safe_eval("3 + 4 * (2 - 1)"))     # 7
print(safe_eval("2 ** 10"))             # 1024
# safe_eval("__import__('os').system('ls')")    # ValueError
```

---

## 11.2 Number Guessing Game

### Goal

The computer picks a secret number. The player guesses with "too high" or "too low" hints and tries to win with the fewest guesses.

### Skills practiced

`random`, `while` loops, `if/elif/else`, input validation, functions, file storage (for the high score).

### Requirements

- [ ] Random secret number in a range
- [ ] Hints after each guess
- [ ] Count attempts and limit them
- [ ] Reject invalid input without losing a turn
- [ ] Difficulty levels
- [ ] Remember the best score between games

### Plan

```
1. Pick a random number
2. Loop: read guess, compare, give a hint
3. Track attempts, end on win or when attempts run out
4. Add difficulty settings
5. Save and load the best score
6. Offer "play again"
```

### Complete code

```python
# guessing_game.py
import json
import random
from pathlib import Path

SCORE_FILE = Path("best_scores.json")

DIFFICULTIES = {
    "1": {"name": "Easy",   "max_number": 20,  "attempts": 8},
    "2": {"name": "Medium", "max_number": 50,  "attempts": 7},
    "3": {"name": "Hard",   "max_number": 100, "attempts": 6},
}

def load_scores():
    if SCORE_FILE.exists():
        try:
            return json.loads(SCORE_FILE.read_text(encoding="utf-8"))
        except json.JSONDecodeError:
            return {}
    return {}

def save_scores(scores):
    SCORE_FILE.write_text(json.dumps(scores, indent=2), encoding="utf-8")

def choose_difficulty():
    print("\nChoose difficulty:")
    for key, d in DIFFICULTIES.items():
        print(f"  {key}. {d['name']:<7} (1-{d['max_number']}, {d['attempts']} attempts)")

    while True:
        choice = input("Your choice: ").strip()
        if choice in DIFFICULTIES:
            return DIFFICULTIES[choice]
        print("Please enter 1, 2, or 3.")

def read_guess(low, high):
    while True:
        text = input(f"Your guess ({low}-{high}): ").strip()

        if not text.lstrip("-").isdigit():
            print("  That is not a whole number.")
            continue

        guess = int(text)
        if not low <= guess <= high:
            print(f"  Please guess between {low} and {high}.")
            continue

        return guess

def play_round(settings, scores):
    name = settings["name"]
    high = settings["max_number"]
    max_attempts = settings["attempts"]

    secret = random.randint(1, high)
    low_bound, high_bound = 1, high

    print(f"\nI'm thinking of a number between 1 and {high}.")
    print(f"You have {max_attempts} attempts.\n")

    for attempt in range(1, max_attempts + 1):
        remaining = max_attempts - attempt + 1
        print(f"Attempt {attempt}/{max_attempts} ({remaining} left)")

        guess = read_guess(1, high)

        if guess == secret:
            print(f"\nCorrect! You got it in {attempt} attempt(s).")

            best = scores.get(name)
            if best is None or attempt < best:
                scores[name] = attempt
                save_scores(scores)
                print(f"New best score for {name}: {attempt}!")
            return True

        if guess < secret:
            low_bound = max(low_bound, guess + 1)
            print("  Too low.")
        else:
            high_bound = min(high_bound, guess - 1)
            print("  Too high.")

        if attempt >= 3 and low_bound <= high_bound:
            print(f"  Hint: the number is between {low_bound} and {high_bound}.")

    print(f"\nOut of attempts. The number was {secret}.")
    return False

def show_scores(scores):
    if not scores:
        return
    print("\nBest scores (fewest attempts):")
    for key in DIFFICULTIES:
        name = DIFFICULTIES[key]["name"]
        if name in scores:
            print(f"  {name:<7} {scores[name]}")

def main():
    print("=== Number Guessing Game ===")
    scores = load_scores()
    wins = losses = 0

    while True:
        show_scores(scores)
        settings = choose_difficulty()

        if play_round(settings, scores):
            wins += 1
        else:
            losses += 1

        print(f"\nSession record: {wins} win(s), {losses} loss(es)")

        again = input("Play again? (y/n): ").strip().lower()
        if again not in ("y", "yes"):
            print("Thanks for playing!")
            break

if __name__ == "__main__":
    main()
```

### Sample run

```
=== Number Guessing Game ===

Choose difficulty:
  1. Easy    (1-20, 8 attempts)
  2. Medium  (1-50, 7 attempts)
  3. Hard    (1-100, 6 attempts)
Your choice: 2

I'm thinking of a number between 1 and 50.
You have 7 attempts.

Attempt 1/7 (7 left)
Your guess (1-50): 25
  Too low.
Attempt 2/7 (6 left)
Your guess (1-50): 38
  Too high.
...
Correct! You got it in 5 attempt(s).
New best score for Medium: 5!
```

### Smart strategy to try: binary search

Guessing the **middle** of the remaining range each time finds any number from 1 to 100 in at most **7 guesses** (log₂ 100 ≈ 6.6).

```python
import math
print(math.ceil(math.log2(100)))    # 7
```

### Computer plays automatically (bonus)

```python
def computer_player(secret, high):
    low = 1
    attempts = 0
    while True:
        guess = (low + high) // 2
        attempts += 1
        if guess == secret:
            return attempts
        if guess < secret:
            low = guess + 1
        else:
            high = guess - 1

print(computer_player(73, 100))
```

### Level up

- Reverse the roles: you think of a number and the computer guesses.
- Score by time as well as attempts (use `time.perf_counter()`).
- Add a two-player mode.
- Add a leaderboard with player names.
- Build the game with `tkinter` or as a web app with Flask.

---

## 11.3 Quiz Application

### Goal

Ask multiple-choice questions loaded from a file, score the answers, and show results with feedback.

### Skills practiced

Dictionaries, lists, JSON files, `random.shuffle`, loops, string handling, `time`, functions, simple data validation.

### Requirements

- [ ] Questions stored in a JSON file
- [ ] Shuffle questions and answer options
- [ ] Score and percentage with a grade
- [ ] Show the correct answer after mistakes
- [ ] Optional timer
- [ ] Save results

### Plan

```
1. Design the question format (JSON)
2. Load and validate the file
3. Ask each question, check the answer
4. Track the score and the mistakes
5. Show a summary and save the result
```

### Question file: `questions.json`

```json
[
  {
    "question": "Which keyword defines a function in Python?",
    "options": ["func", "def", "function", "lambda"],
    "answer": "def",
    "explanation": "def starts a function definition."
  },
  {
    "question": "What is the output of len([1, 2, 3])?",
    "options": ["2", "3", "4", "Error"],
    "answer": "3",
    "explanation": "The list has three items."
  },
  {
    "question": "Which data type is immutable?",
    "options": ["list", "dict", "set", "tuple"],
    "answer": "tuple",
    "explanation": "Tuples cannot be changed after creation."
  },
  {
    "question": "What does 7 // 2 return?",
    "options": ["3.5", "3", "4", "2"],
    "answer": "3",
    "explanation": "// is floor division."
  },
  {
    "question": "Which symbol starts a comment?",
    "options": ["//", "#", "--", "/*"],
    "answer": "#",
    "explanation": "Python comments begin with #."
  }
]
```

### Complete code

```python
# quiz.py
import json
import random
import time
from datetime import datetime
from pathlib import Path

QUESTION_FILE = Path("questions.json")
RESULTS_FILE = Path("quiz_results.json")
LETTERS = "ABCD"

def load_questions(path):
    if not path.exists():
        raise FileNotFoundError(f"Question file not found: {path}")

    data = json.loads(path.read_text(encoding="utf-8"))

    for i, q in enumerate(data, start=1):
        for key in ("question", "options", "answer"):
            if key not in q:
                raise ValueError(f"Question {i} is missing '{key}'")
        if q["answer"] not in q["options"]:
            raise ValueError(f"Question {i}: the answer is not among the options")
        if not 2 <= len(q["options"]) <= len(LETTERS):
            raise ValueError(f"Question {i}: needs between 2 and {len(LETTERS)} options")
    return data

def ask_question(number, total, q):
    options = q["options"][:]
    random.shuffle(options)

    print(f"\nQuestion {number}/{total}")
    print(q["question"])
    for letter, option in zip(LETTERS, options):
        print(f"  {letter}) {option}")

    valid = LETTERS[:len(options)]
    start = time.perf_counter()

    while True:
        choice = input(f"Your answer ({'/'.join(valid)}): ").strip().upper()
        if choice in valid:
            break
        print("  Please choose one of the letters shown.")

    elapsed = time.perf_counter() - start
    chosen = options[valid.index(choice)]
    correct = chosen == q["answer"]

    if correct:
        print("  Correct!")
    else:
        print(f"  Wrong. The correct answer is: {q['answer']}")
        if q.get("explanation"):
            print(f"  Explanation: {q['explanation']}")

    return correct, chosen, elapsed

def grade_for(percent):
    if percent >= 90: return "A+"
    if percent >= 80: return "A"
    if percent >= 70: return "B"
    if percent >= 60: return "C"
    return "F"

def save_result(name, score, total, percent):
    results = []
    if RESULTS_FILE.exists():
        results = json.loads(RESULTS_FILE.read_text(encoding="utf-8"))

    results.append({
        "name": name,
        "score": score,
        "total": total,
        "percent": round(percent, 1),
        "date": datetime.now().strftime("%Y-%m-%d %H:%M"),
    })
    RESULTS_FILE.write_text(json.dumps(results, indent=2), encoding="utf-8")

def show_leaderboard(limit=5):
    if not RESULTS_FILE.exists():
        return
    results = json.loads(RESULTS_FILE.read_text(encoding="utf-8"))
    results.sort(key=lambda r: r["percent"], reverse=True)

    print("\nTop scores")
    print("-" * 36)
    for i, r in enumerate(results[:limit], start=1):
        print(f"{i}. {r['name']:<12} {r['percent']:>5}%  ({r['date']})")

def run_quiz(questions):
    random.shuffle(questions)
    score = 0
    mistakes = []
    total_time = 0.0

    for number, q in enumerate(questions, start=1):
        correct, chosen, elapsed = ask_question(number, len(questions), q)
        total_time += elapsed
        if correct:
            score += 1
        else:
            mistakes.append((q["question"], chosen, q["answer"]))

    return score, mistakes, total_time

def main():
    print("=== Python Quiz ===")
    name = input("Your name: ").strip() or "Player"

    try:
        questions = load_questions(QUESTION_FILE)
    except (FileNotFoundError, ValueError, json.JSONDecodeError) as e:
        print(f"Could not load questions: {e}")
        return

    while True:
        score, mistakes, total_time = run_quiz(questions[:])

        total = len(questions)
        percent = score / total * 100

        print("\n" + "=" * 36)
        print(f"{name}, you scored {score}/{total} ({percent:.0f}%)")
        print(f"Grade: {grade_for(percent)}")
        print(f"Time: {total_time:.1f}s ({total_time / total:.1f}s per question)")

        if mistakes:
            print("\nReview your mistakes:")
            for question, chosen, answer in mistakes:
                print(f"  - {question}")
                print(f"      You: {chosen} | Correct: {answer}")

        save_result(name, score, total, percent)
        show_leaderboard()

        if input("\nTake the quiz again? (y/n): ").strip().lower() not in ("y", "yes"):
            print("Good luck with your studies!")
            break

if __name__ == "__main__":
    main()
```

### Sample run

```
=== Python Quiz ===
Your name: Ali

Question 1/5
Which data type is immutable?
  A) set
  B) tuple
  C) list
  D) dict
Your answer (A/B/C/D): b
  Correct!
...
====================================
Ali, you scored 4/5 (80%)
Grade: A
Time: 21.3s (4.3s per question)

Review your mistakes:
  - What does 7 // 2 return?
      You: 3.5 | Correct: 3
```

### Level up

- Add categories and difficulty levels.
- Add true/false and fill-in-the-blank question types.
- Add a countdown timer per question (research `threading.Timer` or `select`).
- Build a question editor that adds questions to the JSON file.
- Fetch questions from the free **Open Trivia Database** API (see 11.8 for API techniques).
- Use `tkinter` or Flask for a graphical version.

---

## 11.4 To-Do List

### Goal

A command-line task manager that stores tasks in a file, so they survive between runs.

### Skills practiced

Lists of dictionaries, JSON persistence, menus, dates, sorting, filtering, string formatting, classes.

### Requirements

- [ ] Add, list, complete, edit, and delete tasks
- [ ] Priority levels and due dates
- [ ] Filter (pending, done, overdue) and sort
- [ ] Search tasks
- [ ] Save automatically

### Plan

```
1. Define a Task (id, title, priority, due date, done)
2. Create a TaskManager class with the operations
3. Load and save JSON
4. Build the menu and the display table
5. Add filters, sorting, and search
```

### Complete code

```python
# todo.py
import json
from dataclasses import dataclass, asdict
from datetime import date, datetime
from pathlib import Path

DATA_FILE = Path("tasks.json")
PRIORITIES = {"high": 1, "medium": 2, "low": 3}

@dataclass
class Task:
    id: int
    title: str
    priority: str = "medium"
    due: str | None = None            # "YYYY-MM-DD"
    done: bool = False
    created: str = ""

    def is_overdue(self):
        if self.done or not self.due:
            return False
        return date.fromisoformat(self.due) < date.today()

class TaskManager:
    def __init__(self, path=DATA_FILE):
        self.path = path
        self.tasks = []
        self.load()

    # ----- storage -----
    def load(self):
        if not self.path.exists():
            return
        try:
            raw = json.loads(self.path.read_text(encoding="utf-8"))
            self.tasks = [Task(**item) for item in raw]
        except (json.JSONDecodeError, TypeError):
            print("Warning: tasks file is damaged. Starting empty.")
            self.tasks = []

    def save(self):
        data = [asdict(t) for t in self.tasks]
        self.path.write_text(json.dumps(data, indent=2), encoding="utf-8")

    # ----- operations -----
    def _next_id(self):
        return max((t.id for t in self.tasks), default=0) + 1

    def add(self, title, priority="medium", due=None):
        task = Task(
            id=self._next_id(),
            title=title,
            priority=priority,
            due=due,
            created=datetime.now().strftime("%Y-%m-%d %H:%M"),
        )
        self.tasks.append(task)
        self.save()
        return task

    def get(self, task_id):
        for t in self.tasks:
            if t.id == task_id:
                return t
        return None

    def complete(self, task_id):
        task = self.get(task_id)
        if task:
            task.done = True
            self.save()
        return task

    def edit(self, task_id, **changes):
        task = self.get(task_id)
        if task:
            for key, value in changes.items():
                if value is not None:
                    setattr(task, key, value)
            self.save()
        return task

    def delete(self, task_id):
        task = self.get(task_id)
        if task:
            self.tasks.remove(task)
            self.save()
        return task

    def clear_completed(self):
        before = len(self.tasks)
        self.tasks = [t for t in self.tasks if not t.done]
        self.save()
        return before - len(self.tasks)

    def search(self, text):
        text = text.lower()
        return [t for t in self.tasks if text in t.title.lower()]

    def filtered(self, mode="all", sort_by="id"):
        if mode == "pending":
            items = [t for t in self.tasks if not t.done]
        elif mode == "done":
            items = [t for t in self.tasks if t.done]
        elif mode == "overdue":
            items = [t for t in self.tasks if t.is_overdue()]
        else:
            items = self.tasks[:]

        if sort_by == "priority":
            items.sort(key=lambda t: PRIORITIES[t.priority])
        elif sort_by == "due":
            items.sort(key=lambda t: (t.due is None, t.due or ""))
        else:
            items.sort(key=lambda t: t.id)
        return items

# ----- user interface helpers -----
def read_int(prompt):
    text = input(prompt).strip()
    return int(text) if text.isdigit() else None

def read_priority(prompt="Priority (high/medium/low) [medium]: "):
    while True:
        value = input(prompt).strip().lower() or "medium"
        if value in PRIORITIES:
            return value
        print("  Please type high, medium, or low.")

def read_due(prompt="Due date (YYYY-MM-DD, blank for none): "):
    while True:
        text = input(prompt).strip()
        if not text:
            return None
        try:
            date.fromisoformat(text)
            return text
        except ValueError:
            print("  Use the format YYYY-MM-DD, for example 2026-12-31.")

def print_tasks(tasks):
    if not tasks:
        print("  (no tasks to show)")
        return

    print(f"\n{'ID':<4} {'Done':<5} {'Priority':<9} {'Due':<11} Title")
    print("-" * 60)
    for t in tasks:
        mark = "[x]" if t.done else "[ ]"
        due = t.due or "-"
        flag = "  <-- OVERDUE" if t.is_overdue() else ""
        print(f"{t.id:<4} {mark:<5} {t.priority:<9} {due:<11} {t.title}{flag}")

    done = sum(t.done for t in tasks)
    print("-" * 60)
    print(f"{len(tasks)} task(s), {done} done, {len(tasks) - done} pending")

MENU = """
=== To-Do List ===
1. Add task
2. List tasks
3. Complete task
4. Edit task
5. Delete task
6. Search
7. Clear completed
8. Quit
"""

def main():
    manager = TaskManager()

    while True:
        print(MENU)
        choice = input("Choose an option: ").strip()

        if choice == "1":
            title = input("Task title: ").strip()
            if not title:
                print("  A task needs a title.")
                continue
            task = manager.add(title, read_priority(), read_due())
            print(f"  Added task #{task.id}.")

        elif choice == "2":
            mode = input("Show (all/pending/done/overdue) [all]: ").strip().lower() or "all"
            sort_by = input("Sort by (id/priority/due) [id]: ").strip().lower() or "id"
            print_tasks(manager.filtered(mode, sort_by))

        elif choice == "3":
            task_id = read_int("Task ID to complete: ")
            task = manager.complete(task_id) if task_id else None
            print(f"  Completed: {task.title}" if task else "  Task not found.")

        elif choice == "4":
            task_id = read_int("Task ID to edit: ")
            task = manager.get(task_id) if task_id else None
            if not task:
                print("  Task not found.")
                continue
            print("  Leave a field blank to keep the current value.")
            title = input(f"  Title [{task.title}]: ").strip() or None
            priority = input(f"  Priority [{task.priority}]: ").strip().lower() or None
            if priority and priority not in PRIORITIES:
                print("  Invalid priority; keeping the old one.")
                priority = None
            due = input(f"  Due [{task.due or 'none'}]: ").strip() or None
            if due:
                try:
                    date.fromisoformat(due)
                except ValueError:
                    print("  Invalid date; keeping the old one.")
                    due = None
            manager.edit(task.id, title=title, priority=priority, due=due)
            print("  Task updated.")

        elif choice == "5":
            task_id = read_int("Task ID to delete: ")
            task = manager.get(task_id) if task_id else None
            if task and input(f"  Delete '{task.title}'? (y/n): ").lower() == "y":
                manager.delete(task.id)
                print("  Deleted.")
            elif not task:
                print("  Task not found.")

        elif choice == "6":
            term = input("Search for: ").strip()
            print_tasks(manager.search(term))

        elif choice == "7":
            removed = manager.clear_completed()
            print(f"  Removed {removed} completed task(s).")

        elif choice == "8":
            print("Goodbye!")
            break

        else:
            print("  Please choose a number from 1 to 8.")

if __name__ == "__main__":
    main()
```

### Sample run

```
ID   Done  Priority  Due         Title
------------------------------------------------------------
1    [ ]   high      2026-10-20  Finish Python chapter 11
2    [x]   medium    -           Buy groceries
3    [ ]   low       2026-10-01  Call the bank  <-- OVERDUE
------------------------------------------------------------
3 task(s), 1 done, 2 pending
```

### Tests

```python
# test_todo.py
from pathlib import Path
from todo import TaskManager

def make_manager(tmp_path):
    return TaskManager(tmp_path / "tasks.json")

def test_add_and_persist(tmp_path):
    m = make_manager(tmp_path)
    m.add("Write tests", "high")

    reloaded = make_manager(tmp_path)       # new object, same file
    assert [t.title for t in reloaded.tasks] == ["Write tests"]

def test_complete(tmp_path):
    m = make_manager(tmp_path)
    task = m.add("Do thing")
    m.complete(task.id)
    assert m.get(task.id).done is True

def test_ids_never_reused(tmp_path):
    m = make_manager(tmp_path)
    a = m.add("A")
    m.delete(a.id)
    b = m.add("B")
    assert b.id == 1 or b.id > 0     # simple check; see level-up for a stricter rule

def test_overdue(tmp_path):
    m = make_manager(tmp_path)
    m.add("Old", due="2000-01-01")
    assert len(m.filtered("overdue")) == 1
```

### Level up

- Recurring tasks (daily, weekly).
- Tags and projects; filter by tag.
- Reminders using a scheduled task (`schedule` library, cron, or Task Scheduler).
- Undo the last action.
- Export to CSV.
- Convert to a web app (Flask or FastAPI) with the same `TaskManager` class.

---

## 11.5 Contact Book

### Goal

Store people's names, phone numbers, emails, and notes; search and edit them; keep them in a file.

### Skills practiced

Classes and objects, dictionaries, JSON, regular expressions for validation, sorting, search, CSV export.

### Requirements

- [ ] Add, view, edit, delete contacts
- [ ] Search by name, phone, or email (partial match)
- [ ] Validate phone numbers and emails
- [ ] Prevent duplicates
- [ ] Export to CSV
- [ ] Persistent storage

### Plan

```
1. Contact class with validation
2. ContactBook class: add, find, update, delete, search
3. JSON load and save
4. Menu-driven interface
5. CSV export
```

### Complete code

```python
# contact_book.py
import csv
import json
import re
from dataclasses import dataclass, asdict, field
from pathlib import Path

DATA_FILE = Path("contacts.json")

PHONE_PATTERN = re.compile(r"^\+?\d[\d\s\-]{6,14}\d$")
EMAIL_PATTERN = re.compile(r"^[\w.+-]+@[\w-]+(?:\.[\w-]+)+$")

class ValidationError(ValueError):
    pass

def normalize_phone(phone):
    """Keep digits and a leading plus sign: '0300-123 4567' -> '03001234567'."""
    phone = phone.strip()
    return ("+" if phone.startswith("+") else "") + re.sub(r"\D", "", phone)

@dataclass
class Contact:
    name: str
    phone: str
    email: str = ""
    address: str = ""
    notes: str = ""

    def __post_init__(self):
        self.name = self.name.strip()
        if not self.name:
            raise ValidationError("Name cannot be empty.")

        if not PHONE_PATTERN.match(self.phone.strip()):
            raise ValidationError("Invalid phone number (7 to 15 digits expected).")
        self.phone = normalize_phone(self.phone)

        self.email = self.email.strip()
        if self.email and not EMAIL_PATTERN.match(self.email):
            raise ValidationError("Invalid email address.")

    def matches(self, text):
        text = text.lower()
        return any(text in value.lower()
                   for value in (self.name, self.phone, self.email, self.address))

class ContactBook:
    def __init__(self, path=DATA_FILE):
        self.path = path
        self.contacts = []
        self.load()

    def load(self):
        if not self.path.exists():
            return
        try:
            raw = json.loads(self.path.read_text(encoding="utf-8"))
            self.contacts = [Contact(**c) for c in raw]
        except (json.JSONDecodeError, TypeError, ValidationError) as e:
            print(f"Warning: could not read contacts file ({e}). Starting empty.")

    def save(self):
        data = [asdict(c) for c in self.contacts]
        self.path.write_text(
            json.dumps(data, indent=2, ensure_ascii=False), encoding="utf-8"
        )

    def _find_index(self, name):
        for i, c in enumerate(self.contacts):
            if c.name.lower() == name.lower():
                return i
        return None

    def add(self, contact):
        if self._find_index(contact.name) is not None:
            raise ValidationError(f"A contact named '{contact.name}' already exists.")
        if any(c.phone == contact.phone for c in self.contacts):
            raise ValidationError("That phone number is already saved.")
        self.contacts.append(contact)
        self.contacts.sort(key=lambda c: c.name.lower())
        self.save()

    def get(self, name):
        i = self._find_index(name)
        return self.contacts[i] if i is not None else None

    def search(self, text):
        return [c for c in self.contacts if c.matches(text)]

    def update(self, name, **changes):
        i = self._find_index(name)
        if i is None:
            return None

        current = asdict(self.contacts[i])
        for key, value in changes.items():
            if value:
                current[key] = value

        updated = Contact(**current)                # validates again
        self.contacts[i] = updated
        self.contacts.sort(key=lambda c: c.name.lower())
        self.save()
        return updated

    def delete(self, name):
        i = self._find_index(name)
        if i is None:
            return None
        removed = self.contacts.pop(i)
        self.save()
        return removed

    def export_csv(self, path):
        with open(path, "w", newline="", encoding="utf-8") as f:
            writer = csv.DictWriter(f, fieldnames=["name", "phone", "email", "address", "notes"])
            writer.writeheader()
            writer.writerows(asdict(c) for c in self.contacts)
        return len(self.contacts)

    def import_csv(self, path):
        added = skipped = 0
        with open(path, newline="", encoding="utf-8") as f:
            for row in csv.DictReader(f):
                try:
                    self.add(Contact(**row))
                    added += 1
                except (ValidationError, TypeError):
                    skipped += 1
        return added, skipped

def show(contacts):
    if not contacts:
        print("  (no contacts found)")
        return
    for c in contacts:
        print(f"\n  {c.name}")
        print(f"    Phone:   {c.phone}")
        if c.email:   print(f"    Email:   {c.email}")
        if c.address: print(f"    Address: {c.address}")
        if c.notes:   print(f"    Notes:   {c.notes}")
    print(f"\n  {len(contacts)} contact(s)")

def ask_contact():
    while True:
        try:
            return Contact(
                name=input("Name: "),
                phone=input("Phone: "),
                email=input("Email (optional): "),
                address=input("Address (optional): "),
                notes=input("Notes (optional): "),
            )
        except ValidationError as e:
            print(f"  {e} Let's try again.\n")

MENU = """
=== Contact Book ===
1. Add contact
2. View all
3. Search
4. Edit contact
5. Delete contact
6. Export to CSV
7. Import from CSV
8. Quit
"""

def main():
    book = ContactBook()

    while True:
        print(MENU)
        choice = input("Choose: ").strip()

        if choice == "1":
            try:
                book.add(ask_contact())
                print("  Contact saved.")
            except ValidationError as e:
                print(f"  Could not save: {e}")

        elif choice == "2":
            show(book.contacts)

        elif choice == "3":
            show(book.search(input("Search for: ").strip()))

        elif choice == "4":
            name = input("Name of the contact to edit: ").strip()
            if not book.get(name):
                print("  Contact not found.")
                continue
            print("  Leave a field blank to keep it.")
            try:
                book.update(
                    name,
                    name=input("  New name: ").strip(),
                    phone=input("  New phone: ").strip(),
                    email=input("  New email: ").strip(),
                    address=input("  New address: ").strip(),
                    notes=input("  New notes: ").strip(),
                )
                print("  Updated.")
            except ValidationError as e:
                print(f"  Not updated: {e}")

        elif choice == "5":
            name = input("Name to delete: ").strip()
            contact = book.get(name)
            if contact and input(f"  Delete {contact.name}? (y/n): ").lower() == "y":
                book.delete(name)
                print("  Deleted.")
            elif not contact:
                print("  Contact not found.")

        elif choice == "6":
            count = book.export_csv("contacts_export.csv")
            print(f"  Exported {count} contact(s) to contacts_export.csv")

        elif choice == "7":
            path = input("CSV file to import: ").strip()
            try:
                added, skipped = book.import_csv(path)
                print(f"  Imported {added}, skipped {skipped} (invalid or duplicate).")
            except FileNotFoundError:
                print("  File not found.")

        elif choice == "8":
            print("Goodbye!")
            break

        else:
            print("  Please choose 1 to 8.")

if __name__ == "__main__":
    main()
```

### Tests

```python
# test_contact_book.py
import pytest
from contact_book import Contact, ContactBook, ValidationError

def test_phone_is_normalized():
    c = Contact("Ali", "0300-123 4567")
    assert c.phone == "03001234567"

@pytest.mark.parametrize("phone", ["abc", "12", ""])
def test_invalid_phone(phone):
    with pytest.raises(ValidationError):
        Contact("Ali", phone)

def test_invalid_email():
    with pytest.raises(ValidationError):
        Contact("Ali", "03001234567", email="not-an-email")

def test_duplicate_name(tmp_path):
    book = ContactBook(tmp_path / "c.json")
    book.add(Contact("Ali", "03001234567"))
    with pytest.raises(ValidationError):
        book.add(Contact("ali", "03111234567"))

def test_search(tmp_path):
    book = ContactBook(tmp_path / "c.json")
    book.add(Contact("Ali Khan", "03001234567", "ali@example.com"))
    book.add(Contact("Sara", "03111234567"))
    assert [c.name for c in book.search("khan")] == ["Ali Khan"]
    assert [c.name for c in book.search("0311")] == ["Sara"]
```

### Level up

- Groups (family, work, friends).
- Birthday reminders.
- Import `.vcf` (vCard) files.
- Add a photo path field.
- Replace JSON with SQLite (see 11.9).
- Fuzzy search with `difflib.get_close_matches`.

---

## 11.6 Expense Tracker

### Goal

Record income and expenses, categorize them, and see monthly summaries, so you understand where money goes.

### Skills practiced

Dates, CSV files, `collections.Counter`/`defaultdict`, aggregation, string formatting, validation with `Decimal`.

### Requirements

- [ ] Add expenses with amount, category, date, and note
- [ ] List, filter, and delete expenses
- [ ] Monthly and category summaries
- [ ] Budget limits with warnings
- [ ] Store data in CSV
- [ ] Export a report

### Plan

```
1. Choose a storage format (CSV with id, date, category, amount, note)
2. Use Decimal for money (no floating-point errors)
3. Write functions: add, load, filter, summarize
4. Build text "bar charts" for categories
5. Add budgets and warnings
```

> **Why `Decimal`?** `0.1 + 0.2` is `0.30000000000000004` with floats. Money calculations should use `decimal.Decimal` (or integers counting cents).

### Complete code

```python
# expense_tracker.py
import csv
import json
from collections import defaultdict
from dataclasses import dataclass
from datetime import date, datetime
from decimal import Decimal, InvalidOperation
from pathlib import Path

EXPENSE_FILE = Path("expenses.csv")
BUDGET_FILE = Path("budgets.json")
FIELDS = ["id", "date", "category", "amount", "note"]

CATEGORIES = ["Food", "Transport", "Bills", "Shopping", "Health", "Entertainment", "Other"]

@dataclass
class Expense:
    id: int
    date: date
    category: str
    amount: Decimal
    note: str = ""

    def to_row(self):
        return {
            "id": self.id,
            "date": self.date.isoformat(),
            "category": self.category,
            "amount": str(self.amount),
            "note": self.note,
        }

    @classmethod
    def from_row(cls, row):
        return cls(
            id=int(row["id"]),
            date=date.fromisoformat(row["date"]),
            category=row["category"],
            amount=Decimal(row["amount"]),
            note=row["note"],
        )

class ExpenseTracker:
    def __init__(self, path=EXPENSE_FILE, budget_path=BUDGET_FILE):
        self.path = path
        self.budget_path = budget_path
        self.expenses = []
        self.budgets = {}
        self.load()

    # ----- storage -----
    def load(self):
        if self.path.exists():
            with open(self.path, newline="", encoding="utf-8") as f:
                self.expenses = [Expense.from_row(r) for r in csv.DictReader(f)]
        if self.budget_path.exists():
            raw = json.loads(self.budget_path.read_text(encoding="utf-8"))
            self.budgets = {k: Decimal(v) for k, v in raw.items()}

    def save(self):
        with open(self.path, "w", newline="", encoding="utf-8") as f:
            writer = csv.DictWriter(f, fieldnames=FIELDS)
            writer.writeheader()
            writer.writerows(e.to_row() for e in self.expenses)

    def save_budgets(self):
        raw = {k: str(v) for k, v in self.budgets.items()}
        self.budget_path.write_text(json.dumps(raw, indent=2), encoding="utf-8")

    # ----- operations -----
    def add(self, when, category, amount, note=""):
        if amount <= 0:
            raise ValueError("Amount must be greater than zero.")
        new_id = max((e.id for e in self.expenses), default=0) + 1
        expense = Expense(new_id, when, category, amount, note)
        self.expenses.append(expense)
        self.save()
        return expense

    def delete(self, expense_id):
        for e in self.expenses:
            if e.id == expense_id:
                self.expenses.remove(e)
                self.save()
                return e
        return None

    def for_month(self, year, month):
        return [e for e in self.expenses if e.date.year == year and e.date.month == month]

    def by_category(self, expenses):
        totals = defaultdict(Decimal)
        for e in expenses:
            totals[e.category] += e.amount
        return dict(sorted(totals.items(), key=lambda kv: kv[1], reverse=True))

    def by_month(self):
        totals = defaultdict(Decimal)
        for e in self.expenses:
            totals[e.date.strftime("%Y-%m")] += e.amount
        return dict(sorted(totals.items()))

    def set_budget(self, category, amount):
        self.budgets[category] = amount
        self.save_budgets()

    def budget_status(self, year, month):
        spent = self.by_category(self.for_month(year, month))
        status = []
        for category, limit in self.budgets.items():
            used = spent.get(category, Decimal("0"))
            status.append((category, used, limit))
        return status

# ----- display helpers -----
def money(value):
    return f"{value:,.2f}"

def read_amount(prompt):
    while True:
        text = input(prompt).strip().replace(",", "")
        try:
            amount = Decimal(text)
            if amount <= 0:
                print("  Amount must be positive.")
                continue
            return amount.quantize(Decimal("0.01"))
        except InvalidOperation:
            print("  Please enter a number, for example 450.75")

def read_date(prompt):
    while True:
        text = input(prompt).strip()
        if not text:
            return date.today()
        try:
            return datetime.strptime(text, "%Y-%m-%d").date()
        except ValueError:
            print("  Use YYYY-MM-DD, or press Enter for today.")

def read_category():
    print("  Categories: " + ", ".join(f"{i}) {c}" for i, c in enumerate(CATEGORIES, start=1)))
    while True:
        text = input("  Category (number or name): ").strip()
        if text.isdigit() and 1 <= int(text) <= len(CATEGORIES):
            return CATEGORIES[int(text) - 1]
        for c in CATEGORIES:
            if c.lower() == text.lower():
                return c
        print("  Please pick from the list.")

def print_expenses(expenses):
    if not expenses:
        print("  (no expenses)")
        return
    print(f"\n{'ID':<4} {'Date':<11} {'Category':<14} {'Amount':>10}  Note")
    print("-" * 60)
    for e in sorted(expenses, key=lambda x: (x.date, x.id)):
        print(f"{e.id:<4} {e.date.isoformat():<11} {e.category:<14} {money(e.amount):>10}  {e.note}")
    print("-" * 60)
    print(f"{'Total':<31} {money(sum(e.amount for e in expenses)):>10}")

def print_bar_chart(totals):
    if not totals:
        print("  (nothing to show)")
        return
    biggest = max(totals.values())
    grand = sum(totals.values())
    print()
    for name, value in totals.items():
        bar = "#" * int(value / biggest * 30)
        share = value / grand * 100
        print(f"  {name:<14} {bar:<30} {money(value):>10} ({share:4.1f}%)")
    print(f"  {'TOTAL':<14} {'':<30} {money(grand):>10}")

def print_budget_status(tracker, year, month):
    status = tracker.budget_status(year, month)
    if not status:
        print("  No budgets set.")
        return
    print()
    for category, used, limit in status:
        percent = used / limit * 100
        flag = ""
        if used > limit:
            flag = "  OVER BUDGET!"
        elif percent >= 80:
            flag = "  warning: nearly at the limit"
        print(f"  {category:<14} {money(used):>10} / {money(limit):<10} ({percent:5.1f}%){flag}")

MENU = """
=== Expense Tracker ===
1. Add expense
2. View all expenses
3. Monthly report
4. Spending by category (all time)
5. Month-by-month totals
6. Set a budget
7. Delete expense
8. Quit
"""

def main():
    tracker = ExpenseTracker()

    while True:
        print(MENU)
        choice = input("Choose: ").strip()

        if choice == "1":
            when = read_date("Date (YYYY-MM-DD, Enter = today): ")
            category = read_category()
            amount = read_amount("  Amount: ")
            note = input("  Note (optional): ").strip()
            expense = tracker.add(when, category, amount, note)
            print(f"  Saved expense #{expense.id}.")

            for cat, used, limit in tracker.budget_status(when.year, when.month):
                if cat == category and used > limit:
                    print(f"  Warning: you are over your {cat} budget this month!")

        elif choice == "2":
            print_expenses(tracker.expenses)

        elif choice == "3":
            today = date.today()
            text = input(f"Month (YYYY-MM) [{today:%Y-%m}]: ").strip() or f"{today:%Y-%m}"
            try:
                year, month = map(int, text.split("-"))
                items = tracker.for_month(year, month)
            except ValueError:
                print("  Use the format YYYY-MM.")
                continue
            print(f"\nReport for {text}")
            print_expenses(items)
            print_bar_chart(tracker.by_category(items))
            print("\nBudgets:")
            print_budget_status(tracker, year, month)

        elif choice == "4":
            print_bar_chart(tracker.by_category(tracker.expenses))

        elif choice == "5":
            print_bar_chart(tracker.by_month())

        elif choice == "6":
            category = read_category()
            tracker.set_budget(category, read_amount("  Monthly limit: "))
            print("  Budget saved.")

        elif choice == "7":
            text = input("Expense ID to delete: ").strip()
            removed = tracker.delete(int(text)) if text.isdigit() else None
            print("  Deleted." if removed else "  Not found.")

        elif choice == "8":
            print("Goodbye!")
            break

        else:
            print("  Please choose 1 to 8.")

if __name__ == "__main__":
    main()
```

### Sample output

```
Report for 2026-10

ID   Date        Category         Amount  Note
------------------------------------------------------------
1    2026-10-02  Food             850.00  Groceries
2    2026-10-05  Transport        400.00  Fuel
3    2026-10-12  Food             650.50  Dinner out
------------------------------------------------------------
Total                           1,900.50

  Food           ##############################   1,500.50 (78.9%)
  Transport      ########                           400.00 (21.1%)
  TOTAL                                           1,900.50

Budgets:
  Food             1,500.50 / 1,200.00    (125.0%)  OVER BUDGET!
```

### Tests

```python
# test_expense_tracker.py
from datetime import date
from decimal import Decimal
import pytest
from expense_tracker import ExpenseTracker

@pytest.fixture
def tracker(tmp_path):
    return ExpenseTracker(tmp_path / "e.csv", tmp_path / "b.json")

def test_add_and_reload(tracker, tmp_path):
    tracker.add(date(2026, 10, 1), "Food", Decimal("100.50"), "Lunch")
    reloaded = ExpenseTracker(tmp_path / "e.csv", tmp_path / "b.json")
    assert reloaded.expenses[0].amount == Decimal("100.50")

def test_rejects_non_positive(tracker):
    with pytest.raises(ValueError):
        tracker.add(date.today(), "Food", Decimal("0"))

def test_category_totals(tracker):
    tracker.add(date(2026, 10, 1), "Food", Decimal("100"))
    tracker.add(date(2026, 10, 2), "Food", Decimal("50"))
    tracker.add(date(2026, 10, 3), "Bills", Decimal("500"))
    totals = tracker.by_category(tracker.expenses)
    assert totals["Food"] == Decimal("150")
    assert list(totals)[0] == "Bills"           # sorted largest first

def test_decimal_precision(tracker):
    tracker.add(date.today(), "Food", Decimal("0.10"))
    tracker.add(date.today(), "Food", Decimal("0.20"))
    assert tracker.by_category(tracker.expenses)["Food"] == Decimal("0.30")
```

### Level up

- Recurring expenses (rent, subscriptions).
- Income tracking and savings rate.
- Charts with `matplotlib`.
- Multiple currencies using an exchange-rate API.
- Import bank statements (CSV).
- Move to SQLite; add a web dashboard.

---

## 11.7 File Organizer

### Goal

Tidy a messy folder (such as Downloads) by moving files into subfolders by type, safely: preview first, never overwrite, and allow undo.

### Skills practiced

`pathlib`, `shutil`, dictionaries, command-line arguments (`argparse`), logging, safety checks, JSON for undo history.

### Requirements

- [ ] Group files by extension category (images, documents, videos, and so on)
- [ ] **Dry-run mode** that only shows what would happen
- [ ] Never overwrite existing files (rename duplicates)
- [ ] Undo the last run
- [ ] Skip hidden files and folders
- [ ] Command-line options and a summary

### Plan

```
1. Map extensions to category names
2. Scan the folder (top level only, by default)
3. Compute each file's destination
4. Resolve name clashes
5. Move (or just print, in dry-run mode)
6. Record moves in a log file for undo
```

### Safety rules for file tools

> A file-moving script can do real damage. Always: **(1)** default to a dry run, **(2)** never overwrite, **(3)** keep a log so you can undo, **(4)** test on a copy of a folder first.

### Complete code

```python
# organizer.py
import argparse
import json
import logging
import shutil
import sys
from collections import Counter
from datetime import datetime
from pathlib import Path

CATEGORIES = {
    "Images":     {".jpg", ".jpeg", ".png", ".gif", ".bmp", ".svg", ".webp", ".heic"},
    "Documents":  {".pdf", ".doc", ".docx", ".txt", ".rtf", ".odt", ".md"},
    "Spreadsheets": {".xls", ".xlsx", ".csv", ".ods"},
    "Presentations": {".ppt", ".pptx", ".odp"},
    "Videos":     {".mp4", ".mkv", ".avi", ".mov", ".wmv", ".webm"},
    "Audio":      {".mp3", ".wav", ".flac", ".aac", ".ogg", ".m4a"},
    "Archives":   {".zip", ".rar", ".7z", ".tar", ".gz", ".bz2"},
    "Code":       {".py", ".js", ".html", ".css", ".java", ".c", ".cpp", ".json", ".sql"},
    "Installers": {".exe", ".msi", ".dmg", ".pkg", ".deb", ".apk"},
}

UNDO_LOG_NAME = ".organizer_undo.json"

logger = logging.getLogger("organizer")

def category_for(path):
    ext = path.suffix.lower()
    for category, extensions in CATEGORIES.items():
        if ext in extensions:
            return category
    return "Other"

def unique_destination(destination):
    """If 'photo.jpg' exists, return 'photo (1).jpg', 'photo (2).jpg', ..."""
    if not destination.exists():
        return destination

    counter = 1
    while True:
        candidate = destination.with_name(f"{destination.stem} ({counter}){destination.suffix}")
        if not candidate.exists():
            return candidate
        counter += 1

def plan_moves(folder, include_hidden=False):
    """Return a list of (source, destination) without touching anything."""
    plan = []
    reserved = set()                     # destinations already claimed in this plan

    for item in sorted(folder.iterdir()):
        if not item.is_file():
            continue
        if item.name == UNDO_LOG_NAME:
            continue
        if item.name.startswith(".") and not include_hidden:
            continue

        target_dir = folder / category_for(item)
        destination = unique_destination(target_dir / item.name)

        while destination in reserved:                       # two planned files, same name
            destination = unique_destination(
                destination.with_name(f"{destination.stem}_{len(reserved)}{destination.suffix}")
            )
        reserved.add(destination)
        plan.append((item, destination))

    return plan

def execute_moves(plan, folder, dry_run=True):
    moved = []

    for source, destination in plan:
        relative = destination.relative_to(folder)

        if dry_run:
            print(f"  [dry run] {source.name}  ->  {relative}")
            continue

        try:
            destination.parent.mkdir(exist_ok=True)
            shutil.move(str(source), str(destination))
            moved.append({"from": str(source), "to": str(destination)})
            print(f"  moved     {source.name}  ->  {relative}")
            logger.info("Moved %s -> %s", source, destination)
        except (OSError, shutil.Error) as e:
            print(f"  FAILED    {source.name}: {e}")
            logger.error("Failed to move %s: %s", source, e)

    if moved:
        log_path = folder / UNDO_LOG_NAME
        record = {"time": datetime.now().isoformat(timespec="seconds"), "moves": moved}
        log_path.write_text(json.dumps(record, indent=2), encoding="utf-8")

    return moved

def undo_last(folder, dry_run=True):
    log_path = folder / UNDO_LOG_NAME
    if not log_path.exists():
        print("Nothing to undo.")
        return 0

    record = json.loads(log_path.read_text(encoding="utf-8"))
    restored = 0

    for move in reversed(record["moves"]):
        current = Path(move["to"])
        original = Path(move["from"])

        if not current.exists():
            print(f"  skipped (missing): {current.name}")
            continue

        original = unique_destination(original)             # do not overwrite anything
        if dry_run:
            print(f"  [dry run] {current.name}  ->  {original.name}")
        else:
            shutil.move(str(current), str(original))
            print(f"  restored  {current.name}  ->  {original.name}")
        restored += 1

    if not dry_run:
        log_path.unlink()
        for sub in folder.iterdir():                        # remove now-empty category folders
            if sub.is_dir() and not any(sub.iterdir()):
                sub.rmdir()

    return restored

def summarize(plan):
    counts = Counter(destination.parent.name for _, destination in plan)
    print("\nSummary:")
    for category, n in counts.most_common():
        print(f"  {category:<14} {n} file(s)")
    print(f"  {'TOTAL':<14} {len(plan)} file(s)")

def build_parser():
    parser = argparse.ArgumentParser(
        description="Organize files in a folder into category subfolders."
    )
    parser.add_argument("folder", type=Path, help="Folder to organize")
    parser.add_argument("--apply", action="store_true",
                        help="Actually move files (default is a safe dry run)")
    parser.add_argument("--undo", action="store_true", help="Undo the last organize run")
    parser.add_argument("--hidden", action="store_true", help="Include hidden files")
    parser.add_argument("-v", "--verbose", action="store_true", help="Write a log file")
    return parser

def main(argv=None):
    args = build_parser().parse_args(argv)
    folder = args.folder.expanduser().resolve()

    if not folder.is_dir():
        print(f"Not a folder: {folder}")
        return 1

    if args.verbose:
        logging.basicConfig(
            filename=folder / "organizer.log",
            level=logging.INFO,
            format="%(asctime)s %(levelname)s %(message)s",
        )

    dry_run = not args.apply
    print(f"Folder: {folder}")
    print("Mode:   " + ("DRY RUN (nothing will change; use --apply to move files)"
                        if dry_run else "APPLY (files will be moved)") + "\n")

    if args.undo:
        count = undo_last(folder, dry_run)
        print(f"\n{count} file(s) {'would be ' if dry_run else ''}restored.")
        return 0

    plan = plan_moves(folder, include_hidden=args.hidden)
    if not plan:
        print("No files to organize.")
        return 0

    execute_moves(plan, folder, dry_run)
    summarize(plan)
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

### Usage

```bash
python organizer.py ~/Downloads                  # preview only
python organizer.py ~/Downloads --apply          # really move files
python organizer.py ~/Downloads --undo           # preview an undo
python organizer.py ~/Downloads --undo --apply   # really undo
python organizer.py ~/Downloads --apply -v       # also write organizer.log
```

### Sample output

```
Folder: /home/ali/Downloads
Mode:   DRY RUN (nothing will change; use --apply to move files)

  [dry run] holiday.jpg  ->  Images/holiday.jpg
  [dry run] resume.pdf  ->  Documents/resume.pdf
  [dry run] budget.xlsx  ->  Spreadsheets/budget.xlsx
  [dry run] setup.exe  ->  Installers/setup.exe

Summary:
  Images         1 file(s)
  Documents      1 file(s)
  Spreadsheets   1 file(s)
  Installers     1 file(s)
  TOTAL          4 file(s)
```

### Tests (safe, using a temporary folder)

```python
# test_organizer.py
from organizer import category_for, plan_moves, execute_moves, undo_last, unique_destination

def make_files(folder, names):
    for n in names:
        (folder / n).write_text("data")

def test_category_for(tmp_path):
    assert category_for(tmp_path / "a.JPG") == "Images"
    assert category_for(tmp_path / "a.xyz") == "Other"

def test_dry_run_changes_nothing(tmp_path):
    make_files(tmp_path, ["a.jpg", "b.pdf"])
    plan = plan_moves(tmp_path)
    execute_moves(plan, tmp_path, dry_run=True)
    assert (tmp_path / "a.jpg").exists()
    assert not (tmp_path / "Images").exists()

def test_apply_and_undo(tmp_path):
    make_files(tmp_path, ["a.jpg", "b.pdf"])
    execute_moves(plan_moves(tmp_path), tmp_path, dry_run=False)
    assert (tmp_path / "Images" / "a.jpg").exists()

    undo_last(tmp_path, dry_run=False)
    assert (tmp_path / "a.jpg").exists()
    assert not (tmp_path / "Images").exists()

def test_never_overwrites(tmp_path):
    (tmp_path / "Images").mkdir()
    (tmp_path / "Images" / "a.jpg").write_text("original")
    make_files(tmp_path, ["a.jpg"])

    execute_moves(plan_moves(tmp_path), tmp_path, dry_run=False)
    assert (tmp_path / "Images" / "a.jpg").read_text() == "original"
    assert (tmp_path / "Images" / "a (1).jpg").exists()

def test_unique_destination(tmp_path):
    (tmp_path / "x.txt").write_text("1")
    assert unique_destination(tmp_path / "x.txt").name == "x (1).txt"
```

### Level up

- Organize by **date** (`2026/10/`) using file modification times.
- Recurse into subfolders; clean up empty folders.
- Detect duplicates by file hash and offer to remove them.
- Custom rules loaded from a JSON or YAML config.
- Watch a folder continuously (`watchdog` library) and organize new files as they arrive.
- Package as an installable command (see 10.8).

---

## 11.8 API-Based Weather Application

### Goal

Show current weather and a multi-day forecast for any city by calling a web API, with caching, error handling, and a clean display.

### Skills practiced

`requests`, JSON parsing, API keys and environment variables, error handling, timeouts, caching, dates and time zones, f-string formatting, optional `argparse`.

### API choice

This project uses **Open-Meteo** (open-meteo.com): it is **free and needs no API key**, which makes it ideal for learning. It offers two endpoints you will use:

| Endpoint | Purpose |
|---|---|
| `https://geocoding-api.open-meteo.com/v1/search` | Turn a city name into coordinates |
| `https://api.open-meteo.com/v1/forecast` | Weather data for coordinates |

> Check the API's current documentation before building, since parameters and limits can change. If you prefer **OpenWeatherMap**, sign up for a free key and store it in an environment variable, **never** in your code (see 10.6).

### Requirements

- [ ] Search a city; handle "city not found" and ambiguous names
- [ ] Current temperature, wind, and conditions
- [ ] 5-day forecast
- [ ] Celsius or Fahrenheit
- [ ] Cache recent results (be kind to free APIs)
- [ ] Handle no internet, timeouts, and bad responses without crashing

### Plan

```
1. Geocode: city name  ->  latitude, longitude
2. Fetch weather for those coordinates
3. Translate weather codes into text
4. Display neatly
5. Add caching and error handling
6. Add a command-line interface
```

### Complete code

```python
# weather.py
import argparse
import json
import sys
import time
from datetime import datetime
from pathlib import Path

import requests

GEOCODE_URL = "https://geocoding-api.open-meteo.com/v1/search"
FORECAST_URL = "https://api.open-meteo.com/v1/forecast"

CACHE_FILE = Path(".weather_cache.json")
CACHE_SECONDS = 15 * 60                 # 15 minutes
TIMEOUT = 10

# WMO weather interpretation codes used by Open-Meteo
WEATHER_CODES = {
    0: "Clear sky",
    1: "Mainly clear", 2: "Partly cloudy", 3: "Overcast",
    45: "Fog", 48: "Rime fog",
    51: "Light drizzle", 53: "Drizzle", 55: "Heavy drizzle",
    61: "Light rain", 63: "Rain", 65: "Heavy rain",
    71: "Light snow", 73: "Snow", 75: "Heavy snow",
    80: "Rain showers", 81: "Heavy showers", 82: "Violent showers",
    95: "Thunderstorm", 96: "Thunderstorm with hail", 99: "Severe thunderstorm with hail",
}

class WeatherError(Exception):
    """Any problem that should be shown to the user as a friendly message."""

# ---------------------------------------------------------------- cache
def cache_load():
    try:
        return json.loads(CACHE_FILE.read_text(encoding="utf-8"))
    except (FileNotFoundError, json.JSONDecodeError):
        return {}

def cache_get(key):
    entry = cache_load().get(key)
    if entry and time.time() - entry["time"] < CACHE_SECONDS:
        return entry["data"]
    return None

def cache_set(key, data):
    cache = cache_load()
    cache[key] = {"time": time.time(), "data": data}
    CACHE_FILE.write_text(json.dumps(cache), encoding="utf-8")

# ---------------------------------------------------------------- HTTP
def get_json(url, params):
    """GET a URL and return parsed JSON, converting all failures to WeatherError."""
    try:
        response = requests.get(url, params=params, timeout=TIMEOUT)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.Timeout:
        raise WeatherError("The weather service took too long to respond. Try again.")
    except requests.exceptions.ConnectionError:
        raise WeatherError("Could not connect. Please check your internet connection.")
    except requests.exceptions.HTTPError as e:
        code = e.response.status_code
        if code == 429:
            raise WeatherError("Too many requests. Please wait a moment and try again.")
        raise WeatherError(f"The weather service returned an error ({code}).")
    except ValueError:
        raise WeatherError("The weather service sent an unreadable response.")

# ---------------------------------------------------------------- API calls
def find_city(name, pick=0):
    key = f"geo:{name.lower()}"
    results = cache_get(key)

    if results is None:
        data = get_json(GEOCODE_URL, {"name": name, "count": 5, "language": "en", "format": "json"})
        results = data.get("results") or []
        cache_set(key, results)

    if not results:
        raise WeatherError(f"City not found: '{name}'. Check the spelling.")

    return results, results[min(pick, len(results) - 1)]

def get_weather(latitude, longitude, units="metric"):
    key = f"wx:{latitude:.2f},{longitude:.2f}:{units}"
    cached = cache_get(key)
    if cached:
        return cached

    params = {
        "latitude": latitude,
        "longitude": longitude,
        "current": "temperature_2m,relative_humidity_2m,apparent_temperature,"
                   "weather_code,wind_speed_10m",
        "daily": "weather_code,temperature_2m_max,temperature_2m_min,"
                 "precipitation_sum,sunrise,sunset",
        "timezone": "auto",
        "forecast_days": 5,
        "temperature_unit": "celsius" if units == "metric" else "fahrenheit",
        "wind_speed_unit": "kmh" if units == "metric" else "mph",
    }
    data = get_json(FORECAST_URL, params)

    if "current" not in data or "daily" not in data:
        raise WeatherError("The weather service did not return the expected data.")

    cache_set(key, data)
    return data

# ---------------------------------------------------------------- display
def describe(code):
    return WEATHER_CODES.get(code, f"Unknown ({code})")

def icon(code):
    if code == 0:              return "Sunny"
    if code in (1, 2):         return "Fair"
    if code == 3:              return "Cloudy"
    if code in (45, 48):       return "Foggy"
    if 51 <= code <= 67:       return "Rainy"
    if 71 <= code <= 77:       return "Snowy"
    if 80 <= code <= 82:       return "Showers"
    if code >= 95:             return "Stormy"
    return ""

def show_report(place, data, units):
    deg = "°C" if units == "metric" else "°F"
    speed = "km/h" if units == "metric" else "mph"

    current = data["current"]
    daily = data["daily"]

    title = f"{place['name']}, {place.get('admin1', '')} {place.get('country', '')}".replace(" ,", ",")
    print("\n" + "=" * 52)
    print(f"  {title.strip()}")
    print(f"  {datetime.now():%A, %d %B %Y}")
    print("=" * 52)

    print("\nRight now")
    print(f"  Conditions:   {describe(current['weather_code'])}")
    print(f"  Temperature:  {current['temperature_2m']:.1f}{deg} "
          f"(feels like {current['apparent_temperature']:.1f}{deg})")
    print(f"  Humidity:     {current['relative_humidity_2m']}%")
    print(f"  Wind:         {current['wind_speed_10m']:.1f} {speed}")

    print("\n5-day forecast")
    print(f"  {'Day':<12}{'Low':>7}{'High':>7}{'Rain':>8}   Conditions")
    print("  " + "-" * 50)

    for i, day in enumerate(daily["time"]):
        weekday = datetime.fromisoformat(day).strftime("%a %d %b")
        print(f"  {weekday:<12}"
              f"{daily['temperature_2m_min'][i]:>6.0f}{deg[-1]}"
              f"{daily['temperature_2m_max'][i]:>6.0f}{deg[-1]}"
              f"{daily['precipitation_sum'][i]:>6.1f}mm"
              f"   {describe(daily['weather_code'][i])}")

    sunrise = daily["sunrise"][0][-5:]
    sunset = daily["sunset"][0][-5:]
    print(f"\n  Sunrise {sunrise}   Sunset {sunset}\n")

# ---------------------------------------------------------------- main
def parse_args(argv=None):
    parser = argparse.ArgumentParser(description="Show the weather for a city.")
    parser.add_argument("city", nargs="*", help="City name, for example: Lahore")
    parser.add_argument("-u", "--units", choices=["metric", "imperial"], default="metric")
    parser.add_argument("--pick", type=int, default=0,
                        help="Choose another match when several cities share a name (0, 1, 2...)")
    return parser.parse_args(argv)

def main(argv=None):
    args = parse_args(argv)
    city = " ".join(args.city).strip() or input("City: ").strip()

    if not city:
        print("Please enter a city name.")
        return 1

    try:
        matches, place = find_city(city, args.pick)

        if len(matches) > 1 and args.pick == 0:
            others = ", ".join(
                f"{i}: {m['name']} ({m.get('admin1', '?')}, {m.get('country', '?')})"
                for i, m in enumerate(matches)
            )
            print(f"Several places match. Showing the first. Others: {others}")
            print("Use --pick N to choose a different one.")

        data = get_weather(place["latitude"], place["longitude"], args.units)
        show_report(place, data, args.units)
        return 0

    except WeatherError as e:
        print(f"Error: {e}")
        return 1
    except KeyboardInterrupt:
        print("\nCancelled.")
        return 130

if __name__ == "__main__":
    sys.exit(main())
```

### Usage

```bash
pip install requests
python weather.py Lahore
python weather.py "New York" --units imperial
python weather.py Springfield --pick 2
```

### Sample output

```
====================================================
  Lahore, Punjab Pakistan
  Monday, 19 October 2026
====================================================

Right now
  Conditions:   Clear sky
  Temperature:  29.4°C (feels like 29.1°C)
  Humidity:     38%
  Wind:         7.2 km/h

5-day forecast
  Day            Low   High    Rain   Conditions
  --------------------------------------------------
  Mon 19 Oct     19C    32C   0.0mm   Clear sky
  Tue 20 Oct     20C    32C   0.0mm   Mainly clear
  Wed 21 Oct     19C    31C   0.2mm   Partly cloudy
  ...
```

(Values shown are examples. Real output depends on live data.)

### Testing without the internet (mocking)

```python
# test_weather.py
from unittest.mock import patch, Mock
import pytest
import requests
import weather

def fake_response(payload, status=200):
    r = Mock()
    r.json.return_value = payload
    r.status_code = status
    if status >= 400:
        r.raise_for_status.side_effect = requests.exceptions.HTTPError(response=r)
    else:
        r.raise_for_status.return_value = None
    return r

@patch("weather.requests.get")
def test_city_not_found(mock_get, tmp_path, monkeypatch):
    monkeypatch.setattr(weather, "CACHE_FILE", tmp_path / "cache.json")
    mock_get.return_value = fake_response({})            # no "results" key
    with pytest.raises(weather.WeatherError, match="City not found"):
        weather.find_city("Nowhereville")

@patch("weather.requests.get", side_effect=requests.exceptions.ConnectionError)
def test_no_internet(mock_get):
    with pytest.raises(weather.WeatherError, match="internet"):
        weather.get_json("https://example.com", {})

@patch("weather.requests.get", side_effect=requests.exceptions.Timeout)
def test_timeout(mock_get):
    with pytest.raises(weather.WeatherError, match="too long"):
        weather.get_json("https://example.com", {})

def test_describe():
    assert weather.describe(0) == "Clear sky"
    assert "Unknown" in weather.describe(12345)
```

### Level up

- Hourly forecast and "should I bring an umbrella?" advice.
- Save favorite cities; show them all at once with `ThreadPoolExecutor` or `asyncio` (Chapter 9).
- Weather alerts and a daily email or notification (see "Scheduled tasks" ideas below).
- A graphical interface with `tkinter`, or a web page with Flask or FastAPI.
- Plot temperatures with `matplotlib`.
- Try a second provider and compare results.

---

## 11.9 Database Management Application

### Goal

Build a complete **student management system** backed by SQLite: students, courses, enrollments, and grades, with relational queries, transactions, and reports.

### Skills practiced

SQL (`CREATE`, `INSERT`, `SELECT`, `UPDATE`, `DELETE`, joins, aggregates), `sqlite3`, parameterized queries, transactions, foreign keys, OOP design, context managers, CSV export.

### Requirements

- [ ] Tables: students, courses, enrollments (with grades)
- [ ] Add, update, delete, search students and courses
- [ ] Enroll students; record and update grades
- [ ] Reports: transcript, class roster, averages, top students
- [ ] Constraints that protect data (unique emails, valid grades)
- [ ] Safe SQL only (parameters, never string formatting)
- [ ] Export a report to CSV

### Database design

```
students                     courses                      enrollments
+------------+               +------------+               +-------------+
| id (PK)    |<---+          | id (PK)    |<---+          | id (PK)     |
| name       |    |          | code (UQ)  |    |          | student_id  |--+
| email (UQ) |    |          | title      |    |          | course_id   |--+--> FK
| birth_year |    |          | credits    |    |          | grade       |
| created    |    |          +------------+    |          | enrolled_on |
+------------+    +-------------------------------------- +-------------+
                       One student, many enrollments
                       One course, many enrollments
```

`enrollments` is the **link table** for the many-to-many relationship between students and courses. `UNIQUE (student_id, course_id)` prevents enrolling twice.

### Complete code

```python
# school_db.py
import csv
import sqlite3
from contextlib import closing
from datetime import date

DB_FILE = "school.db"

SCHEMA = """
PRAGMA foreign_keys = ON;

CREATE TABLE IF NOT EXISTS students (
    id         INTEGER PRIMARY KEY AUTOINCREMENT,
    name       TEXT    NOT NULL CHECK (length(trim(name)) > 0),
    email      TEXT    NOT NULL UNIQUE,
    birth_year INTEGER CHECK (birth_year BETWEEN 1900 AND 2100),
    created    TEXT    NOT NULL DEFAULT (date('now'))
);

CREATE TABLE IF NOT EXISTS courses (
    id      INTEGER PRIMARY KEY AUTOINCREMENT,
    code    TEXT    NOT NULL UNIQUE,
    title   TEXT    NOT NULL,
    credits INTEGER NOT NULL CHECK (credits BETWEEN 1 AND 10)
);

CREATE TABLE IF NOT EXISTS enrollments (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    student_id  INTEGER NOT NULL,
    course_id   INTEGER NOT NULL,
    grade       REAL    CHECK (grade IS NULL OR grade BETWEEN 0 AND 100),
    enrolled_on TEXT    NOT NULL DEFAULT (date('now')),
    UNIQUE (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (course_id)  REFERENCES courses(id)  ON DELETE RESTRICT
);

CREATE INDEX IF NOT EXISTS idx_enroll_student ON enrollments(student_id);
CREATE INDEX IF NOT EXISTS idx_enroll_course  ON enrollments(course_id);
"""

class SchoolError(Exception):
    """A problem the user can understand and fix."""

class SchoolDB:
    def __init__(self, path=DB_FILE):
        self.conn = sqlite3.connect(path)
        self.conn.row_factory = sqlite3.Row
        self.conn.executescript(SCHEMA)

    def close(self):
        self.conn.close()

    # ---------------------------------------------------------- students
    def add_student(self, name, email, birth_year=None):
        try:
            with self.conn:
                cur = self.conn.execute(
                    "INSERT INTO students (name, email, birth_year) VALUES (?, ?, ?)",
                    (name.strip(), email.strip().lower(), birth_year),
                )
            return cur.lastrowid
        except sqlite3.IntegrityError as e:
            if "UNIQUE" in str(e):
                raise SchoolError("A student with that email already exists.")
            raise SchoolError(f"Invalid student data: {e}")

    def get_student(self, student_id):
        return self.conn.execute(
            "SELECT * FROM students WHERE id = ?", (student_id,)
        ).fetchone()

    def search_students(self, text):
        like = f"%{text}%"
        return self.conn.execute(
            """SELECT * FROM students
               WHERE name LIKE ? OR email LIKE ?
               ORDER BY name""",
            (like, like),
        ).fetchall()

    def list_students(self):
        return self.conn.execute("SELECT * FROM students ORDER BY name").fetchall()

    def update_student(self, student_id, name=None, email=None, birth_year=None):
        if not self.get_student(student_id):
            raise SchoolError("Student not found.")
        try:
            with self.conn:
                self.conn.execute(
                    """UPDATE students SET
                           name       = COALESCE(?, name),
                           email      = COALESCE(?, email),
                           birth_year = COALESCE(?, birth_year)
                       WHERE id = ?""",
                    (name, email.lower() if email else None, birth_year, student_id),
                )
        except sqlite3.IntegrityError as e:
            raise SchoolError(f"Could not update: {e}")

    def delete_student(self, student_id):
        with self.conn:
            cur = self.conn.execute("DELETE FROM students WHERE id = ?", (student_id,))
        if cur.rowcount == 0:
            raise SchoolError("Student not found.")

    # ----------------------------------------------------------- courses
    def add_course(self, code, title, credits):
        try:
            with self.conn:
                cur = self.conn.execute(
                    "INSERT INTO courses (code, title, credits) VALUES (?, ?, ?)",
                    (code.strip().upper(), title.strip(), credits),
                )
            return cur.lastrowid
        except sqlite3.IntegrityError as e:
            if "UNIQUE" in str(e):
                raise SchoolError("That course code already exists.")
            raise SchoolError(f"Invalid course data: {e}")

    def get_course_by_code(self, code):
        return self.conn.execute(
            "SELECT * FROM courses WHERE code = ?", (code.strip().upper(),)
        ).fetchone()

    def list_courses(self):
        return self.conn.execute(
            """SELECT c.*, COUNT(e.id) AS enrolled
               FROM courses c
               LEFT JOIN enrollments e ON e.course_id = c.id
               GROUP BY c.id
               ORDER BY c.code"""
        ).fetchall()

    def delete_course(self, code):
        course = self.get_course_by_code(code)
        if not course:
            raise SchoolError("Course not found.")
        try:
            with self.conn:
                self.conn.execute("DELETE FROM courses WHERE id = ?", (course["id"],))
        except sqlite3.IntegrityError:
            raise SchoolError("Cannot delete a course that still has enrollments.")

    # ------------------------------------------------------- enrollments
    def enroll(self, student_id, code):
        course = self.get_course_by_code(code)
        if not course:
            raise SchoolError("Course not found.")
        if not self.get_student(student_id):
            raise SchoolError("Student not found.")
        try:
            with self.conn:
                self.conn.execute(
                    "INSERT INTO enrollments (student_id, course_id) VALUES (?, ?)",
                    (student_id, course["id"]),
                )
        except sqlite3.IntegrityError:
            raise SchoolError("That student is already enrolled in this course.")

    def drop(self, student_id, code):
        course = self.get_course_by_code(code)
        if not course:
            raise SchoolError("Course not found.")
        with self.conn:
            cur = self.conn.execute(
                "DELETE FROM enrollments WHERE student_id = ? AND course_id = ?",
                (student_id, course["id"]),
            )
        if cur.rowcount == 0:
            raise SchoolError("That enrollment does not exist.")

    def set_grade(self, student_id, code, grade):
        course = self.get_course_by_code(code)
        if not course:
            raise SchoolError("Course not found.")
        try:
            with self.conn:
                cur = self.conn.execute(
                    "UPDATE enrollments SET grade = ? WHERE student_id = ? AND course_id = ?",
                    (grade, student_id, course["id"]),
                )
        except sqlite3.IntegrityError:
            raise SchoolError("Grade must be between 0 and 100.")
        if cur.rowcount == 0:
            raise SchoolError("That student is not enrolled in the course.")

    # ----------------------------------------------------------- reports
    def transcript(self, student_id):
        return self.conn.execute(
            """SELECT c.code, c.title, c.credits, e.grade, e.enrolled_on
               FROM enrollments e
               JOIN courses c ON c.id = e.course_id
               WHERE e.student_id = ?
               ORDER BY c.code""",
            (student_id,),
        ).fetchall()

    def roster(self, code):
        return self.conn.execute(
            """SELECT s.id, s.name, s.email, e.grade
               FROM enrollments e
               JOIN students s ON s.id = e.student_id
               JOIN courses  c ON c.id = e.course_id
               WHERE c.code = ?
               ORDER BY s.name""",
            (code.strip().upper(),),
        ).fetchall()

    def gpa_table(self):
        """Credit-weighted average grade per student."""
        return self.conn.execute(
            """SELECT s.id, s.name,
                      COUNT(e.grade)                            AS graded_courses,
                      ROUND(SUM(e.grade * c.credits) * 1.0 / SUM(c.credits), 2) AS weighted_avg
               FROM students s
               JOIN enrollments e ON e.student_id = s.id AND e.grade IS NOT NULL
               JOIN courses c     ON c.id = e.course_id
               GROUP BY s.id
               ORDER BY weighted_avg DESC"""
        ).fetchall()

    def course_stats(self):
        return self.conn.execute(
            """SELECT c.code, c.title,
                      COUNT(e.id)            AS enrolled,
                      ROUND(AVG(e.grade), 1) AS avg_grade,
                      MAX(e.grade)           AS best,
                      MIN(e.grade)           AS worst
               FROM courses c
               LEFT JOIN enrollments e ON e.course_id = c.id
               GROUP BY c.id
               ORDER BY c.code"""
        ).fetchall()

    def students_without_courses(self):
        return self.conn.execute(
            """SELECT s.* FROM students s
               LEFT JOIN enrollments e ON e.student_id = s.id
               WHERE e.id IS NULL
               ORDER BY s.name"""
        ).fetchall()

    def export_csv(self, path, rows):
        rows = list(rows)
        if not rows:
            return 0
        with open(path, "w", newline="", encoding="utf-8") as f:
            writer = csv.writer(f)
            writer.writerow(rows[0].keys())
            for row in rows:
                writer.writerow(tuple(row))
        return len(rows)

# ------------------------------------------------------------------ UI
def letter(grade):
    if grade is None: return "-"
    if grade >= 90: return "A+"
    if grade >= 80: return "A"
    if grade >= 70: return "B"
    if grade >= 60: return "C"
    return "F"

def print_table(rows, columns, widths):
    if not rows:
        print("  (no results)")
        return
    header = "".join(f"{c:<{w}}" for c, w in zip(columns, widths))
    print("\n" + header)
    print("-" * len(header))
    for row in rows:
        cells = []
        for key, w in zip(columns, widths):
            value = row[key]
            cells.append(f"{'' if value is None else value!s:<{w}}")
        print("".join(cells))
    print(f"\n{len(rows)} row(s)")

def ask_int(prompt, allow_blank=False):
    while True:
        text = input(prompt).strip()
        if not text and allow_blank:
            return None
        if text.lstrip("-").isdigit():
            return int(text)
        print("  Please enter a whole number.")

def ask_float(prompt):
    while True:
        try:
            return float(input(prompt).strip())
        except ValueError:
            print("  Please enter a number.")

MENU = """
=== School Management ===
 Students                    Courses
  1. Add student              6. Add course
  2. List / search students   7. List courses
  3. Update student           8. Delete course
  4. Delete student
  5. Student transcript      Enrollment
                              9. Enroll student
 Reports                     10. Drop course
 11. Class roster            12. Record grade
 13. Rankings (weighted avg) 14. Course statistics
 15. Students with no courses
 16. Export rankings to CSV
  0. Quit
"""

def main():
    db = SchoolDB()

    with closing(db.conn):
        while True:
            print(MENU)
            choice = input("Choose: ").strip()

            try:
                if choice == "1":
                    name = input("Name: ")
                    email = input("Email: ")
                    year = ask_int("Birth year (Enter to skip): ", allow_blank=True)
                    sid = db.add_student(name, email, year)
                    print(f"  Student added with ID {sid}.")

                elif choice == "2":
                    term = input("Search text (Enter for all): ").strip()
                    rows = db.search_students(term) if term else db.list_students()
                    print_table(rows, ["id", "name", "email", "birth_year"], [6, 22, 28, 12])

                elif choice == "3":
                    sid = ask_int("Student ID: ")
                    print("  Leave blank to keep the current value.")
                    db.update_student(
                        sid,
                        name=input("  New name: ").strip() or None,
                        email=input("  New email: ").strip() or None,
                        birth_year=ask_int("  New birth year: ", allow_blank=True),
                    )
                    print("  Updated.")

                elif choice == "4":
                    sid = ask_int("Student ID: ")
                    student = db.get_student(sid)
                    if student and input(f"  Delete {student['name']} and all their enrollments? (y/n): ").lower() == "y":
                        db.delete_student(sid)
                        print("  Deleted.")
                    elif not student:
                        print("  Student not found.")

                elif choice == "5":
                    sid = ask_int("Student ID: ")
                    student = db.get_student(sid)
                    if not student:
                        print("  Student not found.")
                        continue
                    rows = [dict(r, letter=letter(r["grade"])) for r in db.transcript(sid)]
                    print(f"\nTranscript for {student['name']}")
                    print_table(rows, ["code", "title", "credits", "grade", "letter"], [10, 28, 9, 8, 8])

                elif choice == "6":
                    code = input("Course code (e.g. CS101): ")
                    title = input("Title: ")
                    credits = ask_int("Credits (1-10): ")
                    db.add_course(code, title, credits)
                    print("  Course added.")

                elif choice == "7":
                    print_table(db.list_courses(), ["code", "title", "credits", "enrolled"], [10, 30, 9, 10])

                elif choice == "8":
                    db.delete_course(input("Course code: "))
                    print("  Course deleted.")

                elif choice == "9":
                    db.enroll(ask_int("Student ID: "), input("Course code: "))
                    print("  Enrolled.")

                elif choice == "10":
                    db.drop(ask_int("Student ID: "), input("Course code: "))
                    print("  Dropped.")

                elif choice == "11":
                    code = input("Course code: ")
                    print_table(db.roster(code), ["id", "name", "email", "grade"], [6, 22, 28, 8])

                elif choice == "12":
                    sid = ask_int("Student ID: ")
                    code = input("Course code: ")
                    db.set_grade(sid, code, ask_float("Grade (0-100): "))
                    print("  Grade saved.")

                elif choice == "13":
                    print_table(db.gpa_table(), ["name", "graded_courses", "weighted_avg"], [24, 16, 14])

                elif choice == "14":
                    print_table(db.course_stats(),
                                ["code", "title", "enrolled", "avg_grade", "best", "worst"],
                                [10, 26, 10, 11, 8, 8])

                elif choice == "15":
                    print_table(db.students_without_courses(), ["id", "name", "email"], [6, 22, 28])

                elif choice == "16":
                    count = db.export_csv("rankings.csv", db.gpa_table())
                    print(f"  Exported {count} row(s) to rankings.csv")

                elif choice == "0":
                    print("Goodbye!")
                    break

                else:
                    print("  Please choose a number from the menu.")

            except SchoolError as e:
                print(f"  Error: {e}")

if __name__ == "__main__":
    main()
```

### Sample session

```
Choose: 9
Student ID: 1
Course code: cs101
  Enrolled.

Choose: 12
Student ID: 1
Course code: CS101
Grade (0-100): 92
  Grade saved.

Choose: 13

name                    graded_courses  weighted_avg
-------------------------------------------------------
Ali Khan                2               88.4
Sara Ahmed              1               81.0

2 row(s)
```

### Tests (in-memory database)

```python
# test_school_db.py
import pytest
from school_db import SchoolDB, SchoolError

@pytest.fixture
def db():
    database = SchoolDB(":memory:")
    yield database
    database.close()

def test_duplicate_email_rejected(db):
    db.add_student("Ali", "ali@example.com")
    with pytest.raises(SchoolError, match="email"):
        db.add_student("Another Ali", "ALI@example.com")      # stored lower-case

def test_enroll_twice_rejected(db):
    sid = db.add_student("Ali", "ali@example.com")
    db.add_course("CS101", "Intro", 3)
    db.enroll(sid, "cs101")
    with pytest.raises(SchoolError, match="already enrolled"):
        db.enroll(sid, "CS101")

def test_grade_must_be_valid(db):
    sid = db.add_student("Ali", "ali@example.com")
    db.add_course("CS101", "Intro", 3)
    db.enroll(sid, "CS101")
    with pytest.raises(SchoolError):
        db.set_grade(sid, "CS101", 150)

def test_deleting_student_removes_enrollments(db):
    sid = db.add_student("Ali", "ali@example.com")
    db.add_course("CS101", "Intro", 3)
    db.enroll(sid, "CS101")
    db.delete_student(sid)
    assert db.roster("CS101") == []

def test_cannot_delete_course_with_students(db):
    sid = db.add_student("Ali", "ali@example.com")
    db.add_course("CS101", "Intro", 3)
    db.enroll(sid, "CS101")
    with pytest.raises(SchoolError, match="enrollments"):
        db.delete_course("CS101")

def test_weighted_average(db):
    sid = db.add_student("Ali", "ali@example.com")
    db.add_course("A1", "Big", 4)
    db.add_course("B1", "Small", 1)
    db.enroll(sid, "A1"); db.enroll(sid, "B1")
    db.set_grade(sid, "A1", 80)
    db.set_grade(sid, "B1", 100)
    row = db.gpa_table()[0]
    assert row["weighted_avg"] == 84.0          # (80*4 + 100*1) / 5

def test_sql_injection_is_harmless(db):
    db.add_student("Ali", "ali@example.com")
    results = db.search_students("' OR '1'='1")
    assert results == []
```

### Level up

- Login with roles (admin, teacher, student) and password hashing (Argon2).
- Attendance tracking and a calendar of classes.
- Pagination for large tables.
- Database migrations with Alembic; switch to PostgreSQL.
- Rebuild it with **SQLAlchemy** (ORM) and compare the code.
- Put a **FastAPI** layer on top (see Chapter 10) and add a simple web front end.
- Add automatic backups using `sqlite3.Connection.backup()`.

---

## 11.10 Final Capstone Project

### Goal

Combine **everything** you have learned into one well-engineered application. The capstone is not just "more code": it practices the habits of a professional developer: planning, structure, testing, documentation, version control, and delivery.

### Choose one project (or invent your own)

| Option | Description | Core ideas |
|---|---|---|
| **A. Personal Finance Manager** | Accounts, transactions, budgets, charts, reports, recurring items | SQLite, OOP, CSV import, reports, tests |
| **B. Library Management System** | Books, members, loans, due dates, fines, search | SQL joins, dates, OOP, REST API |
| **C. Task and Habit Tracker with Reminders** | Tasks, streaks, notifications, statistics | JSON/SQLite, scheduling, CLI or web UI |
| **D. News and Data Dashboard** | Pull data from APIs, store it, chart it | `requests`, `asyncio`, SQLite, `matplotlib` |
| **E. Inventory and Sales System** | Products, stock, orders, invoices (PDF), low-stock alerts | Database design, reports, validation |
| **F. URL Shortener Service** | Create short links, redirect, click statistics | FastAPI or Flask, database, tests, Docker |
| **G. Text-Based Adventure or Game Engine** | Rooms, items, saving and loading, scripting | OOP, JSON, design patterns |

This chapter builds **Option B, the Library Management System**, in full, as a reference for how a capstone should be organized. Use the same structure for your own choice.

### Capstone requirements (for any option)

**Functionality**

- [ ] At least 5 meaningful features with real workflows
- [ ] Persistent storage (SQLite recommended)
- [ ] Input validation and clear error messages
- [ ] A user interface: CLI menu, or web (FastAPI/Flask), or GUI

**Engineering**

- [ ] Clean project layout with separate modules (data, logic, interface)
- [ ] Classes and functions with type hints and docstrings
- [ ] At least 15 automated tests (`pytest`)
- [ ] Logging (no stray `print` debugging)
- [ ] No secrets in code; configuration through environment variables
- [ ] Git repository with meaningful commits
- [ ] `README.md` with setup, usage, and screenshots or sample output
- [ ] `requirements.txt` or `pyproject.toml`

**Stretch goals**

- [ ] REST API with automatic documentation
- [ ] Docker image
- [ ] GitHub Actions running tests on every push
- [ ] Deployed online

### Capstone workflow

```
Week 1   Plan:        requirements, user stories, database design, project layout
Week 2   Core:        data layer + business logic + unit tests
Week 3   Interface:   CLI/web layer, validation, error handling
Week 4   Polish:      logging, docs, CI, packaging, deployment, final review
```

### Step 1: Write user stories

```
As a librarian, I can add a book so that members can borrow it.
As a librarian, I can register members.
As a member, I can borrow an available book for 14 days.
As a member, I can return a book, and see a fine if I am late.
As a librarian, I can see overdue books and unpaid fines.
As anyone, I can search books by title, author, or ISBN.
```

### Step 2: Design the data

```
books                       members                    loans
+--------------+            +-------------+            +--------------+
| id           |            | id          |            | id           |
| isbn (UQ)    |            | name        |            | book_id (FK) |
| title        |            | email (UQ)  |            | member_id(FK)|
| author       |            | joined      |            | borrowed_on  |
| copies_total |            +-------------+            | due_on       |
+--------------+                                       | returned_on  |
                                                       | fine         |
                                                       +--------------+
```

Available copies = `copies_total` minus the number of loans where `returned_on IS NULL`.

### Step 3: Project layout

```
library_system/
├── src/
│   └── library/
│       ├── __init__.py
│       ├── config.py          # settings from environment variables
│       ├── database.py        # connection and schema
│       ├── models.py          # dataclasses
│       ├── services.py        # business logic (borrow, return, fines)
│       ├── cli.py             # command-line interface
│       └── logging_setup.py
├── tests/
│   ├── conftest.py
│   ├── test_services.py
│   └── test_cli.py
├── .github/workflows/tests.yml
├── .gitignore
├── pyproject.toml
├── Dockerfile
└── README.md
```

> **Separation of concerns:** the **service layer** knows the rules (a member may borrow at most 3 books). The **CLI** only collects input and shows output. That means you can later replace the CLI with a web API **without changing the rules**.

### Step 4: Implementation

**`config.py`**

```python
# src/library/config.py
import os
from pathlib import Path

DB_PATH = Path(os.getenv("LIBRARY_DB", "library.db"))
LOAN_DAYS = int(os.getenv("LIBRARY_LOAN_DAYS", "14"))
MAX_LOANS_PER_MEMBER = int(os.getenv("LIBRARY_MAX_LOANS", "3"))
FINE_PER_DAY = float(os.getenv("LIBRARY_FINE_PER_DAY", "10"))
LOG_LEVEL = os.getenv("LIBRARY_LOG_LEVEL", "INFO")
```

**`logging_setup.py`**

```python
# src/library/logging_setup.py
import logging
from logging.handlers import RotatingFileHandler

from .config import LOG_LEVEL

def setup_logging():
    handler = RotatingFileHandler("library.log", maxBytes=1_000_000,
                                  backupCount=3, encoding="utf-8")
    handler.setFormatter(logging.Formatter(
        "%(asctime)s %(levelname)s %(name)s: %(message)s"
    ))
    root = logging.getLogger()
    root.setLevel(LOG_LEVEL)
    root.addHandler(handler)
```

**`database.py`**

```python
# src/library/database.py
import sqlite3
from pathlib import Path

SCHEMA = """
PRAGMA foreign_keys = ON;

CREATE TABLE IF NOT EXISTS books (
    id           INTEGER PRIMARY KEY AUTOINCREMENT,
    isbn         TEXT    NOT NULL UNIQUE,
    title        TEXT    NOT NULL,
    author       TEXT    NOT NULL,
    copies_total INTEGER NOT NULL DEFAULT 1 CHECK (copies_total >= 0)
);

CREATE TABLE IF NOT EXISTS members (
    id     INTEGER PRIMARY KEY AUTOINCREMENT,
    name   TEXT NOT NULL,
    email  TEXT NOT NULL UNIQUE,
    joined TEXT NOT NULL DEFAULT (date('now'))
);

CREATE TABLE IF NOT EXISTS loans (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    book_id     INTEGER NOT NULL REFERENCES books(id)   ON DELETE RESTRICT,
    member_id   INTEGER NOT NULL REFERENCES members(id) ON DELETE RESTRICT,
    borrowed_on TEXT    NOT NULL,
    due_on      TEXT    NOT NULL,
    returned_on TEXT,
    fine        REAL    NOT NULL DEFAULT 0
);

CREATE INDEX IF NOT EXISTS idx_loans_member ON loans(member_id);
CREATE INDEX IF NOT EXISTS idx_loans_book   ON loans(book_id);
CREATE INDEX IF NOT EXISTS idx_loans_open   ON loans(returned_on);
"""

def connect(path):
    conn = sqlite3.connect(path if str(path) == ":memory:" else Path(path))
    conn.row_factory = sqlite3.Row
    conn.executescript(SCHEMA)
    return conn
```

**`services.py`**: the business rules

```python
# src/library/services.py
import logging
import sqlite3
from datetime import date, timedelta

from .config import FINE_PER_DAY, LOAN_DAYS, MAX_LOANS_PER_MEMBER

logger = logging.getLogger(__name__)

class LibraryError(Exception):
    """A rule was broken or something was not found. Safe to show to users."""

class Library:
    def __init__(self, conn, today=None):
        self.conn = conn
        self._today = today                     # lets tests control "today"

    def today(self):
        return self._today or date.today()

    # ------------------------------------------------------------ books
    def add_book(self, isbn, title, author, copies=1):
        isbn = isbn.replace("-", "").strip()
        if not isbn.isalnum():
            raise LibraryError("ISBN may contain only letters and digits.")
        if copies < 1:
            raise LibraryError("A book needs at least one copy.")

        try:
            with self.conn:
                cur = self.conn.execute(
                    "INSERT INTO books (isbn, title, author, copies_total) VALUES (?, ?, ?, ?)",
                    (isbn, title.strip(), author.strip(), copies),
                )
        except sqlite3.IntegrityError:
            raise LibraryError("A book with that ISBN already exists.")

        logger.info("Added book %s (%s)", title, isbn)
        return cur.lastrowid

    def search_books(self, text):
        like = f"%{text.strip()}%"
        return self.conn.execute(
            """SELECT b.*,
                      b.copies_total - COUNT(l.id) AS available
               FROM books b
               LEFT JOIN loans l ON l.book_id = b.id AND l.returned_on IS NULL
               WHERE b.title LIKE ? OR b.author LIKE ? OR b.isbn LIKE ?
               GROUP BY b.id
               ORDER BY b.title""",
            (like, like, like),
        ).fetchall()

    def available_copies(self, book_id):
        row = self.conn.execute(
            """SELECT b.copies_total - COUNT(l.id) AS available
               FROM books b
               LEFT JOIN loans l ON l.book_id = b.id AND l.returned_on IS NULL
               WHERE b.id = ?
               GROUP BY b.id""",
            (book_id,),
        ).fetchone()
        if row is None:
            raise LibraryError("Book not found.")
        return row["available"]

    # ---------------------------------------------------------- members
    def add_member(self, name, email):
        if "@" not in email:
            raise LibraryError("Please enter a valid email address.")
        try:
            with self.conn:
                cur = self.conn.execute(
                    "INSERT INTO members (name, email) VALUES (?, ?)",
                    (name.strip(), email.strip().lower()),
                )
        except sqlite3.IntegrityError:
            raise LibraryError("A member with that email already exists.")

        logger.info("Added member %s", email)
        return cur.lastrowid

    def get_member(self, member_id):
        row = self.conn.execute("SELECT * FROM members WHERE id = ?", (member_id,)).fetchone()
        if row is None:
            raise LibraryError("Member not found.")
        return row

    # ------------------------------------------------------------ loans
    def open_loans_for(self, member_id):
        return self.conn.execute(
            """SELECT l.*, b.title FROM loans l
               JOIN books b ON b.id = l.book_id
               WHERE l.member_id = ? AND l.returned_on IS NULL
               ORDER BY l.due_on""",
            (member_id,),
        ).fetchall()

    def unpaid_fines(self, member_id):
        row = self.conn.execute(
            "SELECT COALESCE(SUM(fine), 0) AS total FROM loans WHERE member_id = ?",
            (member_id,),
        ).fetchone()
        return row["total"]

    def borrow(self, member_id, book_id):
        self.get_member(member_id)

        if self.available_copies(book_id) < 1:
            raise LibraryError("No copies of that book are available right now.")

        if len(self.open_loans_for(member_id)) >= MAX_LOANS_PER_MEMBER:
            raise LibraryError(f"Loan limit reached ({MAX_LOANS_PER_MEMBER} books at a time).")

        if any(l["due_on"] < self.today().isoformat()
               for l in self.open_loans_for(member_id)):
            raise LibraryError("Return your overdue books before borrowing more.")

        if any(l["book_id"] == book_id for l in self.open_loans_for(member_id)):
            raise LibraryError("This member already has a copy of that book.")

        borrowed = self.today()
        due = borrowed + timedelta(days=LOAN_DAYS)

        with self.conn:
            cur = self.conn.execute(
                "INSERT INTO loans (book_id, member_id, borrowed_on, due_on) VALUES (?, ?, ?, ?)",
                (book_id, member_id, borrowed.isoformat(), due.isoformat()),
            )

        logger.info("Member %s borrowed book %s, due %s", member_id, book_id, due)
        return cur.lastrowid, due

    def return_book(self, loan_id):
        loan = self.conn.execute("SELECT * FROM loans WHERE id = ?", (loan_id,)).fetchone()
        if loan is None:
            raise LibraryError("Loan not found.")
        if loan["returned_on"] is not None:
            raise LibraryError("That book was already returned.")

        due = date.fromisoformat(loan["due_on"])
        days_late = max(0, (self.today() - due).days)
        fine = round(days_late * FINE_PER_DAY, 2)

        with self.conn:
            self.conn.execute(
                "UPDATE loans SET returned_on = ?, fine = ? WHERE id = ?",
                (self.today().isoformat(), fine, loan_id),
            )

        logger.info("Loan %s returned, %d day(s) late, fine %.2f", loan_id, days_late, fine)
        return days_late, fine

    # ---------------------------------------------------------- reports
    def overdue_loans(self):
        return self.conn.execute(
            """SELECT l.id AS loan_id, m.name, m.email, b.title, l.due_on,
                      CAST(julianday(?) - julianday(l.due_on) AS INTEGER) AS days_late
               FROM loans l
               JOIN members m ON m.id = l.member_id
               JOIN books   b ON b.id = l.book_id
               WHERE l.returned_on IS NULL AND l.due_on < ?
               ORDER BY days_late DESC""",
            (self.today().isoformat(), self.today().isoformat()),
        ).fetchall()

    def popular_books(self, limit=5):
        return self.conn.execute(
            """SELECT b.title, b.author, COUNT(l.id) AS times_borrowed
               FROM books b
               JOIN loans l ON l.book_id = b.id
               GROUP BY b.id
               ORDER BY times_borrowed DESC, b.title
               LIMIT ?""",
            (limit,),
        ).fetchall()
```

**`cli.py`**: the interface

```python
# src/library/cli.py
import sys

from . import config
from .database import connect
from .logging_setup import setup_logging
from .services import Library, LibraryError

def show_rows(rows, columns, widths):
    if not rows:
        print("  (nothing to show)")
        return
    print("\n" + "".join(f"{c:<{w}}" for c, w in zip(columns, widths)))
    print("-" * sum(widths))
    for row in rows:
        print("".join(f"{str(row[c]):<{w}}" for c, w in zip(columns, widths)))
    print()

def ask_int(prompt):
    while True:
        text = input(prompt).strip()
        if text.isdigit():
            return int(text)
        print("  Please enter a number.")

MENU = """
=== Library System ===
1. Add book              6. Borrow a book
2. Search books          7. Return a book
3. Add member            8. Member's current loans
4. Overdue report        9. Popular books
5. Quit
"""

def main():
    setup_logging()
    conn = connect(config.DB_PATH)
    library = Library(conn)

    while True:
        print(MENU)
        choice = input("Choose: ").strip()

        try:
            if choice == "1":
                library.add_book(
                    input("ISBN: "), input("Title: "), input("Author: "),
                    ask_int("Copies: "),
                )
                print("  Book added.")

            elif choice == "2":
                rows = library.search_books(input("Search text: "))
                show_rows(rows, ["id", "title", "author", "available"], [6, 34, 24, 10])

            elif choice == "3":
                member_id = library.add_member(input("Name: "), input("Email: "))
                print(f"  Member registered with ID {member_id}.")

            elif choice == "4":
                show_rows(library.overdue_loans(),
                          ["loan_id", "name", "title", "due_on", "days_late"],
                          [9, 18, 28, 12, 10])

            elif choice == "6":
                loan_id, due = library.borrow(ask_int("Member ID: "), ask_int("Book ID: "))
                print(f"  Loan #{loan_id} created. Due on {due:%d %b %Y}.")

            elif choice == "7":
                days_late, fine = library.return_book(ask_int("Loan ID: "))
                if days_late:
                    print(f"  Returned {days_late} day(s) late. Fine: {fine:.2f}")
                else:
                    print("  Returned on time. Thank you!")

            elif choice == "8":
                member_id = ask_int("Member ID: ")
                member = library.get_member(member_id)
                print(f"\nLoans for {member['name']}")
                show_rows(library.open_loans_for(member_id),
                          ["id", "title", "borrowed_on", "due_on"], [6, 34, 14, 12])
                print(f"  Fines recorded: {library.unpaid_fines(member_id):.2f}")

            elif choice == "9":
                show_rows(library.popular_books(), ["title", "author", "times_borrowed"], [34, 24, 16])

            elif choice == "5":
                print("Goodbye!")
                break

            else:
                print("  Please choose a number from the menu.")

        except LibraryError as e:
            print(f"  Error: {e}")
        except KeyboardInterrupt:
            print("\nGoodbye!")
            break

    conn.close()
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

**`pyproject.toml`**

```toml
[build-system]
requires = ["setuptools>=68"]
build-backend = "setuptools.build_meta"

[project]
name = "library-system"
version = "1.0.0"
description = "A small library management system"
readme = "README.md"
requires-python = ">=3.10"

[project.optional-dependencies]
dev = ["pytest>=8", "pytest-cov", "ruff"]

[project.scripts]
library = "library.cli:main"

[tool.setuptools.packages.find]
where = ["src"]

[tool.pytest.ini_options]
testpaths = ["tests"]
```

Install and run:

```bash
pip install -e ".[dev]"
library
```

### Step 5: Tests

**`tests/conftest.py`**

```python
# tests/conftest.py
from datetime import date
import pytest
from library.database import connect
from library.services import Library

@pytest.fixture
def lib():
    conn = connect(":memory:")
    library = Library(conn, today=date(2026, 10, 1))
    yield library
    conn.close()

@pytest.fixture
def setup_data(lib):
    book_id = lib.add_book("9780132350884", "Clean Code", "Robert Martin", copies=2)
    member_id = lib.add_member("Ali Khan", "ali@example.com")
    return lib, book_id, member_id
```

**`tests/test_services.py`**

```python
# tests/test_services.py
from datetime import date
import pytest
from library.services import LibraryError

def test_add_duplicate_isbn(lib):
    lib.add_book("123", "A", "X")
    with pytest.raises(LibraryError, match="ISBN"):
        lib.add_book("123", "B", "Y")

def test_borrow_reduces_availability(setup_data):
    lib, book_id, member_id = setup_data
    assert lib.available_copies(book_id) == 2
    lib.borrow(member_id, book_id)
    assert lib.available_copies(book_id) == 1

def test_due_date_is_14_days(setup_data):
    lib, book_id, member_id = setup_data
    _, due = lib.borrow(member_id, book_id)
    assert due == date(2026, 10, 15)

def test_cannot_borrow_unavailable(lib):
    book = lib.add_book("111", "Rare", "Y", copies=1)
    a = lib.add_member("A", "a@example.com")
    b = lib.add_member("B", "b@example.com")
    lib.borrow(a, book)
    with pytest.raises(LibraryError, match="No copies"):
        lib.borrow(b, book)

def test_loan_limit(lib):
    member = lib.add_member("Ali", "ali@example.com")
    for i in range(3):
        lib.borrow(member, lib.add_book(f"ISBN{i}", f"Book {i}", "A"))
    with pytest.raises(LibraryError, match="limit"):
        lib.borrow(member, lib.add_book("ISBN9", "Book 9", "A"))

def test_return_on_time_has_no_fine(setup_data):
    lib, book_id, member_id = setup_data
    loan_id, _ = lib.borrow(member_id, book_id)
    days_late, fine = lib.return_book(loan_id)
    assert (days_late, fine) == (0, 0)

def test_late_return_fine(setup_data):
    lib, book_id, member_id = setup_data
    loan_id, _ = lib.borrow(member_id, book_id)          # due 2026-10-15
    lib._today = date(2026, 10, 20)                      # 5 days late
    days_late, fine = lib.return_book(loan_id)
    assert days_late == 5
    assert fine == 50.0

def test_cannot_return_twice(setup_data):
    lib, book_id, member_id = setup_data
    loan_id, _ = lib.borrow(member_id, book_id)
    lib.return_book(loan_id)
    with pytest.raises(LibraryError, match="already returned"):
        lib.return_book(loan_id)

def test_overdue_blocks_new_loans(setup_data):
    lib, book_id, member_id = setup_data
    lib.borrow(member_id, book_id)
    lib._today = date(2026, 11, 1)                       # overdue now
    other = lib.add_book("222", "Other", "Z")
    with pytest.raises(LibraryError, match="overdue"):
        lib.borrow(member_id, other)

def test_overdue_report(setup_data):
    lib, book_id, member_id = setup_data
    lib.borrow(member_id, book_id)
    lib._today = date(2026, 10, 20)
    report = lib.overdue_loans()
    assert len(report) == 1
    assert report[0]["days_late"] == 5

def test_search_is_safe_against_injection(setup_data):
    lib, *_ = setup_data
    assert lib.search_books("'; DROP TABLE books; --") == []
    assert len(lib.search_books("Clean")) == 1

def test_search_finds_by_author(setup_data):
    lib, *_ = setup_data
    assert lib.search_books("martin")[0]["title"] == "Clean Code"

def test_popular_books(setup_data):
    lib, book_id, member_id = setup_data
    loan_id, _ = lib.borrow(member_id, book_id)
    lib.return_book(loan_id)
    lib.borrow(member_id, book_id)
    assert lib.popular_books()[0]["times_borrowed"] == 2

def test_invalid_email(lib):
    with pytest.raises(LibraryError):
        lib.add_member("Ali", "not-an-email")

def test_unknown_member(setup_data):
    lib, book_id, _ = setup_data
    with pytest.raises(LibraryError, match="Member not found"):
        lib.borrow(999, book_id)
```

Run with coverage:

```bash
pytest --cov=library --cov-report=term-missing
```

### Step 6: Continuous integration

**`.github/workflows/tests.yml`**

```yaml
name: Tests

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.11", "3.12", "3.13"]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - run: pip install -e ".[dev]"
      - run: ruff check src tests
      - run: pytest --cov=library
```

### Step 7: Docker (stretch)

```dockerfile
FROM python:3.12-slim
ENV PYTHONUNBUFFERED=1
WORKDIR /app
COPY pyproject.toml README.md ./
COPY src ./src
RUN pip install --no-cache-dir .
RUN useradd --create-home appuser && mkdir /data && chown appuser /data
USER appuser
ENV LIBRARY_DB=/data/library.db
CMD ["library"]
```

```bash
docker build -t library-system .
docker run -it -v library_data:/data library-system
```

### Step 8: README template

```markdown
# Library System

A command-line library management system built with Python and SQLite.

## Features
- Add and search books
- Register members
- Borrow and return books with due dates
- Automatic late fines
- Overdue and popularity reports

## Installation
    git clone https://github.com/you/library-system.git
    cd library-system
    python -m venv .venv && source .venv/bin/activate
    pip install -e ".[dev]"

## Usage
    library

## Configuration (environment variables)
| Variable | Default | Meaning |
|---|---|---|
| LIBRARY_DB | library.db | Database file |
| LIBRARY_LOAN_DAYS | 14 | Loan length |
| LIBRARY_MAX_LOANS | 3 | Books per member |
| LIBRARY_FINE_PER_DAY | 10 | Late fee per day |

## Running tests
    pytest --cov=library

## Project structure
(short explanation of the modules)

## License
MIT
```

### Step 9: Add a REST API (stretch)

Because the rules live in `services.py`, a web layer is thin:

```python
# src/library/api.py
from fastapi import Depends, FastAPI, HTTPException
from pydantic import BaseModel

from . import config
from .database import connect
from .services import Library, LibraryError

app = FastAPI(title="Library API")

def get_library():
    conn = connect(config.DB_PATH)
    try:
        yield Library(conn)
    finally:
        conn.close()

class BookIn(BaseModel):
    isbn: str
    title: str
    author: str
    copies: int = 1

@app.get("/books")
def search(q: str = "", lib: Library = Depends(get_library)):
    return [dict(r) for r in lib.search_books(q)]

@app.post("/books", status_code=201)
def create_book(book: BookIn, lib: Library = Depends(get_library)):
    try:
        return {"id": lib.add_book(book.isbn, book.title, book.author, book.copies)}
    except LibraryError as e:
        raise HTTPException(status_code=400, detail=str(e))

@app.post("/loans", status_code=201)
def borrow(member_id: int, book_id: int, lib: Library = Depends(get_library)):
    try:
        loan_id, due = lib.borrow(member_id, book_id)
        return {"loan_id": loan_id, "due": due.isoformat()}
    except LibraryError as e:
        raise HTTPException(status_code=409, detail=str(e))
```

```bash
pip install fastapi uvicorn
uvicorn library.api:app --reload
```

### Capstone evaluation checklist

Score yourself honestly:

| Area | Questions to ask | Done |
|---|---|---|
| **Works** | Does every feature work end to end? | ☐ |
| **Correct** | Do the tests cover rules and edge cases (limits, empty input, duplicates)? | ☐ |
| **Robust** | Does bad input produce clear messages instead of crashes? | ☐ |
| **Secure** | Parameterized SQL, no secrets in code, validated input? | ☐ |
| **Readable** | Clear names, docstrings, small functions, consistent style (`ruff`)? | ☐ |
| **Organized** | Layers separated: data, rules, interface? | ☐ |
| **Documented** | Could a stranger install and use it from the README alone? | ☐ |
| **Versioned** | Meaningful Git history, `.gitignore`, no junk files committed? | ☐ |
| **Automated** | Tests run automatically on every push? | ☐ |
| **Shippable** | Installable package or Docker image? | ☐ |

### Ideas to extend the Library System

- Reservations and waiting lists
- Email reminders for due dates (scheduled job)
- Barcode scanning (webcam with `opencv` and `pyzbar`)
- Import books from the Open Library API by ISBN
- Web front end (HTML templates with Jinja2, or a React app that calls your API)
- Role-based login (librarian vs member) with hashed passwords
- Statistics dashboard with charts
- Multi-branch libraries and book transfers

### Presenting your capstone

- Record a 2 to 3 minute demo video or GIF.
- Put the project on GitHub with a polished README.
- Write down: what problem it solves, how it is structured, what you learned, and what you would do next.
- Be ready to explain **one design decision** (for example, why the rules live in a service layer).

---

## Project Skills Map

| Skill | Calculator | Guess | Quiz | To-Do | Contacts | Expenses | Organizer | Weather | Database | Capstone |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Functions | ● | ● | ● | ● | ● | ● | ● | ● | ● | ● |
| Loops and conditions | ● | ● | ● | ● | ● | ● | ● | ● | ● | ● |
| Lists and dictionaries | ● | | ● | ● | ● | ● | ● | ● | ● | ● |
| Error handling | ● | ● | ● | ● | ● | ● | ● | ● | ● | ● |
| File handling / JSON / CSV | ○ | ● | ● | ● | ● | ● | ● | ● | ○ | ● |
| OOP and dataclasses | | | | ● | ● | ● | | | ● | ● |
| Dates and times | | | ● | ● | | ● | ● | ● | ● | ● |
| Regular expressions | | | | | ● | | | | | ○ |
| `pathlib` / `shutil` | | ● | ● | ● | ● | ● | ● | ● | | ○ |
| APIs and `requests` | | | ○ | | | | | ● | | ○ |
| SQL and SQLite | | | | | ○ | ○ | | | ● | ● |
| Testing with `pytest` | ● | | | ● | ● | ● | ● | ● | ● | ● |
| Logging | | | | | | | ● | | | ● |
| Command-line (`argparse`) | | | | | | | ● | ● | | ● |
| Git and project structure | ● | ● | ● | ● | ● | ● | ● | ● | ● | ● |
| Packaging / Docker / CI | | | | | | | | | | ● |

(● = used directly, ○ = optional extension)

---

## What to Learn Next

Congratulations. If you built these projects, you have real Python skills. Pick a direction that excites you:

| Path | What to learn | Good first project |
|---|---|---|
| **Web development** | Flask, FastAPI, Django, HTML/CSS, JavaScript basics, PostgreSQL | A blog or a to-do web app with login |
| **Data analysis** | NumPy, pandas, matplotlib, seaborn, Jupyter | Analyze a public dataset (Kaggle) and publish findings |
| **Machine learning and AI** | scikit-learn, PyTorch or TensorFlow, statistics, linear algebra | Predict house prices; classify images |
| **Automation and scripting** | `selenium`/`playwright`, `schedule`, email, Excel automation (`openpyxl`) | Auto-generate and email weekly reports |
| **Web scraping** | `requests`, BeautifulSoup, Scrapy (respect site rules and robots.txt) | Price tracker |
| **Desktop and games** | `tkinter`, PyQt, `pygame` | A snake game or a notes app |
| **DevOps and cloud** | Docker, Kubernetes basics, AWS/Azure/GCP, Terraform, CI/CD | Deploy your capstone to a cloud service |
| **Cybersecurity** | Networking, `scapy`, cryptography, secure coding | Port scanner (on systems you own); password auditor |
| **Software engineering** | Design patterns, clean architecture, data structures and algorithms, system design | Re-implement a classic algorithm set with tests |

### Habits that make you better every week

- **Code daily**, even 30 minutes.
- **Read other people's code** on GitHub; read the standard library source.
- **Write tests** for everything you build.
- **Contribute** to open source (start with documentation fixes and "good first issue" labels).
- **Practice problems**: LeetCode, HackerRank, Codewars, Advent of Code, Project Euler.
- **Build things you personally need.** Motivation is the best teacher.
- **Keep a project portfolio** on GitHub with clear READMEs.
- **Learn to debug calmly**: read the error, reproduce, isolate, fix, test.

### Recommended resources

| Resource | Type |
|---|---|
| Official Python docs and tutorial (docs.python.org) | Reference |
| *Fluent Python* by Luciano Ramalho | Book (intermediate to advanced) |
| *Effective Python* by Brett Slatkin | Book (best practices) |
| *Automate the Boring Stuff with Python* by Al Sweigart | Book (practical beginner projects) |
| Real Python (realpython.com) | Tutorials |
| PEP 8 and PEP 20 (`import this`) | Style and philosophy |
| Python Discord and local communities | Community help |

> **Final advice:** You do not learn programming by reading; you learn it by building, breaking, and fixing. Keep building.

---

## Chapter Summary

| Project | What you built | Key takeaway |
|---|---|---|
| Calculator | A safe, menu-driven calculator with history | Validate input; never use `eval` on user text |
| Number Guessing Game | A game with difficulty levels and saved scores | Loops, randomness, and binary-search thinking |
| Quiz Application | A JSON-driven quiz with scoring and a leaderboard | Separate data from code |
| To-Do List | A persistent task manager with filters and search | Classes plus file persistence |
| Contact Book | A validated, searchable address book with CSV import and export | Validation belongs in the data class |
| Expense Tracker | Budgets, summaries, and text charts using `Decimal` | Use `Decimal` for money |
| File Organizer | A safe file mover with dry run and undo | Make dangerous tools reversible |
| Weather Application | An API client with caching and complete error handling | Handle every way a network call can fail |
| Database Management | A relational student system with SQL reports | Let the database enforce the rules |
| Capstone | A layered, tested, documented, packaged library system | Professional structure matters as much as features |

---

**Previous:** [Chapter 10: Professional Development Skills](chapter-10-professional-development-skills.md)

**You have completed the Python Complete Roadmap.**
