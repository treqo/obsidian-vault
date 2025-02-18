**Type:** [[textbook]] – 
**Topics:** [[computers]] [[programming]] [[python]] 
**Date:** 2024-07-01  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- #computers 

---
# Notes

```ad-note
title: Python runtimes
There are many popular runtimes for python

* CPython
* Jython
* IronPython
* PyPy
```

- [?] What is a python [[runtime]]?
	A runtime is the environment in which a program or code is executed. This includes the interpreter, and any imported libraries or packages.

## PEP 8 Style Guide

[Python Enhancement Proposal #8](https://peps.python.org/pep-0008/) dictates a preferred style on how to write python code.

Here are the main points from it:

A style guide is for **consistency**. The whole point is to write consistent code so that you know where to look. It's like the difference between an organized room and a disorganized room. Both can work and you can get stuff done, but at the end of the day, wouldn't you want to have a clean room?
### Indentation

**Long Functions**
```python
# Aligned with opening delimiter.
foo = long_function_name(var_one, var_two,
						 var_three, var_four)

# Add an extra indentation on the params so print distinguishable
def long_function_name(
		var_one, var_two, var_three,
		var_four):
	print(var_one)

# Hanging indents should add a level.
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

**Long `if` statement**
```Python
# No extra indentation.
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# Add a comment, which will provide some distinction in editors
# supporting syntax highlighting.
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# Add some extra indentation on the conditional continuation line.
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```

**Closing brackets/parenthesis/braces**
```Python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```

**Wrapping Lines**
```ad-note
title: Note
An upperbound of 79 characters in a line – 72 characters for comments and docstrings.
```

**Docstring** inside functions
```Python
def example_function():
    """
    This is an example function that demonstrates how to properly format
    a docstring to ensure it does not exceed 72 characters per line. This
    makes it easier to read in various tools and environments.
    """
    pass
```

Use **Comments** everywhere else
```Python
# This is a comment that demonstrates how to properly format a comment to
# ensure it does not exceed 72 characters per line. This makes it easier
# to read in various tools and environments.
def example_function():
    pass

```

Breaking a long line using **parenthesis**
```Python
# Before
result = some_function_with_a_really_long_name(argument_one, argument_two, argument_three, argument_four)

# After
result = some_function_with_a_really_long_name(
    argument_one, argument_two, argument_three, argument_four
)
```

Breaking a **dict** and **list** over multiple lines
```Python
# Before
my_dict = {'key1': 'value1', 'key2': 'value2', 'key3': 'value3', 'key4': 'value4'}

# After
my_dict = {
    'key1': 'value1',
    'key2': 'value2',
    'key3': 'value3',
    'key4': 'value4'
}

# Before
my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

# After
my_list = [
    1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15
]

# OR

my_list = [
	1, 2, 3,
	4, 5, 6,
	7, 8, 9,
	10, 11, 12,
	13, 14, 15
]
```

Breaking **`with`** statement
``` Python
# Before
with open('/path/to/some/file/you/want/to/read') as file_1, open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())

# After
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

Breaking **`asserts`**
```Python
# Before
assert some_condition and another_condition and yet_another_condition, "Conditions not met!"

# After
assert (
    some_condition and
    another_condition and
    yet_another_condition
), "Conditions not met!"
```

### Binary Operators

Line breaks should be **before** binary operators

```Python
# Correct:
# easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

### Blank Lines

Use **2 blank lines** to separate class and top-level function definitions. You may use **1 blank line** as a separator for [[methods]] inside classes and top-level functions that are related.

```Python
# Before
def function_one():
    pass
def function_two():
    pass

class MyClass:
    def method_one(self):
        pass
    def method_two(self):
        pass

# After
def function_one():
    pass


def function_two():
    pass


class MyClass:
    def method_one(self):
        pass

    def method_two(self):
        pass
```

```ad-warning
title: Thoughts
Personally, I don't agree very much with everything said in this section. I believe that separating the code into modules and perhaps using double blank lines sparingly is the right thing to do.
```
### Imports

```ad-info
title: Info
- Imports should *usually* be on separate lines.
- Imports should always be put at the top of a file, after any comments, and before any global declarations.
- Grouped in the following order:
	1. Standard library imports.
	2. Related third party imports.
	3. Local application/library specific imports.
- You should put a blank line between each group of imports.
```

```Python
"""
This module demonstrates the correct way to handle imports in Python
according to PEP 8 guidelines.
"""

# Standard library imports
import os
import sys

# Third-party imports
from flask import Flask
from requests import get

# Local application/library specific imports
from myapp.utils import helper_function
from .models import MyModel

# Function using the imported modules and classes
def main():
    # Using standard library import
    current_dir = os.getcwd()
    
    # Using third-party import
    response = get('https://example.com')
    
    # Using local application/library import
    model_instance = MyModel()
    helper_function(model_instance)
    
    # Print results
    print("Current Directory:", current_dir)
    print("Response Status Code:", response.status_code)

if __name__ == "__main__":
    main()
```
### Naming

```Python
# Module-level constant
PI = 3.14159

# Function in lowercase_underscore format
def calculate_area(radius):
    return PI * radius * radius

# Class in CapitalizedWord format
class Circle:
    
    # Class-level attribute in ALL_CAPS format
    DEFAULT_COLOR = "red"
    
    # Constructor with self as the first parameter
    def __init__(self, radius):
	    # Public instance attribute in lowercase_underscore format
        self.radius = radius
        
        # Protected instance attribute in _leading_underscore format
        self._color = Circle.DEFAULT_COLOR
        
        # Private instance attribute in __double_leading_underscore format
        self.__secret_value = 42
    
    # Instance method
    def get_area(self):
        return calculate_area(self.radius)
    
    # Class method with cls as the first parameter
    @classmethod
    def set_default_color(cls, color):
        cls.DEFAULT_COLOR = color
    
    # Static method
    @staticmethod
    def is_valid_radius(radius):
        return radius > 0

# Example usage
if __name__ == "__main__":
    my_circle = Circle(5)
    print(f"Area: {my_circle.get_area()}")
    print(f"Default color: {Circle.DEFAULT_COLOR}")
    Circle.set_default_color("blue")
    print(f"New default color: {Circle.DEFAULT_COLOR}")
    print(f"Is valid radius (-1): {Circle.is_valid_radius(-1)}")
    print(f"Is valid radius (3): {Circle.is_valid_radius(3)}")
```
#### Summary of Naming Conventions (PEP 8)

- **Functions, variables, and attributes**: `lowercase_underscore`
- **Protected instance attributes**: `_leading_underscore`
- **Private instance attributes**: `__double_leading_underscore`
- **Classes and exceptions**: `CapitalizedWord`
- **Module-level constants**: `ALL_CAPS`
- **Instance methods in classes**: First parameter `self`
- **Class methods**: First parameter `cls`

### Expressions and Statements

```Python
"""
This module demonstrates PEP 8 guidelines for expressions, statements, and imports.
"""

# Standard library imports
import os
import sys

# Third-party imports
import requests
from flask import Flask

# Local application/library specific imports
from myapp.utilities import helper_function
from .models import MyModel

# Correct usage of inline negation
if a is not b:
    print("a is not b")

# Correct checking for empty values
some_list = []
if not some_list:
    print("some_list is empty")

# Correct checking for non-empty values
some_list = [1, 2, 3]
if some_list:
    print("some_list is not empty")

# Avoid single-line control structures
for item in some_list:
    if item > 0:
        print(item)

try:
    result = 1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

# Example function demonstrating good practices
def process_data(data):
    """
    Process the input data and return the result.
    """
    if not data:
        return None

    result = []
    for item in data:
        if item.is_valid():
            result.append(item.processed())

    return result

# Example class demonstrating good practices
class DataProcessor:
    """
    A class to process data.
    """
    
    def __init__(self, data):
        self.data = data
        self._processed_data = None  # Protected instance attribute
        self.__secret_data = "secret"  # Private instance attribute

    def process(self):
        if not self.data:
            return None
        self._processed_data = [item.processed() for item in self.data]
        return self._processed_data

    @classmethod
    def create_processor(cls, data_source):
        data = data_source.get_data()
        return cls(data)
```
#### Summary

1. **Inline Negation**:
    - Use `if a is not b` instead of `if not a is b`.
2. **Checking for Empty Values**:
    - Use `if not somelist` instead of `if len(somelist) == 0`.
    - Non-empty values evaluate to `True`: `if somelist`.
3. **Avoid Single-Line Control Structures**:
    - Spread `if`, `for`, `while`, and `except` statements over multiple lines for clarity.
4. **Import Statements**:
    - Always place import statements at the top of the file.
    - Use absolute imports: `from bar import foo` instead of `import foo`.
    - For relative imports, use explicit syntax: `from . import foo`.
    - Organize imports into sections in this order:
        - Standard library modules
        - Third-party modules
        - Your own modules
    - Within each section, imports should be in alphabetical order.
### Using Pylint

**`pylint`** is a static analyzer for python source code. Once installed in the virtual environment:

```sh
pip install pylint
```

you can run

```sh
# Run it on a python script
plyint script.py 

# Run it on a directory
pylint my_project/
```

and it will automatically give feedback on your codes convention.

### Character Types

In Python 3 there are two primitive types for characters (implicitly declared)
- `bytes`: contain raw 8-bit values
- `str`: contain [[unicode]] characters

In Python 2 the character primitive types are:
- `str`: contains 8-bit values
- `unicode`: contains unicode characters

To convert unicode characters as raw binary data (raw 8-bit values), need to use the `encode` method. For the reverse operation use `decode` method. These operations use a **UTF-8** encoding.
#### Type Annotation

Although python is a [[dynamically typed]] language, you are able to explicitly annotate the [[types]] in variable declaration, method parameters, and method return type. Note that Python **does not** do type checking, even if declared, neither in pre-compile, or runtime.

That being said, one can enforce type checking with `mypy`, a [[static type checker]]. 

To install `mypy`

```sh
pip install mypy
```

Run `mypy` on a script

```sh
mypy script.py
```

**Type Annotated Variable Declaration**
```Python
name: str = "Alice"
age: int = 30
```

**Type Annotated Function declaration**
```Python
def add(x: int, y: int) -> int:
    return x + y
```

#### Python Types

List of built-in types in python

```Python
# Numerical Types
x: int = 5
y: float = 5.5
z: complex = 2 + 3j

# Sequence Types
s: str = "hello"
b: bytes = b"hello"
my_list: list[int] = [1, 2, 3]
my_tuple: tuple[int, str] = (1, "apple")
r: range = range(5)

# Collection Types
my_dict: dict[str, int] = {"one": 1, "two": 2}
my_set: set[int] = {1, 2, 3}
my_frozenset: frozenset[int] = frozenset([1, 2, 3])

# Boolean Type
flag: bool = True

# NoneType
n: None = None
```

### Write Helper Functions

In python, you have the capability to perform a lot of operations in a small amount of space. Although that may seem better, it's harder to debug and can look quite ugly. Instead, make **helper functions**
#### Example

Consider the following code snippet

```Python
from urllib.parse import parse_qs
my_values = parse_qs('red=5&blue=0&green=', keep_blank_values=True)
print(repr(my_values))

>>> {'red': ['5'], 'green': [''], 'blue': ['0']}
```

```ad-summary
title: Analysis
This code uses the urllib package, parse module, and specifically imports the parse_qs function. This function takes in a string of queries (looks like something from a url) and returns a representation in the form of a dictionary
`dict[str, list[str]]`.

The result is printed as shown.
```

```ad-tip
Use `type()` method to see what type `my_values` is. Spoiler, it is a `dict`.
```

To see the values in the dictionary, we can use the `get()` method or use the `[]` operator.

```Python
print(my_values.get('red'))       # ['5']
print(my_values['green'])         # ['']
print(my_values.get('opacity'))   # None
```

```ad-note
The empty `string`, empty `list`, and 0 all evaluate to `False` in Python syntax
```

**Positional-Only `/`**
```Python
def get(
    key: str,
    /
) -> (list[str] | None):
```

The `/` operator indicates that previous parameters before it are *positional-only*, meaning that it depends on position, and it won't accept writing `key=` when using the method.

```Python
green = my_values.get('green', [''])[0] or 0 
# This checks the dict to see if 'green' is inside
# if not then returns empty list by default
# check the first element of the string, if empty string then would return 0
```

**Making things less complex**
```Python
red = int(my_values.get('red',[''])[0] or 0)
# There is too much going on in one line

red = my_values.get('red',[''])
red = int(red[0]) if red[0] else 0
# This means, let red be int(red[0]) if red[0] is defined 
# Empty string evaluates to false in which case set as 0
```

**Helper Function**
```Python
def get_first_int(values, key, default=0):
	query = values.get(key, [''])
	if query[0]:
		return int(query[0])
	else:
		return default
```
This is the best course of action because
- cleaner
- better to debug
- reusable

### Know How to Slice Sequences



---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
