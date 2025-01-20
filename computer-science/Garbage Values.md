#course_cs50 

- Recall that if you define a variable, but don't assign a value to it at first - what's stored in that variable is a *garbage value*.
    - These are essentially remnants of your computer being on for a while. There are residual bits in there from previous processes on the computer.

- We can see this when we do the following:

```C
#include <stdio.h>

int main(void) {

    int scores[1024];

    for (int i = 0; i < 1024; i++) {

        // Note we're never actually putting values in here
        printf("%i\n", scores[i]);
    
    }

}

#$ make garbage
#$ ./garbage
#> 0
#> 1
#> 0
#> 399085632
#> ...
```

- How might we think about potential problems with this? Consider this function:
    - Note how we never allocated an address for `y`. 
    - Therefore, `y` could contain any address; and if we put a value in to somewhere we're not supposed to touch, we'll crash.
    - Removing the `*y = 13;` will make the function work properly and both `x` and `y` point to the same `13` in memory.

```C
int main(void) {

    int *x;
    int *y;

    // Allocates a pointee to the pointer, x
    x = malloc(sizeof(int));

    *x = 42; // Dereference the x pointer to put 42 into it
    *y = 13; // We never actually allocated memory for y (or pointed to a pointee)

    y = x;

    *y = 13;


}
```