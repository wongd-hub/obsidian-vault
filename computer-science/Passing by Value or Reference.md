#course_cs50 

# Passing by Value

```C
#include <stdio.h>

void swap(int a, int b);

int main(void) {

    int x = 1; 
    int y = 2;

    printf("x is %i, y is %i\n", x, y);
    swap(x, y);
    printf("x is %i, y is %i\n", x, y);

}

void swap(int a, int b) {

    int tmp = a;
    a = b;
    b = tmp;

}

#$ code swap.c
#$ make swap
#$ ./swap
#> x is 1, y is 2
#> x is 1, y is 2
```

- Logically, we need to store a variable being moved into a temp variable, while we switch the other variable.

> [!tip]
> The example in the lecture was two glasses; one containing orange liquid, the other containing blue. There is no way to put the blue liquid into the orange glass, and vice versa, without introducing a third glass to temporarily store the liquid being moved first.

- However, this code does not work because of matters of scope.
    - We are *passing by value* or *passing by copy*, the values of `x` and `y` into the `swap()` function. i.e. The `swap()` function gets copies of `x` and `y`, and therefore does not affect where the original `x` or `y` 

- What's happening to your computer's memory when you're executing code? In a simplification:
    - The machine code being executed gets brought to the 'top' of your computer's memory
    - Below that is where global variables go - if you define variables outside of every function in a C script, that is a global variable
    - After that, we have what's called the heap which grows downward
    - At the bottom of the memory is what's called the stack, which grows upwards
        - When you use `malloc()` and request memory, it comes from the heap
        - When you use functions and arguments you're using stack memory, e.g. `main()`, `swap()`, 
        - Bad things will happen if the heap and stack overflow into each other

> [!note]
> If the call stack gets so large that you end up overwriting parts of the heap, you get _stack overflow_. The opposite is called _heap overflow_, and both are examples of _buffer (memory) overflow_.

```C
void swap(int a, int b) {

    int tmp = a;
    a = b;
    b = tmp;

}
```

- So when we call `main()`, it occupies the bottom of the stack, when `main()` calls `swap()`, that is 'stacked' on top of `main()`.
    - Then since C passes arguments by value, the value of `x` and `y` get copied into `a` and `b` where they are swapped. Note that `a` and `b` are true [[Memory#Deep copy|deep copies]] and occupy different bytes.
    - However, the `swap()` function then returns and we end back up in `main()`. `a` and `b` were swapped and are now discarded, but `x` and `y` remain the same.

# Passing by Reference

- To avoid this issue, we instead *pass by reference*, i.e. we pass pointers instead:

```C
void swap(int *a, int *b) {

    int tmp = *a; // Go to address a, get its value, put it in tmp
    *a = *b;      // Go to value at b, change value at a to what's in b
    *b = tmp;     // Go to address b and put tmp in there instead

}

int main(void) {

    int x = 1; 
    int y = 2;

    printf("x is %i, y is %i\n", x, y);
    swap(&x, &y);
    printf("x is %i, y is %i\n", x, y);

}
```