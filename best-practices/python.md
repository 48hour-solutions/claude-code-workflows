# Python Best Practices and Style Guide

## Table of Contents
1.  [Introduction](#1-introduction)
    * [1.1 What is Python?](#11-what-is-python)
    * [1.2 The Zen of Python](#12-the-zen-of-python)
2.  [Environment and Setup](#2-environment-and-setup)
    * [2.1 Python Versions](#21-python-versions)
    * [2.2 Virtual Environments](#22-virtual-environments)
    * [2.3 Dependency Management](#23-dependency-management)
3.  [PEP 8: The Style Guide for Python Code](#3-pep-8-the-style-guide-for-python-code)
    * [3.1 Code Layout](#31-code-layout)
    * [3.2 Naming Conventions](#32-naming-conventions)
    * [3.3 Comments](#33-comments)
    * [3.4 Programming Recommendations](#34-programming-recommendations)
4.  [Core Python Language Constructs](#4-core-python-language-constructs)
    * [4.1 Variables and Data Types](#41-variables-and-data-types)
    * [4.2 Data Structures](#42-data-structures)
    * [4.3 Control Flow](#43-control-flow)
    * [4.4 Functions](#44-functions)
    * [4.5 Exception Handling](#45-exception-handling)
5.  [Advanced Python Concepts](#5-advanced-python-concepts)
    * [5.1 Iterators and Generators](#51-iterators-and-generators)
    * [5.2 Decorators](#52-decorators)
    * [5.3 Context Managers](#53-context-managers)
    * [5.4 Comprehensions](#54-comprehensions)
6.  [Object-Oriented Programming (OOP) Best Practices](#6-object-oriented-programming-oop-best-practices)
    * [6.1 Classes and Instances](#61-classes-and-instances)
    * [6.2 Inheritance and Composition](#62-inheritance-and-composition)
    * [6.3 Dunder (Magic) Methods](#63-dunder-magic-methods)
    * [6.4 Properties and Encapsulation](#64-properties-and-encapsulation)
7.  [Modules and Packages](#7-modules-and-packages)
    * [7.1 Structuring a Project](#71-structuring-a-project)
    * [7.2 Imports](#72-imports)
    * [7.3 `__init__.py`](#73-__init__py)
8.  [Working with the Standard Library](#8-working-with-the-standard-library)
    * [8.1 `os` and `pathlib` for File System Operations](#81-os-and-pathlib-for-file-system-operations)
    * [8.2 `datetime` for Dates and Times](#82-datetime-for-dates-and-times)
    * [8.3 `collections` for Advanced Data Structures](#83-collections-for-advanced-data-structures)
    * [8.4 `json` for Data Serialization](#84-json-for-data-serialization)
9.  [Testing and Code Quality](#9-testing-and-code-quality)
    * [9.1 Importance of Testing](#91-importance-of-testing)
    * [9.2 Using `pytest`](#92-using-pytest)
    * [9.3 Linters and Formatters](#93-linters-and-formatters)
10. [Common Pitfalls and Anti-Patterns](#10-common-pitfalls-and-anti-patterns)
    * [10.1 Mutable Default Arguments](#101-mutable-default-arguments)
    * [10.2 Misunderstanding Python's Scope](#102-misunderstanding-pythons-scope)
    * [10.3 Floating-Point Inaccuracies](#103-floating-point-inaccuracies)
11. [Security Best Practices](#11-security-best-practices)
    * [11.1 Input Validation](#111-input-validation)
    * [11.2 Using `subprocess` Securely](#112-using-subprocess-securely)
    * [11.3 Managing Secrets](#113-managing-secrets)
12. [Performance Considerations](#12-performance-considerations)
    * [12.1 Profiling Your Code](#121-profiling-your-code)
    * [12.2 Algorithmic Complexity](#122-algorithmic-complexity)
    * [12.3 Built-in Functions are Faster](#123-built-in-functions-are-faster)
13. [Further Resources](#13-further-resources)

---

## 1. Introduction

### 1.1 What is Python?

Python is a high-level, interpreted, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. Its language constructs and object-oriented approach aim to help programmers write clear, logical code for small and large-scale projects.

### 1.2 The Zen of Python

The guiding principles of Python's design are captured in "The Zen of Python" (PEP 20). You can view it by running `import this` in a Python interpreter.

> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> Special cases aren't special enough to break the rules.
> Although practicality beats purity.
> Errors should never pass silently.
> Unless explicitly silenced.
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.
> Although that way may not be obvious at first unless you're Dutch.
> Now is better than never.
> Although never is often better than *right* now.
> If the implementation is hard to explain, it's a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea -- let's do more of those!

---

## 2. Environment and Setup

### 2.1 Python Versions

**Best Practice:** Always use a modern, supported version of Python (e.g., Python 3.8 or newer). Python 2 has been sunset since 2020 and should not be used for new projects.

Check your Python version with:
```bash
python3 --version
````

### 2.2 Virtual Environments

**Best Practice:** Always use a virtual environment for every Python project. This isolates project dependencies, preventing conflicts between projects that require different versions of the same library.

#### Creating a virtual environment with `venv`:

```bash
# Create a directory for your project
mkdir my_project
cd my_project

# Create the virtual environment (e.g., named '.venv')
python3 -m venv .venv

# Activate the virtual environment
# On macOS/Linux:
source .venv/bin/activate
# On Windows:
.venv\Scripts\activate

# Your shell prompt will now show (.venv)
```

To deactivate, simply run `deactivate`.

### 2.3 Dependency Management

**Best Practice:** Explicitly declare your project's dependencies in a file. This ensures reproducibility.

#### Using `pip` and `requirements.txt`:

1.  While your virtual environment is active, install packages using `pip`:

    ```bash
    pip install requests
    pip install numpy
    ```

2.  Generate a `requirements.txt` file to lock the versions:

    ```bash
    pip freeze > requirements.txt
    ```

3.  Another developer (or you, on another machine) can replicate the environment:

    ```bash
    # Assuming a new virtual environment is active
    pip install -r requirements.txt
    ```

#### Modern Tools: Poetry and Pipenv

For more advanced dependency management, consider tools like **Poetry** or **Pipenv**. They combine virtual environment management and package management, offering dependency resolution and project packaging features.

-----

## 3\. PEP 8: The Style Guide for Python Code

PEP 8 is the official style guide for Python code. Adhering to it makes your code more readable and consistent. Automated tools like `flake8` (linter) and `black` (formatter) can help enforce these rules.

### 3.1 Code Layout

  * **Indentation:** Use 4 spaces per indentation level. Never mix tabs and spaces.
  * **Line Length:** Limit all lines to a maximum of 79-99 characters. The official PEP 8 suggests 79, but many modern tools like `black` default to 88.
  * **Blank Lines:**
      * Use two blank lines to separate top-level functions and class definitions.
      * Use one blank line to separate methods inside a class.
      * Use blank lines sparingly inside functions to show logical sections.

<!-- end list -->

```python
# Good example of layout
class MyClass:
    def __init__(self, value):
        self.value = value

    def first_method(self):
        # A single blank line separates methods
        print("First method")


def top_level_function():
    # Two blank lines separate this from the class
    print("Top-level function")
```

### 3.2 Naming Conventions

  * **Variables, Functions, Methods:** Use `snake_case` (lowercase with underscores).
      * `my_variable`, `calculate_total()`
  * **Constants:** Use `ALL_CAPS_WITH_UNDERSCORES`.
      * `MAX_OVERFLOW`, `PI`
  * **Classes:** Use `PascalCase` or `CapWords` (uppercamelcase).
      * `MySuperClass`, `DataModel`
  * **Modules and Packages:** Use short, `all_lowercase` names. Underscores can be used if it improves readability.
      * `my_module`, `data_utils`
  * **Internal Use:** A single leading underscore (`_variable`) is a weak convention indicating an internal-use variable or method.
  * **Name Mangling:** Two leading underscores (`__variable`) trigger Python's name mangling mechanism, making it harder to access from outside the class.

<!-- end list -->

```python
# Good example of naming
MAX_CONNECTIONS = 100
_INTERNAL_COUNTER = 0

class DatabaseHandler:
    def __init__(self, connection_string):
        self.connection_string = connection_string

    def connect_to_db(self):
        pass

def fetch_user_data(user_id):
    pass
```

### 3.3 Comments

Write comments as complete sentences. Keep them updated when the code changes.

  * **Block Comments:** Indent to the same level as the code they describe. Start each line with a `#` followed by a single space.
  * **Inline Comments:** Use them sparingly. They should be separated by at least two spaces from the statement. `x = x + 1  # Compensate for border`.
  * **Docstrings:** Use docstrings for all public modules, functions, classes, and methods. Use `"""Triple quotes"""`.

#### Docstring Example (Google Style):

```python
def fetch_data(url: str, timeout: int = 10) -> str:
    """Fetches data from a given URL.

    Args:
        url: The URL to fetch data from.
        timeout: The timeout in seconds for the request.

    Returns:
        The content of the response as a string.

    Raises:
        requests.exceptions.RequestException: For connection errors.
    """
    # function implementation...
    pass
```

### 3.4 Programming Recommendations

  * **Comparisons:**
      * Use `is` or `is not` for singleton comparisons like `None`.
        ```python
        # Good
        if my_var is None:
            ...
        # Bad
        if my_var == None:
            ...
        ```
      * Don't compare boolean values to `True` or `False` using `==`.
        ```python
        # Good
        if is_ready:
            ...
        # Bad
        if is_ready == True:
            ...
        ```
  * **Use `isinstance()` for type checking:**
    ```python
    # Good
    if isinstance(my_var, int):
        ...
    # Bad and less flexible
    if type(my_var) is int:
        ...
    ```

-----

## 4\. Core Python Language Constructs

### 4.1 Variables and Data Types

Python is dynamically typed. You don't need to declare a variable's type.

  * **Primitive Types:** `int`, `float`, `bool`, `str`, `NoneType`
  * **Immutability:** Be aware of which types are mutable (can be changed in-place) vs. immutable (cannot).
      * **Immutable:** `int`, `float`, `str`, `tuple`, `frozenset`
      * **Mutable:** `list`, `dict`, `set`

<!-- end list -->

```python
# String is immutable
my_string = "hello"
# my_string[0] = "H" # This will raise a TypeError

# List is mutable
my_list = [1, 2, 3]
my_list[0] = 100 # This is valid
print(my_list) # Output: [100, 2, 3]
```

### 4.2 Data Structures

#### Lists

Ordered, mutable collections.

```python
numbers = [1, 2, 3, 4, 5]
numbers.append(6)
first_three = numbers[0:3] # Slicing
```

#### Tuples

Ordered, immutable collections. Often used for data that should not change, like coordinates.

```python
point = (10, 20)
x, y = point # Unpacking
```

#### Dictionaries

Unordered (in Python \< 3.7) key-value pairs. Keys must be immutable types.

```python
user = {"name": "Alice", "age": 30}
print(user["name"]) # Accessing a value

# Use .get() to avoid KeyErrors
age = user.get("age")
country = user.get("country", "Unknown") # Provide a default
```

#### Sets

Unordered collections of unique elements. Useful for membership testing and eliminating duplicates.

```python
unique_numbers = {1, 2, 3, 2, 1}
print(unique_numbers) # Output: {1, 2, 3}

# Set operations
set_a = {1, 2, 3}
set_b = {3, 4, 5}
print(set_a | set_b) # Union: {1, 2, 3, 4, 5}
print(set_a & set_b) # Intersection: {3}
```

### 4.3 Control Flow

#### `if/elif/else` Statements

```python
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
else:
    grade = "C"
```

#### Ternary Operator

A concise way to write simple `if/else` statements.

```python
result = "Pass" if score > 60 else "Fail"
```

#### `for` Loops

Iterate over a sequence.

```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Use enumerate to get both index and value
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

#### `while` Loops

Execute as long as a condition is true.

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

### 4.4 Functions

Functions are first-class objects in Python.

#### Defining Functions

Use type hints for clarity and for static analysis tools.

```python
def greet(name: str) -> str:
    """Returns a greeting string."""
    return f"Hello, {name}!"
```

#### `*args` and `**kwargs`

Use `*args` to pass a variable number of positional arguments and `**kwargs` for keyword arguments.

```python
def process_data(*args, **kwargs):
    print("Positional arguments:", args)
    print("Keyword arguments:", kwargs)

process_data(1, "hello", status="active", user_id=123)
# Output:
# Positional arguments: (1, 'hello')
# Keyword arguments: {'status': 'active', 'user_id': 123}
```

### 4.5 Exception Handling

Use `try...except` blocks to handle errors gracefully.

**Best Practice:** Be specific about the exceptions you catch. Avoid bare `except:` clauses.

```python
# Good: Specific exception handling
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Error: Cannot divide by zero.")
except TypeError:
    print("Error: Invalid types for division.")

# Bad: Catches everything, including SystemExit and KeyboardInterrupt
try:
    # some code
    pass
except:
    print("An unknown error occurred.")
```

#### `try...except...else...finally`

  * `else`: Executes if no exception is raised in the `try` block.
  * `finally`: Always executes, regardless of whether an exception occurred. Useful for cleanup actions like closing files or database connections.

<!-- end list -->

```python
try:
    f = open("my_file.txt", "r")
    content = f.read()
except FileNotFoundError:
    print("File not found.")
else:
    print("File read successfully.")
    print(content)
finally:
    # Ensure the file is closed even if an error occurs
    if 'f' in locals() and not f.closed:
        f.close()
        print("File closed.")
```

-----

## 5\. Advanced Python Concepts

### 5.1 Iterators and Generators

  * **Iterator:** An object that represents a stream of data. It implements the `__iter__()` and `__next__()` methods.
  * **Generator:** A simpler way to create iterators using a function with the `yield` keyword. Generators are memory-efficient because they produce items one at a time and only when requested.

<!-- end list -->

```python
# Generator function
def count_up_to(max_val):
    """Generates numbers from 1 to max_val."""
    count = 1
    while count <= max_val:
        yield count
        count += 1

# Using the generator
counter = count_up_to(5)
for number in counter:
    print(number) # Prints 1, 2, 3, 4, 5
```

### 5.2 Decorators

A decorator is a function that takes another function and extends its behavior without explicitly modifying it.

```python
import time

def timing_decorator(func):
    """A decorator to measure the execution time of a function."""
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function {func.__name__} took {end_time - start_time:.4f} seconds.")
        return result
    return wrapper

@timing_decorator
def slow_function(delay):
    """A function that simulates work."""
    time.sleep(delay)
    return "Done"

slow_function(2)
# Output: Function slow_function took 2.00xx seconds.
```

### 5.3 Context Managers

Context managers allow you to allocate and release resources precisely. The `with` statement is the most common way to use them.

```python
# The with statement ensures the file is closed automatically
with open("data.txt", "w") as f:
    f.write("This is a context manager example.")

# Creating your own context manager
from contextlib import contextmanager

@contextmanager
def database_connection(db_name):
    print(f"Connecting to {db_name}...")
    connection = {"db": db_name, "status": "open"}
    try:
        yield connection # The 'with' block code runs here
    finally:
        connection["status"] = "closed"
        print(f"Closing connection to {db_name}.")


with database_connection("my_db") as conn:
    print(f"Performing operations with connection: {conn}")

# Output:
# Connecting to my_db...
# Performing operations with connection: {'db': 'my_db', 'status': 'open'}
# Closing connection to my_db.
```

### 5.4 Comprehensions

Comprehensions provide a concise way to create lists, dictionaries, and sets.

#### List Comprehensions

```python
# Classic for loop
squares = []
for i in range(10):
    squares.append(i * i)

# List comprehension (more Pythonic)
squares_comp = [i * i for i in range(10)]

# With a condition
even_squares = [i * i for i in range(10) if i % 2 == 0]
```

#### Dictionary and Set Comprehensions

```python
# Dictionary comprehension
square_dict = {i: i * i for i in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Set comprehension
unique_squares = {i * i for i in [-1, 1, -2, 2]}
# {1, 4}
```

-----

## 6\. Object-Oriented Programming (OOP) Best Practices

### 6.1 Classes and Instances

A class is a blueprint for creating objects. An object is an instance of a class.

```python
class Dog:
    # Class attribute, shared by all instances
    species = "Canis familiaris"

    def __init__(self, name: str, age: int):
        # Instance attributes, unique to each instance
        self.name = name
        self.age = age

    def bark(self) -> None:
        print(f"{self.name} says woof!")

# Creating instances
buddy = Dog("Buddy", 5)
lucy = Dog("Lucy", 3)

buddy.bark() # Output: Buddy says woof!
print(lucy.species) # Output: Canis familiaris
```

### 6.2 Inheritance and Composition

  * **Inheritance (`is-a` relationship):** Create a new class that is a modified version of an existing class.
  * **Composition (`has-a` relationship):** Create a new class that contains instances of other classes.

**Best Practice:** Prefer composition over inheritance. It often leads to more flexible and maintainable designs.

```python
# Inheritance Example
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError("Subclass must implement abstract method")

class Cat(Animal):
    def speak(self):
        return "Meow"

# Composition Example
class Engine:
    def start(self):
        return "Engine starts..."

class Car:
    def __init__(self, model):
        self.model = model
        self.engine = Engine() # Car 'has-a' Engine

    def drive(self):
        print(self.engine.start())
        print(f"{self.model} is driving.")

my_car = Car("Tesla Model S")
my_car.drive()
```

### 6.3 Dunder (Magic) Methods

Methods with double underscores (e.g., `__init__`, `__str__`) allow your objects to implement and integrate with Python's built-in behaviors.

  * `__init__(self, ...)`: The constructor, called when an object is created.
  * `__str__(self)`: Called by `str(obj)`. Should return a user-friendly string representation.
  * `__repr__(self)`: Called by `repr(obj)`. Should return an unambiguous, developer-friendly string that, ideally, can recreate the object.
  * `__len__(self)`: Called by `len(obj)`.
  * `__eq__(self, other)`: Implements the `==` operator.

<!-- end list -->

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __str__(self):
        return f"({self.x}, {self.y})"

    def __add__(self, other):
        # Allows using the '+' operator on Vector objects
        if isinstance(other, Vector):
            return Vector(self.x + other.x, self.y + other.y)
        return NotImplemented

v1 = Vector(2, 3)
v2 = Vector(3, 4)
v3 = v1 + v2

print(repr(v3)) # Output: Vector(5, 7)
print(str(v3))  # Output: (5, 7)
```

### 6.4 Properties and Encapsulation

Use properties to create "managed attributes" with getter, setter, and deleter methods.

**Best Practice:** Start with simple public attributes. Only introduce properties when you need to add logic to getting or setting an attribute. Avoid Java-style `get_x()` and `set_x()` methods.

```python
class Circle:
    def __init__(self, radius):
        if radius < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = radius

    @property
    def radius(self):
        """The radius of the circle."""
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value

    @property
    def area(self):
        """Calculate the area. This is a read-only property."""
        return 3.14159 * (self._radius ** 2)

c = Circle(10)
print(c.radius) # Accesses like an attribute, runs the getter
c.radius = 12   # Accesses like an attribute, runs the setter
print(c.area)   # Read-only property
```

-----

## 7\. Modules and Packages

### 7.1 Structuring a Project

A typical Python project structure:

```
my_project/
│
├── .venv/                      # Virtual environment
├── my_package/                 # The main package source code
│   ├── __init__.py
│   ├── module1.py
│   ├── module2.py
│   └── subpackage/
│       ├── __init__.py
│       └── module3.py
│
├── tests/                      # Tests for the package
│   ├── __init__.py
│   ├── test_module1.py
│   └── test_module2.py
│
├── .gitignore
├── README.md
└── requirements.txt
```

### 7.2 Imports

Follow a standard order for imports to improve readability.

1.  Standard library imports (`os`, `sys`).
2.  Third-party library imports (`requests`, `numpy`).
3.  Local application/library imports.

Separate each group with a blank line. Use an automated tool like `isort` to handle this for you.

```python
# Good import style
import os
import sys

import requests

from my_package import module1
from my_package.subpackage import module3
```

**Avoid wildcard imports (`from module import *`)**. They pollute the namespace and make it unclear where names are coming from.

### 7.3 `__init__.py`

The `__init__.py` file serves two purposes:

1.  It tells Python that the directory should be treated as a package.
2.  It can be used to execute package initialization code or to define the `__all__` variable, which controls what is imported with `from package import *`.

A minimal `__init__.py` can be empty.

-----

## 8\. Working with the Standard Library

### 8.1 `os` and `pathlib` for File System Operations

**Best Practice:** Use the `pathlib` module for modern, object-oriented file system paths. It's generally easier and more intuitive than `os.path`.

```python
from pathlib import Path

# Create a path object
current_dir = Path(".")
data_file = Path("/path/to/my/data.csv")

# Get parts of the path
print(data_file.parent) # /path/to/my
print(data_file.name)   # data.csv
print(data_file.stem)   # data
print(data_file.suffix) # .csv

# Join paths
config_path = current_dir / "config" / "settings.ini"

# Check existence
if config_path.exists():
    print("Config exists.")

# Read/write text
config_path.write_text("API_KEY=12345")
content = config_path.read_text()
```

### 8.2 `datetime` for Dates and Times

The `datetime` module supplies classes for manipulating dates and times.

**Best Practice:** Be mindful of time zones. Use timezone-aware `datetime` objects whenever possible, especially for applications that deal with users in different locations. Use the `zoneinfo` library (or `pytz` for older Python versions).

```python
from datetime import datetime
from zoneinfo import ZoneInfo

# Naive datetime (no timezone info)
now_naive = datetime.now()

# Timezone-aware datetime
utc_now = datetime.now(ZoneInfo("UTC"))
tokyo_now = datetime.now(ZoneInfo("Asia/Tokyo"))

print(f"UTC Time: {utc_now}")
print(f"Tokyo Time: {tokyo_now}")

# Parsing and formatting
date_str = "2025-09-01T15:30:00"
parsed_date = datetime.fromisoformat(date_str)
formatted_date = parsed_date.strftime("%A, %B %d, %Y")
print(formatted_date) # Monday, September 01, 2025
```

### 8.3 `collections` for Advanced Data Structures

  * `collections.Counter`: A dict subclass for counting hashable objects.
  * `collections.defaultdict`: A dict subclass that calls a factory function to supply missing values.
  * `collections.deque`: A list-like container with fast appends and pops from both ends.
  * `collections.namedtuple`: A factory function for creating tuple subclasses with named fields.
  * `collections.OrderedDict`: A dict subclass that remembers the order entries were added. (Less critical since Python 3.7+, where standard dicts are ordered).

<!-- end list -->

```python
from collections import defaultdict, Counter

# defaultdict example
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = defaultdict(list)
for k, v in s:
    d[k].append(v)

print(d.items())
# dict_items([('yellow', [1, 3]), ('blue', [2, 4]), ('red', [1])])

# Counter example
word_counts = Counter("mississippi")
print(word_counts)
# Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```

### 8.4 `json` for Data Serialization

The `json` module allows you to encode Python objects as a JSON string, and decode JSON strings into Python objects.

```python
import json

# Python dictionary
data = {
    "name": "John Doe",
    "age": 30,
    "isStudent": False,
    "courses": [
        {"title": "History", "credits": 3},
        {"title": "Math", "credits": 4}
    ]
}

# Serialize to JSON string
json_string = json.dumps(data, indent=4)
print(json_string)

# Deserialize from JSON string back to Python object
reloaded_data = json.loads(json_string)
print(reloaded_data["name"]) # John Doe
```

-----

## 9\. Testing and Code Quality

### 9.1 Importance of Testing

Writing tests for your code is crucial for:

  * Ensuring correctness.
  * Preventing regressions (introducing new bugs when changing code).
  * Providing documentation on how the code is intended to be used.
  * Facilitating refactoring.

### 9.2 Using `pytest`

`pytest` is a popular third-party testing framework that is more expressive and less boilerplate-heavy than the built-in `unittest` module.

#### Example Test File (`tests/test_calculations.py`)

Assume you have a function `add(x, y)` in `my_package/calculations.py`.

```python
# my_package/calculations.py
def add(x, y):
    return x + y
```

```python
# tests/test_calculations.py
import pytest
from my_package.calculations import add

def test_add_positive_numbers():
    """Test adding two positive numbers."""
    assert add(1, 2) == 3

def test_add_negative_numbers():
    """Test adding two negative numbers."""
    assert add(-1, -1) == -2

def test_add_mixed_numbers():
    """Test adding a positive and a negative number."""
    assert add(5, -3) == 2

# Use parametrize to run the same test with different inputs
@pytest.mark.parametrize("x, y, expected", [
    (1, 2, 3),      # Test case 1
    (-1, 1, 0),     # Test case 2
    (10, -5, 5),    # Test case 3
    (0, 0, 0),      # Test case 4
])
def test_add_parametrized(x, y, expected):
    assert add(x, y) == expected
```

To run the tests, navigate to the project's root directory and simply run `pytest`.

### 9.3 Linters and Formatters

  * **Linter (`flake8`, `pylint`):** A tool that analyzes source code to flag programming errors, bugs, stylistic errors, and suspicious constructs.
  * **Formatter (`black`, `isort`):** A tool that automatically reformats your code to be consistent with a style guide.

**Best Practice:** Integrate these tools into your development workflow. Use pre-commit hooks to automatically run them before you commit code.

#### Example `black` usage:

```bash
# Install black
pip install black

# Format a file or directory
black my_package/
```

`black` is "uncompromising," meaning it provides very few configuration options. This ensures consistency across projects and teams.

-----

## 10\. Common Pitfalls and Anti-Patterns

### 10.1 Mutable Default Arguments

A default argument is evaluated only once, when the function is defined. If that default is a mutable object, it will be shared across all calls to the function.

```python
# The WRONG way
def add_item(item, my_list=[]):
    my_list.append(item)
    return my_list

# First call seems fine
print(add_item(1)) # [1]
# Second call has unexpected behavior
print(add_item(2)) # [1, 2] -- the list from the first call is reused!

# The CORRECT way
def add_item_correct(item, my_list=None):
    if my_list is None:
        my_list = []
    my_list.append(item)
    return my_list

print(add_item_correct(1)) # [1]
print(add_item_correct(2)) # [2]
```

### 10.2 Misunderstanding Python's Scope

Python's scope resolution follows the **LEGB** rule: **L**ocal, **E**nclosing, **G**lobal, **B**uilt-in. If you want to modify a global variable from within a function, you must use the `global` keyword. If you want to modify a variable in an enclosing (non-local) scope, use `nonlocal`.

```python
x = "global"

def outer_func():
    x = "enclosing"
    def inner_func():
        # To modify the enclosing scope 'x', you would use:
        # nonlocal x
        x = "local"
        print(f"Inside inner_func: {x}") # local
    
    inner_func()
    print(f"Inside outer_func: {x}") # enclosing

outer_func()
print(f"Global scope: {x}") # global
```

### 10.3 Floating-Point Inaccuracies

Floating-point numbers have precision limitations. Don't use them for exact calculations, especially financial ones. Use the `decimal` module instead.

```python
# Imprecision with floats
val = 0.1 + 0.1 + 0.1 - 0.3
print(val) # Output is a very small number, not 0

# Using the Decimal module for precision
from decimal import Decimal

val_decimal = Decimal('0.1') + Decimal('0.1') + Decimal('0.1') - Decimal('0.3')
print(val_decimal) # Output: 0.0
```

-----

## 11\. Security Best Practices

### 11.1 Input Validation

Never trust user input. Always validate and sanitize input from users, APIs, or any external source to prevent injection attacks (e.g., SQL injection, command injection).

### 11.2 Using `subprocess` Securely

When running external commands, avoid `shell=True` with unvalidated input, as it can lead to command injection vulnerabilities.

```python
import subprocess

# DANGEROUS: user_input could be "; rm -rf /"
user_input = "some_file.txt; ls"
# subprocess.run(f"ls {user_input}", shell=True)

# SAFE: The command and its arguments are passed as a list
# Each argument is treated as a single token, not interpreted by the shell
safe_input = "some_file.txt"
subprocess.run(["ls", safe_input])
```

### 11.3 Managing Secrets

Do not hardcode secrets (API keys, passwords, database credentials) in your source code.

  * Use environment variables.
  * Use a dedicated secrets management tool (e.g., HashiCorp Vault, AWS Secrets Manager).
  * Use `.env` files for local development (and ensure `.env` is in your `.gitignore`).

<!-- end list -->

```python
# Good practice: load secrets from environment variables
import os

api_key = os.getenv("MY_API_KEY")
if not api_key:
    raise ValueError("MY_API_KEY environment variable not set.")
```

-----

## 12\. Performance Considerations

### 12.1 Profiling Your Code

Don't guess where bottlenecks are. Profile your code to find the parts that are slow. Python's built-in `cProfile` module is a good starting point.

```bash
python -m cProfile -s tottime my_script.py
```

This command will run `my_script.py` and print a report of the functions where the most time was spent.

### 12.2 Algorithmic Complexity

The biggest performance gains often come from choosing the right data structures and algorithms.

  * Searching for an item in a `list` is O(n).
  * Searching for an item in a `set` or `dict` (by key) is O(1) on average.

<!-- end list -->

```python
# Inefficient for large lists
if item in my_large_list:
    ...

# Much faster for large sets
if item in my_large_set:
    ...
```

### 12.3 Built-in Functions are Faster

Built-in functions implemented in C (like `sum()`, `map()`, `sorted()`) are almost always faster than an equivalent implementation in pure Python.

-----

## 13\. Further Resources

  * [The Official Python Tutorial](https://docs.python.org/3/tutorial/)
  * [The Python Standard Library Documentation](https://docs.python.org/3/library/)
  * [PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)
  * [The Hitchhiker's Guide to Python\!](https://docs.python-guide.org/)
  * [Real Python](https://realpython.com/)
