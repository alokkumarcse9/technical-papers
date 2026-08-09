# Python Cheatsheet

## Technical Reference: Arrays, Strings, OOP, Decorators, Virtual Environments, pip, and PEP 8

> A practical Python reference for learning, revision, coding practice,
> and interviews.

------------------------------------------------------------------------

# Table of Contents

1.  [Array Methods](#1-array-methods)
2.  [String Methods](#2-string-methods)
3.  [Objects and Object-Oriented
    Programming](#3-objects-and-object-oriented-programming)
4.  [Decorators](#4-decorators)
5.  [virtualenv](#5-virtualenv)
6.  [pip Package Manager](#6-pip-package-manager)
7.  [PEP 8 Standards Summary](#7-pep-8-standards-summary)
8.  [Quick Revision](#8-quick-revision)

------------------------------------------------------------------------

# 1. Array Methods

## 1.1 What is an Array in Python?

Python does not have a built-in `array` type that behaves exactly like
arrays in languages such as C or Java.

In everyday Python programming, the **list** is commonly used as a
general-purpose dynamic array.

``` python
numbers = [10, 20, 30, 40]
```

Python also provides the `array` module for storing values of a single
basic type efficiently.

``` python
from array import array

numbers = array("i", [10, 20, 30])
```

For most beginner and application-level Python code, focus primarily on
**list methods**.

## 1.2 List/Array Methods Cheat Sheet

| Method | Purpose | Example |
|---|---|---|
| `append()` | Add one item at the end | `a.append(10)` |
| `extend()` | Add multiple items | `a.extend([20, 30])` |
| `insert()` | Insert at an index | `a.insert(1, 20)` |
| `remove()` | Remove first matching value | `a.remove(20)` |
| `pop()` | Remove and return an item | `a.pop()` |
| `clear()` | Remove all items | `a.clear()` |
| `index()` | Find index of a value | `a.index(20)` |
| `count()` | Count occurrences | `a.count(20)` |
| `sort()` | Sort the list in place | `a.sort()` |
| `reverse()` | Reverse the list in place | `a.reverse()` |
| `copy()` | Create a shallow copy | `b = a.copy()` |

## 1.3 Important Examples

``` python
numbers = [10, 20]

numbers.append(30)
# [10, 20, 30]

numbers.extend([40, 50])
# [10, 20, 30, 40, 50]

numbers.insert(1, 15)
# [10, 15, 20, 30, 40, 50]

numbers.remove(15)
numbers.pop()
numbers.clear()
```

### `append()` vs `extend()`

``` python
a = [1, 2]

a.append([3, 4])
# [1, 2, [3, 4]]
```

``` python
a = [1, 2]

a.extend([3, 4])
# [1, 2, 3, 4]
```

### Useful List Operations

``` python
len(numbers)       # Length
20 in numbers      # Membership
numbers[0]         # Indexing
numbers[-1]        # Last element
numbers[1:4]       # Slicing
numbers[::-1]      # Reverse using slicing
```

------------------------------------------------------------------------

# 2. String Methods

## 2.1 What is a String?

A string is a sequence of characters.

``` python
name = "Alok"
```

Strings are **immutable**, so string methods generally return a new
string rather than modifying the original string.

## 2.2 Common String Methods

| Method | Purpose |
|---|---|
| `lower()` | Convert to lowercase |
| `upper()` | Convert to uppercase |
| `capitalize()` | Capitalize first character |
| `title()` | Capitalize each word |
| `swapcase()` | Swap upper/lower case |
| `strip()` | Remove leading/trailing whitespace |
| `lstrip()` | Remove leading whitespace |
| `rstrip()` | Remove trailing whitespace |
| `replace()` | Replace text |
| `split()` | Convert string into a list |
| `join()` | Join iterable into a string |
| `find()` | Find substring position |
| `index()` | Find substring position |
| `count()` | Count occurrences |
| `startswith()` | Check beginning |
| `endswith()` | Check ending |
| `isdigit()` | Check digits |
| `isalpha()` | Check letters |
| `isalnum()` | Check letters/numbers |
| `isspace()` | Check whitespace |

## 2.3 Examples

``` python
text = "hello world"

text.upper()               # "HELLO WORLD"
text.lower()               # "hello world"
text.capitalize()          # "Hello world"
text.title()               # "Hello World"
text.replace("world", "Python")
```

### `strip()`

``` python
text = "   hello   "

text.strip()
# "hello"
```

### `split()`

``` python
text = "Python is easy"

text.split()
# ['Python', 'is', 'easy']
```

### `join()`

``` python
words = ["Python", "is", "easy"]

" ".join(words)
# "Python is easy"
```

### `find()` vs `index()`

``` python
text = "Hello Python"

text.find("Python")
# 6

text.find("Java")
# -1
```

`index()` raises `ValueError` when the substring is not found.

### Validation

``` python
"123".isdigit()        # True
"Python".isalpha()     # True
"Python123".isalnum()  # True
"   ".isspace()        # True
```

### f-Strings

``` python
name = "Alok"
age = 23

message = f"My name is {name} and I am {age} years old."
```

------------------------------------------------------------------------

# 3. Objects and Object-Oriented Programming

## 3.1 What is OOP?

**Object-Oriented Programming (OOP)** is a programming approach where
programs are designed around objects containing data and behavior.

``` text
Student
├── Data
│   ├── name
│   └── marks
└── Behavior
    ├── add_marks()
    └── calculate_average()
```

## 3.2 Class

A class is a blueprint for creating objects.

``` python
class Student:
    pass
```

## 3.3 Object

An object is an instance of a class.

``` python
student1 = Student()
student2 = Student()
```

``` text
Student  → Class
student1 → Object
student2 → Object
```

## 3.4 Constructor: `__init__()`

`__init__()` initializes an object when it is created.

``` python
class Student:

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
```

``` python
student = Student("Alok", 90)
```

Conceptually:

``` text
Student("Alok", 90)
        ↓
__init__(self, "Alok", 90)
        ↓
self.name = "Alok"
self.marks = 90
```

## 3.5 `self`

`self` refers to the current object.

``` python
class Student:

    def __init__(self, name):
        self.name = name
```

For:

``` python
s1 = Student("Alok")
```

`self` refers to `s1`.

For:

``` python
s2 = Student("Rahul")
```

`self` refers to `s2`.

## 3.6 Instance Variables

``` python
class Student:

    def __init__(self, name):
        self.name = name
```

``` python
s1 = Student("Alok")
s2 = Student("Rahul")

print(s1.name)
print(s2.name)
```

Each object has its own `name`.

## 3.7 Instance Methods

``` python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")
```

``` python
s = Student("Alok")
s.introduce()
```

# 3.8 Four Pillars of OOP

## 1. Encapsulation

Keep data and related behavior together and control how data is
accessed.

``` python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        self._balance += amount

    def get_balance(self):
        return self._balance
```

## 2. Inheritance

A child class can reuse or extend a parent class.

``` python
class Animal:

    def speak(self):
        print("Animal speaks")


class Dog(Animal):

    def bark(self):
        print("Dog barks")
```

``` python
dog = Dog()

dog.speak()
dog.bark()
```

## 3. Polymorphism

The same interface can behave differently for different objects.

``` python
class Dog:

    def speak(self):
        print("Woof")


class Cat:

    def speak(self):
        print("Meow")
```

``` python
animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

## 4. Abstraction

Expose essential behavior while hiding implementation details.

``` python
from abc import ABC, abstractmethod


class Payment(ABC):

    @abstractmethod
    def pay(self, amount):
        pass
```

``` python
class CreditCardPayment(Payment):

    def pay(self, amount):
        print(f"Paid {amount} using credit card")
```

## 3.9 Class Variables

A class variable is associated with the class and can be shared by
instances.

``` python
class Student:

    school = "ABC School"

    def __init__(self, name):
        self.name = name
```

## 3.10 Class Method

A class method receives `cls`.

``` python
class Student:

    school = "ABC School"

    @classmethod
    def change_school(cls, name):
        cls.school = name
```

Usage:

``` python
Student.change_school("XYZ School")
```

## 3.11 Static Method

A static method does not automatically receive `self` or `cls`.

``` python
class Calculator:

    @staticmethod
    def add(a, b):
        return a + b
```

``` python
Calculator.add(10, 20)
```

## 3.12 Method Types

  Method            First Parameter   Purpose
  ----------------- ----------------- ------------------
  Instance method   `self`            Object data
  Class method      `cls`             Class-level data
  Static method     None              Utility logic

------------------------------------------------------------------------

# 4. Decorators

## 4.1 What is a Decorator?

A decorator is a function that adds or modifies the behavior of another
function without changing its original source code.

Basic pattern:

``` python
def decorator(func):

    def wrapper():
        # Extra behavior
        func()

    return wrapper
```

## 4.2 Simple Decorator

``` python
def login_required(func):

    def wrapper():
        print("Checking login...")
        func()

    return wrapper
```

Apply it:

``` python
@login_required
def dashboard():
    print("Welcome to dashboard")
```

Call:

``` python
dashboard()
```

This is conceptually equivalent to:

``` python
dashboard = login_required(dashboard)
```

## 4.3 Decorator Flow

``` text
@login_required
def dashboard():
    ...
```

``` text
dashboard
   ↓
login_required(dashboard)
   ↓
wrapper
   ↓
dashboard()
```

## 4.4 Decorators with Arguments

Use `*args` and `**kwargs` when the decorated function can accept
different arguments.

``` python
def logger(func):

    def wrapper(*args, **kwargs):
        print("Function called")
        result = func(*args, **kwargs)
        return result

    return wrapper
```

``` python
@logger
def add(a, b):
    return a + b
```

## 4.5 `functools.wraps`

Use `wraps()` to preserve function metadata.

``` python
from functools import wraps


def logger(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        print("Function called")
        return func(*args, **kwargs)

    return wrapper
```

## 4.6 Decorators with Their Own Arguments

``` python
def repeat(times):

    def decorator(func):

        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)

        return wrapper

    return decorator
```

``` python
@repeat(3)
def hello():
    print("Hello")
```

## 4.7 Common Uses

-   Logging
-   Authentication
-   Authorization
-   Caching
-   Timing
-   Validation
-   Retry logic
-   Access control
-   Framework features

Built-in examples:

``` python
@property
@classmethod
@staticmethod
```

------------------------------------------------------------------------

# 5. virtualenv

## 5.1 What is a Virtual Environment?

A virtual environment creates an **isolated Python environment** for a
project.

Without isolation:

``` text
Project A → package version 1
Project B → package version 2
```

With virtual environments:

``` text
Project A
└── .venv
    └── packages

Project B
└── .venv
    └── different packages
```

## 5.2 Why Use It?

-   Isolate project dependencies
-   Avoid package conflicts
-   Keep projects reproducible
-   Avoid unnecessary global installations
-   Simplify deployment

## 5.3 Create a Virtual Environment

Modern Python includes `venv`:

``` bash
python3 -m venv .venv
```

## 5.4 Activate on Linux/macOS

``` bash
source .venv/bin/activate
```

You will usually see:

``` text
(.venv)
```

## 5.5 Windows CMD

``` cmd
.venv\Scripts\activate
```

## 5.6 Windows PowerShell

``` powershell
.venv\Scripts\Activate.ps1
```

## 5.7 Deactivate

``` bash
deactivate
```

## 5.8 Delete

Linux/macOS:

``` bash
rm -rf .venv
```

Windows:

``` cmd
rmdir /s /q .venv
```

## 5.9 Recommended Structure

``` text
my_project/
├── .venv/
├── src/
├── tests/
├── requirements.txt
└── README.md
```

Add this to `.gitignore`:

``` gitignore
.venv/
```

------------------------------------------------------------------------

# 6. pip Package Manager

## 6.1 What is pip?

`pip` is Python's standard package installer.

It is used to:

-   Install packages
-   Upgrade packages
-   Remove packages
-   List installed packages
-   Inspect package information
-   Install dependency files

## 6.2 Check pip

``` bash
python3 -m pip --version
```

Using `python -m pip` is often preferable because it associates pip with
a specific Python interpreter.

## 6.3 Install

``` bash
python -m pip install requests
```

## 6.4 Specific Version

``` bash
python -m pip install requests==2.32.4
```

## 6.5 Upgrade

``` bash
python -m pip install --upgrade requests
```

## 6.6 Uninstall

``` bash
python -m pip uninstall requests
```

## 6.7 List

``` bash
python -m pip list
```

## 6.8 Show Package Information

``` bash
python -m pip show requests
```

## 6.9 Generate `requirements.txt`

``` bash
python -m pip freeze > requirements.txt
```

## 6.10 Install Dependencies

``` bash
python -m pip install -r requirements.txt
```

## 6.11 Upgrade pip

``` bash
python -m pip install --upgrade pip
```

## 6.12 Typical Setup

``` bash
mkdir my_project
cd my_project

python3 -m venv .venv
source .venv/bin/activate

python -m pip install requests

python -m pip freeze > requirements.txt
```

## 6.13 Workflow

``` text
Create project
      ↓
Create virtual environment
      ↓
Activate environment
      ↓
Install packages with pip
      ↓
Develop
      ↓
Freeze dependencies
      ↓
requirements.txt
```

------------------------------------------------------------------------

# 7. PEP 8 Standards Summary

## 7.1 What is PEP 8?

**PEP 8** is Python's style guide.

PEP means **Python Enhancement Proposal**.

PEP 8 provides conventions that make Python code:

-   Readable
-   Consistent
-   Maintainable
-   Easier to review

PEP 8 is primarily a style guide, not a set of requirements enforced by
the Python interpreter.

## 7.2 Indentation

Use **4 spaces**.

``` python
if age >= 18:
    print("Adult")
```

Avoid mixing tabs and spaces.

## 7.3 Line Length

Traditional PEP 8 guidance recommends approximately **79 characters**
for code and documentation, although modern projects may define their
own formatter/linter limits.

Break long expressions when needed:

``` python
result = some_function(
    first_argument,
    second_argument,
    third_argument,
)
```

## 7.4 Blank Lines

Top-level functions and classes are conventionally separated by two
blank lines.

Methods inside a class are separated by one blank line.

``` python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(self.name)
```

## 7.5 Naming Conventions

### Variables and Functions

Use `snake_case`:

``` python
student_name = "Alok"
total_marks = 450


def calculate_total():
    pass
```

### Classes

Use `CapWords` / PascalCase:

``` python
class BankAccount:
    pass
```

### Constants

Use uppercase:

``` python
MAX_RETRIES = 3
DEFAULT_TIMEOUT = 30
```

### Internal/Non-Public Convention

A leading underscore often communicates internal/non-public intent:

``` python
_internal_value = 10
```

## 7.6 Imports

Put imports near the top:

``` python
import os
import sys

from pathlib import Path
```

A common organization is:

``` python
# Standard library
import os
from pathlib import Path

# Third-party
import requests

# Local application
from myapp.models import User
```

## 7.7 Operators

Good:

``` python
x = 10
total = price + tax
```

Avoid:

``` python
x=10
total=price+tax
```

## 7.8 Commas

Good:

``` python
numbers = [10, 20, 30]
```

Avoid:

``` python
numbers = [10,20,30]
```

## 7.9 Function Calls and Indexing

Good:

``` python
numbers[0]
func(value)
```

Avoid unnecessary spaces:

``` python
numbers [0]
func (value)
```

## 7.10 Comments

Comments should explain **why**, not merely repeat the code.

Weak:

``` python
# Add 1
count += 1
```

Better:

``` python
# Track the number of successfully processed records.
count += 1
```

## 7.11 Docstrings

Use docstrings to document functions, classes, and modules when useful.

``` python
def calculate_area(length, width):
    """Return the area of a rectangle."""
    return length * width
```

## 7.12 Boolean Comparisons

Prefer:

``` python
if is_valid:
    ...
```

Instead of:

``` python
if is_valid == True:
    ...
```

For `None`, use:

``` python
if value is None:
    ...
```

## 7.13 Mutable Default Arguments

Avoid:

``` python
def add_item(item, items=[]):
    items.append(item)
    return items
```

Use:

``` python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

## 7.14 Readability Over Cleverness

Readable:

``` python
filtered_data = [item for item in data if item > 10]
```

For more complex logic, use multiple statements rather than forcing
everything into one expression.

The goal is not simply fewer lines. The goal is **clear, maintainable
code**.

## 7.15 Useful Python Code-Quality Tools

Common tools include:

-   `pycodestyle` --- style checking
-   `ruff` --- fast linting and formatting
-   `black` --- code formatting
-   `isort` --- import sorting

Example:

``` bash
python -m pip install ruff
```

Check:

``` bash
ruff check .
```

Format:

``` bash
ruff format .
```

Always follow the conventions established by your project/team.

------------------------------------------------------------------------

# 8. Quick Revision

## List / Array

``` python
items.append(x)
items.extend(values)
items.insert(index, x)
items.remove(x)
items.pop()
items.clear()
items.index(x)
items.count(x)
items.sort()
items.reverse()
items.copy()
```

## Strings

``` python
text.lower()
text.upper()
text.capitalize()
text.title()
text.strip()
text.replace(old, new)
text.split()
separator.join(items)
text.find(substring)
text.index(substring)
text.count(substring)
text.startswith(prefix)
text.endswith(suffix)
text.isdigit()
text.isalpha()
text.isalnum()
```

## OOP

``` text
Class
  ↓
Object
  ↓
Attributes + Methods
```

Four pillars:

``` text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

Important:

``` text
__init__()
self
Instance variable
Class variable
Instance method
Class method
Static method
```

## Decorators

``` python
def decorator(func):

    def wrapper(*args, **kwargs):
        # Extra behavior
        return func(*args, **kwargs)

    return wrapper
```

Usage:

``` python
@decorator
def function():
    pass
```

Equivalent idea:

``` python
function = decorator(function)
```

## Virtual Environment

Create:

``` bash
python3 -m venv .venv
```

Activate:

``` bash
source .venv/bin/activate
```

Deactivate:

``` bash
deactivate
```

## pip

Install:

``` bash
python -m pip install package_name
```

Uninstall:

``` bash
python -m pip uninstall package_name
```

List:

``` bash
python -m pip list
```

Freeze:

``` bash
python -m pip freeze > requirements.txt
```

Install dependencies:

``` bash
python -m pip install -r requirements.txt
```

## PEP 8

``` text
4 spaces indentation
snake_case → variables/functions
PascalCase → classes
UPPER_CASE → constants
Imports → top of file
Spaces around operators
Readable line length
Useful comments
Meaningful docstrings
Avoid unnecessary complexity
Use linters/formatters
```

------------------------------------------------------------------------

# Final Mental Model

``` text
Python
│
├── Data Structures
│   ├── List / Array
│   └── String
│
├── OOP
│   ├── Class
│   ├── Object
│   ├── Encapsulation
│   ├── Inheritance
│   ├── Polymorphism
│   └── Abstraction
│
├── Advanced Functions
│   └── Decorators
│
├── Environment Management
│   └── virtualenv / venv
│
├── Package Management
│   └── pip
│
└── Code Quality
    └── PEP 8
```

> **Core idea:** Write Python that is correct first, then make it
> readable, maintainable, testable, and consistent with your project's
> conventions.