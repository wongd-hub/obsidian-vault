#course_cs50 

- We step into the realm of high-level languages now, that *abstract* away all of the low-level implementation details that we had to think about when working with [[C]].
- Python's popularity derives from relatively easy to read it is; it also has a large ecosystem of libraries, allowing you to get real work done faster.

> [!tip]
> The term *Pythonic* refers to design patterns that are the best way to do things in Python (partially by consensus of the Python community).

# Differences to C

- We will now run Python scripts with the following commands. 
    - We no longer need to compile the language before running anymore. This is because Python is an *interpreted* language

```shell
python hello.py
```

- Syntactic differences:
    - We can use single and double quotes interchangeably. 
    -  We no longer need to terminate lines of code with `;`.

- Behavioural differences:
    - We do away with the need to load libraries for functions that are included in base Python; as well as the `main()` function.
    - Python also manages your memory for you. No more need for `malloc()` and pointers.
    - Python is *dynamically typed*, you no longer specify the variable's type when you declare it.

> [!tip]
> Learning C teaches us what is happening underneath the hood, but Python lets us express our solutions to the same problems more readily and with less code.
# So why use C?

- C is seen as faster to run than Python. 
- C is also more memory efficient - Python is managing memory for you and won't know *a priori* how much memory you will need overall. It will likely over-allocate for you.
# Syntax tips / syntactic sugar

- Python introduces syntactic sugar to:
    - Concatenate strings with the `+` operator. e.g.
    - Repeat a string $n$ times: `print("?" * n)`

# Module imports / libraries

```python
# Import the full module
import math

# Import some functions from the module
from random import randint
from cs50 import get_int, get_string

# Aliasing modules
import math as m

# Accessing functions from libraries
random.randint()
m.factorial()
```

## `pip`

- `pip` is how Python packages are installed.

```bash
pip install cowsay
```

## The `sys` library

- We got access to command line arguments with `argc` and `argv` in C ([[Command Line Arguments for C]]).
- Here's how we do it in Python.
    - Note we're technically typing 3 words at the prompt but the `python` part of the command is ignored, so `python greet.py` technically has a `len(argv)` of 1.

```python
from sys import argv

if len(argv) == 2:
    print(f"hello, {argv[1]}")
else:
    print("hello, world")

#$ python greet.py
#> hello, world
#$ python greet.py Carter
#> hello, Carter
```

- Here's how we exit a running process:

```python
import sys

if len(sys.argv) != 2:
    print("Missing command-line argument")
    sys.exit(1) # Setting exit code

print(f"hello, {sys.argv[1]}")
sys.exit(0)
```

## Generating QR codes

```python
#$ pip install qrcode

import qrcode

img = qrcode.make("https://youtu.be/xvFZjo5PgG0")
img.save("qr.png", "PNG")
```
# Format strings (f-strings)

- Python has built-in string interpolation with *f-strings*:

```python
answer = input("What's your name? ")
print(f"hello, {answer}")
```

# Variables

- Declare variables simply like so:

```python
counter = 0
```

- Increment them like so:

```python
counter += 1

# OR

counter -= 1

# C's counter++, counter-- are no longer present
```

# Data types

- We have the following data types in Python:

```
# Familiar data-types
bool
float
int
str

# Newer data-types
range 
list  - Similar to an array, but better
tuple - Combinations of values that don't change (x, y)
dict  - Built-in hash tables
set   - Gets rid of duplicates for you
```

- We're missing double/longs which use more bits to store more information as Python simplifies these to floats.

- Python's dynamic typing leads to some interesting behaviour - when providing inputs to `input()`, the result is always a string.
    - We need to coerce these strings to ints.

> [!tip]
> - Note the lecturer uses the terminology of 'converting'/'coercing' instead of 'casting' here. This is because we no longer deal with single characters that can map clearly to single numbers in decimal (like 65 to A).

```python
x = input("x: ")
y = input("y: ")

print(x + y)

#$ python calculator.py
#> x: 1
#> y: 2
#> 12

x = int(input("x: "))
y = int(input("y: "))

print(x + y)

#$ python calculator.py
#> x: 1
#> y: 2
#> 3
```

## Lists

- Denoted by square brackets, `[]`. This recalls the syntax of C [[Arrays|arrays]], however in Python lists are more like linked lists: the memory is automatically handled for you.
- Lists, like other data types, have a range of methods that come pre-packaged.
    - `len(list)`
    - `sum(list)`
    - `list.append(4)`: append an item to the end of the list

## Dicts

- Recall that this [[Abstract Data Types|abstract data type]] is a collection of key-value pairs. This is implemented as a hash table in Python.
- We can do things like give ourselves a list of dictionaries:

```python
people = [
    {
        "name": "David",
        "number": "..."
    },
    {"name": "Carter", "number": "…"},
    {"name": "John", "number": "…"},
]

# We can then access a person's number doing things like this:
name = input("Name: ")

for person in people:
    if person["name"] == name:
        print(f"Found {person['number']}")
        break
else:
    print("Not found")

# An even better version of this - since we only have the number as an attribute
people = {
    "Carter": "...",
    "David": "...",
    "John": "..."
}

if name in people:
    print(f"Found {people[name]}")
else:
    print("Not found")
```

# Control flow & boolean operators

- No curly braces, we use indentation to denote scoping
    - And no parentheses around the boolean test either

```python
if x < y:
    print("x is less than y")
elif x > y:
    print("x is greater than y")
else:
    print("x is qeual to y")
```

- We can compare strings directly instead of needing [[C#String comparison]].
    - Recall in C, when we do this we're attempting to compare the `char*` which is the memory address of the first character in the string.

```python
x = get_input("word 1: ")
y = get_input("word 2: ")

if x == y:
    print("Same")
```

- *And* and *or* are no longer denoted by `&` and `|`.

```python
if x == "Y" or y == "N":
    ...

if x == "Y" and y == "N":
    ...
```

- Checking if a value is in an array of possible values:

```python
s = input("Do you agree? ").lower()

if s in ["y", "yes"]:
    print("Agreed")
elif s in ["n", "no"]:
    print("Not agreed")
```

# Looping

- It's a lot easier to loop in Python than in C because it's easy to loop over anything that is *iterable*.

- Note that you don't define `i` inside of the loop declaration

```python
i = 0
while i < 3:
    print("meow")
    i += 1

while True:
    print("meow")
```

```python
# Correct, but bad design
for i in [0, 1, 2]:
    print("hello, world")

# Note, no need to manually iterate i, this is automatic

# Better to create the range dynamically
for i in range(3):
    print("hello, world")

# If you don't care about the value of the iterator, use an underscore
#  Note, underscore is a valid character - there is no special meaning,
#  this is just convention.
for _ in range(3):
    print("hello, world")
```

## For-loop else clause

- Python has an `else` clause to accompany its for-loop. It will trigger if the for-loop gets to the end of processing without ever triggering `break`.

```Python
names = ["Carter", "David", "John"]

name = input("Name: ")

for n in names:
    if name == n:
        print("Found")
        break
else:
    print("Not found")

# More Pythonic would be:
if name in names:
    print("Found")
else:
    print("Not found")
```

# Object-oriented programming

- When you passed a variable to `tolower()` or `toupper()` in C, they just trusted that you passed them a string.

- Python is an Object-Oriented Programming (OOP) language, where variables can not only have values, they can have functionality built into them.
    - If you have a string data-type, it makes sense that they should be able to be converted to lowercase, or to get a count of characters in them. So these functions (called methods when they're attached to data types) don't sit in some library you need to load, they're built into the strings themselves.

- This is what we're calling when we're doing the following in the above block:
    - Using the `lower()` method on the input string object

```python
s = input("Do you agree? ").lower()
```

# Truncation

- In C, [[C#Truncation|truncation]] was an issue. This is where we divided two integers and would only get back the integer portion of the fractional answer by default.
- Python is a little smarter when converting one value into another. An integer divided by an integer will be a float.

# Floating point imprecision

- In C, we had the problem with [[C#Floating point imprecision|floating point imprecision]].
- To see more significant figures, we need to use an f-string and format the number like so (use a colon and the format string):

```Python
x = 1
y = 3

z = x / y
print(f"{z:.50f}")

#$ 0.3333333333333331482961
```

- With this, we can see that floating point imprecision is still a problem in Python.
- There do exist packages that let you get more precision.

# Integer overflow

- In C, if we increment a number past the maximum representable number for that data type, we end up going back to 0 ([[C#Memory usage & integer overflow|integer overflow]]).
- Python will grow an integer dynamically to keep fitting the number - it is not a static number of bits; thus doing away with the problem of integer overflow.

# Exceptions

- A better way to handle errors. In C, we could only really handle errors by having functions [[Exit Status|return special values]].
- There are a whole list of possible exceptions in Python such as ValueError or NameError.

```python
def get_int(prompt):
    while True:
        try:
            # Returning also breaks you out of this loop
            return int(input(prompt))
        except ValueError:
            # print("Not an integer")
            pass # This silently moves to the next iteration


def main(): 
    ...
```

# Code/function examples
## To upper case

```python
before = input("Before: ")
print("After: ", end="")

for c in before:
    print(c.upper(), end="")

print()
#$ python uppercase.py
#> Before: cat
#> After:  CAT

# But we don't need to loop over whole strings. Can just do:
before = input("Before: ")
print(f"After: {before.upper()}")
```

## Meow 3 times

- It is not required to have a `main()` function in Python, but it is the convention to have one.
    - This means that we can define our functions *after* we show how we're calling them (and we can't rely on [[C#^4d6eb2|function prototypes like in C]]).
    - However, since this is convention and not enforced by the language, `main()` is not actually called for you; so you need to call it yourself.

```python
def main():
    meow(5)


def meow(n):
    for _ in range(n):
        print("meow")


main()
```

## Opening and analysing CSVs

- Here's how we'd analyse some simple data using Python.

```python
import csv

file = open("favourites.csv", "r")
# Do something with file
close(file)

# File will be automatically closed if we do this:
with open("favourites.csv", "r") as file:
    reader = csv.reader(file)
    next(reader) # Skip the first header line in the CSV
    for row in reader:
        # Returns the row as an array
        favourite = row[1] # Print contents of second column
        print(favourite)

# What would be better? 
# We should be using the names of the columns to query them
with open("favourites.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        # Returns the row as a dictionary with keys = the column names
        print(row["language"])

# Count the number of favourite language responses
with open("favourites.csv", "r") as file:
    reader = csv.DictReader(file)
    counts = {}
    for row in reader:
        favourite = row['language']
        if favourite in counts:
            counts[favourite] += 1
        else:
            counts[favourite] = 0

for favourite in sorted(counts, key=counts.get, reverse=True): 
    # sorted() sorts the keys by the size of the values (gotten by .get)
    print(f"{favourite}: {counts[favourite]}")

# Or we can do this with collections without needing an ifelse
from collections import Counter

with open("favourites.csv", "r") as file:
    reader = csv.DictReader(file)
    counts = Counter()

    for row in reader:
        favourite = row["language"]
        counts[favourite] += 1

# Returns a key-value pair, and sorts the dict in descending order of value
for favourite, count in counts.most_common():
    print(f"{favourite}: {count}")

# Or query what the most popular language is
favourite = input("Favourite: ")
print(f"{favourite}: {counts[favourite]}")
```