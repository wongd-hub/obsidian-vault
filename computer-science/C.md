#course_cs50 #C

- The introductory code block is as follows - in a file named `hello.c`

```C
#include <stdio.h>

int main(void) {
    printf("hello, world!\n");
}
```

- As a high level overview, here's what everything in this block is doing:

```C
#include <stdio.h> // Loads the stdio.h (Standard I/O) library

int main(void) {   // Defines a function, main, that takes no arguments

    // Print hello world, strings must be wrapped in DOUBLE quotes
    printf("hello, world!\n"); // Terminate with a semi-colon

}
```

- `main()` is the entry point of every C program, and serves as the starting point for code execution.
    - `main()` always returns a `0` by default (if the program executed without issue), which is why we set its type to `int`.

- Note also that CS50 has its own [C library](https://github.com/cs50/libcs50/releases) which implements a range of functions that ask the user for an input and return their answer. e.g.
    - `get_string("What is your name? ")`
    - `get_int("What is your age? ")`
## Compiling and running scripts

- The lecturer is using the `code` command to both create an empty text file and open it in VS Code at the same time.
- The `make` command compiles a `.c` script into a binary executable of the same name - sans file extension.
- Invoke the executable to run the program.

i.e. 

```shell
code hello.c # Create and open a new C file
make hello   # Use hello.c to create an executable of the name hello
./hello      # 
```

## Variable declaration / assignment

- In addition to declaring the definition of a variable, C requires us to declare the type of the variable as well:

```C
// Declare a string variable using user input
string answer = get_string("What's your name? ");

// Declare a counter integer
int counter = 0;
```

## Strings

- Strings with more than one character must be wrapped in *double* quotes, `"hello"`.
- Characters with only one character must be wrapped in *single* quotes, `'a'`, and is of the type `char` instead of type `string`.

### Format strings

- C also provides a way to print the contents of variables in a string with [format strings](https://www.cprogramming.com/tutorial/printf-format-strings.html).

```C
printf("hello %s\n", answer);
```

- `s` stands for string. There is also `%c`, `%f`, `%i`, `%li`, etc.

## Syntactic sugar

- Like JavaScript, we can do the following to increment (or decrement) a counter by 1

```C
int counter = 0;

counter = counter + 1; 
// Is the same as:
counter++;
counter += 1;

counter--;
counter -= 1;
```

## Logical operators

```C
|| // OR
&& // AND
== // EQUALS
```

## If-else

```C
if (x < y) {
    // Do something if `true`
} else if (x == y) {
    // Do something if `true`
} else {
    // Otherwise, do this
}
```

## For-loops

```C
// Define a counter, i
// For i < n, iterate...
// ... At end of iteration, increment i by 1

for (int i = 0; i < n; i++) {

    // Do something

}
```


## While-loops

```C
int i = 0; // Almost always start counting from 0
while (i < 3) {

    printf("meow\n");
    i++;

}

// Have something happen forever
while (true) {

    // Do something

}
```

## Do while-loop

```C
int n; // Declare n in the current scope (not in the do{})
do {

    n = get_int("Size: ");

}
while(n < 1); // Do this at least once, and continue if n < 1
```

## Function definitions

```C
// void type means there is no object returned from this function apart from side effects
void meow(void) { // void arg means no arguments expected

    printf("meow\n");

}
```

```C
int add(int x, int y) {

    return x + y;

}
```

## Scoping

- Like other programming languages, there exists the concept of scope - i.e. what variables a function can see.

## Structure of a script

```C
#include <stdio.h>

// We need to define functions before we use them.
// Define meow() here

void meow(void) {
    printf("meow\n");
}

int main(void) {

    for (int i = 0; i < 3; i++) {

        meow();
    
    }

}
```

- However, it's useful for `main()` to be at the top of the file since that's your entry-point. We can hoist the function definition to the top so to speak by copying the function's 'prototype' to the top.
    - This tells C that there will be a function that returns a certain type, and takes certain arguments, that is defined later in the script.

```C
#include <stdio.h>

// meow() prototype
void meow(int n);

int main(void) {

    meow(3);

}

void meow(int n) {

    for (int i = 0; i < n; i++) {

        printf("meow\n");
    
    }
    
}
```

## What is C not good at?

- Random Access Memory (RAM) in the computer is where working data is stored. We have a finite amount of memory in each device.
- In the world of numbers, if we're only using 3 digits we can count to 7
    - If we're omitting the 4th digit, if we try to count past 7, we get a 0 again. i.e. we experience _integer overflow_ where we wrap around to 0 or a negative number. 

`1000 (if we only see the last 3 digits, it's a 0)`

- We often use 8 or 32 bits to represent numbers. 32 bits gives us counts up to 4.2 billion. However, if we need to count to 4.2 billion and 1, we'd need another bit.
    - If we want to also represent negative numbers, we'll need to split that in half again; so we can really only count up to 2.1 billion or down to -2.1 billion.

- When using data types in C, you can stipulate how much memory to use to represent numbers.
    - `int` is by convention 32 bits
    - A `long` can be 64 bits - which can represent numbers exponentially bigger than `int`. `%li` is how you represent a `long` in `printf()`.


## Coding Mario

- We can represent the Mario game in code
- For 4 "?" blocks in sequence:

```C
#include <stdio.h>

int main(void) {

    for (int i = 0; i < 4; i++) {

        // Create 4 "?" blocks
        printf("?");

    }
    printf("\n");

}
```

- For a grid of 3x3 bricks:

```C
#include <stdio.h>

void create_grid(int height, int width);

int main(void) {

    const int h = 3; // This creates an immutable variable
    const int w = 3;
    create_grid(h, w);

}

void create_grid(int height, int width) {

    if (height < 1 || width < 1) {
    
        printf("height or width is < 1")
        
    }

    for (int i = 0; i < height; i++) {

        for (int j = 0; j < width; j++) {
        
            printf('#');

        }
        printf("\n");

    }

}
```
