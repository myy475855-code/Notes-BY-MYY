# Python Notes — Unit 3: File Handling

> **Purpose:** Learn how Python reads, creates, updates, organizes, and safely manages files and folders.  
> **Suggested order:** Study the chapters in sequence, type every example yourself, and complete the practice tasks at the end.

## Table of Contents

1. [Introduction to File Handling](#1-introduction-to-file-handling)
2. [Files, Paths, and File Extensions](#2-files-paths-and-file-extensions)
3. [Opening and Closing Files](#3-opening-and-closing-files)
4. [File Modes](#4-file-modes)
5. [Reading Text Files](#5-reading-text-files)
6. [Writing, Creating, and Appending Files](#6-writing-creating-and-appending-files)
7. [The `with` Statement and Context Managers](#7-the-with-statement-and-context-managers)
8. [File Position: `tell()` and `seek()`](#8-file-position-tell-and-seek)
9. [Text Encoding and Newlines](#9-text-encoding-and-newlines)
10. [Working with Binary Files](#10-working-with-binary-files)
11. [Working with File Paths Using `pathlib`](#11-working-with-file-paths-using-pathlib)
12. [Creating and Managing Directories](#12-creating-and-managing-directories)
13. [File and Directory Operations with `os` and `shutil`](#13-file-and-directory-operations-with-os-and-shutil)
14. [Checking Whether Files Exist](#14-checking-whether-files-exist)
15. [Handling File Exceptions](#15-handling-file-exceptions)
16. [Working with CSV Files](#16-working-with-csv-files)
17. [Working with JSON Files](#17-working-with-json-files)
18. [Temporary Files and Folders](#18-temporary-files-and-folders)
19. [Processing Large Files Efficiently](#19-processing-large-files-efficiently)
20. [File Security and Best Practices](#20-file-security-and-best-practices)
21. [Common Errors and Debugging](#21-common-errors-and-debugging)
22. [Practice Programs](#22-practice-programs)
23. [Mini Projects](#23-mini-projects)
24. [Recommended Study Method](#24-recommended-study-method)
25. [Unit 3 Completion Checklist](#25-unit-3-completion-checklist)

---

## 1. Introduction to File Handling

File handling allows a program to save information permanently and retrieve it later. Unlike a variable, which usually exists only while a program is running, a file can preserve data after the program closes.

### Why file handling is useful

- Save user information and application settings.
- Store logs, reports, and results.
- Read data from text files, CSV files, and JSON files.
- Work with images, audio, PDFs, and other binary files.
- Organize application data in folders.

### Main file operations

- **Create:** make a new file.
- **Read:** retrieve existing content.
- **Write:** create or replace content.
- **Append:** add content to the end.
- **Update:** read and modify existing content.
- **Rename, copy, or delete:** manage files and folders.

---

## 2. Files, Paths, and File Extensions

A **file** stores information. A **path** tells the operating system where the file is located.

### Common extensions

| Extension | Typical use |
|---|---|
| `.txt` | Plain text |
| `.py` | Python source code |
| `.csv` | Tabular data |
| `.json` | Structured data |
| `.log` | Application logs |
| `.jpg`, `.png` | Images |
| `.pdf` | Documents |

### Relative and absolute paths

- **Relative path:** interpreted from the program's current working directory.
- **Absolute path:** specifies the full location from the filesystem root.

```python
# Relative path
file_path = "data/notes.txt"

# Absolute path example on Windows
file_path = r"C:\Users\Student\Documents\notes.txt"
```

Use raw strings (`r"..."`) or forward slashes for Windows paths when writing paths directly. Prefer `pathlib` for portable path construction; see Chapter 11.

### Current working directory

```python
import os

print(os.getcwd())
```

The current working directory may differ from the folder containing your Python script.

---

## 3. Opening and Closing Files

Python's built-in `open()` function opens a file and returns a file object.

```python
file = open("notes.txt", "r", encoding="utf-8")
content = file.read()
file.close()
```

### Why closing matters

Closing a file releases system resources and helps ensure buffered output is written. If an exception occurs before `close()`, a manually opened file might remain open longer than intended. The `with` statement is generally safer and is explained in Chapter 7.

### Basic syntax

```python
open(file_path, mode="r", encoding=None)
```

The available arguments depend on the task. Text files commonly use an explicit encoding such as `"utf-8"`.

---

## 4. File Modes

The mode determines what Python can do with the file.

| Mode | Meaning |
|---|---|
| `"r"` | Read; file must already exist |
| `"w"` | Write; creates a file or truncates an existing file |
| `"a"` | Append; creates the file if needed and writes at the end |
| `"x"` | Create a new file; fails if it already exists |
| `"b"` | Binary mode modifier |
| `"t"` | Text mode modifier; the default |
| `"r+"` | Read and write without immediately truncating the file |
| `"w+"` | Read and write; truncates or creates the file |
| `"a+"` | Read and append; creates the file if needed |

Modes can be combined, such as `"rb"` for reading binary data or `"wb"` for writing binary data.

**Important:** `"w"` can erase existing contents immediately. Use it carefully.

```python
# Create a new file only if it does not already exist
with open("new_notes.txt", "x", encoding="utf-8") as file:
    file.write("First version")
```

---

## 5. Reading Text Files

Suppose `notes.txt` contains:

```text
Python is useful.
File handling saves data.
```

### Read the entire file

```python
with open("notes.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

### Read one line

```python
with open("notes.txt", "r", encoding="utf-8") as file:
    first_line = file.readline()
    print(first_line)
```

`readline()` returns one line at a time, including its newline character when present.

### Read all lines into a list

```python
with open("notes.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()

print(lines)
```

Each list item usually represents one line.

### Iterate over a file

```python
with open("notes.txt", "r", encoding="utf-8") as file:
    for line in file:
        print(line.rstrip())
```

Iteration is often more memory-efficient than reading a very large file into one string.

### Remove surrounding whitespace

- `strip()` removes whitespace from both ends.
- `lstrip()` removes whitespace from the left.
- `rstrip()` removes whitespace from the right.

Use these methods when appropriate; avoid removing whitespace that is meaningful to the data.

---

## 6. Writing, Creating, and Appending Files

### Write text

```python
with open("message.txt", "w", encoding="utf-8") as file:
    file.write("Hello from Python!\n")
    file.write("This is the second line.\n")
```

`write()` returns the number of characters written. It does not automatically add a newline.

### Append text

```python
with open("message.txt", "a", encoding="utf-8") as file:
    file.write("This line was appended.\n")
```

Append mode preserves existing contents and writes new content at the end.

### Write several lines

```python
lines = ["Apple\n", "Banana\n", "Mango\n"]

with open("fruits.txt", "w", encoding="utf-8") as file:
    file.writelines(lines)
```

`writelines()` does not automatically add newline characters. Include them in the strings when required.

### Create a file without overwriting an existing file

```python
with open("unique.txt", "x", encoding="utf-8") as file:
    file.write("Created safely if the name was available.")
```

If the file already exists, Python raises `FileExistsError`.

---

## 7. The `with` Statement and Context Managers

The `with` statement manages resources and closes the file automatically when the block ends, including when an exception occurs.

```python
with open("notes.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

After leaving the block, the file is closed.

### Why prefer `with`?

- Less cleanup code.
- Fewer resource leaks.
- Cleaner, easier-to-read programs.
- Safer when an exception occurs.

For ordinary file work, prefer `with open(...)` over manually opening and closing files.

---

## 8. File Position: `tell()` and `seek()`

A file object maintains a current position used for reading or writing.

- `tell()` returns the current stream position.
- `seek()` moves the stream position.

```python
with open("notes.txt", "r", encoding="utf-8") as file:
    print(file.tell())
    print(file.read(5))
    print(file.tell())
    file.seek(0)
    print(file.read(5))
```

For text files, positions are not always simple character counts, especially with multibyte encodings. Use `seek(0)` to return to the start. More advanced seeking behavior is easier to reason about with binary files.

### Common uses

- Read the file again from the beginning.
- Revisit a known position.
- Work with structured binary data.

---

## 9. Text Encoding and Newlines

Text is stored as bytes, so Python needs an encoding to translate between bytes and characters.

### Use UTF-8

```python
with open("urdu_or_unicode.txt", "w", encoding="utf-8") as file:
    file.write("Hello! Python supports Unicode text.")
```

Specifying `encoding="utf-8"` helps make text behavior consistent across systems.

### Newline handling

Use `"\n"` to represent a line break in Python strings:

```python
text = "First line\nSecond line\n"
```

When working with CSV files, the standard practice is to open the file with `newline=""`; see Chapter 16.

### Encoding errors

A `UnicodeDecodeError` may occur if a file's actual encoding does not match the encoding used to read it. Determine the file's encoding when possible rather than guessing or silently discarding data.

---

## 10. Working with Binary Files

Binary files contain bytes rather than ordinary decoded text. Examples include images, audio, PDFs, and many other formats.

Use binary modes such as `"rb"` and `"wb"`. Do not pass a text encoding in binary mode.

### Read bytes

```python
with open("image.png", "rb") as file:
    data = file.read()

print(type(data))  # <class 'bytes'>
```

### Copy a file in chunks

```python
with open("source.bin", "rb") as source:
    with open("copy.bin", "wb") as destination:
        while True:
            chunk = source.read(8192)
            if not chunk:
                break
            destination.write(chunk)
```

For ordinary file copying, `shutil.copyfile()` is usually simpler. The chunk example teaches how to process large binary data without loading the entire file into memory.

---

## 11. Working with File Paths Using `pathlib`

`pathlib.Path` provides an object-oriented way to work with paths and is recommended for many modern Python projects.

```python
from pathlib import Path

path = Path("data") / "notes.txt"
print(path)
print(path.name)
print(path.suffix)
print(path.parent)
```

### Create a folder and write a file

```python
from pathlib import Path

folder = Path("data")
folder.mkdir(parents=True, exist_ok=True)

path = folder / "notes.txt"
path.write_text("Saved using pathlib.\n", encoding="utf-8")
```

### Read a text file

```python
from pathlib import Path

path = Path("data/notes.txt")
content = path.read_text(encoding="utf-8")
print(content)
```

`read_text()` and `write_text()` are convenient for smaller text files. For large files or repeated line-by-line processing, use `open()` with a `with` block.

### Useful `Path` methods

| Method or property | Purpose |
|---|---|
| `Path.exists()` | Check whether a path exists |
| `Path.is_file()` | Check whether it is a file |
| `Path.is_dir()` | Check whether it is a directory |
| `Path.mkdir()` | Create a directory |
| `Path.iterdir()` | Iterate over direct children |
| `Path.glob("*.txt")` | Match a pattern in a directory |
| `Path.read_text()` | Read text |
| `Path.write_text()` | Write text |
| `Path.rename()` | Rename or move a path |
| `Path.unlink()` | Delete a file |

---

## 12. Creating and Managing Directories

### Create one directory

```python
from pathlib import Path

Path("reports").mkdir(exist_ok=True)
```

### Create nested directories

```python
Path("data/2026/october").mkdir(parents=True, exist_ok=True)
```

- `parents=True` creates missing parent directories.
- `exist_ok=True` avoids an error when the directory already exists.

### List files in a directory

```python
from pathlib import Path

for item in Path(".").iterdir():
    print(item.name)
```

### Find files by extension

```python
from pathlib import Path

for path in Path("reports").glob("*.txt"):
    print(path)
```

Use `rglob("*.txt")` to search recursively through subdirectories.

---

## 13. File and Directory Operations with `os` and `shutil`

Python includes modules for common filesystem tasks.

### Rename a file

```python
from pathlib import Path

Path("old_name.txt").rename("new_name.txt")
```

### Delete a file

```python
from pathlib import Path

path = Path("temporary.txt")
if path.exists() and path.is_file():
    path.unlink()
```

Deletion is permanent in ordinary filesystem operations, so confirm the target before deleting it.

### Copy a file

```python
import shutil

shutil.copyfile("source.txt", "destination.txt")
```

`copyfile()` copies file contents. Other `shutil` functions can also preserve metadata or copy directory trees.

### Move a file

```python
import shutil

shutil.move("source.txt", "archive/source.txt")
```

Ensure the destination folder exists and handle possible conflicts.

### Useful `os` operations

```python
import os

print(os.getcwd())          # Current working directory
print(os.listdir("."))      # Names in the current directory
```

For new code, `pathlib` often makes path-related code easier to read, while `os` and `shutil` remain useful for particular filesystem tasks.

---

## 14. Checking Whether Files Exist

Use `Path.exists()` and `Path.is_file()` when checking paths.

```python
from pathlib import Path

path = Path("notes.txt")

if path.is_file():
    print("The file exists.")
else:
    print("The file is missing or is not a regular file.")
```

### Avoid check-then-use assumptions

A file may change between checking it and opening it. For important operations, perform the operation and handle the resulting exception rather than relying only on a previous existence check.

For example, use mode `"x"` when you need creation to fail if the file already exists.

---

## 15. Handling File Exceptions

Filesystem operations can fail for many reasons: a missing file, insufficient permissions, an invalid path, or an encoding mismatch.

### Common exceptions

| Exception | Typical cause |
|---|---|
| `FileNotFoundError` | Requested path does not exist |
| `FileExistsError` | Exclusive creation target already exists |
| `PermissionError` | Operation is not permitted |
| `IsADirectoryError` | A file operation was attempted on a directory |
| `NotADirectoryError` | A path component is not a directory |
| `UnicodeDecodeError` | Bytes cannot be decoded using the selected encoding |
| `OSError` | General operating-system or filesystem failure |

### Handle a missing file

```python
try:
    with open("missing.txt", "r", encoding="utf-8") as file:
        content = file.read()
except FileNotFoundError:
    print("The requested file was not found.")
```

### Use multiple exception handlers when behavior differs

```python
try:
    with open("notes.txt", "r", encoding="utf-8") as file:
        print(file.read())
except FileNotFoundError:
    print("Create the file first.")
except PermissionError:
    print("You do not have permission to read this file.")
except OSError as error:
    print(f"File operation failed: {error}")
```

Catch specific exceptions where possible. Avoid a bare `except:` that hides unrelated programming errors.

---

## 16. Working with CSV Files

CSV stands for **Comma-Separated Values**. It is commonly used for spreadsheets and tabular data.

Use Python's built-in `csv` module instead of manually splitting lines on commas. CSV fields can contain commas, quotation marks, and other characters that require proper parsing.

### Read a CSV file as rows

Example `students.csv`:

```csv
name,age,city
Ayesha,20,Lahore
Bilal,21,Karachi
```

```python
import csv

with open("students.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

### Read rows as dictionaries

```python
import csv

with open("students.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row["name"], row["city"])
```

### Write a CSV file

```python
import csv

rows = [
    ["name", "age", "city"],
    ["Ayesha", 20, "Lahore"],
    ["Bilal", 21, "Karachi"],
]

with open("students_output.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerows(rows)
```

### Write dictionaries

```python
import csv

students = [
    {"name": "Ayesha", "age": 20},
    {"name": "Bilal", "age": 21},
]

with open("students_dict.csv", "w", newline="", encoding="utf-8") as file:
    fieldnames = ["name", "age"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(students)
```

---

## 17. Working with JSON Files

JSON (JavaScript Object Notation) is a common text format for structured data, such as configuration files and API responses.

Python's `json` module converts between Python objects and JSON text.

### Write JSON

```python
import json

student = {
    "name": "Ayesha",
    "age": 20,
    "skills": ["Python", "HTML"],
}

with open("student.json", "w", encoding="utf-8") as file:
    json.dump(student, file, indent=4)
```

### Read JSON

```python
import json

with open("student.json", "r", encoding="utf-8") as file:
    student = json.load(file)

print(student["name"])
print(student["skills"])
```

### Convert between strings and Python objects

```python
import json

json_text = json.dumps({"active": True, "count": 3}, indent=2)
data = json.loads(json_text)
```

- `dump()` writes a Python object to a file.
- `load()` reads JSON data from a file.
- `dumps()` returns JSON text as a string.
- `loads()` parses JSON text from a string.

### Important note

JSON supports a limited set of data types. Convert unsupported objects, such as custom class instances, to suitable dictionaries or other JSON-compatible structures first. Do not use `eval()` to parse JSON.

---

## 18. Temporary Files and Folders

Temporary files are useful for intermediate data that does not need to remain permanently.

The `tempfile` module can create temporary files and directories safely.

```python
import tempfile
from pathlib import Path

with tempfile.TemporaryDirectory() as temp_dir:
    path = Path(temp_dir) / "example.txt"
    path.write_text("Temporary data", encoding="utf-8")
    print(path.read_text(encoding="utf-8"))
```

The temporary directory is cleaned up when the block ends. Do not store data there if it must survive beyond the temporary resource's lifetime.

---

## 19. Processing Large Files Efficiently

Reading an entire large file into memory with `read()` may use a lot of RAM.

### Process one line at a time

```python
with open("large_log.txt", "r", encoding="utf-8") as file:
    for line in file:
        if "ERROR" in line:
            print(line.rstrip())
```

### Process binary files in chunks

```python
chunk_size = 64 * 1024

with open("large_file.bin", "rb") as file:
    while chunk := file.read(chunk_size):
        # Process this chunk here
        print(len(chunk))
```

The assignment expression (`:=`) requires Python 3.8 or later. If you are using an older version, use a normal `while True` loop.

### Practical tips

- Stream data line by line when possible.
- Use chunks for large binary files.
- Avoid creating unnecessary copies of large strings or byte sequences.
- Use CSV readers and other iterators rather than loading everything into a list unless needed.

---

## 20. File Security and Best Practices

- **Use `with`:** ensure files are closed properly.
- **Specify encodings:** prefer `encoding="utf-8"` for text unless the format requires another encoding.
- **Validate user-provided paths:** do not blindly trust paths received from users.
- **Avoid path traversal:** a path such as `../../private.txt` may escape an intended directory. Resolve and validate paths when restricting file access.
- **Be careful with deletion:** check the exact target before removing files or folders.
- **Avoid accidental overwrites:** use exclusive creation mode `"x"` or an explicit confirmation strategy when appropriate.
- **Do not store secrets in public files:** protect passwords, API keys, and personal information.
- **Handle exceptions:** provide useful messages without exposing sensitive system details.
- **Use safe temporary-file APIs:** do not create predictable temporary filenames in shared locations.
- **Back up important data:** file writes and deletes may not be reversible.
- **Avoid unsafe deserialization:** never load untrusted data using `pickle`; loading malicious pickle data can execute code.
- **Validate uploaded files:** in web applications, check size, type, permissions, and storage location. Do not rely on a filename extension alone.

### A safer pattern for user-selected paths

When an application must restrict files to a particular folder, resolve the candidate path and verify that it remains inside the allowed base directory. Path validation must be designed for the application and operating system; a simple string-prefix check is not sufficient.

---

## 21. Common Errors and Debugging

### `FileNotFoundError`

**Possible cause:** The path is wrong or the current working directory is unexpected.

**Try:**
```python
from pathlib import Path

print(Path.cwd())
print(Path("notes.txt").resolve())
```

### `PermissionError`

**Possible cause:** The current user or process lacks the required permission, or another program is restricting access.

**Try:** Check file permissions and whether the target is protected or in use.

### File contents disappear after writing

**Possible cause:** The file was opened in `"w"` mode, which truncates existing contents.

**Try:** Use `"a"` to append, or choose a safe update strategy.

### Text displays incorrectly

**Possible cause:** The selected encoding does not match the file's encoding.

**Try:** Identify the source encoding and specify it explicitly.

### Extra blank lines in CSV output

**Possible cause:** On some platforms, the file was opened without `newline=""`.

**Try:**
```python
open("output.csv", "w", newline="", encoding="utf-8")
```

### A file appears in an unexpected folder

**Possible cause:** The program's working directory differs from what you expected.

**Try:** Print `Path.cwd()` and use a deliberate path.

---

## 22. Practice Programs

Complete these in order, starting with the simpler exercises.

1. Create a text file and write three lines into it.
2. Read and display the entire contents of a text file.
3. Count the number of lines in a file.
4. Count words and characters in a text file.
5. Search a file for a word and report matching line numbers.
6. Append a timestamped entry to a log file.
7. Copy a text file to another location.
8. Create a folder if it does not exist, then save a report inside it.
9. List all `.txt` files in a directory.
10. Rename a file after checking that the destination is appropriate.
11. Handle a missing-file error gracefully.
12. Read student records from a CSV file.
13. Write a list of dictionaries to a CSV file.
14. Save a dictionary to JSON and load it again.
15. Read a large log file line by line and print only lines containing `"ERROR"`.
16. Copy a binary file using chunks.
17. Build a simple notes program that supports viewing, adding, and searching notes.
18. Build a program that counts how many times each word occurs in a text file.

---

## 23. Mini Projects

### Project 1: Student Record Manager

**Features**
- Add student name, roll number, and marks.
- Save records in CSV or JSON.
- Display all records.
- Search for a student by roll number.
- Handle missing or invalid files.

**Concepts practiced:** file reading and writing, CSV/JSON, dictionaries, exception handling.

### Project 2: Personal Notes App

**Features**
- Add a note.
- List saved notes.
- Search notes by keyword.
- Append new notes without deleting old ones.
- Store notes in a dedicated folder.

**Concepts practiced:** append mode, paths, text processing, directories.

### Project 3: Log Analyzer

**Features**
- Read a log file line by line.
- Count occurrences of `INFO`, `WARNING`, and `ERROR`.
- Print matching lines.
- Save a summary report.

**Concepts practiced:** iteration, large-file processing, string operations, output files.

### Project 4: File Organizer

**Features**
- Scan a selected folder.
- Group files by extension.
- Create destination folders.
- Move files only after checking destination conflicts.
- Provide a preview or confirmation before moving files.

**Concepts practiced:** `pathlib`, `shutil`, directory traversal, safe filesystem operations.

### Project 5: JSON-Based Contact Book

**Features**
- Add, view, search, update, and delete contacts.
- Store contact data in JSON.
- Handle missing or malformed JSON files.
- Keep private data in an appropriate location.

**Concepts practiced:** JSON serialization, dictionaries, validation, exception handling.

---

## 24. Recommended Study Method

1. Read one chapter and explain the key idea in your own words.
2. Type each code example rather than only copying and pasting it.
3. Run the code and inspect the file it creates or changes.
4. Change one detail at a time, such as the filename, mode, or text.
5. Intentionally test a missing path and observe the exception.
6. Complete at least two practice programs without looking at the solution.
7. Build one mini project and document how to run it.
8. Review safe writing, deletion, path validation, and exception handling before using file operations in real applications.

---

## 25. Unit 3 Completion Checklist

Mark each item when you can do it without relying on a step-by-step tutorial.

- [ ] Explain why programs use files for persistent data.
- [ ] Distinguish relative paths from absolute paths.
- [ ] Open and close files correctly.
- [ ] Choose the correct file mode for reading, writing, appending, or exclusive creation.
- [ ] Read whole files, individual lines, and line-by-line streams.
- [ ] Write text and append content without accidental overwriting.
- [ ] Use `with open(...)` confidently.
- [ ] Explain the purpose of `tell()` and `seek()`.
- [ ] Work with UTF-8 text and binary data.
- [ ] Use `pathlib` to build and inspect paths.
- [ ] Create directories and list matching files.
- [ ] Copy, move, rename, and delete files carefully.
- [ ] Handle common file-related exceptions.
- [ ] Read and write CSV data using the `csv` module.
- [ ] Read and write JSON data using the `json` module.
- [ ] Process large files without loading all data into memory.
- [ ] Apply basic file security and data-validation practices.
- [ ] Complete at least one file-handling mini project.

**Unit 3 is complete when you can build a small program that saves data, reads it back, handles errors, and avoids accidental data loss.**
