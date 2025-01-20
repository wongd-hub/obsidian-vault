#course_cs50 

- Getting user input is a difficult problem in C, because you don't know how big of a string (for example) the user will provide. Therefore you don't know exactly how much space to allocate.
- Let's create our own `get_*` function with `scanf`.

# `get_int()`

```C
#include <stdio.h>

int main(void) {

    int n;
    printf("n: ");

    scanf("%i", &n); // Telling scanf to scan a single integer from user input
                     // Then put it into the address of n

    printf("n: %i\n", n);

}

#$ make get
#$ ./get
#> n: 50
#> n: 50
```

# `get_string()`

- This is the incorrect way to do this. We'll use `clang` to compile this so we can ignore the warnings we'd get otherwise.

```C
#include <stdio.h>

int main(void) {

    char *s;
    printf("s: ");

    scanf("%s", s); // s is already a pointer

    printf("s: %s\n", s);

}

#$ clang -o get -Wno-uninitialized get.c
#$ ./get
#> s: HI!
#> Segmentation fault (core dumped)
```

- A segmentation fault is a problem related to a segment of memory. A segment of memory has been touched that we shouldn't have.
    - When we declared `n` to be an integer before, we know that it was going to be 4 bytes long. It doesn't matter if there were already garbage values there - we know how much memory we need.
    - We can initialised a pointer (as in the code above), and that itself will take [[Memory#^bce4f7|8 bytes]] by convention. But we haven't initialised `s` itself so this pointer is currently just garbage values.
        - i.e. We haven't allocated the bytes this pointer is pointing to using something like `malloc()`.
    - So we end up touching memory we shouldn't have (the pointer is pointing us somewhere random and incorrect).

- How do we fix this? We could use `malloc()`, or we could just declare `s` as an array of characters.

```C
#include <stdio.h>

int main(void) {

    char s[4];
    printf("s: ");

    scanf("%s", s); // s is already a pointer

    printf("s: %s\n", s);

}

#$ make get
#$ ./get
#> s: HI!
#> s: HI!

#> // However, now we're open to issues if the user exceeds the string length
#> s: HI!HI!HI!HI!HI!HI!
#> s: HI!HI!HI!HI!HI!HI!
#> Segmentation fault (core dumped)

```

- If the user inputs more characters than expected by the function, we might end up with characters in memory addresses that we didn't allocate initially.
- The alternative is we over-allocate memory, say 4,000 characters for `s`, but this is clearly inefficient:
    - What if we type in 4,000 characters or more
    - This may pre-allocate too much memory
    - So we can use `scanf` to implement our own `get_*` functions, but this is particularly dangerous for strings.

- How does `get_string()` actually work?
    - It goes character by character as the user is typing - allocating memory with `malloc()` until the user is done typing. Then one extra byte is allocated for the `NUL` character.