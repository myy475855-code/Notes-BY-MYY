# Chapter 7: Python File Handling

> Programs need to save and load data. This chapter shows how to open, read, write, and delete files, and how to work with paths and folders safely on any operating system.

## Table of Contents

- [7.1 Python File Handling](#71-python-file-handling)
- [7.2 Python Read Files](#72-python-read-files)
- [7.3 Python Write/Create Files](#73-python-writecreate-files)
- [7.4 Python Delete Files](#74-python-delete-files)
- [7.5 Python Paths and Directories](#75-python-paths-and-directories)
- [Chapter Summary](#chapter-summary)
- [Practice Exercises](#practice-exercises)

---

## 7.1 Python File Handling

### The `open()` function

**Syntax**

```python
open(file, mode="r", encoding=None)
```

| Parameter | Meaning |
|---|---|
| `file` | File name or path |
| `mode` | How to open it (read, write, append, ...) |
| `encoding` | Text encoding. Use `"utf-8"` to be safe. |

It returns a **file object** that you read from or write to.

### File modes

| Mode | Name | Behavior |
|---|---|---|
| `"r"` | Read (default) | Open for reading. Error if the file does not exist. |
| `"w"` | Write | Create the file, or **erase** it if it exists. |
| `"a"` | Append | Add to the end. Create the file if it does not exist. |
| `"x"` | Exclusive create | Create the file. Error if it already exists. |
| `"r+"` | Read and write | File must exist. Does not erase. |
| `"w+"` | Write and read | Erases the file first. |
| `"a+"` | Append and read | Add to the end, and read. |

| Add-on | Meaning |
|---|---|
| `"t"` | Text mode (default) |
| `"b"` | Binary mode (images, PDFs, zip files) |

```python
open("notes.txt")              # same as "rt"
open("notes.txt", "w")         # write text
open("photo.jpg", "rb")        # read binary
open("data.bin", "wb")         # write binary
```

> `"w"` deletes existing content immediately. Use `"a"` to keep it, or `"x"` to avoid overwriting by accident.

### The safe way: `with`

The `with` statement closes the file automatically, even if an error happens.

```python
with open("notes.txt", "r", encoding="utf-8") as f:
    content = f.read()

print(content)
# f is closed here
```

### Without `with` (manual close)

```python
f = open("notes.txt", encoding="utf-8")
try:
    print(f.read())
finally:
    f.close()
```

Always prefer `with`.

### File object attributes

```python
with open("notes.txt", encoding="utf-8") as f:
    print(f.name)       # notes.txt
    print(f.mode)       # r
    print(f.closed)     # False
    print(f.encoding)   # utf-8

print(f.closed)         # True
```

### Text vs binary

| | Text mode | Binary mode |
|---|---|---|
| Data type | `str` | `bytes` |
| Handles | Letters, numbers | Any file: images, audio, PDFs |
| Newlines | Converted to `\n` | Left as-is |
| Mode | `"r"`, `"w"` | `"rb"`, `"wb"` |

```python
with open("note.txt", "rb") as f:
    data = f.read()

print(type(data))    # <class 'bytes'>
```

### Encoding

Always set `encoding="utf-8"` for text. The default depends on the operating system and can cause errors with non-English text such as Urdu or emoji.

```python
with open("urdu.txt", "w", encoding="utf-8") as f:
    f.write("پاکستان زندہ باد")
```

### Create a sample file for the examples

```python
with open("notes.txt", "w", encoding="utf-8") as f:
    f.write("Line one\n")
    f.write("Line two\n")
    f.write("Line three\n")
```

### Common file errors

| Error | Cause |
|---|---|
| `FileNotFoundError` | File or folder does not exist |
| `PermissionError` | No permission to read or write |
| `IsADirectoryError` | The path is a folder, not a file |
| `FileExistsError` | Mode `"x"` used on an existing file |
| `UnicodeDecodeError` | Wrong encoding for the file |
| `ValueError: I/O operation on closed file` | Using the file after `with` ended |

```python
try:
    with open("missing.txt", encoding="utf-8") as f:
        print(f.read())
except FileNotFoundError:
    print("That file does not exist")
except PermissionError:
    print("No permission")
```

### Where does Python look for the file?

A relative path like `"notes.txt"` is looked up in the **current working directory**, which is not always the script's folder.

```python
import os
print(os.getcwd())
```

Build a path relative to the script instead:

```python
from pathlib import Path

BASE = Path(__file__).parent
file = BASE / "notes.txt"
```

(See 7.5 for more.)

---

## 7.2 Python Read Files

Assume `notes.txt` contains:

```
Line one
Line two
Line three
```

### `read()`: the whole file

```python
with open("notes.txt", encoding="utf-8") as f:
    text = f.read()

print(text)
```

```
Line one
Line two
Line three
```

### `read(n)`: the first n characters

```python
with open("notes.txt", encoding="utf-8") as f:
    print(f.read(8))    # Line one
```

### `readline()`: one line at a time

```python
with open("notes.txt", encoding="utf-8") as f:
    print(f.readline())    # Line one\n
    print(f.readline())    # Line two\n
```

Each call returns the next line, **including** the newline character. At the end of the file it returns an empty string `""`.

### `readlines()`: all lines as a list

```python
with open("notes.txt", encoding="utf-8") as f:
    lines = f.readlines()

print(lines)
# ['Line one\n', 'Line two\n', 'Line three\n']
```

### Loop through lines (best for large files)

```python
with open("notes.txt", encoding="utf-8") as f:
    for line in f:
        print(line.strip())
```

```
Line one
Line two
Line three
```

This reads **one line at a time**, so memory use stays low even for huge files.

### Removing the newline

```python
with open("notes.txt", encoding="utf-8") as f:
    lines = [line.rstrip("\n") for line in f]

print(lines)    # ['Line one', 'Line two', 'Line three']
```

Or:

```python
with open("notes.txt", encoding="utf-8") as f:
    lines = f.read().splitlines()
```

### Number the lines

```python
with open("notes.txt", encoding="utf-8") as f:
    for number, line in enumerate(f, start=1):
        print(f"{number}: {line.rstrip()}")
```

```
1: Line one
2: Line two
3: Line three
```

### Read the first N lines

```python
from itertools import islice

with open("notes.txt", encoding="utf-8") as f:
    for line in islice(f, 2):
        print(line.rstrip())
```

### Read the last line

```python
with open("notes.txt", encoding="utf-8") as f:
    lines = f.read().splitlines()

print(lines[-1])    # Line three
```

### Moving around: `tell()` and `seek()`

```python
with open("notes.txt", encoding="utf-8") as f:
    print(f.read(4))     # Line
    print(f.tell())      # 4  (current position)

    f.seek(0)            # go back to the start
    print(f.readline())  # Line one
```

### Read the whole file in chunks

```python
with open("big_file.txt", encoding="utf-8") as f:
    while chunk := f.read(1024):    # 1 KB at a time
        print(len(chunk))
```

### Check a file exists before reading

```python
import os

if os.path.exists("notes.txt"):
    with open("notes.txt", encoding="utf-8") as f:
        print(f.read())
else:
    print("File not found")
```

Or use `try...except FileNotFoundError` (often safer, since the file could disappear between the check and the open).

### Read binary files

```python
with open("photo.jpg", "rb") as f:
    data = f.read()

print(len(data), "bytes")
print(data[:4])    # first 4 bytes
```

**Copy a binary file**

```python
with open("photo.jpg", "rb") as src, open("copy.jpg", "wb") as dst:
    dst.write(src.read())
```

### Read with `pathlib` (shortcut)

```python
from pathlib import Path

text = Path("notes.txt").read_text(encoding="utf-8")
lines = text.splitlines()
data = Path("photo.jpg").read_bytes()
```

### Read CSV files

```python
import csv

with open("students.csv", newline="", encoding="utf-8") as f:
    reader = csv.reader(f)
    header = next(reader)
    for row in reader:
        print(row)
```

Given this `students.csv`:

```
name,age,city
Ali,25,Lahore
Sara,22,Karachi
```

Output:

```
['Ali', '25', 'Lahore']
['Sara', '22', 'Karachi']
```

**As dictionaries (easier)**

```python
import csv

with open("students.csv", newline="", encoding="utf-8") as f:
    for row in csv.DictReader(f):
        print(row["name"], row["city"])
```

```
Ali Lahore
Sara Karachi
```

### Read JSON files

```python
import json

with open("data.json", encoding="utf-8") as f:
    data = json.load(f)

print(data)
```

### Read a file line by line and process it

**Count lines, words, characters**

```python
lines = words = chars = 0

with open("notes.txt", encoding="utf-8") as f:
    for line in f:
        lines += 1
        words += len(line.split())
        chars += len(line)

print(f"Lines: {lines}, Words: {words}, Characters: {chars}")
```

**Word frequency**

```python
from collections import Counter

with open("notes.txt", encoding="utf-8") as f:
    words = f.read().lower().split()

print(Counter(words).most_common(3))
```

**Search for a word**

```python
keyword = "two"

with open("notes.txt", encoding="utf-8") as f:
    for number, line in enumerate(f, start=1):
        if keyword in line.lower():
            print(f"Line {number}: {line.strip()}")
```

### Reading from several files

```python
files = ["a.txt", "b.txt", "c.txt"]

for name in files:
    try:
        with open(name, encoding="utf-8") as f:
            print(name, len(f.read()))
    except FileNotFoundError:
        print(name, "is missing")
```

### Handling encoding errors

```python
with open("odd.txt", encoding="utf-8", errors="replace") as f:
    text = f.read()    # bad bytes become �

with open("odd.txt", encoding="utf-8", errors="ignore") as f:
    text = f.read()    # bad bytes are skipped
```

### Read methods compared

| Method | Returns | Best for |
|---|---|---|
| `read()` | One string | Small files |
| `read(n)` | n characters | Chunked reading |
| `readline()` | One line | Step-by-step reading |
| `readlines()` | List of lines | Small files, need a list |
| `for line in f` | One line per loop | **Large files** |

---

## 7.3 Python Write/Create Files

### Write with `"w"`

Creates the file, or **overwrites** it if it exists.

**Syntax**

```python
with open("file.txt", "w", encoding="utf-8") as f:
    f.write("text")
```

```python
with open("hello.txt", "w", encoding="utf-8") as f:
    f.write("Hello, World!\n")
    f.write("Second line\n")
```

> `write()` does **not** add a newline automatically. Add `\n` yourself.

### Append with `"a"`

Adds to the end and keeps existing content.

```python
with open("hello.txt", "a", encoding="utf-8") as f:
    f.write("Added later\n")
```

### Create a new file only if it does not exist: `"x"`

```python
try:
    with open("unique.txt", "x", encoding="utf-8") as f:
        f.write("New file")
except FileExistsError:
    print("File already exists")
```

### `write()` returns the character count

```python
with open("a.txt", "w", encoding="utf-8") as f:
    n = f.write("Hello")
    print(n)    # 5
```

### Writing non-text values

`write()` accepts **strings only**.

```python
with open("nums.txt", "w", encoding="utf-8") as f:
    # f.write(42)               # TypeError
    f.write(str(42) + "\n")
    f.write(f"{3.14}\n")
```

### `writelines()`: write a list of strings

```python
lines = ["apple\n", "banana\n", "cherry\n"]

with open("fruits.txt", "w", encoding="utf-8") as f:
    f.writelines(lines)
```

`writelines()` also does not add newlines.

```python
fruits = ["apple", "banana", "cherry"]

with open("fruits.txt", "w", encoding="utf-8") as f:
    f.writelines(name + "\n" for name in fruits)
```

### `print()` to a file

```python
with open("log.txt", "w", encoding="utf-8") as f:
    print("Hello", file=f)
    print("Name:", "Ali", file=f)
    print("a", "b", "c", sep=", ", file=f)
```

`print()` adds the newline for you.

### Write in a loop

```python
with open("squares.txt", "w", encoding="utf-8") as f:
    for n in range(1, 6):
        f.write(f"{n} squared is {n ** 2}\n")
```

```
1 squared is 1
2 squared is 4
...
```

### Read and write (`"r+"`)

```python
with open("hello.txt", "r+", encoding="utf-8") as f:
    content = f.read()
    f.seek(0)
    f.write("START\n" + content)
```

### Replace text inside a file

```python
with open("hello.txt", encoding="utf-8") as f:
    text = f.read()

text = text.replace("World", "Python")

with open("hello.txt", "w", encoding="utf-8") as f:
    f.write(text)
```

### Insert a line at the top

```python
with open("hello.txt", encoding="utf-8") as f:
    old = f.read()

with open("hello.txt", "w", encoding="utf-8") as f:
    f.write("TITLE\n" + old)
```

### Writing binary files

```python
data = bytes([72, 101, 108, 108, 111])

with open("data.bin", "wb") as f:
    f.write(data)

with open("data.bin", "rb") as f:
    print(f.read())    # b'Hello'
```

### Write a CSV file

```python
import csv

rows = [
    ["name", "age", "city"],
    ["Ali", 25, "Lahore"],
    ["Sara", 22, "Karachi"],
]

with open("students.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerows(rows)
```

> Use `newline=""` with CSV files to avoid blank lines on Windows.

**From dictionaries**

```python
import csv

people = [
    {"name": "Ali", "age": 25},
    {"name": "Sara", "age": 22},
]

with open("people.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "age"])
    writer.writeheader()
    writer.writerows(people)
```

### Write a JSON file

```python
import json

data = {"name": "Ali", "scores": [90, 85]}

with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=4, ensure_ascii=False)
```

### Write with `pathlib`

```python
from pathlib import Path

Path("note.txt").write_text("Hello\n", encoding="utf-8")
Path("data.bin").write_bytes(b"\x00\x01")
```

### Create a file inside a folder

The folder must exist first.

```python
from pathlib import Path

folder = Path("reports/2026")
folder.mkdir(parents=True, exist_ok=True)

(folder / "october.txt").write_text("Report\n", encoding="utf-8")
```

### Safe writing: write to a temp file, then rename

Prevents a half-written file if the program crashes.

```python
import os
import tempfile

def safe_write(path, text):
    folder = os.path.dirname(os.path.abspath(path))
    fd, tmp = tempfile.mkstemp(dir=folder)
    try:
        with os.fdopen(fd, "w", encoding="utf-8") as f:
            f.write(text)
        os.replace(tmp, path)     # atomic replace
    except Exception:
        os.remove(tmp)
        raise

safe_write("config.txt", "important data\n")
```

### Flushing

Data may sit in a buffer before reaching the disk. `with` flushes on exit. To force it earlier:

```python
with open("log.txt", "a", encoding="utf-8") as f:
    f.write("Event\n")
    f.flush()
```

### Temporary files

```python
import tempfile

with tempfile.NamedTemporaryFile("w+", suffix=".txt", delete=True) as tmp:
    tmp.write("temporary data")
    tmp.seek(0)
    print(tmp.read())
# file is deleted automatically
```

### Real-world example: simple logger

```python
from datetime import datetime

def log(message, file="app.log"):
    stamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open(file, "a", encoding="utf-8") as f:
        f.write(f"[{stamp}] {message}\n")

log("Program started")
log("User logged in")
```

```
[2026-10-18 19:45:12] Program started
[2026-10-18 19:45:12] User logged in
```

### Real-world example: to-do list saved in a file

```python
FILE = "todo.txt"

def load():
    try:
        with open(FILE, encoding="utf-8") as f:
            return [line.strip() for line in f if line.strip()]
    except FileNotFoundError:
        return []

def save(tasks):
    with open(FILE, "w", encoding="utf-8") as f:
        for task in tasks:
            f.write(task + "\n")

tasks = load()
tasks.append("Learn file handling")
save(tasks)
print(load())
```

### Write modes compared

| Mode | Existing file | Missing file |
|---|---|---|
| `"w"` | Erased | Created |
| `"a"` | Kept, adds to the end | Created |
| `"x"` | `FileExistsError` | Created |
| `"r+"` | Kept, can edit | `FileNotFoundError` |

---

## 7.4 Python Delete Files

> Deleting is **permanent**. Files do not go to the recycle bin. Double-check paths before running delete code.

### Delete a file: `os.remove()`

```python
import os

os.remove("old.txt")
```

If the file does not exist, `FileNotFoundError` is raised.

### Check first

```python
import os

if os.path.exists("old.txt"):
    os.remove("old.txt")
    print("Deleted")
else:
    print("File does not exist")
```

### Handle the error (preferred)

```python
import os

try:
    os.remove("old.txt")
    print("Deleted")
except FileNotFoundError:
    print("File does not exist")
except PermissionError:
    print("No permission, or the file is open elsewhere")
```

### Ignore a missing file

```python
import os
from contextlib import suppress

with suppress(FileNotFoundError):
    os.remove("maybe.txt")
```

Python 3.8+:

```python
from pathlib import Path

Path("maybe.txt").unlink(missing_ok=True)
```

### Delete with `pathlib`

```python
from pathlib import Path

p = Path("old.txt")
if p.exists():
    p.unlink()
```

### Delete an empty folder

```python
import os

os.rmdir("empty_folder")
```

`OSError` is raised if the folder is not empty.

```python
from pathlib import Path

Path("empty_folder").rmdir()
```

### Delete a folder and everything in it

```python
import shutil

shutil.rmtree("my_folder")
```

> `shutil.rmtree()` deletes **all contents** without asking. Use it with care.

```python
import shutil

shutil.rmtree("maybe_folder", ignore_errors=True)
```

### Delete files by pattern

```python
from pathlib import Path

for f in Path(".").glob("*.tmp"):
    f.unlink()
    print("Deleted", f)
```

**Preview first (dry run)**

```python
from pathlib import Path

DRY_RUN = True

for f in Path(".").glob("*.log"):
    if DRY_RUN:
        print("Would delete:", f)
    else:
        f.unlink()
```

### Delete files older than 30 days

```python
import time
from pathlib import Path

cutoff = time.time() - 30 * 24 * 60 * 60

for f in Path("downloads").iterdir():
    if f.is_file() and f.stat().st_mtime < cutoff:
        print("Deleting", f)
        f.unlink()
```

### Move to the recycle bin instead (safer)

Python has no built-in way. Use the `send2trash` package:

```bash
pip install send2trash
```

```python
from send2trash import send2trash

send2trash("old.txt")    # recoverable
```

### Empty a file without deleting it

```python
open("log.txt", "w").close()
```

or

```python
with open("log.txt", "w", encoding="utf-8"):
    pass
```

### Rename and move files

```python
import os

os.rename("old_name.txt", "new_name.txt")
os.replace("a.txt", "b.txt")             # overwrites b.txt if it exists
```

```python
import shutil

shutil.move("report.txt", "archive/report.txt")
```

```python
from pathlib import Path

Path("old.txt").rename("new.txt")
```

### Copy files

```python
import shutil

shutil.copy("a.txt", "b.txt")              # copy contents
shutil.copy2("a.txt", "b.txt")             # also copy timestamps
shutil.copytree("src_folder", "dst_folder")   # copy a whole folder
```

### Which delete tool to use

| Task | Tool |
|---|---|
| Delete one file | `os.remove(path)` or `Path.unlink()` |
| Delete an empty folder | `os.rmdir(path)` or `Path.rmdir()` |
| Delete a folder with contents | `shutil.rmtree(path)` |
| Delete matching files | `Path.glob()` + `unlink()` |
| Move to the recycle bin | `send2trash` |

### Safety rules

- Print what will be deleted before deleting (dry run).
- Never build delete paths from raw user input without checking them.
- Avoid deleting while a file is open (it fails on Windows).
- Keep backups of important data.

---

## 7.5 Python Paths and Directories

A **path** is the address of a file or folder.

| Type | Example |
|---|---|
| **Absolute** | `C:\Users\Ali\notes.txt` or `/home/ali/notes.txt` |
| **Relative** | `notes.txt` or `data/notes.txt` |

### Path separators

| System | Separator |
|---|---|
| Windows | `\` |
| macOS / Linux | `/` |

Python accepts `/` on **all** systems, so you can use it everywhere:

```python
open("data/notes.txt")
```

For Windows backslashes, use a raw string:

```python
path = r"C:\Users\Ali\notes.txt"
```

### `pathlib`: the modern way

```python
from pathlib import Path
```

#### Creating paths

```python
from pathlib import Path

p = Path("data") / "reports" / "october.txt"
print(p)                # data/reports/october.txt  (data\reports\october.txt on Windows)
print(Path.cwd())       # current working directory
print(Path.home())      # your home folder
```

The `/` operator joins parts and handles separators for you.

#### Path parts

```python
from pathlib import Path

p = Path("/home/ali/projects/report.final.txt")

print(p.name)        # report.final.txt
print(p.stem)        # report.final
print(p.suffix)      # .txt
print(p.suffixes)    # ['.final', '.txt']
print(p.parent)      # /home/ali/projects
print(p.parents[1])  # /home/ali
print(p.parts)       # ('/', 'home', 'ali', 'projects', 'report.final.txt')
print(p.anchor)      # /
```

#### Changing parts

```python
from pathlib import Path

p = Path("data/report.txt")

print(p.with_name("summary.txt"))     # data/summary.txt
print(p.with_suffix(".csv"))          # data/report.csv
print(p.with_stem("final"))           # data/final.txt   (Python 3.9+)
```

#### Checking a path

```python
from pathlib import Path

p = Path("notes.txt")

print(p.exists())       # True or False
print(p.is_file())
print(p.is_dir())
print(p.is_absolute())
```

#### Absolute and resolved paths

```python
from pathlib import Path

p = Path("notes.txt")

print(p.absolute())     # full path
print(p.resolve())      # full path, with ".." and symlinks resolved
```

#### File information

```python
from pathlib import Path
from datetime import datetime

p = Path("notes.txt")
info = p.stat()

print(info.st_size, "bytes")
print(datetime.fromtimestamp(info.st_mtime))    # last modified
```

### The script's own folder

```python
from pathlib import Path

HERE = Path(__file__).resolve().parent
data_file = HERE / "data" / "notes.txt"
```

This works no matter where the script is launched from.

### The `os` and `os.path` modules

| Task | `os` / `os.path` | `pathlib` |
|---|---|---|
| Current directory | `os.getcwd()` | `Path.cwd()` |
| Join paths | `os.path.join("a", "b")` | `Path("a") / "b"` |
| File name | `os.path.basename(p)` | `Path(p).name` |
| Folder part | `os.path.dirname(p)` | `Path(p).parent` |
| Extension | `os.path.splitext(p)[1]` | `Path(p).suffix` |
| Exists | `os.path.exists(p)` | `Path(p).exists()` |
| Is file | `os.path.isfile(p)` | `Path(p).is_file()` |
| Is folder | `os.path.isdir(p)` | `Path(p).is_dir()` |
| Absolute path | `os.path.abspath(p)` | `Path(p).absolute()` |
| File size | `os.path.getsize(p)` | `Path(p).stat().st_size` |

```python
import os

path = os.path.join("data", "reports", "october.txt")
print(path)

print(os.path.basename(path))           # october.txt
print(os.path.dirname(path))            # data/reports
print(os.path.splitext(path))           # ('data/reports/october', '.txt')
print(os.path.getsize("notes.txt"))
```

### Create folders

```python
import os

os.mkdir("new_folder")                       # one level; error if it exists
os.makedirs("a/b/c", exist_ok=True)          # nested; no error if present
```

```python
from pathlib import Path

Path("new_folder").mkdir(exist_ok=True)
Path("a/b/c").mkdir(parents=True, exist_ok=True)
```

### List the contents of a folder

```python
import os

print(os.listdir("."))
```

```python
from pathlib import Path

for item in Path(".").iterdir():
    kind = "DIR " if item.is_dir() else "FILE"
    print(kind, item.name)
```

**Faster, with file info: `os.scandir()`**

```python
import os

with os.scandir(".") as entries:
    for e in entries:
        print(e.name, e.is_file(), e.stat().st_size)
```

### Find files with patterns: `glob`

```python
from pathlib import Path

for f in Path(".").glob("*.txt"):
    print(f)

for f in Path(".").rglob("*.py"):      # search subfolders too
    print(f)
```

| Pattern | Matches |
|---|---|
| `*` | Any characters |
| `?` | One character |
| `[abc]` | One of a, b, c |
| `**` | Any number of nested folders (use `rglob`) |

```python
import glob

print(glob.glob("*.txt"))
print(glob.glob("**/*.csv", recursive=True))
```

### Walk through a folder tree: `os.walk()`

```python
import os

for folder, subfolders, files in os.walk("."):
    print("Folder:", folder)
    for name in files:
        print("   ", name)
```

**Count files by extension**

```python
from collections import Counter
from pathlib import Path

counts = Counter(p.suffix.lower() for p in Path(".").rglob("*") if p.is_file())
print(counts)
```

**Total size of a folder**

```python
from pathlib import Path

total = sum(f.stat().st_size for f in Path(".").rglob("*") if f.is_file())
print(f"{total / 1024:.1f} KB")
```

### Change the working directory

```python
import os

os.chdir("/tmp")
print(os.getcwd())
```

Changing the working directory affects the whole program. Prefer building full paths.

### Environment-related paths

```python
import os
from pathlib import Path
import tempfile

print(Path.home())                  # home folder
print(os.environ.get("PATH"))       # PATH variable
print(tempfile.gettempdir())        # temp folder
```

### Expanding `~` and variables

```python
import os
from pathlib import Path

print(Path("~/notes.txt").expanduser())
print(os.path.expandvars("$HOME/notes"))
```

### Path safety

```python
from pathlib import Path

BASE = Path("uploads").resolve()

def safe_path(name):
    target = (BASE / name).resolve()
    if BASE not in target.parents and target != BASE:
        raise ValueError("Path escapes the allowed folder")
    return target

safe_path("report.txt")          # ok
# safe_path("../secret.txt")     # ValueError
```

This helps block "path traversal" tricks when file names come from users.

### Real-world example: organize files by extension

```python
import shutil
from pathlib import Path

folder = Path("downloads")

for item in folder.iterdir():
    if item.is_file():
        ext = item.suffix.lower().lstrip(".") or "no_extension"
        target = folder / ext
        target.mkdir(exist_ok=True)
        shutil.move(str(item), target / item.name)
        print(f"Moved {item.name} -> {ext}/")
```

### Real-world example: find the largest files

```python
from pathlib import Path

files = [f for f in Path(".").rglob("*") if f.is_file()]
files.sort(key=lambda f: f.stat().st_size, reverse=True)

for f in files[:5]:
    print(f"{f.stat().st_size / 1024:8.1f} KB  {f}")
```

### Real-world example: bulk rename

```python
from pathlib import Path

for i, f in enumerate(sorted(Path("photos").glob("*.jpg")), start=1):
    f.rename(f.with_name(f"photo_{i:03}.jpg"))
```

### Real-world example: backup a folder

```python
import shutil
from datetime import datetime

stamp = datetime.now().strftime("%Y%m%d_%H%M%S")
shutil.make_archive(f"backup_{stamp}", "zip", "my_folder")
```

### Real-world example: duplicate file finder

```python
import hashlib
from collections import defaultdict
from pathlib import Path

def file_hash(path, chunk=65536):
    h = hashlib.sha256()
    with open(path, "rb") as f:
        while block := f.read(chunk):
            h.update(block)
    return h.hexdigest()

seen = defaultdict(list)

for f in Path(".").rglob("*"):
    if f.is_file():
        seen[file_hash(f)].append(f)

for digest, files in seen.items():
    if len(files) > 1:
        print("Duplicates:", *files)
```

### `pathlib` vs `os.path`

| | `pathlib` | `os.path` |
|---|---|---|
| Style | Object-oriented | Functions on strings |
| Readability | Higher | Lower |
| Cross-platform | Yes | Yes |
| Recommended for new code | **Yes** | Fine for older code |

---

## Chapter Summary

| Topic | Key points |
|---|---|
| File handling | `open(file, mode, encoding)`; modes `r`, `w`, `a`, `x`, `b`; always use `with`; set `encoding="utf-8"` |
| Reading | `read()`, `readline()`, `readlines()`, loop over the file; CSV and JSON helpers |
| Writing | `write()`, `writelines()`, `print(file=f)`; `"w"` overwrites, `"a"` appends, `"x"` creates only |
| Deleting | `os.remove()`, `Path.unlink()`, `os.rmdir()`, `shutil.rmtree()`; deletion is permanent |
| Paths and directories | Use `pathlib`; `/` joins paths; `mkdir`, `iterdir`, `glob`, `rglob`; `os.walk()` for trees |

### Quick syntax cheat sheet

```python
# Read
with open("f.txt", encoding="utf-8") as f:
    text = f.read()
    # or: for line in f: ...

# Write / append
with open("f.txt", "w", encoding="utf-8") as f:
    f.write("text\n")

with open("f.txt", "a", encoding="utf-8") as f:
    f.write("more\n")

# pathlib shortcuts
from pathlib import Path
p = Path("data") / "f.txt"
p.read_text(encoding="utf-8")
p.write_text("hi", encoding="utf-8")
p.exists(); p.is_file(); p.is_dir()
p.parent.mkdir(parents=True, exist_ok=True)
list(Path(".").glob("*.txt"))

# Delete / move / copy
import os, shutil
os.remove("f.txt")
shutil.rmtree("folder")
shutil.move("a.txt", "dir/a.txt")
shutil.copy("a.txt", "b.txt")
```

---

## Practice Exercises

### Beginner

1. Create a file `hello.txt` containing your name on the first line and your city on the second.
2. Read `hello.txt` and print its contents.
3. Append the line `"Python is fun"` to `hello.txt` without erasing it.
4. Read the file line by line and print each line with its line number.
5. Check whether `hello.txt` exists, and print a message either way.

### Intermediate

6. Write a program that counts the lines, words, and characters in a text file.
7. Copy the contents of one text file into another file, converting all text to uppercase.
8. Save a list of 10 numbers to a file (one per line), then read them back and print their sum.
9. Read a CSV file of students and print only those older than 20.
10. Write a program that lists all `.txt` files in a folder, including subfolders, along with their sizes.
11. Delete all files ending in `.tmp` in a folder (use a dry run first).

### Challenge

12. Build a simple **notes app**: menu with *add note*, *list notes*, *search notes*, and *delete all notes*, storing data in a text or JSON file.
13. Write a program that reads a text file and prints the 5 most common words.
14. Write a **file organizer** that moves files from a folder into subfolders by extension (`images`, `documents`, `others`).
15. Write a program that finds and prints duplicate files in a folder by comparing their hashes.
16. Build a **log analyzer** that reads `app.log` and counts how many lines contain `ERROR`, `WARNING`, and `INFO`.

### Sample solution (Exercise 6)

```python
lines = words = chars = 0

try:
    with open("notes.txt", encoding="utf-8") as f:
        for line in f:
            lines += 1
            words += len(line.split())
            chars += len(line)
    print(f"Lines: {lines}\nWords: {words}\nCharacters: {chars}")
except FileNotFoundError:
    print("File not found")
```

### Sample solution (Exercise 8)

```python
import random

numbers = [random.randint(1, 100) for _ in range(10)]

with open("numbers.txt", "w", encoding="utf-8") as f:
    for n in numbers:
        f.write(f"{n}\n")

with open("numbers.txt", encoding="utf-8") as f:
    loaded = [int(line) for line in f if line.strip()]

print(loaded)
print("Sum:", sum(loaded))
```

### Sample solution (Exercise 12)

```python
import json
from pathlib import Path

FILE = Path("notes.json")

def load():
    if FILE.exists():
        return json.loads(FILE.read_text(encoding="utf-8"))
    return []

def save(notes):
    FILE.write_text(json.dumps(notes, indent=2, ensure_ascii=False), encoding="utf-8")

notes = load()

while True:
    choice = input("add / list / search / clear / quit: ").strip().lower()

    if choice == "add":
        notes.append(input("Note: "))
        save(notes)
    elif choice == "list":
        for i, n in enumerate(notes, start=1):
            print(f"{i}. {n}")
    elif choice == "search":
        term = input("Search: ").lower()
        for n in notes:
            if term in n.lower():
                print("-", n)
    elif choice == "clear":
        if input("Delete all notes? (yes/no): ").lower() == "yes":
            notes = []
            save(notes)
    elif choice == "quit":
        break
    else:
        print("Invalid choice")
```

### Sample solution (Exercise 13)

```python
import re
from collections import Counter
from pathlib import Path

text = Path("notes.txt").read_text(encoding="utf-8").lower()
words = re.findall(r"[a-z']+", text)

for word, count in Counter(words).most_common(5):
    print(f"{word}: {count}")
```

### Sample solution (Exercise 14)

```python
import shutil
from pathlib import Path

CATEGORIES = {
    "images": {".jpg", ".jpeg", ".png", ".gif"},
    "documents": {".pdf", ".docx", ".txt", ".xlsx"},
}

folder = Path("downloads")

for item in folder.iterdir():
    if not item.is_file():
        continue

    category = "others"
    for name, extensions in CATEGORIES.items():
        if item.suffix.lower() in extensions:
            category = name
            break

    target = folder / category
    target.mkdir(exist_ok=True)
    shutil.move(str(item), target / item.name)
    print(f"{item.name} -> {category}/")
```

### Sample solution (Exercise 16)

```python
levels = {"ERROR": 0, "WARNING": 0, "INFO": 0}

with open("app.log", encoding="utf-8") as f:
    for line in f:
        for level in levels:
            if level in line:
                levels[level] += 1
                break

for level, count in levels.items():
    print(f"{level}: {count}")
```

---

**Previous:** [Chapter 6: Python Error Handling](chapter-06-python-error-handling.md)
**Next:** [Chapter 8: Python Object-Oriented Programming (OOP)](#)
