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
# Syntax tips

- Python introduces syntactic sugar to:
    - Concatenate strings with the `+` operator. e.g. 

# Module imports

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

- 
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

# Meow 3 times

- It is not required to have a `main()` function in Python, but it is the convention to have one.
    - This means that we can define our functions _after_ we show how we're calling them (and we can't rely on [[C#^4d6eb2|function prototypes like in C]]).
    - However, since this is convention and not enforced by the language, `main()` is not actually called for you; so you need to call it yourself.

```python
def main():
    meow(5)

def meow(n):
    for _ in range(n):
        print("meow")

main()
```

