# Chapter 10: Professional Development Skills

> Writing code that works is only the start. Professional developers also test their code, track changes with Git, talk to APIs and databases, log what happens, keep software secure, make it fast, and package it so others can install and run it. This chapter covers those skills.

## Table of Contents

- [10.1 Testing (`unittest` / `pytest`)](#101-testing-unittest--pytest)
- [10.2 Git and GitHub](#102-git-and-github)
- [10.3 Working with APIs](#103-working-with-apis)
- [10.4 SQL and Databases](#104-sql-and-databases)
- [10.5 Logging](#105-logging)
- [10.6 Security Best Practices](#106-security-best-practices)
- [10.7 Performance Optimization](#107-performance-optimization)
- [10.8 Packaging and Deployment](#108-packaging-and-deployment)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 10.1 Testing (`unittest` / `pytest`)

**Testing** means writing code that checks your other code. Tests catch bugs early, protect against regressions (old bugs returning), and let you change code with confidence.

### Types of tests

| Type | Checks | Speed |
|---|---|---|
| **Unit test** | One small piece (a function or class) | Very fast |
| **Integration test** | Several pieces working together (code + database) | Medium |
| **End-to-end test** | The whole application from the user's side | Slow |

A healthy project has **many unit tests**, **some integration tests**, and **few end-to-end tests**.

### Code to test

Save this as `calculator.py`:

```python
# calculator.py
def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

def is_even(n):
    return n % 2 == 0
```

### Testing with `assert` (the simplest test)

```python
from calculator import add

assert add(2, 3) == 5
assert add(-1, 1) == 0
print("All good")
```

If an assertion is false, Python raises `AssertionError`. Real test frameworks give much better reports and organization.

---

### Part A: `unittest` (built in)

Python's standard testing framework. Nothing to install.

#### Your first test file

Save as `test_calculator.py`:

```python
import unittest
from calculator import add, divide, is_even

class TestCalculator(unittest.TestCase):

    def test_add_positive(self):
        self.assertEqual(add(2, 3), 5)

    def test_add_negative(self):
        self.assertEqual(add(-1, -1), -2)

    def test_divide(self):
        self.assertEqual(divide(10, 2), 5)

    def test_divide_by_zero(self):
        with self.assertRaises(ValueError):
            divide(1, 0)

    def test_is_even(self):
        self.assertTrue(is_even(4))
        self.assertFalse(is_even(5))

if __name__ == "__main__":
    unittest.main()
```

#### Running the tests

```bash
python -m unittest                          # discover and run all tests
python -m unittest test_calculator          # one module
python -m unittest test_calculator.TestCalculator.test_add_positive
python -m unittest -v                       # verbose
python -m unittest discover -s tests        # look in the tests folder
```

Sample output:

```
.....
----------------------------------------------------------------------
Ran 5 tests in 0.001s

OK
```

A failing test shows what went wrong:

```
FAIL: test_add_positive (test_calculator.TestCalculator)
AssertionError: 6 != 5
```

#### Rules for `unittest`

- Test classes inherit from `unittest.TestCase`.
- Test method names **must start with `test`**.
- Test files are normally named `test_*.py`.

#### Common assertion methods

| Method | Checks |
|---|---|
| `assertEqual(a, b)` | `a == b` |
| `assertNotEqual(a, b)` | `a != b` |
| `assertTrue(x)` / `assertFalse(x)` | Truthiness |
| `assertIs(a, b)` / `assertIsNone(x)` | Identity |
| `assertIn(a, b)` / `assertNotIn(a, b)` | Membership |
| `assertIsInstance(a, T)` | Type |
| `assertAlmostEqual(a, b, places=7)` | Floats nearly equal |
| `assertGreater(a, b)` / `assertLess(a, b)` | Ordering |
| `assertListEqual(a, b)` / `assertDictEqual(a, b)` | Collections |
| `assertRaises(Error)` | An exception is raised |
| `assertRaisesRegex(Error, "text")` | Exception with matching message |
| `assertLogs()` | Something was logged |

```python
def test_float(self):
    self.assertAlmostEqual(0.1 + 0.2, 0.3)

def test_error_message(self):
    with self.assertRaisesRegex(ValueError, "zero"):
        divide(1, 0)
```

#### `setUp` and `tearDown`

Run **before and after every test**.

```python
class TestAccount(unittest.TestCase):

    def setUp(self):
        self.account = BankAccount("Ali", 100)

    def tearDown(self):
        pass    # clean up resources

    def test_deposit(self):
        self.account.deposit(50)
        self.assertEqual(self.account.balance, 150)

    def test_withdraw(self):
        self.account.withdraw(30)
        self.assertEqual(self.account.balance, 70)
```

Each test gets a **fresh** `self.account`, so tests do not affect each other.

Class-level and module-level versions:

```python
@classmethod
def setUpClass(cls):       # once before all tests in the class
    cls.db = connect()

@classmethod
def tearDownClass(cls):    # once after all tests in the class
    cls.db.close()
```

#### Skipping and expected failures

```python
import sys

class TestExamples(unittest.TestCase):

    @unittest.skip("Not implemented yet")
    def test_future(self):
        ...

    @unittest.skipIf(sys.platform == "win32", "Unix only")
    def test_unix(self):
        ...

    @unittest.expectedFailure
    def test_known_bug(self):
        self.assertEqual(1, 2)
```

#### Subtests

```python
def test_is_even_many(self):
    for n in [2, 4, 6, 8]:
        with self.subTest(n=n):
            self.assertTrue(is_even(n))
```

---

### Part B: `pytest` (the popular choice)

`pytest` is shorter, more powerful, and has better error messages.

```bash
pip install pytest
```

#### Writing tests

Plain functions and plain `assert`. No classes needed.

```python
# test_calculator.py
import pytest
from calculator import add, divide, is_even

def test_add():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -1) == -2

def test_divide():
    assert divide(10, 2) == 5

def test_divide_by_zero():
    with pytest.raises(ValueError, match="zero"):
        divide(1, 0)

def test_is_even():
    assert is_even(4)
    assert not is_even(5)
```

#### Running `pytest`

```bash
pytest                           # run everything
pytest test_calculator.py        # one file
pytest tests/                    # one folder
pytest -v                        # verbose
pytest -q                        # quiet
pytest -x                        # stop at first failure
pytest -k "add"                  # only tests with "add" in the name
pytest -m slow                   # only tests marked "slow"
pytest --lf                      # re-run last failures
pytest -s                        # show print() output
pytest --maxfail=3
```

#### Rules for `pytest`

- Files: `test_*.py` or `*_test.py`
- Functions: names start with `test_`
- Classes: names start with `Test` (no `__init__`)

#### Helpful failure output

```python
def test_lists():
    assert [1, 2, 3] == [1, 2, 4]
```

```
>       assert [1, 2, 3] == [1, 2, 4]
E       At index 2 diff: 3 != 4
```

#### Floats

```python
import pytest

def test_float():
    assert 0.1 + 0.2 == pytest.approx(0.3)
    assert 100 == pytest.approx(101, rel=0.02)    # within 2%
```

#### Parametrized tests

Run one test with many inputs.

```python
import pytest

@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (0, 0, 0),
    (-1, 1, 0),
    (2.5, 2.5, 5.0),
])
def test_add_many(a, b, expected):
    assert add(a, b) == expected

@pytest.mark.parametrize("n, expected", [(2, True), (3, False), (0, True)])
def test_is_even(n, expected):
    assert is_even(n) is expected
```

Each row appears as a separate test in the report.

#### Fixtures

A **fixture** prepares data or resources for tests.

```python
import pytest

@pytest.fixture
def account():
    return BankAccount("Ali", 100)

def test_deposit(account):
    account.deposit(50)
    assert account.balance == 150

def test_withdraw(account):
    account.withdraw(30)
    assert account.balance == 70
```

The test asks for a fixture by naming it as a parameter.

**Setup and teardown with `yield`**

```python
@pytest.fixture
def temp_file(tmp_path):
    path = tmp_path / "data.txt"
    path.write_text("hello")
    yield path                 # the test runs here
    # cleanup code after the test goes here

def test_read(temp_file):
    assert temp_file.read_text() == "hello"
```

**Fixture scopes**

| Scope | Runs once per |
|---|---|
| `function` (default) | Test |
| `class` | Test class |
| `module` | File |
| `session` | Whole test run |

```python
@pytest.fixture(scope="session")
def database():
    db = connect()
    yield db
    db.close()
```

**`conftest.py`**: put shared fixtures here and every test in the folder can use them without importing.

#### Built-in fixtures

| Fixture | Purpose |
|---|---|
| `tmp_path` | A temporary folder (`pathlib.Path`) |
| `capsys` | Capture printed output |
| `monkeypatch` | Temporarily change objects, environment variables |
| `caplog` | Capture log messages |
| `request` | Information about the current test |

```python
def test_output(capsys):
    print("hello")
    captured = capsys.readouterr()
    assert captured.out == "hello\n"

def test_env(monkeypatch):
    monkeypatch.setenv("API_KEY", "test-key")
    import os
    assert os.environ["API_KEY"] == "test-key"
```

#### Markers

```python
import pytest

@pytest.mark.skip(reason="Not ready")
def test_a(): ...

@pytest.mark.skipif(sys.version_info < (3, 12), reason="Needs 3.12")
def test_b(): ...

@pytest.mark.xfail(reason="Known bug")
def test_c(): ...

@pytest.mark.slow
def test_d(): ...
```

Register custom markers in `pytest.ini`:

```ini
[pytest]
markers =
    slow: tests that take a long time
```

#### Grouping tests in a class

```python
class TestBankAccount:
    def test_deposit(self):
        acc = BankAccount("Ali", 100)
        acc.deposit(50)
        assert acc.balance == 150

    def test_overdraft(self):
        acc = BankAccount("Ali", 100)
        with pytest.raises(ValueError):
            acc.withdraw(500)
```

#### Project layout

```
my_project/
├── src/
│   └── mypackage/
│       ├── __init__.py
│       └── calculator.py
├── tests/
│   ├── conftest.py
│   ├── test_calculator.py
│   └── test_accounts.py
├── pyproject.toml
└── pytest.ini
```

**`pytest.ini`**

```ini
[pytest]
testpaths = tests
addopts = -ra -q
```

---

### Mocking: replacing real dependencies

Tests should not send real emails, hit real websites, or use the real clock. **Mocks** stand in for those.

```python
# weather.py
import requests

def get_temperature(city):
    r = requests.get(f"https://api.example.com/weather?q={city}", timeout=5)
    r.raise_for_status()
    return r.json()["temp"]
```

```python
# test_weather.py
from unittest.mock import patch, Mock
from weather import get_temperature

@patch("weather.requests.get")
def test_get_temperature(mock_get):
    mock_get.return_value = Mock(
        status_code=200,
        json=lambda: {"temp": 31},
        raise_for_status=lambda: None,
    )

    assert get_temperature("Lahore") == 31
    mock_get.assert_called_once()
```

> Patch the name **where it is used** (`weather.requests.get`), not where it is defined.

#### Useful mock features

```python
from unittest.mock import Mock, MagicMock, patch

m = Mock()
m.method.return_value = 10
print(m.method(1, 2))                       # 10
m.method.assert_called_with(1, 2)

m = Mock(side_effect=ConnectionError("down"))     # raises when called
m = Mock(side_effect=[1, 2, 3])                   # different value each call

with patch("os.getcwd", return_value="/fake"):
    import os
    print(os.getcwd())                      # /fake
```

**With `pytest`'s `monkeypatch`**

```python
def test_time(monkeypatch):
    import time
    monkeypatch.setattr(time, "time", lambda: 1_000)
    assert time.time() == 1_000
```

### Test-driven development (TDD)

```
1. RED:      Write a failing test for the feature
2. GREEN:    Write the least code to make it pass
3. REFACTOR: Clean up, keeping the tests green
```

### Code coverage

Shows which lines your tests ran.

```bash
pip install pytest-cov
pytest --cov=mypackage --cov-report=term-missing
pytest --cov=mypackage --cov-report=html      # open htmlcov/index.html
```

```
Name                     Stmts   Miss  Cover   Missing
------------------------------------------------------
mypackage/calc.py           12      2    83%   18-19
```

> 100% coverage does not mean no bugs. It only means every line ran. Aim for **meaningful** tests, with high coverage on important logic.

### Testing files and databases

```python
def test_save_and_load(tmp_path):
    file = tmp_path / "notes.txt"
    save_note(file, "Hello")
    assert load_note(file) == "Hello"
```

### Doctest

Tests inside docstrings.

```python
def square(n):
    """Return the square of n.

    >>> square(3)
    9
    >>> square(-2)
    4
    """
    return n * n
```

```bash
python -m doctest -v mymodule.py
pytest --doctest-modules
```

### Property-based testing (preview)

`hypothesis` generates many test inputs for you.

```bash
pip install hypothesis
```

```python
from hypothesis import given, strategies as st

@given(st.integers(), st.integers())
def test_add_commutative(a, b):
    assert add(a, b) == add(b, a)
```

### `unittest` vs `pytest`

| | `unittest` | `pytest` |
|---|---|---|
| Installation | Built in | `pip install pytest` |
| Style | Classes, `self.assertX` | Plain functions, plain `assert` |
| Fixtures | `setUp`/`tearDown` | Flexible `@pytest.fixture` |
| Parametrization | `subTest` | `@pytest.mark.parametrize` |
| Failure messages | Good | Excellent |
| Plugins | Few | Hundreds (`pytest-cov`, `pytest-asyncio`, ...) |
| Runs `unittest` tests | N/A | **Yes** |

### Good testing habits

- One test checks **one behavior**.
- Name tests clearly: `test_withdraw_more_than_balance_raises_error`.
- Use the **Arrange, Act, Assert** pattern.
- Tests must be **independent** and run in any order.
- Do not test the framework or the language.
- Test edge cases: empty input, zero, negative, very large, `None`.
- Keep tests fast so you run them often.
- Run tests automatically on every push (see CI in 10.2).

```python
def test_discount_applied():
    # Arrange
    cart = Cart()
    cart.add("Book", 100)

    # Act
    total = cart.total(discount=0.1)

    # Assert
    assert total == 90
```

### Testing async code

```bash
pip install pytest-asyncio
```

```python
import pytest

@pytest.mark.asyncio
async def test_fetch():
    result = await fetch_data()
    assert result == "ok"
```

---

## 10.2 Git and GitHub

**Git** is a version control system: it records every change to your project so you can go back, compare, and work with others. **GitHub** is a website that hosts Git repositories and adds collaboration tools.

| | Git | GitHub |
|---|---|---|
| What | Software on your computer | Online service |
| Job | Track changes | Host, share, review, automate |
| Alternatives | | GitLab, Bitbucket |

### Install and set up

```bash
git --version
```

Download from [git-scm.com](https://git-scm.com) if needed. Then configure your identity once:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --list
```

### Key concepts

| Term | Meaning |
|---|---|
| **Repository (repo)** | A project folder tracked by Git |
| **Commit** | A saved snapshot with a message |
| **Branch** | A separate line of development |
| **Working directory** | Your files as they are now |
| **Staging area (index)** | Changes prepared for the next commit |
| **Remote** | A copy of the repo on a server (usually named `origin`) |
| **Clone** | Download a copy of a remote repo |
| **Merge** | Combine branches |
| **Pull request (PR)** | A GitHub request to merge your branch, with review |

### The Git workflow

```
Working directory  --git add-->  Staging area  --git commit-->  Local repo  --git push-->  Remote (GitHub)
        ^                                                            |
        +------------------------- git pull -------------------------+
```

### Start a repository

```bash
mkdir my_project
cd my_project
git init
```

Or copy an existing one:

```bash
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git my_folder
```

### Daily commands

```bash
git status                    # what changed?
git add file.py               # stage one file
git add .                     # stage everything in this folder
git add -p                    # stage chosen pieces interactively
git commit -m "Add login form"
git commit -am "Fix typo"     # stage tracked files and commit
git log                       # history
git log --oneline --graph --all
git diff                      # unstaged changes
git diff --staged             # staged changes
git show abc1234              # one commit
```

### First commit example

```bash
echo "print('Hello')" > hello.py
git status
git add hello.py
git commit -m "Add hello script"
git log --oneline
```

### `.gitignore`

Lists files Git should **not** track.

```
# .gitignore
__pycache__/
*.pyc
.venv/
.env
.idea/
.vscode/
*.log
dist/
build/
*.egg-info/
.pytest_cache/
.coverage
```

> Never commit passwords, API keys, or `.env` files. Once pushed, assume they are public. Rotate any leaked secret.

If a file is already tracked:

```bash
git rm --cached secrets.txt
```

### Good commit messages

```
Short summary in the imperative (50 characters or fewer)

Optional longer explanation of WHAT changed and WHY,
wrapped at about 72 characters.
```

| Good | Poor |
|---|---|
| `Add password validation to signup` | `changes` |
| `Fix crash when file is empty` | `fix` |
| `Refactor database module` | `asdf` |

Make **small, focused commits**. Each should do one thing.

### Branching

Branches let you work on a feature without touching the stable code.

```bash
git branch                       # list branches
git branch feature-login         # create
git switch feature-login         # move to it (newer command)
git switch -c feature-login      # create and move
git checkout feature-login       # older equivalent
git branch -d feature-login      # delete (after merging)
git branch -D feature-login      # force delete
git branch -m new-name           # rename the current branch
```

### Merging

```bash
git switch main
git merge feature-login
```

**Fast-forward merge:** main had no new commits, so Git just moves the pointer.
**Three-way merge:** both branches changed, Git creates a merge commit.

```
main:     A---B---C-------M
                   \     /
feature:            D---E
```

### Merge conflicts

A conflict happens when two branches change the same lines.

```
<<<<<<< HEAD
print("Hello from main")
=======
print("Hello from feature")
>>>>>>> feature-login
```

To resolve:

1. Open the file and edit it to the final version. Remove the `<<<<<<<`, `=======`, and `>>>>>>>` lines.
2. `git add file.py`
3. `git commit` (or `git merge --continue`)

```bash
git merge --abort            # cancel the merge instead
```

### Rebase (tidy history)

```bash
git switch feature-login
git rebase main              # replay your commits on top of main
```

```
Before:   main:  A---B---C
                      \
          feature:     D---E

After:    main:  A---B---C
                          \
          feature:         D'---E'
```

> **Golden rule:** never rebase commits that others have already pulled (shared history).

Interactive cleanup of your own recent commits:

```bash
git rebase -i HEAD~3         # squash, reword, reorder
```

### Working with remotes (GitHub)

```bash
git remote -v
git remote add origin https://github.com/user/repo.git
git push -u origin main      # first push, set upstream
git push                     # later pushes
git pull                     # fetch and merge
git fetch                    # download only, no merge
git pull --rebase            # fetch and rebase instead of merge
```

### Creating a GitHub project

1. On GitHub, click **New repository**. Choose a name, public or private.
2. Connect your local repo:

```bash
git remote add origin https://github.com/YOUR-NAME/my_project.git
git branch -M main
git push -u origin main
```

### Authentication

| Method | Notes |
|---|---|
| **HTTPS + personal access token** | Use a token instead of a password |
| **SSH key** | Set up once, no repeated prompts |
| **GitHub CLI** (`gh auth login`) | Easiest for beginners |

```bash
ssh-keygen -t ed25519 -C "you@example.com"
# add ~/.ssh/id_ed25519.pub to GitHub: Settings > SSH and GPG keys
ssh -T git@github.com
```

### Undoing things

| Goal | Command |
|---|---|
| Discard changes in a file | `git restore file.py` |
| Unstage a file | `git restore --staged file.py` |
| Change the last commit message | `git commit --amend -m "New message"` |
| Add a forgotten file to the last commit | `git add file.py && git commit --amend --no-edit` |
| Undo a commit safely (new "undo" commit) | `git revert abc1234` |
| Move back, keep changes staged | `git reset --soft HEAD~1` |
| Move back, keep changes unstaged | `git reset HEAD~1` |
| Move back and **discard** changes | `git reset --hard HEAD~1` |
| Recover "lost" commits | `git reflog` |

> `git reset --hard` and `git clean -fd` permanently delete work. Be sure first. Use `git reflog` to find lost commits.

### Stashing

Save unfinished work temporarily.

```bash
git stash
git stash push -m "work in progress"
git stash list
git stash pop              # reapply and remove
git stash apply            # reapply and keep
git stash drop
```

### Tags (releases)

```bash
git tag v1.0.0
git tag -a v1.0.0 -m "First release"
git tag
git push origin v1.0.0
git push --tags
```

### Inspecting history

```bash
git log --oneline
git log --author="Ali"
git log --since="2 weeks ago"
git log -p file.py                  # changes to one file
git blame file.py                   # who changed each line
git shortlog -sn                    # commits per author
git bisect start                    # binary-search for the commit that broke something
```

### Pull requests (GitHub collaboration)

The standard team workflow:

```
1. Fork or clone the repo
2. git switch -c fix-login-bug
3. Make changes, commit
4. git push -u origin fix-login-bug
5. Open a Pull Request on GitHub
6. Teammates review and comment
7. Fix issues, push more commits
8. Merge the PR (squash, merge, or rebase)
9. Delete the branch; git switch main && git pull
```

### Forks and open source

```bash
git clone https://github.com/YOU/repo.git          # your fork
git remote add upstream https://github.com/ORIGINAL/repo.git
git fetch upstream
git switch main
git merge upstream/main                            # sync with the original
```

### Branching strategies

| Strategy | Idea |
|---|---|
| **GitHub Flow** | `main` is always deployable; short feature branches and PRs |
| **Git Flow** | `main`, `develop`, feature, release, and hotfix branches |
| **Trunk-based** | Very short-lived branches, merge to `main` many times a day |

### Protect your main branch

On GitHub: **Settings, Branches, Branch protection rules**. Require pull requests, reviews, and passing checks before merging.

### GitHub features

| Feature | Purpose |
|---|---|
| **Issues** | Track bugs and tasks |
| **Projects** | Kanban boards |
| **Pull requests** | Code review |
| **Actions** | Automatic tests and deployments (CI/CD) |
| **Releases** | Publish versions |
| **README.md** | The project's front page |
| **Wiki / Pages** | Documentation and websites |
| **Dependabot** | Alerts and updates for vulnerable dependencies |

### Continuous Integration with GitHub Actions

Create `.github/workflows/tests.yml`:

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

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest

      - name: Run tests
        run: pytest
```

Every push now runs your tests on three Python versions. Failures show as red marks on the pull request.

### GitHub CLI

```bash
gh auth login
gh repo create my_project --public --source=. --push
gh pr create --fill
gh pr list
gh pr checkout 42
gh issue create
```

### A good README

```markdown
# Project Name

One-sentence description.

## Features
- Feature one
- Feature two

## Installation
    pip install -r requirements.txt

## Usage
    python main.py --help

## Running tests
    pytest

## Contributing
Pull requests are welcome.

## License
MIT
```

### Useful aliases

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.lg "log --oneline --graph --all --decorate"
```

### Git best practices

- Commit early and often, in small logical steps.
- Write clear commit messages.
- Pull before you push; keep branches short-lived.
- Never commit secrets, large binaries, or generated files.
- Review your own diff (`git diff --staged`) before committing.
- Do not rewrite shared history.
- Use `.gitignore` from day one.

---

## 10.3 Working with APIs

An **API** (Application Programming Interface) lets programs talk to each other. A **web API** (often a **REST API**) lets your code request data from a server over HTTP, typically getting JSON back.

### How a web API works

```
Your program  --- HTTP request --->  Server
              <-- HTTP response ---  (status code + JSON data)
```

### HTTP basics

**Request methods**

| Method | Purpose | Example |
|---|---|---|
| `GET` | Read data | Get a list of users |
| `POST` | Create data | Create a new user |
| `PUT` | Replace data | Replace a user |
| `PATCH` | Partly update data | Change one field |
| `DELETE` | Remove data | Delete a user |

**Common status codes**

| Code | Meaning |
|---|---|
| `200` | OK |
| `201` | Created |
| `204` | No Content |
| `301`, `302` | Redirect |
| `400` | Bad Request (your input is wrong) |
| `401` | Unauthorized (not logged in) |
| `403` | Forbidden (logged in, no permission) |
| `404` | Not Found |
| `409` | Conflict |
| `422` | Unprocessable Entity (validation failed) |
| `429` | Too Many Requests (rate limited) |
| `500` | Server error |
| `503` | Service unavailable |

Ranges: `2xx` success, `3xx` redirect, `4xx` client error, `5xx` server error.

### Install `requests`

```bash
pip install requests
```

### A simple GET request

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/posts/1", timeout=10)

print(response.status_code)    # 200
print(response.ok)             # True for status < 400
print(response.headers["Content-Type"])
print(response.text)           # raw text
print(response.json())         # parsed JSON (dict or list)
```

```python
data = response.json()
print(data["title"])
```

> **Always set a `timeout`.** Without one, a request can hang forever.

### Checking for errors

```python
import requests

try:
    response = requests.get("https://jsonplaceholder.typicode.com/posts/9999", timeout=10)
    response.raise_for_status()           # raises HTTPError for 4xx and 5xx
    data = response.json()
except requests.exceptions.HTTPError as e:
    print("HTTP error:", e)
except requests.exceptions.ConnectionError:
    print("Cannot connect")
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.RequestException as e:
    print("Something went wrong:", e)
```

### Query parameters

```python
import requests

params = {"userId": 1, "_limit": 3}
r = requests.get("https://jsonplaceholder.typicode.com/posts", params=params, timeout=10)

print(r.url)        # https://jsonplaceholder.typicode.com/posts?userId=1&_limit=3
for post in r.json():
    print(post["id"], post["title"])
```

`requests` handles URL encoding for you.

### POST: sending data

**JSON body**

```python
import requests

new_post = {"title": "Hello", "body": "My first post", "userId": 1}

r = requests.post(
    "https://jsonplaceholder.typicode.com/posts",
    json=new_post,
    timeout=10,
)

print(r.status_code)     # 201
print(r.json())
```

`json=` sets the body and the `Content-Type: application/json` header for you.

**Form data**

```python
r = requests.post("https://httpbin.org/post", data={"name": "Ali", "age": 25}, timeout=10)
```

### PUT, PATCH, DELETE

```python
import requests

BASE = "https://jsonplaceholder.typicode.com/posts/1"

requests.put(BASE, json={"id": 1, "title": "New", "body": "x", "userId": 1}, timeout=10)
requests.patch(BASE, json={"title": "Only the title"}, timeout=10)
r = requests.delete(BASE, timeout=10)
print(r.status_code)    # 200
```

### Headers

```python
headers = {
    "Accept": "application/json",
    "User-Agent": "my-app/1.0",
}

r = requests.get("https://httpbin.org/headers", headers=headers, timeout=10)
print(r.json())
```

### Authentication

**API key in a header**

```python
import os
import requests

api_key = os.environ["MY_API_KEY"]

r = requests.get(
    "https://api.example.com/data",
    headers={"Authorization": f"Bearer {api_key}"},
    timeout=10,
)
```

**API key as a query parameter**

```python
r = requests.get("https://api.example.com/data", params={"api_key": api_key}, timeout=10)
```

**Basic authentication**

```python
r = requests.get("https://httpbin.org/basic-auth/user/pass", auth=("user", "pass"), timeout=10)
```

> Never hard-code API keys in source code. Use environment variables or a `.env` file (and add `.env` to `.gitignore`).

```python
# pip install python-dotenv
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("MY_API_KEY")
```

### Sessions

A `Session` reuses the connection and shares headers and cookies across calls. It is faster and tidier.

```python
import requests

with requests.Session() as session:
    session.headers.update({"Authorization": "Bearer TOKEN", "Accept": "application/json"})

    r1 = session.get("https://api.example.com/users", timeout=10)
    r2 = session.get("https://api.example.com/orders", timeout=10)
```

### Retries and backoff

```python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

retry = Retry(
    total=3,
    backoff_factor=1,                       # waits 1s, 2s, 4s
    status_forcelist=[429, 500, 502, 503, 504],
    allowed_methods=["GET"],
)

session = requests.Session()
session.mount("https://", HTTPAdapter(max_retries=retry))

r = session.get("https://api.example.com/data", timeout=10)
```

### Pagination

APIs return large lists in pages.

```python
import requests

def get_all(url, per_page=50):
    page = 1
    while True:
        r = requests.get(url, params={"_page": page, "_limit": per_page}, timeout=10)
        r.raise_for_status()
        items = r.json()
        if not items:
            break
        yield from items
        page += 1

for post in get_all("https://jsonplaceholder.typicode.com/posts"):
    print(post["id"])
```

Some APIs use a `next` link or a cursor token instead of page numbers. Check each API's documentation.

### Rate limiting

```python
import time
import requests

r = requests.get("https://api.example.com/data", timeout=10)

if r.status_code == 429:
    wait = int(r.headers.get("Retry-After", 5))
    print(f"Rate limited, waiting {wait}s")
    time.sleep(wait)
```

### Downloading files

```python
import requests

url = "https://example.com/large-file.zip"

with requests.get(url, stream=True, timeout=30) as r:
    r.raise_for_status()
    with open("large-file.zip", "wb") as f:
        for chunk in r.iter_content(chunk_size=8192):
            f.write(chunk)
```

### Uploading files

```python
with open("report.pdf", "rb") as f:
    r = requests.post("https://httpbin.org/post", files={"file": f}, timeout=30)
```

### Response details

```python
r = requests.get("https://httpbin.org/get", timeout=10)

r.status_code        # 200
r.ok                 # True
r.reason             # 'OK'
r.url                # final URL
r.headers            # dict-like headers
r.cookies
r.encoding           # 'utf-8'
r.content            # bytes
r.text               # str
r.json()             # parsed JSON
r.elapsed            # time taken
r.history            # redirect responses
```

### Building a reusable API client

```python
import requests

class GitHubClient:
    BASE = "https://api.github.com"

    def __init__(self, token=None):
        self.session = requests.Session()
        self.session.headers["Accept"] = "application/vnd.github+json"
        if token:
            self.session.headers["Authorization"] = f"Bearer {token}"

    def _get(self, path, **params):
        r = self.session.get(f"{self.BASE}{path}", params=params, timeout=10)
        r.raise_for_status()
        return r.json()

    def user(self, username):
        return self._get(f"/users/{username}")

    def repos(self, username, limit=5):
        return self._get(f"/users/{username}/repos", per_page=limit, sort="updated")

client = GitHubClient()
user = client.user("octocat")
print(user["name"], user["public_repos"])

for repo in client.repos("octocat"):
    print("-", repo["name"], repo["stargazers_count"])
```

### Async API calls

```python
import asyncio
import httpx

async def main():
    async with httpx.AsyncClient(timeout=10) as client:
        urls = [f"https://jsonplaceholder.typicode.com/posts/{i}" for i in range(1, 6)]
        responses = await asyncio.gather(*(client.get(u) for u in urls))
    print([r.json()["id"] for r in responses])

asyncio.run(main())
```

### Standard library alternative: `urllib`

```python
import json
from urllib.request import urlopen, Request

req = Request("https://jsonplaceholder.typicode.com/posts/1", headers={"Accept": "application/json"})
with urlopen(req, timeout=10) as resp:
    data = json.load(resp)
print(data["title"])
```

### OAuth 2.0 (overview)

Many APIs (Google, GitHub, Spotify) use OAuth to let users grant limited access without sharing passwords.

```
1. Your app sends the user to the provider's login page
2. User approves access
3. Provider redirects back with a temporary code
4. Your app exchanges the code for an access token
5. Your app sends the token with each API request
```

Use a library (`authlib`, `requests-oauthlib`) rather than building it by hand.

### Building your own API with FastAPI

```bash
pip install fastapi uvicorn
```

```python
# main.py
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

items: dict[int, Item] = {}

@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return items[item_id]

@app.post("/items/{item_id}", status_code=201)
def create_item(item_id: int, item: Item):
    items[item_id] = item
    return item
```

```bash
uvicorn main:app --reload
```

Open `http://127.0.0.1:8000/docs` for automatic interactive documentation.

### Flask alternative

```bash
pip install flask
```

```python
from flask import Flask, jsonify, request

app = Flask(__name__)
tasks = []

@app.get("/tasks")
def list_tasks():
    return jsonify(tasks)

@app.post("/tasks")
def add_task():
    task = request.get_json()
    tasks.append(task)
    return jsonify(task), 201

if __name__ == "__main__":
    app.run(debug=True)
```

### Testing API code

```python
from unittest.mock import patch, Mock

@patch("mymodule.requests.get")
def test_user(mock_get):
    mock_get.return_value = Mock(
        status_code=200,
        json=lambda: {"name": "Ali"},
        raise_for_status=lambda: None,
    )
    assert get_user_name(1) == "Ali"
```

Or use the `responses` / `requests-mock` libraries.

### API best practices

- Always set `timeout`, call `raise_for_status()`, and handle exceptions.
- Keep keys and tokens out of code and out of Git.
- Respect rate limits; use backoff on `429` and `5xx`.
- Use `Session` for repeated calls.
- Read the API documentation and check the terms of use.
- Validate the data you receive. Do not assume fields exist (`data.get("key")`).
- Cache responses when data rarely changes.
- Use `https://` only.

### Real-world example: weather report

```python
import os
import requests

API_KEY = os.environ["OPENWEATHER_KEY"]

def get_weather(city):
    r = requests.get(
        "https://api.openweathermap.org/data/2.5/weather",
        params={"q": city, "appid": API_KEY, "units": "metric"},
        timeout=10,
    )
    if r.status_code == 404:
        return None
    r.raise_for_status()
    data = r.json()
    return {
        "city": data["name"],
        "temp": data["main"]["temp"],
        "description": data["weather"][0]["description"],
    }

weather = get_weather("Lahore")
if weather:
    print(f"{weather['city']}: {weather['temp']}°C, {weather['description']}")
else:
    print("City not found")
```

---

## 10.4 SQL and Databases

A **database** stores data permanently and lets you search and update it efficiently. **SQL** (Structured Query Language) is the language used to talk to relational databases.

### Database types

| Type | Examples | Best for |
|---|---|---|
| **Relational (SQL)** | SQLite, PostgreSQL, MySQL, SQL Server | Structured data with relationships |
| **Document (NoSQL)** | MongoDB | Flexible JSON-like records |
| **Key-value** | Redis | Caching, sessions, fast lookups |
| **Graph** | Neo4j | Highly connected data |

Relational databases store data in **tables** with **rows** (records) and **columns** (fields).

```
users
+----+-------+-------------------+-----+
| id | name  | email             | age |
+----+-------+-------------------+-----+
|  1 | Ali   | ali@example.com   |  25 |
|  2 | Sara  | sara@example.com  |  22 |
+----+-------+-------------------+-----+
```

### SQLite: a database built into Python

`sqlite3` is in the standard library. It stores the whole database in one file. It is excellent for learning, small apps, and prototypes.

```python
import sqlite3

conn = sqlite3.connect("school.db")      # creates the file if it does not exist
cursor = conn.cursor()

cursor.execute("SELECT sqlite_version()")
print(cursor.fetchone())

conn.close()
```

`":memory:"` creates a temporary database in RAM, useful for tests:

```python
conn = sqlite3.connect(":memory:")
```

### SQL basics

#### Data types (SQLite)

| Type | Stores |
|---|---|
| `INTEGER` | Whole numbers |
| `REAL` | Decimal numbers |
| `TEXT` | Strings |
| `BLOB` | Binary data |
| `NULL` | No value |

#### CREATE TABLE

```sql
CREATE TABLE IF NOT EXISTS students (
    id      INTEGER PRIMARY KEY AUTOINCREMENT,
    name    TEXT    NOT NULL,
    email   TEXT    UNIQUE,
    age     INTEGER CHECK (age >= 0),
    city    TEXT    DEFAULT 'Unknown'
);
```

| Constraint | Meaning |
|---|---|
| `PRIMARY KEY` | Unique identifier for each row |
| `AUTOINCREMENT` | Number assigned automatically |
| `NOT NULL` | A value is required |
| `UNIQUE` | No duplicates |
| `DEFAULT` | Value used when none is given |
| `CHECK` | Custom rule |
| `FOREIGN KEY` | Links to another table |

#### INSERT

```sql
INSERT INTO students (name, email, age, city)
VALUES ('Ali', 'ali@example.com', 25, 'Lahore');
```

#### SELECT

```sql
SELECT * FROM students;
SELECT name, age FROM students;
SELECT * FROM students WHERE age > 22;
SELECT * FROM students WHERE city = 'Lahore' AND age >= 20;
SELECT * FROM students WHERE name LIKE 'A%';           -- starts with A
SELECT * FROM students WHERE age BETWEEN 20 AND 30;
SELECT * FROM students WHERE city IN ('Lahore', 'Karachi');
SELECT * FROM students WHERE email IS NULL;
SELECT * FROM students ORDER BY age DESC;
SELECT * FROM students ORDER BY city, name LIMIT 10 OFFSET 20;
SELECT DISTINCT city FROM students;
```

#### UPDATE

```sql
UPDATE students SET age = 26 WHERE name = 'Ali';
UPDATE students SET city = 'Islamabad', age = age + 1 WHERE id = 3;
```

#### DELETE

```sql
DELETE FROM students WHERE id = 3;
```

> **Always include a `WHERE` clause** with `UPDATE` and `DELETE`. Without it, **every row** is changed or deleted.

#### Aggregate functions

```sql
SELECT COUNT(*)        FROM students;
SELECT AVG(age)        FROM students;
SELECT MIN(age), MAX(age), SUM(age) FROM students;

SELECT city, COUNT(*) AS total, AVG(age) AS avg_age
FROM students
GROUP BY city
HAVING COUNT(*) > 1
ORDER BY total DESC;
```

| Clause | Purpose |
|---|---|
| `WHERE` | Filters rows **before** grouping |
| `GROUP BY` | Groups rows |
| `HAVING` | Filters groups **after** grouping |
| `ORDER BY` | Sorts the result |
| `LIMIT` | Caps the number of rows |

### Using SQL from Python

#### Create a table and insert data

```python
import sqlite3

conn = sqlite3.connect("school.db")
cur = conn.cursor()

cur.execute("""
    CREATE TABLE IF NOT EXISTS students (
        id    INTEGER PRIMARY KEY AUTOINCREMENT,
        name  TEXT NOT NULL,
        email TEXT UNIQUE,
        age   INTEGER,
        city  TEXT
    )
""")

cur.execute(
    "INSERT INTO students (name, email, age, city) VALUES (?, ?, ?, ?)",
    ("Ali", "ali@example.com", 25, "Lahore"),
)

conn.commit()          # save the changes
conn.close()
```

#### Insert many rows

```python
students = [
    ("Sara", "sara@example.com", 22, "Karachi"),
    ("Bilal", "bilal@example.com", 30, "Islamabad"),
    ("Hina", "hina@example.com", 27, "Lahore"),
]

cur.executemany(
    "INSERT INTO students (name, email, age, city) VALUES (?, ?, ?, ?)",
    students,
)
conn.commit()
```

#### Read data

```python
cur.execute("SELECT id, name, age FROM students WHERE age > ?", (23,))

print(cur.fetchone())       # one row (tuple) or None
print(cur.fetchmany(2))     # up to 2 rows
print(cur.fetchall())       # all remaining rows

# iterate directly
for row in cur.execute("SELECT name, city FROM students"):
    print(row)
```

#### Rows as dictionaries

```python
conn.row_factory = sqlite3.Row

cur = conn.execute("SELECT * FROM students")
for row in cur:
    print(row["name"], row["age"])
    print(dict(row))
```

#### Update and delete

```python
cur.execute("UPDATE students SET age = ? WHERE name = ?", (26, "Ali"))
print(cur.rowcount, "row(s) updated")

cur.execute("DELETE FROM students WHERE id = ?", (4,))
conn.commit()

print(cur.lastrowid)         # id of the last inserted row
```

### SQL injection: the most important security rule

**Never** build SQL with string formatting using user input.

```python
# DANGEROUS
name = input("Name: ")
cur.execute(f"SELECT * FROM students WHERE name = '{name}'")
# Input:  ' OR '1'='1     returns ALL rows
# Input:  '; DROP TABLE students; --     can destroy data
```

**Safe: use parameters (`?`)**

```python
cur.execute("SELECT * FROM students WHERE name = ?", (name,))
```

The database treats the value strictly as data, never as SQL code. Parameters cannot be used for table or column names. Validate those against an allowed list.

### Transactions

A **transaction** groups changes so they **all succeed or all fail**.

```python
import sqlite3

conn = sqlite3.connect("bank.db")

try:
    with conn:                                     # commits on success, rolls back on error
        conn.execute("UPDATE accounts SET balance = balance - 500 WHERE id = 1")
        conn.execute("UPDATE accounts SET balance = balance + 500 WHERE id = 2")
except sqlite3.Error as e:
    print("Transfer failed, rolled back:", e)
finally:
    conn.close()
```

**ACID**

| Letter | Meaning |
|---|---|
| **A**tomicity | All or nothing |
| **C**onsistency | Rules (constraints) always hold |
| **I**solation | Simultaneous transactions do not interfere |
| **D**urability | Committed data survives crashes |

### Closing connections safely

```python
from contextlib import closing
import sqlite3

with closing(sqlite3.connect("school.db")) as conn:
    with conn:
        conn.execute("INSERT INTO students (name) VALUES (?)", ("Omar",))
```

> `with conn:` manages the **transaction**, not the connection. It does not close the connection, so use `closing()` as well.

### Relationships and JOINs

Split data into tables and link them with keys, instead of repeating it.

```sql
CREATE TABLE courses (
    id    INTEGER PRIMARY KEY,
    title TEXT NOT NULL
);

CREATE TABLE enrollments (
    id         INTEGER PRIMARY KEY,
    student_id INTEGER NOT NULL,
    course_id  INTEGER NOT NULL,
    grade      TEXT,
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (course_id)  REFERENCES courses(id)
);
```

SQLite needs foreign keys switched on for each connection:

```python
conn.execute("PRAGMA foreign_keys = ON")
```

**Relationship types**

| Type | Example | Implementation |
|---|---|---|
| One-to-many | One teacher, many classes | Foreign key on the "many" side |
| Many-to-many | Students and courses | A link table (`enrollments`) |
| One-to-one | User and profile | Foreign key with `UNIQUE` |

**JOIN types**

```sql
-- INNER JOIN: only rows that match in both tables
SELECT s.name, c.title, e.grade
FROM enrollments e
JOIN students s ON s.id = e.student_id
JOIN courses  c ON c.id = e.course_id;

-- LEFT JOIN: all students, even with no enrollments
SELECT s.name, c.title
FROM students s
LEFT JOIN enrollments e ON e.student_id = s.id
LEFT JOIN courses c ON c.id = e.course_id;
```

| Join | Returns |
|---|---|
| `INNER JOIN` | Only matching rows |
| `LEFT JOIN` | All left rows, plus matches (or `NULL`) |
| `RIGHT JOIN` | All right rows, plus matches (not in older SQLite) |
| `FULL OUTER JOIN` | All rows from both sides |
| `CROSS JOIN` | Every combination |

**Subqueries**

```sql
SELECT name FROM students
WHERE age > (SELECT AVG(age) FROM students);
```

### Indexes: faster searches

```sql
CREATE INDEX idx_students_city ON students(city);
CREATE UNIQUE INDEX idx_students_email ON students(email);
```

An index is like a book's index: lookups on that column become much faster, but writes become slightly slower and use more space. Index columns used in `WHERE`, `JOIN`, and `ORDER BY`.

```sql
EXPLAIN QUERY PLAN SELECT * FROM students WHERE city = 'Lahore';
```

### Views

```sql
CREATE VIEW lahore_students AS
SELECT id, name, age FROM students WHERE city = 'Lahore';

SELECT * FROM lahore_students;
```

### Database design: normalization

| Rule | Idea |
|---|---|
| **1NF** | Each cell holds one value; no repeating lists |
| **2NF** | Every non-key column depends on the whole key |
| **3NF** | Non-key columns depend only on the key, not on each other |

Bad design (repeated data):

```
orders: id | customer_name | customer_email | product | price
```

Better design:

```
customers: id | name | email
products:  id | name | price
orders:    id | customer_id | order_date
order_items: order_id | product_id | quantity
```

### A complete data-access class

```python
import sqlite3

class StudentDB:
    def __init__(self, path="school.db"):
        self.conn = sqlite3.connect(path)
        self.conn.row_factory = sqlite3.Row
        self.conn.execute("PRAGMA foreign_keys = ON")
        self._create_tables()

    def _create_tables(self):
        with self.conn:
            self.conn.execute("""
                CREATE TABLE IF NOT EXISTS students (
                    id    INTEGER PRIMARY KEY AUTOINCREMENT,
                    name  TEXT NOT NULL,
                    email TEXT UNIQUE,
                    age   INTEGER CHECK (age >= 0),
                    city  TEXT
                )
            """)

    def add(self, name, email, age, city):
        with self.conn:
            cur = self.conn.execute(
                "INSERT INTO students (name, email, age, city) VALUES (?, ?, ?, ?)",
                (name, email, age, city),
            )
            return cur.lastrowid

    def get(self, student_id):
        row = self.conn.execute(
            "SELECT * FROM students WHERE id = ?", (student_id,)
        ).fetchone()
        return dict(row) if row else None

    def search(self, city=None, min_age=None):
        sql = "SELECT * FROM students WHERE 1=1"
        params = []
        if city:
            sql += " AND city = ?"
            params.append(city)
        if min_age is not None:
            sql += " AND age >= ?"
            params.append(min_age)
        return [dict(r) for r in self.conn.execute(sql, params)]

    def update_age(self, student_id, age):
        with self.conn:
            self.conn.execute("UPDATE students SET age = ? WHERE id = ?", (age, student_id))

    def delete(self, student_id):
        with self.conn:
            self.conn.execute("DELETE FROM students WHERE id = ?", (student_id,))

    def close(self):
        self.conn.close()

db = StudentDB(":memory:")
sid = db.add("Ali", "ali@example.com", 25, "Lahore")
db.add("Sara", "sara@example.com", 22, "Karachi")

print(db.get(sid))
print(db.search(city="Lahore"))
db.update_age(sid, 26)
db.delete(sid)
db.close()
```

### Handling database errors

```python
import sqlite3

try:
    cur.execute("INSERT INTO students (name, email) VALUES (?, ?)", ("Ali", "ali@example.com"))
    conn.commit()
except sqlite3.IntegrityError as e:
    print("Constraint violated (duplicate or NOT NULL):", e)
    conn.rollback()
except sqlite3.OperationalError as e:
    print("SQL or database problem:", e)
except sqlite3.Error as e:
    print("Database error:", e)
```

### Working with pandas

```python
import sqlite3
import pandas as pd

conn = sqlite3.connect("school.db")

df = pd.read_sql_query("SELECT * FROM students", conn)
print(df.head())

df.to_sql("students_backup", conn, if_exists="replace", index=False)
conn.close()
```

### Beyond SQLite: PostgreSQL and MySQL

```bash
pip install psycopg[binary]        # PostgreSQL
pip install mysql-connector-python # MySQL
```

```python
import psycopg

with psycopg.connect("dbname=school user=postgres password=secret host=localhost") as conn:
    with conn.cursor() as cur:
        cur.execute("SELECT * FROM students WHERE age > %s", (23,))
        print(cur.fetchall())
```

> The parameter placeholder differs by driver: `?` in SQLite, `%s` in PostgreSQL and MySQL drivers.

### ORMs: SQLAlchemy

An **ORM** (Object-Relational Mapper) lets you work with Python classes instead of writing SQL.

```bash
pip install sqlalchemy
```

```python
from sqlalchemy import create_engine, String, select
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, Session

class Base(DeclarativeBase):
    pass

class Student(Base):
    __tablename__ = "students"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(50))
    age: Mapped[int]

engine = create_engine("sqlite:///school.db")
Base.metadata.create_all(engine)

with Session(engine) as session:
    session.add_all([Student(name="Ali", age=25), Student(name="Sara", age=22)])
    session.commit()

    adults = session.scalars(select(Student).where(Student.age > 23)).all()
    for s in adults:
        print(s.name, s.age)
```

| | Raw SQL | ORM |
|---|---|---|
| Control and speed | Highest | Good |
| Learning curve | SQL needed | ORM concepts needed |
| Portability | Per database | Easy to switch databases |
| Boilerplate | More | Less |

### Migrations

When your tables change over time, use a migration tool such as **Alembic** (for SQLAlchemy) or Django's migrations, instead of editing the database by hand.

### Backups

```python
import sqlite3

src = sqlite3.connect("school.db")
dst = sqlite3.connect("school_backup.db")
with dst:
    src.backup(dst)
src.close()
dst.close()
```

```bash
sqlite3 school.db .dump > backup.sql
```

### Database best practices

- Always use **parameterized queries**.
- Use **transactions** for multi-step changes.
- Add **indexes** on columns you search often.
- Set **constraints** (`NOT NULL`, `UNIQUE`, `FOREIGN KEY`) so the database protects your data.
- Never store passwords in plain text (see 10.6).
- Keep credentials out of source code.
- Back up regularly and test restoring.
- Select only the columns you need; avoid `SELECT *` in production code.
- Close connections (or use a connection pool).

---

## 10.5 Logging

**Logging** records what your program does while it runs. Unlike `print()`, logs have **levels**, **timestamps**, and can go to files, the console, or remote services, and can be switched on and off without editing code.

### Why not just `print()`?

| `print()` | `logging` |
|---|---|
| No severity levels | `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL` |
| No timestamps | Timestamps, module names, line numbers |
| Always on | Filter by level |
| Goes only to the screen | Screen, files, email, network |
| Hard to remove later | Configure once |

### Log levels

| Level | Value | Use for |
|---|---|---|
| `DEBUG` | 10 | Detailed information for diagnosing problems |
| `INFO` | 20 | Normal events ("Server started", "User logged in") |
| `WARNING` | 30 | Something unexpected, but the program continues |
| `ERROR` | 40 | A task failed |
| `CRITICAL` | 50 | A serious failure; the program may stop |

The default level is **`WARNING`**: `DEBUG` and `INFO` are hidden unless you configure them.

### Quick start

```python
import logging

logging.warning("Disk space is low")
logging.info("This will NOT show (level too low)")
```

```
WARNING:root:Disk space is low
```

### `basicConfig`

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s | %(levelname)-8s | %(name)s | %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
)

logging.debug("Debugging details")
logging.info("Program started")
logging.warning("Low memory")
logging.error("Could not open file")
logging.critical("Out of memory!")
```

```
2026-10-18 20:02:11 | DEBUG    | root | Debugging details
2026-10-18 20:02:11 | INFO     | root | Program started
...
```

Call `basicConfig` **once**, at program start, before other logging calls.

**Log to a file**

```python
logging.basicConfig(
    filename="app.log",
    filemode="a",              # append (default)
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s",
    encoding="utf-8",
)
```

### Format fields

| Field | Meaning |
|---|---|
| `%(asctime)s` | Time |
| `%(levelname)s` | Level name |
| `%(name)s` | Logger name |
| `%(message)s` | The message |
| `%(filename)s` | File name |
| `%(module)s` | Module name |
| `%(funcName)s` | Function name |
| `%(lineno)d` | Line number |
| `%(process)d` / `%(thread)d` | Process / thread id |

### Loggers: one per module

```python
import logging

logger = logging.getLogger(__name__)

def process(order_id):
    logger.info("Processing order %s", order_id)
    try:
        ...
    except Exception:
        logger.exception("Order %s failed", order_id)
```

Using `__name__` gives each module its own named logger (`myapp.payments`), so you can adjust verbosity per module.

### Lazy formatting

```python
logger.debug("User %s has %d items", user, count)       # good: only formatted if emitted
logger.debug(f"User {user} has {count} items")          # works, but always builds the string
```

### Logging exceptions

```python
try:
    1 / 0
except ZeroDivisionError:
    logger.exception("Calculation failed")     # ERROR level + full traceback
```

```python
logger.error("Failed", exc_info=True)          # same idea at any level
```

### The logging system's parts

| Part | Job |
|---|---|
| **Logger** | The object you call (`logger.info(...)`) |
| **Handler** | Sends records somewhere (console, file, ...) |
| **Formatter** | Decides how a record looks |
| **Filter** | Decides whether a record is emitted |

```
Logger  ->  Handler(s)  ->  Formatter  ->  Destination
```

### Configuring handlers in code

```python
import logging
import sys

logger = logging.getLogger("myapp")
logger.setLevel(logging.DEBUG)

console = logging.StreamHandler(sys.stdout)
console.setLevel(logging.INFO)
console.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))

file_handler = logging.FileHandler("app.log", encoding="utf-8")
file_handler.setLevel(logging.DEBUG)
file_handler.setFormatter(logging.Formatter(
    "%(asctime)s %(name)s %(levelname)s %(message)s"
))

logger.addHandler(console)
logger.addHandler(file_handler)

logger.debug("Goes to the file only")
logger.info("Goes to the console and the file")
```

### Rotating log files

Prevent logs from growing forever.

```python
from logging.handlers import RotatingFileHandler, TimedRotatingFileHandler
import logging

# By size: 5 MB per file, keep 3 old files
handler = RotatingFileHandler("app.log", maxBytes=5_000_000, backupCount=3, encoding="utf-8")

# By time: new file every midnight, keep 7 days
handler2 = TimedRotatingFileHandler("daily.log", when="midnight", backupCount=7, encoding="utf-8")

logger = logging.getLogger("myapp")
logger.addHandler(handler)
```

### Configuration with a dictionary

```python
import logging.config

LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "standard": {
            "format": "%(asctime)s [%(levelname)s] %(name)s: %(message)s"
        },
    },
    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "level": "INFO",
            "formatter": "standard",
        },
        "file": {
            "class": "logging.handlers.RotatingFileHandler",
            "filename": "app.log",
            "maxBytes": 5_000_000,
            "backupCount": 3,
            "formatter": "standard",
            "encoding": "utf-8",
        },
    },
    "root": {
        "level": "DEBUG",
        "handlers": ["console", "file"],
    },
    "loggers": {
        "urllib3": {"level": "WARNING"},      # quiet a noisy library
    },
}

logging.config.dictConfig(LOGGING)
logging.getLogger(__name__).info("Configured")
```

### Library vs application

| | Libraries | Applications |
|---|---|---|
| Create loggers | Yes (`getLogger(__name__)`) | Yes |
| Configure handlers and levels | **No** | **Yes**, once at startup |
| Add `NullHandler` | Yes (avoids "no handler" warnings) | No |

```python
# in a library's __init__.py
import logging
logging.getLogger(__name__).addHandler(logging.NullHandler())
```

### Logger hierarchy

Logger names use dots to form a tree. Child loggers pass records up to their parents.

```
root
└── myapp
    ├── myapp.db
    └── myapp.api
```

```python
logging.getLogger("myapp").setLevel(logging.INFO)
logging.getLogger("myapp.db").setLevel(logging.DEBUG)
```

### Adding context with `extra`

```python
logging.basicConfig(format="%(asctime)s %(user)s %(message)s")
logger = logging.getLogger("demo")

logger.warning("Login failed", extra={"user": "ali"})
```

Or use `LoggerAdapter`:

```python
adapter = logging.LoggerAdapter(logger, {"user": "ali"})
adapter.warning("Login failed")
```

### Structured (JSON) logging

Machine-readable logs are easier to search in tools like Elasticsearch, Datadog, or Splunk.

```python
import json
import logging

class JsonFormatter(logging.Formatter):
    def format(self, record):
        payload = {
            "time": self.formatTime(record),
            "level": record.levelname,
            "logger": record.name,
            "message": record.getMessage(),
        }
        if record.exc_info:
            payload["exception"] = self.formatException(record.exc_info)
        return json.dumps(payload)

handler = logging.StreamHandler()
handler.setFormatter(JsonFormatter())
logging.getLogger().addHandler(handler)
logging.getLogger().setLevel(logging.INFO)

logging.info("User signed in")
```

Third-party options: `structlog`, `loguru`, `python-json-logger`.

### Filters

```python
import logging

class NoPasswords(logging.Filter):
    def filter(self, record):
        return "password" not in record.getMessage().lower()

handler = logging.StreamHandler()
handler.addFilter(NoPasswords())
```

### Controlling noisy libraries

```python
logging.getLogger("urllib3").setLevel(logging.WARNING)
logging.getLogger("matplotlib").setLevel(logging.ERROR)
```

### Logging from decorators

```python
import functools
import logging
import time

logger = logging.getLogger(__name__)

def log_calls(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        logger.debug("Calling %s", func.__name__)
        start = time.perf_counter()
        try:
            return func(*args, **kwargs)
        except Exception:
            logger.exception("%s failed", func.__name__)
            raise
        finally:
            logger.debug("%s took %.3fs", func.__name__, time.perf_counter() - start)
    return wrapper
```

### Testing logging

```python
def test_logs_warning(caplog):
    with caplog.at_level(logging.WARNING):
        low_disk_check()
    assert "Disk space is low" in caplog.text
```

### The `loguru` alternative

```bash
pip install loguru
```

```python
from loguru import logger

logger.add("app.log", rotation="5 MB", retention="7 days", level="DEBUG")

logger.info("Started")
logger.warning("Careful")

try:
    1 / 0
except ZeroDivisionError:
    logger.exception("Failed")
```

### What to log

| Do log | Do not log |
|---|---|
| Startup and shutdown | Passwords, tokens, API keys |
| Important business events | Full credit card numbers |
| Errors with context (IDs, inputs) | Personal data without need (emails, addresses, health data) |
| External calls and their duration | Huge payloads |
| Configuration at startup (non-secret) | Anything that violates privacy rules |

### Logging best practices

- Use `logging`, not `print`, in real applications.
- Configure logging **once**, at the application's entry point.
- One logger per module: `logging.getLogger(__name__)`.
- Pick levels carefully. If everything is `ERROR`, nothing is.
- Use `logger.exception()` inside `except` blocks.
- Use lazy `%s` formatting.
- Rotate log files.
- Include context (user id, request id, order id).
- Mask or omit secrets and personal data.

---

## 10.6 Security Best Practices

Security is not optional. A single mistake can expose user data, money, or a whole system. This section covers the most important habits for Python developers.

### Mindset

- **Never trust input.** Anything from users, files, APIs, or the network may be hostile or wrong.
- **Least privilege.** Give code and users only the access they need.
- **Defense in depth.** Layer protections; do not rely on one.
- **Keep it simple.** Complex code hides bugs.
- **Fail safely.** Errors should not leak secrets or grant access.

### The OWASP Top 10 (web application risks)

| Risk | Short description |
|---|---|
| Broken access control | Users reach data or actions they should not |
| Cryptographic failures | Weak or missing encryption, exposed secrets |
| Injection | SQL, command, or template injection |
| Insecure design | Missing security thinking at design time |
| Security misconfiguration | Debug mode on, default passwords |
| Vulnerable components | Outdated libraries with known flaws |
| Authentication failures | Weak passwords, broken sessions |
| Data integrity failures | Unsafe deserialization, unsigned updates |
| Logging and monitoring failures | Attacks go unnoticed |
| Server-side request forgery (SSRF) | Tricking the server into calling internal URLs |

### 1. Input validation

```python
def parse_age(text):
    try:
        age = int(text)
    except ValueError:
        raise ValueError("Age must be a whole number")
    if not 0 <= age <= 120:
        raise ValueError("Age must be between 0 and 120")
    return age
```

Principles:

- Use an **allow-list** (accept known good), not a deny-list (block known bad).
- Check **type, length, range, and format**.
- Validate on the **server**, even if the browser already validated.
- Use libraries such as **Pydantic** for structured validation.

```python
from pydantic import BaseModel, EmailStr, Field

class Signup(BaseModel):
    username: str = Field(min_length=3, max_length=30, pattern=r"^[A-Za-z0-9_]+$")
    email: EmailStr
    age: int = Field(ge=13, le=120)
```

### 2. SQL injection

Covered in 10.4. Always use parameters.

```python
# Never
cur.execute(f"SELECT * FROM users WHERE name = '{name}'")

# Always
cur.execute("SELECT * FROM users WHERE name = ?", (name,))
```

### 3. Command injection

```python
import subprocess

filename = input("File: ")

# DANGEROUS: the shell interprets the input
subprocess.run(f"cat {filename}", shell=True)
# Input:  report.txt; rm -rf ~

# SAFE: arguments are passed as a list, no shell
subprocess.run(["cat", filename], check=True)
```

Avoid `shell=True` and `os.system()` with any user-influenced text. Use `shlex.quote()` only if you must build a shell string.

### 4. Dangerous functions

| Avoid | Why | Use instead |
|---|---|---|
| `eval()`, `exec()` on untrusted text | Runs arbitrary code | `ast.literal_eval()` for literals, `json.loads()` |
| `pickle.load()` on untrusted data | Can execute code | JSON, or signed data |
| `yaml.load()` | Can construct arbitrary objects | `yaml.safe_load()` |
| `input()` in very old Python 2 | Evaluates input | Python 3 (`input()` is safe) |
| `os.system()`, `shell=True` | Shell injection | `subprocess.run([...])` |
| `tempfile.mktemp()` | Race condition | `tempfile.mkstemp()`, `NamedTemporaryFile` |
| `assert` for validation | Removed with `python -O` | `if` + `raise` |
| `random` for secrets | Predictable | `secrets` module |
| `xml` parsing of untrusted XML | Entity expansion attacks | `defusedxml` |

```python
import ast

value = ast.literal_eval("[1, 2, {'a': 3}]")     # safe for literals only
# eval("__import__('os').system('...')")         # never
```

### 5. Password handling

**Never store passwords in plain text, and never use fast hashes (MD5, SHA-1, plain SHA-256).** Use a slow, salted password hash.

```bash
pip install argon2-cffi
```

```python
from argon2 import PasswordHasher
from argon2.exceptions import VerifyMismatchError

ph = PasswordHasher()

hash_ = ph.hash("my-secret-password")      # includes a random salt
print(hash_)

try:
    ph.verify(hash_, "my-secret-password")
    print("Correct")
except VerifyMismatchError:
    print("Wrong password")
```

Alternatives: `bcrypt`, or `hashlib.scrypt` / `hashlib.pbkdf2_hmac` from the standard library.

```python
import hashlib
import os

salt = os.urandom(16)
key = hashlib.scrypt(b"password", salt=salt, n=2**14, r=8, p=1)
```

Also:

- Enforce a minimum length (12 or more is a good target); allow long passphrases.
- Rate-limit login attempts.
- Offer multi-factor authentication (MFA).
- Compare secrets with `hmac.compare_digest()` to avoid timing attacks.

```python
import hmac
hmac.compare_digest(token_a, token_b)
```

### 6. Secure randomness

```python
import secrets

token = secrets.token_urlsafe(32)        # session tokens, reset links
api_key = secrets.token_hex(16)
pin = f"{secrets.randbelow(10**6):06d}"
choice = secrets.choice(["a", "b", "c"])
```

The `random` module is predictable and must **not** be used for security.

### 7. Managing secrets

**Never** put secrets in source code or Git.

```python
# Bad
API_KEY = "sk-live-abc123..."

# Good
import os
API_KEY = os.environ["API_KEY"]               # fails loudly if missing
API_KEY = os.getenv("API_KEY", "")            # with a default
```

**Using a `.env` file** (kept out of Git)

```
# .env
API_KEY=sk-live-abc123
DATABASE_URL=postgresql://user:pass@localhost/db
```

```python
from dotenv import load_dotenv
import os

load_dotenv()
API_KEY = os.environ["API_KEY"]
```

```
# .gitignore
.env
```

Better options for production: a secrets manager (AWS Secrets Manager, Azure Key Vault, HashiCorp Vault, Google Secret Manager) or your hosting platform's secret settings.

**If a secret leaks:** revoke and rotate it **immediately**. Removing it from Git history is not enough. Assume it was copied.

**Scan for leaked secrets**

```bash
pip install detect-secrets
detect-secrets scan
```

Tools such as `gitleaks` and GitHub secret scanning help too.

### 8. Encryption basics

| Need | Tool |
|---|---|
| Encrypt data with a shared key | `cryptography` (Fernet) |
| Hash for integrity | `hashlib.sha256` |
| Sign / verify data | `hmac`, `itsdangerous`, JWT libraries |
| Protect data in transit | HTTPS/TLS |
| Passwords | Argon2 / bcrypt / scrypt |

```bash
pip install cryptography
```

```python
from cryptography.fernet import Fernet

key = Fernet.generate_key()          # store securely, not in code
f = Fernet(key)

token = f.encrypt(b"secret message")
print(f.decrypt(token))              # b'secret message'
```

> **Never invent your own cryptography.** Use well-tested libraries.

```python
import hashlib

print(hashlib.sha256(b"data").hexdigest())     # good for file integrity, NOT for passwords
```

### 9. HTTPS and certificates

```python
import requests

requests.get("https://example.com", timeout=10)                # verifies certificates
# requests.get("https://example.com", verify=False)            # never in production
```

Do not disable certificate checks to "make an error go away". Fix the underlying certificate problem.

### 10. Dependencies

Third-party packages can contain vulnerabilities, or be malicious.

```bash
pip install pip-audit
pip-audit                          # check installed packages for known vulnerabilities
pip-audit -r requirements.txt

pip list --outdated
```

Practices:

- Pin versions in `requirements.txt` (or use a lock file with `pip-tools`, `poetry`, or `uv`).
- Install only what you need; remove unused packages.
- Check spelling: **typosquatting** (`reqeusts` instead of `requests`) is a real attack.
- Prefer popular, maintained packages. Check release dates and repository activity.
- Use **hash checking** for sensitive deployments:

```bash
pip install --require-hashes -r requirements.txt
```

- Enable **Dependabot** or **Renovate** for automatic update alerts.

### 11. File and path safety

**Path traversal**

```python
from pathlib import Path

BASE = Path("/srv/uploads").resolve()

def safe_open(user_filename):
    target = (BASE / user_filename).resolve()
    if BASE not in target.parents:
        raise PermissionError("Access outside the upload folder is not allowed")
    return open(target, "rb")

# safe_open("../../etc/passwd")      # PermissionError
```

**Uploads**

- Limit file **size** and **count**.
- Check the extension against an allow-list **and** the real content type.
- Generate your own file names. Never use the uploaded name directly.
- Store uploads outside the web root.

```python
import uuid
from pathlib import Path

ALLOWED = {".png", ".jpg", ".jpeg", ".pdf"}

def save_upload(original_name, data, folder):
    ext = Path(original_name).suffix.lower()
    if ext not in ALLOWED:
        raise ValueError("File type not allowed")
    name = f"{uuid.uuid4().hex}{ext}"
    (Path(folder) / name).write_bytes(data)
    return name
```

**Archives:** extracting zip or tar files from untrusted sources can write outside the target folder ("zip slip"). Check member paths first, or use `tarfile`'s `filter="data"` (Python 3.12+).

```python
import tarfile

with tarfile.open("archive.tar.gz") as tar:
    tar.extractall("output", filter="data")
```

### 12. Web application security

| Threat | Defense |
|---|---|
| **XSS** (cross-site scripting) | Escape output; use a template engine with auto-escaping; set a Content Security Policy |
| **CSRF** (cross-site request forgery) | CSRF tokens, `SameSite` cookies |
| **Session hijacking** | `Secure`, `HttpOnly`, `SameSite` cookies; HTTPS; session expiry |
| **Clickjacking** | `X-Frame-Options` / CSP `frame-ancestors` |
| **Open redirects** | Allow-list redirect targets |
| **SSRF** | Validate and restrict URLs the server fetches |
| **Brute force** | Rate limiting, lockouts, MFA |
| **Debug mode in production** | Turn it off (`debug=False`) |

```python
from markupsafe import escape

print(escape("<script>alert('xss')</script>"))
# &lt;script&gt;alert(&#39;xss&#39;)&lt;/script&gt;
```

```python
# Flask: secure cookie settings
app.config.update(
    SESSION_COOKIE_SECURE=True,
    SESSION_COOKIE_HTTPONLY=True,
    SESSION_COOKIE_SAMESITE="Lax",
)
```

Use your framework's built-in protections (Django and modern frameworks include many by default) rather than writing your own.

### 13. Authentication and authorization

| Term | Question | Example |
|---|---|---|
| **Authentication** | Who are you? | Login |
| **Authorization** | What may you do? | Only admins can delete users |

```python
def delete_user(current_user, user_id):
    if not current_user.is_admin:
        raise PermissionError("Admins only")
    ...
```

- Check authorization on **every** request, on the server.
- Do not rely on hidden buttons or client-side checks.
- Verify the user owns the record they are requesting (prevents "IDOR" bugs).
- Use short-lived tokens; make them revocable.
- Do not roll your own auth. Use proven libraries or services.

### 14. Error handling and information leaks

```python
# Bad: exposes internals to the user
except Exception as e:
    return f"Database error: {e}"

# Good: generic message to the user, details in the logs
except Exception:
    logger.exception("Database error for user %s", user_id)
    return "Something went wrong. Please try again."
```

- Do not show stack traces, SQL, or file paths to users.
- Do not log secrets or personal data.
- Use the **same** error message for "unknown user" and "wrong password" to avoid revealing which accounts exist.

### 15. Safe deserialization

```python
import json
import pickle

data = json.loads(untrusted_text)       # safe: only produces basic types

# pickle.loads(untrusted_bytes)         # can run attacker code. NEVER on untrusted data
```

```python
import yaml
config = yaml.safe_load(text)           # not yaml.load()
```

### 16. Denial of service (resource limits)

- Limit request and upload sizes.
- Set timeouts on all network calls.
- Limit recursion depth and loop counts that depend on input.
- Beware regular expressions with catastrophic backtracking (Chapter 9).
- Rate-limit endpoints.

### 17. Principle of least privilege

- Run services as a normal user, not root or Administrator.
- Give database accounts only the permissions they need (for example, read-only for reports).
- Restrict file permissions.
- In containers, avoid running as root.

### 18. Security tools for Python

| Tool | Purpose |
|---|---|
| **bandit** | Scans your code for common security mistakes |
| **pip-audit** / **safety** | Checks dependencies for known vulnerabilities |
| **semgrep** | Pattern-based code scanning |
| **detect-secrets** / **gitleaks** | Finds committed secrets |
| **ruff** | Linter with security (`S`) rules |
| **mypy / pyright** | Type errors that can hide bugs |
| **Dependabot** | Automated dependency alerts |

```bash
pip install bandit
bandit -r src/
```

Example finding:

```
>> Issue: [B602:subprocess_popen_with_shell_equals_true] subprocess call with shell=True
   Severity: High
```

### 19. Secure coding checklist

- [ ] All user input is validated (type, length, range, format).
- [ ] Database queries are parameterized.
- [ ] No `eval`, `exec`, `pickle`, or `shell=True` on untrusted data.
- [ ] Passwords are hashed with Argon2, bcrypt, or scrypt.
- [ ] Secrets are in environment variables or a vault, not in code or Git.
- [ ] HTTPS is used everywhere; certificates are verified.
- [ ] Dependencies are pinned, audited, and updated.
- [ ] Authorization is checked server-side on every request.
- [ ] Errors do not leak internals; secrets are not logged.
- [ ] Debug mode is off in production.
- [ ] Uploaded files are validated and stored safely.
- [ ] Timeouts, size limits, and rate limits are in place.
- [ ] Security tools (bandit, pip-audit) run in CI.

### Real-world example: secure user registration

```python
import re
import sqlite3
from argon2 import PasswordHasher

ph = PasswordHasher()
USERNAME = re.compile(r"^[A-Za-z0-9_]{3,30}$")

def register(conn, username, password):
    if not USERNAME.fullmatch(username):
        raise ValueError("Username must be 3-30 letters, digits, or underscores")
    if len(password) < 12:
        raise ValueError("Password must be at least 12 characters")

    password_hash = ph.hash(password)

    try:
        with conn:
            conn.execute(
                "INSERT INTO users (username, password_hash) VALUES (?, ?)",
                (username, password_hash),
            )
    except sqlite3.IntegrityError:
        raise ValueError("That username is already taken")

def login(conn, username, password):
    row = conn.execute(
        "SELECT password_hash FROM users WHERE username = ?", (username,)
    ).fetchone()

    try:
        if row and ph.verify(row[0], password):
            return True
    except Exception:
        pass
    return False        # same result for unknown user and wrong password
```

---

## 10.7 Performance Optimization

**Performance optimization** makes code faster or lighter on memory. The most important rule comes first.

> **Make it work, make it right, then make it fast, and only if you need to.**
> "Premature optimization is the root of all evil." (Donald Knuth)

### The optimization workflow

```
1. Measure     →  Is it actually slow? Where?
2. Profile     →  Find the real bottleneck (often 5% of the code takes 95% of the time)
3. Improve     →  Change one thing
4. Re-measure  →  Did it help? By how much?
5. Stop        →  When it is fast enough
```

Never guess. Measure.

### Step 1: Measure time

**`time.perf_counter()`**

```python
import time

start = time.perf_counter()
result = sum(i * i for i in range(1_000_000))
elapsed = time.perf_counter() - start
print(f"Took {elapsed:.4f}s")
```

**`timeit`**: accurate for small snippets (runs many times).

```python
import timeit

print(timeit.timeit("sum(range(1000))", number=10_000))

print(timeit.timeit(
    "[x * 2 for x in data]",
    setup="data = list(range(1000))",
    number=5_000,
))
```

```bash
python -m timeit -s "data = list(range(1000))" "[x*2 for x in data]"
```

**Timing decorator**

```python
import time
from functools import wraps

def timed(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__}: {time.perf_counter() - start:.4f}s")
        return result
    return wrapper
```

### Step 2: Profile

**`cProfile`**: which functions use the most time?

```bash
python -m cProfile -s cumulative main.py
python -m cProfile -o out.prof main.py
```

```python
import cProfile
import pstats

with cProfile.Profile() as pr:
    main()

stats = pstats.Stats(pr)
stats.sort_stats("cumulative").print_stats(10)
```

Sample output:

```
ncalls  tottime  cumtime  filename:lineno(function)
  1000    0.002    2.500  main.py:10(process)
 50000    2.400    2.400  main.py:25(slow_lookup)
```

Look for functions with high `cumtime` (total time including callees) or high `ncalls`.

**Visualize**

```bash
pip install snakeviz
snakeviz out.prof
```

**Line-by-line**

```bash
pip install line_profiler
```

```python
@profile                  # added by kernprof at run time
def process(data):
    ...
```

```bash
kernprof -l -v script.py
```

**Sampling profiler (low overhead)**

```bash
pip install py-spy
py-spy top --pid 12345
py-spy record -o profile.svg -- python main.py
```

### Step 3: Memory profiling

```python
import sys

print(sys.getsizeof([1, 2, 3]))               # bytes for the list object itself
```

```python
import tracemalloc

tracemalloc.start()

data = [str(i) * 10 for i in range(100_000)]

current, peak = tracemalloc.get_traced_memory()
print(f"Current: {current / 1e6:.1f} MB, peak: {peak / 1e6:.1f} MB")

snapshot = tracemalloc.take_snapshot()
for stat in snapshot.statistics("lineno")[:3]:
    print(stat)
```

```bash
pip install memory_profiler
python -m memory_profiler script.py
```

### Big-O: algorithm choice matters most

Choosing the right algorithm and data structure beats micro-tuning.

| Notation | Name | Example |
|---|---|---|
| O(1) | Constant | Dict lookup, list index |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Loop through a list |
| O(n log n) | Linearithmic | `sorted()` |
| O(n²) | Quadratic | Nested loops over the same data |
| O(2ⁿ) | Exponential | Naive recursive Fibonacci |

For 1,000,000 items: O(n) is about 1 million steps; O(n²) is about 1 trillion.

### Choose the right data structure

| Operation | `list` | `set` / `dict` | `deque` |
|---|---|---|---|
| Check `x in collection` | O(n) | **O(1)** | O(n) |
| Append at end | O(1) | O(1) | O(1) |
| Insert/remove at start | O(n) | n/a | **O(1)** |
| Index access | O(1) | by key O(1) | O(n) in the middle |

```python
import timeit

setup = "items = list(range(100_000)); items_set = set(items)"
print(timeit.timeit("99_999 in items", setup=setup, number=1_000))        # slow
print(timeit.timeit("99_999 in items_set", setup=setup, number=1_000))    # very fast
```

**Use `set` for membership tests**

```python
# Slow: O(n * m)
common = [x for x in list_a if x in list_b]

# Fast: O(n + m)
set_b = set(list_b)
common = [x for x in list_a if x in set_b]
```

**Use `deque` for queues**

```python
from collections import deque

queue = deque()
queue.append(1)
queue.popleft()          # O(1); list.pop(0) is O(n)
```

**Use `collections`**

```python
from collections import Counter, defaultdict

counts = Counter(words)                       # instead of manual dict counting
groups = defaultdict(list)
groups["a"].append(1)
```

**Binary search on sorted data**

```python
import bisect

data = [1, 3, 5, 7, 9]
i = bisect.bisect_left(data, 7)               # O(log n)
bisect.insort(data, 4)
```

**Heaps for "top N"**

```python
import heapq

print(heapq.nlargest(3, [5, 1, 9, 3, 7]))     # [9, 7, 5]
print(heapq.nsmallest(3, [5, 1, 9, 3, 7]))    # [1, 3, 5]
```

### Python-level speed tips

#### 1. Use built-ins and the standard library

They are written in C and highly optimized.

```python
# Slow
total = 0
for n in numbers:
    total += n

# Fast
total = sum(numbers)

max(numbers), min(numbers), sorted(numbers), any(...), all(...)
```

#### 2. Comprehensions beat manual loops

```python
# Slower
result = []
for n in range(1000):
    result.append(n * 2)

# Faster
result = [n * 2 for n in range(1000)]
```

#### 3. Build strings with `join`

```python
# Slow: creates a new string every time
text = ""
for word in words:
    text += word + " "

# Fast
text = " ".join(words)
```

#### 4. Avoid repeated work inside loops

```python
# Slow: len() and attribute lookups each time
for i in range(len(items)):
    result.append(math.sqrt(items[i]))

# Faster
sqrt = math.sqrt
append = result.append
for x in items:
    append(sqrt(x))
```

Move anything that does not change **outside** the loop.

```python
# Slow
for row in rows:
    pattern = re.compile(r"\d+")      # recompiled every time
    ...

# Fast
pattern = re.compile(r"\d+")
for row in rows:
    ...
```

#### 5. Local variables are faster than globals

```python
def compute():
    local_pow = pow
    return [local_pow(x, 2) for x in range(1000)]
```

#### 6. Use generators for large data

```python
# Uses lots of memory
total = sum([x * x for x in range(10_000_000)])

# Constant memory
total = sum(x * x for x in range(10_000_000))
```

#### 7. Avoid unnecessary copies

```python
for x in data:            # good
for x in data[:]:         # needless copy
for x in list(data):      # needless copy
```

Slicing `data[1:]` copies the data. Use `itertools.islice` for large sequences.

#### 8. Use `enumerate`, `zip`, `dict.items()`

```python
for i, item in enumerate(items): ...
for a, b in zip(list1, list2): ...
for key, value in d.items(): ...
```

#### 9. Prefer `in` on sets, EAFP for rare errors

```python
# When the key usually exists, try/except is fast
try:
    value = d[key]
except KeyError:
    value = default

# When misses are common, use get
value = d.get(key, default)
```

#### 10. `__slots__` to save memory with many objects

```python
class Point:
    __slots__ = ("x", "y")
    def __init__(self, x, y):
        self.x, self.y = x, y
```

#### 11. Avoid global lookups for functions in hot loops, and avoid unneeded attribute access

Only do this micro-tuning in code the profiler proves is hot.

### Caching (memoization)

```python
from functools import lru_cache, cache

@lru_cache(maxsize=1024)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

print(fib(100))
print(fib.cache_info())

@cache                         # unlimited (3.9+)
def expensive(x):
    ...
```

```python
import timeit

def fib_slow(n):
    return n if n < 2 else fib_slow(n - 1) + fib_slow(n - 2)

print(timeit.timeit(lambda: fib_slow(25), number=1))       # slow
print(timeit.timeit(lambda: fib(25), number=1))            # near-instant
```

Cache API responses, database lookups, and computed results. Remember to **expire** caches when data changes.

### Dynamic programming

```python
# Naive: O(2^n)
def fib_naive(n):
    return n if n < 2 else fib_naive(n - 1) + fib_naive(n - 2)

# Bottom-up: O(n), O(1) memory
def fib_fast(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

### Faster I/O

**Read and write in bulk**

```python
# Slow: many tiny writes
for line in lines:
    f.write(line + "\n")

# Faster
f.write("\n".join(lines))
```

**Iterate files lazily**

```python
with open("big.txt", encoding="utf-8") as f:
    for line in f:
        process(line)
```

**Database**

```python
# Slow: one query per item (the "N+1 problem")
for user_id in ids:
    cur.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# Fast: one query
placeholders = ",".join("?" * len(ids))
cur.execute(f"SELECT * FROM users WHERE id IN ({placeholders})", ids)

# Fast: batch inserts in one transaction
cur.executemany("INSERT INTO t VALUES (?, ?)", rows)
conn.commit()
```

Add indexes for columns you filter on (see 10.4).

**Network**

- Reuse connections with `requests.Session`.
- Make requests in parallel (threads or asyncio).
- Compress and paginate large responses.
- Cache results.

### Concurrency for speed (see Chapter 9)

| Bottleneck | Tool |
|---|---|
| Waiting on network or disk (I/O-bound) | `asyncio`, `ThreadPoolExecutor` |
| Heavy computation (CPU-bound) | `multiprocessing`, `ProcessPoolExecutor` |
| Numeric arrays | NumPy (vectorization) |

### NumPy and vectorization

Replace Python loops over numbers with array operations (done in fast C code).

```python
import numpy as np
import timeit

def python_loop():
    return [x * 2 + 1 for x in range(1_000_000)]

arr = np.arange(1_000_000)
def numpy_version():
    return arr * 2 + 1

print(timeit.timeit(python_loop, number=10))
print(timeit.timeit(numpy_version, number=10))       # often 20-100x faster
```

```python
import pandas as pd

# Slow: row-by-row
for i, row in df.iterrows():
    df.loc[i, "total"] = row["price"] * row["qty"]

# Fast: vectorized
df["total"] = df["price"] * df["qty"]
```

### Compile or extend

| Tool | What it does |
|---|---|
| **Numba** | JIT-compiles numeric Python functions with a decorator (`@njit`) |
| **Cython** | Compile Python-like code to C |
| **PyPy** | Alternative Python interpreter with a JIT; speeds up many pure-Python programs |
| **C extensions / Rust (PyO3)** | Write the slow part in a fast language |
| **Python 3.11+** | Already 10 to 60% faster than 3.10 in many cases; upgrade! |
| **Python 3.13+** | Experimental JIT and free-threaded builds |

```python
from numba import njit

@njit
def sum_squares(n):
    total = 0
    for i in range(n):
        total += i * i
    return total

print(sum_squares(10_000_000))      # very fast after the first call
```

### Memory optimization

```python
# Use generators instead of lists
lines = (line for line in open("big.txt"))

# Use array / NumPy for large numeric data
from array import array
nums = array("i", range(1_000_000))      # much smaller than a list of ints

# Delete large objects you no longer need
del big_list

# Use __slots__ for many small objects
# Use smaller types (float32 instead of float64) in NumPy
# Process data in chunks
import pandas as pd
for chunk in pd.read_csv("big.csv", chunksize=100_000):
    process(chunk)
```

**Garbage collection**

```python
import gc

gc.collect()
print(gc.get_count())
```

### Common slow patterns to avoid

| Slow pattern | Better |
|---|---|
| `x in list` repeated in a loop | Convert to `set` first |
| `s += "text"` in a loop | `"".join(parts)` |
| `list.insert(0, x)` / `list.pop(0)` repeatedly | `collections.deque` |
| `for i in range(len(x))` | `for item in x` or `enumerate` |
| Re-compiling regex in a loop | `re.compile` once |
| Sorting repeatedly | Sort once; use `bisect` or `heapq` |
| `df.iterrows()` | Vectorized pandas |
| Opening and closing a file/connection per item | Open once; reuse |
| Querying a database inside a loop | One query or a join |
| Deep recursion without cache | `@lru_cache` or iteration |
| Converting types repeatedly | Convert once |

### Real-world example: optimizing a function

**Version 1: slow (O(n²))**

```python
def find_duplicates(items):
    duplicates = []
    for i in range(len(items)):
        for j in range(i + 1, len(items)):
            if items[i] == items[j] and items[i] not in duplicates:
                duplicates.append(items[i])
    return duplicates
```

**Version 2: fast (O(n))**

```python
def find_duplicates(items):
    seen = set()
    duplicates = set()
    for item in items:
        if item in seen:
            duplicates.add(item)
        else:
            seen.add(item)
    return list(duplicates)
```

**Compare**

```python
import random
import timeit

data = [random.randint(0, 5_000) for _ in range(10_000)]

print(timeit.timeit(lambda: find_duplicates_v1(data), number=1))     # seconds
print(timeit.timeit(lambda: find_duplicates_v2(data), number=1))     # milliseconds
```

### Performance testing in practice

```bash
pip install pytest-benchmark
```

```python
def test_find_duplicates_speed(benchmark):
    data = list(range(10_000)) * 2
    result = benchmark(find_duplicates, data)
    assert len(result) == 10_000
```

Run benchmarks in CI to detect slowdowns early.

### Performance checklist

- [ ] Measured first; found the real bottleneck.
- [ ] Chose an efficient algorithm and data structure.
- [ ] Used built-ins, comprehensions, and the standard library.
- [ ] Avoided repeated work inside loops; used caching where valuable.
- [ ] Batched database and network operations; added indexes.
- [ ] Used the right concurrency model (async, threads, processes).
- [ ] Vectorized numeric work with NumPy or pandas.
- [ ] Re-measured to confirm the improvement.
- [ ] Kept the code readable. A fast but unreadable function is a future bug.

---

## 10.8 Packaging and Deployment

**Packaging** turns your code into something others can install (`pip install yourpackage`). **Deployment** puts your application somewhere it can run for users.

### Part A: Project structure

A clean layout for a library or application:

```
my_project/
├── src/
│   └── my_package/
│       ├── __init__.py
│       ├── core.py
│       ├── cli.py
│       └── data/
│           └── defaults.json
├── tests/
│   ├── conftest.py
│   └── test_core.py
├── docs/
├── pyproject.toml
├── README.md
├── LICENSE
├── .gitignore
└── requirements.txt          (optional, for applications)
```

The **`src/` layout** prevents accidentally importing your local folder instead of the installed package, which catches packaging mistakes early.

### Modules, packages, and `__init__.py`

```python
# src/my_package/__init__.py
"""My package: does useful things."""

from .core import greet, add

__version__ = "0.1.0"
__all__ = ["greet", "add"]
```

```python
# src/my_package/core.py
def greet(name: str) -> str:
    return f"Hello, {name}!"

def add(a: int, b: int) -> int:
    return a + b
```

### `pyproject.toml`: the modern standard

All packaging settings live in one file.

```toml
[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "my-package"
version = "0.1.0"
description = "A short description of what it does"
readme = "README.md"
requires-python = ">=3.10"
license = { text = "MIT" }
authors = [{ name = "Your Name", email = "you@example.com" }]
keywords = ["example", "demo"]
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]
dependencies = [
    "requests>=2.28",
    "click>=8.0",
]

[project.optional-dependencies]
dev = ["pytest>=8", "ruff", "mypy", "build", "twine"]

[project.scripts]
my-tool = "my_package.cli:main"          # creates a command-line program

[project.urls]
Homepage = "https://github.com/you/my-package"
Issues = "https://github.com/you/my-package/issues"

[tool.setuptools.packages.find]
where = ["src"]

[tool.setuptools.package-data]
my_package = ["data/*.json"]

[tool.pytest.ini_options]
testpaths = ["tests"]
```

Other build backends: **hatchling** (Hatch), **flit**, **poetry-core**, **pdm-backend**.

### Versioning

Use **semantic versioning**: `MAJOR.MINOR.PATCH`

| Part | Change when |
|---|---|
| `MAJOR` (1.0.0 → 2.0.0) | Breaking changes |
| `MINOR` (1.1.0 → 1.2.0) | New features, backward compatible |
| `PATCH` (1.1.1 → 1.1.2) | Bug fixes |

Pre-releases: `1.0.0a1`, `1.0.0b2`, `1.0.0rc1`.

### Dependencies

| Style | Example | Meaning |
|---|---|---|
| Exact | `requests==2.31.0` | Only this version |
| Minimum | `requests>=2.28` | This or newer |
| Compatible | `requests~=2.31` | `>=2.31, <3.0` |
| Range | `requests>=2.28,<3` | Between |

- **Libraries:** use **loose** ranges in `pyproject.toml`, so users can combine your package with others.
- **Applications:** use **pinned** versions (a lock file), so every deployment is identical.

```bash
pip freeze > requirements.txt          # simple lock
pip install pip-tools
pip-compile pyproject.toml             # generates a fully pinned requirements.txt
```

### A command-line interface

```python
# src/my_package/cli.py
import argparse
from .core import greet

def main(argv=None):
    parser = argparse.ArgumentParser(description="Greeting tool")
    parser.add_argument("name", help="Name to greet")
    parser.add_argument("-u", "--upper", action="store_true", help="Shout")
    args = parser.parse_args(argv)

    message = greet(args.name)
    print(message.upper() if args.upper else message)

if __name__ == "__main__":
    main()
```

After installing, `my-tool Ali --upper` works from the terminal.

Popular CLI libraries: **`click`**, **`typer`** (built on type hints), **`argparse`** (standard library).

```python
# with typer: pip install typer
import typer

app = typer.Typer()

@app.command()
def greet(name: str, upper: bool = False):
    msg = f"Hello, {name}!"
    typer.echo(msg.upper() if upper else msg)

if __name__ == "__main__":
    app()
```

### Build the package

```bash
pip install build
python -m build
```

This creates `dist/`:

```
dist/
├── my_package-0.1.0.tar.gz               (source distribution, sdist)
└── my_package-0.1.0-py3-none-any.whl     (wheel, ready to install)
```

### Test it locally

```bash
python -m venv /tmp/testenv
source /tmp/testenv/bin/activate
pip install dist/my_package-0.1.0-py3-none-any.whl
python -c "import my_package; print(my_package.__version__)"
my-tool Ali
```

During development, use an **editable install**: code changes take effect without reinstalling.

```bash
pip install -e ".[dev]"
```

### Publish to PyPI

```bash
pip install twine
twine check dist/*
twine upload --repository testpypi dist/*      # practice on TestPyPI first
twine upload dist/*                             # the real PyPI
```

- Create accounts at pypi.org and test.pypi.org.
- Use **API tokens**, not your password.
- Package names are first come, first served. Check that yours is free.
- **Versions cannot be re-uploaded.** Increase the version for every release.
- Modern approach: **Trusted Publishing** from GitHub Actions (no tokens to manage).

```yaml
# .github/workflows/publish.yml
name: Publish

on:
  release:
    types: [published]

jobs:
  pypi:
    runs-on: ubuntu-latest
    environment: pypi
    permissions:
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install build
      - run: python -m build
      - uses: pypa/gh-action-pypi-publish@release/v1
```

### Modern all-in-one tools

| Tool | Handles |
|---|---|
| **uv** | Very fast: virtual environments, installs, lock files, running scripts, building, publishing |
| **Poetry** | Dependencies, lock file, build, publish |
| **Hatch** | Environments, build, publish, versioning |
| **PDM** | Dependencies and packaging |

```bash
# uv
pip install uv
uv init my_project
uv add requests
uv run python main.py
uv build
uv publish
```

```bash
# Poetry
pip install poetry
poetry new my_project
poetry add requests
poetry install
poetry build
poetry publish
```

### Standalone executables

Share your program with people who do not have Python.

```bash
pip install pyinstaller
pyinstaller --onefile main.py              # creates dist/main (or main.exe)
pyinstaller --onefile --windowed gui.py    # no console window (GUI apps)
```

Alternatives: **Nuitka** (compiles to C), **cx_Freeze**, **briefcase** (desktop and mobile), **PyOxidizer**, **shiv/zipapp** (single-file Python archives).

```bash
python -m zipapp my_app -m "main:run" -o my_app.pyz
python my_app.pyz
```

### Part B: Deployment

#### Deployment options

| Option | Good for | Examples |
|---|---|---|
| **PaaS** (Platform as a Service) | Easiest; push code, it runs | Render, Railway, Fly.io, Heroku, Google App Engine |
| **Containers** | Consistent environments | Docker + any cloud |
| **Serverless** | Small functions, pay per use | AWS Lambda, Google Cloud Functions, Azure Functions |
| **Virtual machine (VPS)** | Full control | AWS EC2, DigitalOcean, Linode, Hetzner |
| **Kubernetes** | Large, scalable systems | EKS, GKE, AKS |
| **Static + API split** | Frontend and backend separate | Vercel/Netlify + a Python API |

#### Preparing an application for deployment

**1. Configuration through environment variables** (twelve-factor style)

```python
import os

DEBUG = os.getenv("DEBUG", "false").lower() == "true"
DATABASE_URL = os.environ["DATABASE_URL"]
SECRET_KEY = os.environ["SECRET_KEY"]
PORT = int(os.getenv("PORT", "8000"))
```

Never hard-code secrets or environment-specific settings. Never commit `.env`.

**2. Pin dependencies**

```bash
pip freeze > requirements.txt
# or: uv pip compile pyproject.toml -o requirements.txt
```

**3. Add a health check endpoint**

```python
@app.get("/health")
def health():
    return {"status": "ok"}
```

**4. Log to standard output** (platforms collect it automatically).

```python
logging.basicConfig(level=logging.INFO, stream=sys.stdout)
```

**5. Use a production server, not the development server**

| Framework | Development | Production |
|---|---|---|
| Flask / Django (WSGI) | `flask run`, `runserver` | **gunicorn**, **uWSGI**, **waitress** (Windows) |
| FastAPI / Starlette (ASGI) | `uvicorn --reload` | **uvicorn** workers, **gunicorn** + uvicorn workers, **hypercorn** |

```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8000 myapp:app                  # Flask
gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app   # FastAPI
```

A common setup puts **Nginx** (or a cloud load balancer) in front of the app for HTTPS, static files, and load balancing.

```
Internet  →  Nginx (HTTPS)  →  Gunicorn  →  Your Python app  →  Database
```

#### Docker

A **container** packages your app with everything it needs, so it runs the same everywhere.

**`Dockerfile`**

```dockerfile
FROM python:3.12-slim

# Don't write .pyc files; show output immediately
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Install dependencies first (cached unless requirements.txt changes)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the application
COPY . .

# Run as a non-root user
RUN useradd --create-home appuser
USER appuser

EXPOSE 8000

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:8000", "myapp:app"]
```

**`.dockerignore`**

```
.git
.venv
__pycache__
*.pyc
.env
tests
```

**Commands**

```bash
docker build -t my-app .
docker run -p 8000:8000 --env-file .env my-app
docker ps
docker logs <container>
docker stop <container>
```

Visit `http://localhost:8000`.

**Multi-stage build** (smaller images)

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH
CMD ["python", "main.py"]
```

**Docker Compose**: app plus database

```yaml
# docker-compose.yml
services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://postgres:secret@db:5432/app
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: app
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

```bash
docker compose up --build
docker compose down
```

> Do not put real passwords in Compose files that are committed. Use `.env` files or secrets.

#### Deploying to a Platform as a Service

Typical steps (Render, Railway, Fly.io, Heroku and similar):

1. Push your code to GitHub.
2. Create a new service on the platform and connect your repo.
3. Set the **build command** (`pip install -r requirements.txt`) and **start command** (`gunicorn myapp:app`).
4. Add **environment variables** (secrets, `DATABASE_URL`).
5. Deploy; the platform provides an HTTPS URL.
6. Enable **auto-deploy** on pushes to `main`.

**`Procfile`** (Heroku style)

```
web: gunicorn myapp:app
```

#### Serverless example (AWS Lambda)

```python
# lambda_function.py
import json

def lambda_handler(event, context):
    name = (event.get("queryStringParameters") or {}).get("name", "world")
    return {
        "statusCode": 200,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps({"message": f"Hello, {name}!"}),
    }
```

Serverless suits small, event-driven tasks. Watch out for cold starts and package size limits.

#### Virtual machine (VPS) basics

```bash
ssh user@server-ip

sudo apt update && sudo apt install -y python3-venv nginx
git clone https://github.com/you/my-app.git
cd my-app
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Run it as a background service with **systemd**:

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Python App
After=network.target

[Service]
User=appuser
WorkingDirectory=/home/appuser/my-app
EnvironmentFile=/home/appuser/my-app/.env
ExecStart=/home/appuser/my-app/.venv/bin/gunicorn -w 4 -b 127.0.0.1:8000 myapp:app
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now myapp
sudo systemctl status myapp
journalctl -u myapp -f
```

Then configure Nginx as a reverse proxy and get a free HTTPS certificate with **Let's Encrypt** (`certbot`).

### CI/CD: automate testing and deployment

```
git push  →  CI: lint + test + build  →  CD: deploy automatically
```

```yaml
# .github/workflows/ci.yml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
          cache: pip
      - run: pip install -r requirements.txt pytest ruff bandit pip-audit
      - run: ruff check .
      - run: bandit -r src -q
      - run: pip-audit -r requirements.txt
      - run: pytest --cov=src

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy
        run: ./deploy.sh
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

Store secrets in **GitHub Settings, Secrets and variables, Actions**, never in the workflow file.

### Environments

| Environment | Purpose |
|---|---|
| **Development** | Your computer; debug on |
| **Testing / CI** | Automated tests |
| **Staging** | A copy of production for final checks |
| **Production** | Real users; debug off, monitoring on |

Keep them as similar as possible. Use different databases and secrets for each.

### Database changes during deployment

- Use **migrations** (Alembic, Django migrations).
- Back up before a migration.
- Make changes backward compatible (add columns before removing old ones).
- Test migrations on staging first.

### Monitoring and maintenance

| Need | Tools |
|---|---|
| **Logs** | Platform logs, ELK, Loki, Datadog |
| **Error tracking** | Sentry |
| **Metrics** | Prometheus + Grafana, Datadog |
| **Uptime checks** | UptimeRobot, Pingdom, the platform's health checks |
| **Alerts** | Email, Slack, PagerDuty |

```bash
pip install sentry-sdk
```

```python
import sentry_sdk

sentry_sdk.init(dsn=os.environ["SENTRY_DSN"], traces_sample_rate=0.1)
```

### Zero-downtime and rollback ideas

- **Rolling deploys:** replace instances one at a time.
- **Blue/green:** run the new version beside the old, then switch traffic.
- **Rollback plan:** always be able to return to the previous version quickly. Keep old images and tags.
- **Feature flags:** release code switched off, enable it gradually.

### Documentation for others

- **README.md:** what it is, how to install, how to use.
- **Docstrings** in code. Tools such as **Sphinx** or **MkDocs** build documentation sites.
- **CHANGELOG.md:** what changed in each version.
- **LICENSE:** without one, others have no right to use your code. Common choices: MIT, Apache-2.0, GPL-3.0.
- **CONTRIBUTING.md:** how to help.

### Pre-deployment checklist

- [ ] Tests pass; coverage is acceptable.
- [ ] Debug mode is off; production settings are used.
- [ ] Secrets are in environment variables or a vault.
- [ ] Dependencies are pinned and audited.
- [ ] A production server (gunicorn/uvicorn) is used.
- [ ] HTTPS is enabled.
- [ ] Logging goes to stdout or a log service; errors are tracked.
- [ ] A health check endpoint exists.
- [ ] Database is backed up; migrations are tested.
- [ ] A rollback plan exists.
- [ ] README and license are included.

### Real-world example: package and ship a small CLI tool

```
word_counter/
├── src/
│   └── word_counter/
│       ├── __init__.py
│       └── cli.py
├── tests/
│   └── test_cli.py
├── pyproject.toml
└── README.md
```

**`src/word_counter/cli.py`**

```python
import argparse
import re
import sys
from collections import Counter
from pathlib import Path

def count_words(text):
    return Counter(re.findall(r"[a-z']+", text.lower()))

def main(argv=None):
    parser = argparse.ArgumentParser(description="Count the most common words in a file")
    parser.add_argument("file", type=Path)
    parser.add_argument("-n", "--top", type=int, default=5)
    args = parser.parse_args(argv)

    if not args.file.exists():
        print(f"File not found: {args.file}", file=sys.stderr)
        return 1

    counts = count_words(args.file.read_text(encoding="utf-8"))
    for word, n in counts.most_common(args.top):
        print(f"{n:>6}  {word}")
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
name = "word-counter"
version = "0.1.0"
description = "Count the most common words in a text file"
readme = "README.md"
requires-python = ">=3.10"

[project.scripts]
wordcount = "word_counter.cli:main"

[tool.setuptools.packages.find]
where = ["src"]
```

**`tests/test_cli.py`**

```python
from word_counter.cli import count_words, main

def test_count_words():
    assert count_words("The cat and the hat").most_common(1) == [("the", 2)]

def test_missing_file(tmp_path):
    assert main([str(tmp_path / "nope.txt")]) == 1
```

**Build, install, and run**

```bash
pip install -e ".[dev]"      # or: pip install -e .
pytest
python -m build
pip install dist/word_counter-0.1.0-py3-none-any.whl
wordcount notes.txt --top 3
```

---

## Chapter Summary

| Topic | Key points |
|---|---|
| Testing | `unittest` (built in) and `pytest` (popular); fixtures, parametrization, mocking; measure coverage; run tests automatically |
| Git and GitHub | Commit small, branch for features, pull requests for review; `.gitignore`; never commit secrets; CI with GitHub Actions |
| APIs | `requests` with `timeout` and `raise_for_status()`; headers, auth, pagination, rate limits; build APIs with FastAPI or Flask |
| SQL and databases | `sqlite3` basics; CRUD; **parameterized queries**; transactions; joins; indexes; ORMs like SQLAlchemy |
| Logging | Levels, loggers per module, handlers, rotation, structured logs; use `logger.exception`; never log secrets |
| Security | Validate input, parameterize SQL, hash passwords (Argon2), manage secrets, audit dependencies, avoid `eval`/`pickle`/`shell=True` |
| Performance | Measure and profile first; choose better algorithms and data structures; caching; batching; NumPy; concurrency |
| Packaging and deployment | `pyproject.toml`, `python -m build`, PyPI; Docker; gunicorn/uvicorn; CI/CD; monitoring; rollback plan |

### Quick command cheat sheet

```bash
# Testing
pytest -v -x -k "name"
pytest --cov=src --cov-report=term-missing

# Git
git status; git add .; git commit -m "Message"
git switch -c feature; git push -u origin feature
git pull --rebase; git log --oneline --graph

# Environments and packages
python -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"
pip-audit; bandit -r src

# Profiling
python -m cProfile -s cumulative main.py
python -m timeit "sum(range(1000))"

# Build and publish
python -m build
twine upload dist/*

# Docker
docker build -t my-app . && docker run -p 8000:8000 my-app
```

```python
# API call template
r = requests.get(url, params=params, headers=headers, timeout=10)
r.raise_for_status()
data = r.json()

# Safe SQL
cur.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# Logging
logger = logging.getLogger(__name__)
logger.info("Processed %d items", n)
logger.exception("Operation failed")

# Secrets
api_key = os.environ["API_KEY"]
token = secrets.token_urlsafe(32)
```

---

## Practice Exercises

### Beginner

1. Write three `pytest` tests for a function `is_palindrome(text)`: a true case, a false case, and an empty string.
2. Initialize a Git repository, create a `.gitignore`, make two commits, and view the history with `git log --oneline`.
3. Call `https://jsonplaceholder.typicode.com/users` with `requests` and print each user's name and email.
4. Create an SQLite database with a `books` table (`id`, `title`, `author`, `year`) and insert three books using parameters.
5. Configure `logging` so messages of level `INFO` and above print to the console with a timestamp.

### Intermediate

6. Write parametrized `pytest` tests for a `grade(marks)` function with at least 8 input/output pairs, including the boundary values.
7. Create a feature branch, make a change, merge it into `main`, and then deliberately cause and resolve a merge conflict.
8. Write a function that fetches a user from an API, with a timeout, `raise_for_status()`, and clear handling for 404, timeouts, and connection errors. Test it using `unittest.mock.patch`.
9. Build a `LibraryDB` class using `sqlite3` with `add_book`, `find_by_author`, `delete_book`, and `list_all`. Use transactions and parameterized queries.
10. Add a rotating file handler (1 MB, 3 backups) to your logging, and log an exception with its traceback.
11. Write a function `register(username, password)` that validates input, hashes the password with Argon2 or `hashlib.scrypt`, and stores it in SQLite. Demonstrate that a SQL injection attempt fails.
12. Use `cProfile` and `timeit` to compare two implementations of "find the common elements of two lists" (nested loops vs sets).

### Challenge

13. Build a small REST API with FastAPI or Flask (`GET /todos`, `POST /todos`, `DELETE /todos/{id}`) backed by SQLite, with tests that use the framework's test client.
14. Create a GitHub Actions workflow that runs `ruff`, `pytest` with coverage, `bandit`, and `pip-audit` on every pull request, across two Python versions.
15. Write a paginated API client that fetches all pages lazily (as a generator), with retries and exponential backoff on `429` and `5xx` responses.
16. Package a command-line tool with `pyproject.toml` and a `[project.scripts]` entry, build the wheel, install it in a fresh virtual environment, and run the command.
17. Write a `Dockerfile` for a small web app (non-root user, pinned dependencies, `.dockerignore`) and a `docker-compose.yml` that adds a PostgreSQL database.
18. Review the following code and list every security problem:

```python
import sqlite3, os, pickle

SECRET = "abc123"

def login(conn, user, pw):
    q = f"SELECT * FROM users WHERE name='{user}' AND password='{pw}'"
    return conn.execute(q).fetchone()

def load_profile(data):
    return pickle.loads(data)

def run_report(name):
    os.system("generate_report " + name)

print(f"User logged in with password {pw}")
```

### Sample solution (Exercise 6)

```python
import pytest

def grade(marks):
    if marks < 0 or marks > 100:
        raise ValueError("Marks must be between 0 and 100")
    if marks >= 90: return "A+"
    if marks >= 80: return "A"
    if marks >= 70: return "B"
    if marks >= 60: return "C"
    return "F"

@pytest.mark.parametrize("marks, expected", [
    (100, "A+"), (90, "A+"), (89, "A"), (80, "A"),
    (79, "B"), (70, "B"), (69, "C"), (60, "C"),
    (59, "F"), (0, "F"),
])
def test_grade(marks, expected):
    assert grade(marks) == expected

@pytest.mark.parametrize("bad", [-1, 101])
def test_grade_invalid(bad):
    with pytest.raises(ValueError):
        grade(bad)
```

### Sample solution (Exercise 8)

```python
import requests

def get_user(user_id):
    try:
        r = requests.get(f"https://jsonplaceholder.typicode.com/users/{user_id}", timeout=5)
    except requests.exceptions.Timeout:
        raise RuntimeError("The server took too long to respond")
    except requests.exceptions.ConnectionError:
        raise RuntimeError("Could not connect to the server")

    if r.status_code == 404:
        return None
    r.raise_for_status()
    return r.json()
```

```python
# test_get_user.py
from unittest.mock import patch, Mock
import pytest
import requests
from users import get_user

@patch("users.requests.get")
def test_found(mock_get):
    mock_get.return_value = Mock(status_code=200, json=lambda: {"name": "Ali"},
                                 raise_for_status=lambda: None)
    assert get_user(1) == {"name": "Ali"}

@patch("users.requests.get")
def test_not_found(mock_get):
    mock_get.return_value = Mock(status_code=404)
    assert get_user(999) is None

@patch("users.requests.get", side_effect=requests.exceptions.Timeout)
def test_timeout(mock_get):
    with pytest.raises(RuntimeError, match="too long"):
        get_user(1)
```

### Sample solution (Exercise 11)

```python
import sqlite3
import re
import hashlib
import hmac
import os

def hash_password(password, salt=None):
    salt = salt or os.urandom(16)
    key = hashlib.scrypt(password.encode(), salt=salt, n=2**14, r=8, p=1)
    return salt.hex() + ":" + key.hex()

def verify_password(stored, password):
    salt_hex, key_hex = stored.split(":")
    salt = bytes.fromhex(salt_hex)
    new = hashlib.scrypt(password.encode(), salt=salt, n=2**14, r=8, p=1)
    return hmac.compare_digest(new.hex(), key_hex)

def register(conn, username, password):
    if not re.fullmatch(r"[A-Za-z0-9_]{3,30}", username):
        raise ValueError("Invalid username")
    if len(password) < 12:
        raise ValueError("Password too short")
    with conn:
        conn.execute("INSERT INTO users (username, password_hash) VALUES (?, ?)",
                     (username, hash_password(password)))

def login(conn, username, password):
    row = conn.execute("SELECT password_hash FROM users WHERE username = ?", (username,)).fetchone()
    return bool(row) and verify_password(row[0], password)

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE users (id INTEGER PRIMARY KEY, username TEXT UNIQUE, password_hash TEXT)")

register(conn, "ali_khan", "correct horse battery")
print(login(conn, "ali_khan", "correct horse battery"))     # True
print(login(conn, "ali_khan", "wrong password!!"))          # False
print(login(conn, "' OR '1'='1", "anything"))               # False: injection has no effect
```

### Sample solution (Exercise 12)

```python
import random
import timeit

a = [random.randint(0, 10_000) for _ in range(5_000)]
b = [random.randint(0, 10_000) for _ in range(5_000)]

def common_nested():
    return [x for x in a if x in b]          # O(n * m)

def common_set():
    set_b = set(b)
    return [x for x in a if x in set_b]      # O(n + m)

assert sorted(common_nested()) == sorted(common_set())

print("nested:", timeit.timeit(common_nested, number=3))
print("set:   ", timeit.timeit(common_set, number=3))
```

### Sample solution (Exercise 15)

```python
import time
import requests

def fetch_pages(url, per_page=50, max_retries=4):
    page = 1
    with requests.Session() as session:
        while True:
            for attempt in range(max_retries):
                r = session.get(url, params={"_page": page, "_limit": per_page}, timeout=10)
                if r.status_code == 429 or r.status_code >= 500:
                    wait = int(r.headers.get("Retry-After", 2 ** attempt))
                    time.sleep(wait)
                    continue
                r.raise_for_status()
                break
            else:
                raise RuntimeError(f"Giving up on page {page}")

            items = r.json()
            if not items:
                return
            yield from items
            page += 1

for post in fetch_pages("https://jsonplaceholder.typicode.com/posts"):
    print(post["id"], post["title"][:40])
```

### Sample solution (Exercise 18)

Problems found:

1. **SQL injection** in `login`: string formatting with user input. Use `?` parameters.
2. **Plain-text passwords** compared in SQL. Store and verify a salted hash (Argon2, bcrypt, scrypt).
3. **`pickle.loads` on external data** can execute arbitrary code. Use JSON.
4. **`os.system` with concatenated input** allows command injection. Use `subprocess.run([...])` without a shell, and validate `name`.
5. **Hard-coded `SECRET`** in source code. Load it from an environment variable or secrets manager.
6. **Logging the password** at the end. Never log secrets, and `pw` is not even defined at module level.

Fixed version:

```python
import os
import json
import subprocess
import logging

logger = logging.getLogger(__name__)
SECRET = os.environ["APP_SECRET"]

def login(conn, user, pw):
    row = conn.execute("SELECT password_hash FROM users WHERE name = ?", (user,)).fetchone()
    return bool(row) and verify_password(row[0], pw)

def load_profile(data: str):
    return json.loads(data)

def run_report(name):
    if not name.isalnum():
        raise ValueError("Invalid report name")
    subprocess.run(["generate_report", name], check=True)

logger.info("User login attempt for %s", "ali")
```

---

**Previous:** [Chapter 9: Advanced Python](chapter-09-advanced-python.md)
**Next:** [Chapter 11: Practice Projects](#)
