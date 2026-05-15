# Comprehensive Python Tutorial

## Table of Contents

1. Introduction to Python
2. Installing Python
3. Your First Python Program
4. Python Syntax and Structure
5. Variables and Data Types
6. Operators
7. Strings
8. Lists
9. Tuples
10. Sets
11. Dictionaries
12. Conditional Statements
13. Loops
14. Functions
15. Scope and Namespaces
16. Modules and Packages
17. File Handling
18. Exception Handling
19. Object-Oriented Programming
20. Functional Programming
21. Iterators and Generators
22. Decorators
23. Context Managers
24. Type Hints
25. Virtual Environments and Package Management
26. Working with APIs
27. Databases in Python
28. Concurrency and Parallelism
29. Testing in Python
30. Logging
31. Working with JSON and CSV
32. Web Development Overview
33. Data Science Overview
34. Automation and Scripting
35. Best Practices
36. Common Python Interview Questions
37. Advanced Topics
38. Useful Tools and Resources
39. Final Project Ideas
40. Conclusion

---

# 1. Introduction to Python

Python is a high-level, interpreted, general-purpose programming language known for:

- Readable syntax
- Large ecosystem
- Rapid development
- Cross-platform support
- Strong community
- Versatility

Python is used in:

- Web development
- Data science
- Machine learning
- Automation
- Cybersecurity
- DevOps
- Game development
- Scientific computing
- APIs and backend systems

## Why Learn Python?

Python is beginner-friendly while also being powerful enough for large-scale systems.

Major companies using Python include:

- Google
- Netflix
- Instagram
- Spotify
- Dropbox
- Reddit

## Python Versions

Modern development uses Python 3.

Check your version:

```bash
python --version
```

or:

```bash
python3 --version
```

---

# 2. Installing Python

## Windows

1. Download Python from the official website.
2. Run the installer.
3. Check “Add Python to PATH”.
4. Verify installation:

```bash
python --version
```

## macOS

Using Homebrew:

```bash
brew install python
```

## Linux

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install python3
```

---

# 3. Your First Python Program

```python
print("Hello, World!")
```

## Running a Script

Save as:

```text
hello.py
```

Run:

```bash
python hello.py
```

---

# 4. Python Syntax and Structure

## Indentation

Python uses indentation instead of braces.

```python
if True:
    print("Indented block")
```

## Comments

```python
# Single-line comment
```

```python
"""
Multi-line comment
"""
```

## Semicolons

Optional but discouraged.

```python
x = 1; y = 2
```

---

# 5. Variables and Data Types

## Variables

```python
name = "Alice"
age = 25
height = 1.75
```

## Dynamic Typing

```python
x = 10
x = "Now a string"
```

## Basic Data Types

| Type | Example |
|---|---|
| int | 10 |
| float | 3.14 |
| str | "Hello" |
| bool | True |
| list | [1, 2, 3] |
| tuple | (1, 2, 3) |
| set | {1, 2, 3} |
| dict | {"a": 1} |
| NoneType | None |

## Type Checking

```python
print(type(10))
```

## Type Conversion

```python
x = int("5")
y = float("3.14")
```

---

# 6. Operators

## Arithmetic Operators

```python
+
-
*
/
//
%
**
```

Example:

```python
print(2 ** 3)
```

## Comparison Operators

```python
==
!=
>
<
>=
<=
```

## Logical Operators

```python
and
or
not
```

## Assignment Operators

```python
+=
-=
*=
/=
```

---

# 7. Strings

## Creating Strings

```python
name = "Python"
```

## String Operations

```python
print(name.upper())
print(name.lower())
print(len(name))
```

## String Formatting

### f-strings

```python
name = "Alice"
print(f"Hello {name}")
```

### format()

```python
print("Hello {}".format(name))
```

## String Slicing

```python
text = "Python"
print(text[0:3])
```

---

# 8. Lists

## Creating Lists

```python
numbers = [1, 2, 3]
```

## Accessing Elements

```python
print(numbers[0])
```

## List Methods

```python
numbers.append(4)
numbers.remove(2)
numbers.sort()
```

## List Comprehensions

```python
squares = [x * x for x in range(10)]
```

---

# 9. Tuples

Tuples are immutable.

```python
point = (10, 20)
```

## Tuple Unpacking

```python
x, y = point
```

---

# 10. Sets

Sets store unique values.

```python
numbers = {1, 2, 3}
```

## Set Operations

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)
print(a & b)
```

---

# 11. Dictionaries

## Creating Dictionaries

```python
user = {
    "name": "Alice",
    "age": 30
}
```

## Accessing Values

```python
print(user["name"])
```

## Dictionary Methods

```python
user.keys()
user.values()
user.items()
```

## Iterating

```python
for key, value in user.items():
    print(key, value)
```

---

# 12. Conditional Statements

## if Statement

```python
age = 18

if age >= 18:
    print("Adult")
```

## if-else

```python
if age >= 18:
    print("Adult")
else:
    print("Minor")
```

## if-elif-else

```python
score = 85

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
else:
    print("C")
```

---

# 13. Loops

## for Loop

```python
for i in range(5):
    print(i)
```

## while Loop

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

## break and continue

```python
for i in range(10):
    if i == 5:
        break
```

---

# 14. Functions

## Defining Functions

```python
def greet(name):
    return f"Hello {name}"
```

## Calling Functions

```python
print(greet("Alice"))
```

## Default Arguments

```python
def greet(name="Guest"):
    print(name)
```

## Keyword Arguments

```python
def user(name, age):
    print(name, age)

user(age=25, name="Alice")
```

## *args and **kwargs

```python
def total(*numbers):
    return sum(numbers)
```

```python
def print_user(**data):
    print(data)
```

## Lambda Functions

```python
square = lambda x: x * x
```

---

# 15. Scope and Namespaces

## Local Scope

```python
def test():
    x = 10
```

## Global Scope

```python
x = 100
```

## global Keyword

```python
count = 0


def increment():
    global count
    count += 1
```

---

# 16. Modules and Packages

## Importing Modules

```python
import math
```

## Using Modules

```python
print(math.sqrt(16))
```

## Import Specific Functions

```python
from math import pi
```

## Creating Modules

File: mymodule.py

```python
def hello():
    print("Hello")
```

---

# 17. File Handling

## Reading Files

```python
with open("file.txt", "r") as file:
    content = file.read()
```

## Writing Files

```python
with open("file.txt", "w") as file:
    file.write("Hello")
```

## Appending

```python
with open("file.txt", "a") as file:
    file.write("More text")
```

---

# 18. Exception Handling

## try-except

```python
try:
    x = 1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
```

## finally

```python
try:
    print("Test")
finally:
    print("Always runs")
```

## Raising Exceptions

```python
raise ValueError("Invalid value")
```

---

# 19. Object-Oriented Programming

## Classes and Objects

```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello {self.name}")
```

## Creating Objects

```python
p = Person("Alice")
p.greet()
```

## Inheritance

```python
class Student(Person):
    def study(self):
        print("Studying")
```

## Encapsulation

```python
class BankAccount:
    def __init__(self):
        self.__balance = 0
```

## Polymorphism

```python
class Dog:
    def speak(self):
        return "Woof"
```

---

# 20. Functional Programming

## map()

```python
numbers = [1, 2, 3]
result = list(map(lambda x: x * 2, numbers))
```

## filter()

```python
result = list(filter(lambda x: x % 2 == 0, numbers))
```

## reduce()

```python
from functools import reduce

result = reduce(lambda x, y: x + y, numbers)
```

---

# 21. Iterators and Generators

## Iterators

```python
nums = iter([1, 2, 3])
print(next(nums))
```

## Generators

```python
def count_up_to(max_num):
    count = 1
    while count <= max_num:
        yield count
        count += 1
```

---

# 22. Decorators

## Basic Decorator

```python
def decorator(func):
    def wrapper():
        print("Before")
        func()
        print("After")
    return wrapper
```

```python
@decorator
def hello():
    print("Hello")
```

---

# 23. Context Managers

## Using with

```python
with open("data.txt") as f:
    data = f.read()
```

## Custom Context Manager

```python
class MyContext:
    def __enter__(self):
        print("Enter")

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Exit")
```

---

# 24. Type Hints

## Basic Type Hints

```python
def add(a: int, b: int) -> int:
    return a + b
```

## Using typing Module

```python
from typing import List


def total(values: List[int]) -> int:
    return sum(values)
```

---

# 25. Virtual Environments and Package Management

## Creating a Virtual Environment

```bash
python -m venv venv
```

## Activating

### Windows

```bash
venv\Scripts\activate
```

### Linux/macOS

```bash
source venv/bin/activate
```

## Installing Packages

```bash
pip install requests
```

## requirements.txt

```bash
pip freeze > requirements.txt
```

Install:

```bash
pip install -r requirements.txt
```

---

# 26. Working with APIs

## requests Library

```python
import requests

response = requests.get("https://api.github.com")
print(response.status_code)
```

## JSON Responses

```python
data = response.json()
print(data)
```

---

# 27. Databases in Python

## SQLite Example

```python
import sqlite3

conn = sqlite3.connect("example.db")
cursor = conn.cursor()
```

## Creating Tables

```python
cursor.execute("""
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT
)
""")
```

## Inserting Data

```python
cursor.execute(
    "INSERT INTO users (name) VALUES (?)",
    ("Alice",)
)
```

---

# 28. Concurrency and Parallelism

## Threading

```python
import threading
```

## Multiprocessing

```python
from multiprocessing import Process
```

## asyncio

```python
import asyncio


async def main():
    print("Hello")
    await asyncio.sleep(1)
    print("World")


asyncio.run(main())
```

---

# 29. Testing in Python

## unittest

```python
import unittest


class TestMath(unittest.TestCase):
    def test_add(self):
        self.assertEqual(1 + 1, 2)
```

## pytest

```python
def test_add():
    assert 1 + 1 == 2
```

---

# 30. Logging

## Basic Logging

```python
import logging

logging.basicConfig(level=logging.INFO)
logging.info("Application started")
```

---

# 31. Working with JSON and CSV

## JSON

```python
import json

person = {"name": "Alice"}
json_data = json.dumps(person)
```

## CSV

```python
import csv

with open("data.csv") as file:
    reader = csv.reader(file)
```

---

# 32. Web Development Overview

Popular frameworks:

- Django
- Flask
- FastAPI

## Flask Example

```python
from flask import Flask

app = Flask(__name__)


@app.route("/")
def home():
    return "Hello"
```

---

# 33. Data Science Overview

Popular libraries:

- NumPy
- pandas
- Matplotlib
- scikit-learn
- TensorFlow
- PyTorch

## NumPy Example

```python
import numpy as np

arr = np.array([1, 2, 3])
```

## pandas Example

```python
import pandas as pd

df = pd.DataFrame({"A": [1, 2]})
```

---

# 34. Automation and Scripting

Python is widely used for:

- File automation
- Web scraping
- Task scheduling
- DevOps scripts
- System administration

## Example Script

```python
import os

for file in os.listdir():
    print(file)
```

---

# 35. Best Practices

## Follow PEP 8

Use readable formatting.

## Use Meaningful Names

Bad:

```python
x = 10
```

Good:

```python
user_age = 10
```

## Keep Functions Small

Each function should do one thing.

## Write Tests

Automated tests improve reliability.

## Use Virtual Environments

Avoid dependency conflicts.

---

# 36. Common Python Interview Questions

## What is Python?

A high-level interpreted programming language.

## Difference Between List and Tuple?

- Lists are mutable.
- Tuples are immutable.

## What is a Decorator?

A function that modifies another function.

## What is GIL?

The Global Interpreter Lock controls execution of Python bytecode in CPython.

## What are Generators?

Functions that yield values lazily.

---

# 37. Advanced Topics

## Metaclasses

Classes that create classes.

## Async Programming

Used for scalable I/O-bound systems.

## Memory Management

Python uses:

- Reference counting
- Garbage collection

## Descriptors

Advanced attribute access mechanism.

## C Extensions

Performance-critical parts can use C/C++.

---

# 38. Useful Tools and Resources

## Editors and IDEs

- VS Code
- PyCharm
- Vim
- Sublime Text

## Package Managers

- pip
- poetry
- pipenv

## Linters

- flake8
- pylint
- ruff

## Formatters

- black
- isort

---

# 39. Final Project Ideas

## Beginner

- Calculator
- To-do app
- Password generator
- Number guessing game

## Intermediate

- REST API
- Web scraper
- Chat application
- Expense tracker

## Advanced

- Machine learning project
- Distributed system
- Real-time dashboard
- Microservices architecture

---

# 40. Conclusion

Python is one of the most versatile and productive programming languages available today.

To become proficient:

1. Practice consistently
2. Build projects
3. Read other people’s code
4. Learn debugging
5. Study algorithms and data structures
6. Contribute to open source

## Recommended Learning Path

1. Basics
2. Data structures
3. Functions
4. OOP
5. File handling
6. APIs
7. Databases
8. Testing
9. Frameworks
10. Advanced topics

## Next Steps

After finishing this tutorial, consider specializing in:

- Backend development
- Data science
- Machine learning
- Automation
- Cybersecurity
- DevOps
- Cloud engineering

Happy coding!
