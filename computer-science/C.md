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
#include <stdio.h> // Loads the stdio.h library

int main(void) {   // Defines a function, main, that takes no arguments

    // Print hello world, strings must be wrapped in DOUBLE quotes
    printf("hello, world!\n"); // Terminate with a semi-colon

}
```

- `main()` is the entry point of every C program, and serves as the starting point for code execution.
    - `main()` always returns a `0` by default, which is why we set its type to `int`.

- Note also that CS50 has its own [C library](https://github.com/cs50/libcs50/releases) which implements a range of functions that ask the user for an input and return their answer. e.g.
    - `get_string("What is your name? ")`
    - `get_int("What is your age? ")`
## Compiling and running scripts

- The lecturer is using the `code` command to both create an empty text file and open it in VS Code at the same time.
- The `make` command compiles a `.c` script into an executable of the same name - sans file extension.
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
```

## Strings

- Strings with more than one character must be wrapped in *double* quotes, `"hello"`.
- Characters with only one character must be wrapped in *single* quotes, `'a'`.

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

counter++;
counter += 1;

counter--;
counter -= 1;
```

## If-else

```C

```

## For-loops

```C
for (int i = 0; i < n; i++)
```


## While-loops

```C
while () {


}
```

## Function definitions

```C
void meow(void) {

    printf("meow\n");

}
```

## Structure of a script

- Hoisting