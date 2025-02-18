#course_cs50

| Type   | Size    |
| ------ | ------- |
| bool   | 1 byte  |
| int    | 4 bytes |
| long   | 8 bytes |
| float  | 4 bytes |
| double | 8 bytes |
| char   | 1 byte  |
| string | ? bytes |
- Consider the memory in your RAM as a canvas of bytes on which your computer can store variables and data. Each item will have an *address* in memory - i.e. where in memory it is stored.
    - For example, an [[Arrays]] of integers is a series of neighbouring addresses in memory - one for each array item.
    - In an array of strings, it's possible to access single characters by first accessing the word you want to look at, then accessing the position of the character. e.g. `words[1][2]`. You can also look at the characters of *other* strings in the array by accessing positions that are higher than the number of characters in the string.

```C
string words = {"HI!", "BYE!"};

words[0][5]; // "B" (note it's the 5th location due to the NUL character)
```

# Memory addresses

- Computers typically use numbers to represent each of the bytes in their memory, and they generally use [[Hexadecimal]] to do this. e.g. `0x1A`.

- Let's consider what this function does in the computer:
    - Defines an `int`, `n`,  which generally takes 4 bytes. i.e. it creates an int with 4 bytes of memory. 
    - This variable lives somewhere in memory so it stands to reason that we can access the address of this variable.

```C
#include <stdio.h>

int main(void) {

    int n = 50;
    printf("%i\n", n);

}
```

## Pointers

- C supports *pointers*, variables that store memory addresses of other variables. Because of this, it provides syntax that gives you access to locations of variables in memory.
    - The `&` operator is the 'address' operator that returns the address of a variable
    - The `*` operator (without declaring a type first, see the next point) is known as the dereference operator. It takes an address and allows you to access its contents
    - Separately, to declare a pointer that contains where `n` sits in memory, we use the following syntax:

```C
int *p = &n;
```

> [!tip]
> - The lecturer notes that this is an unfortunate design choice in C, and that the clearer way to denote that the *type* of `p` is an integer pointer is to do this:
> ```C
> int* p = &n;
> ```
> - Note that white space does not matter in this context; so we can choose to write it like this, but this may be unconventional and therefore confusing.

- So to see the address of `n`, we need to use `%p` in `printf`, and use the `&` operator. Then to go from that address back to its contents, use `*`.

```C
#include <stdio.h>

int main(void) {

    int n = 50;
    int *p = &n;
    printf("%p\n", p);
    printf("%i\n", *p);

}

#$ make addresses
#$ ./addresses
#> 0x7ffc3a7cffbc
#> 50
```

- Note that pointers themselves are integer variables and therefore also occupy a space in memory. They typically take 8 bytes of space.  ^bce4f7
    - This also means that we can perform arithmetic on them with [[Pointer Arithmetic]].

# Copying

## Shallow copy

- Naively, we'd expect that we could copy a string `s` to `t` by doing the following.
    - Lets say we want to modify `t`, but not `s` - we see that this is not actually what's happening. 

```C
#include <cs50.h>
#include <ctype.h> // For toupper()
#include <stdio.h>
#include <string.h> // For strlen()

int main(void) {

    // "Copy"
    string s = get_string("s: ");
    string t = s;

    if (strlen(t) > 0) t[0] = topupper(t[0]);

    printf("%s\n", s);
    printf("%s\n", t);

}

#$ code copy.c
#$ make copy
#$ ./copy
#$ s: hi!
#> Hi!
#> Hi!
```

- This is because both `s` and `t` point to the same string in memory. This is referred to as a shallow copy.

## Deep copy

- To achieve a deep copy, where each copy has their own address in memory, we need to load the `stdlib.h` library. We'll be using the following functions:
    - `malloc()` which allocates memory for the number of bytes passed to the function. It returns a pointer to the first byte in the sequence.
    - `free()`, the opposite of `malloc()`. When we're done with memory, we need to free it up and give it back to the OS.

> [!tip]
> If `malloc()` returns [[NULL|`NULL`]], there is not enough memory available to allocate for the number of bytes you provided. 

```C
#include <cs50.h>
#include <ctype.h> // For toupper()
#include <stdio.h>
#include <string.h> // For strlen(), strcpy()
#include <stdlib.h> // For malloc(), free()

int main(void) {

    char *s = get_string("s: ");

    if (s == NULL) return 1; // If we run out of memory for s, exit

    // Allocate enough memory to duplicate s
    char *t = malloc(strlen(s) + 1)

    if (t == NULL) return 1; // If we run out of memory for t, exit



    // We could do this to copy the string...
    for (int i = 0, n = strlen(s); i < n + 1; i++) {
    // Could also set that condition to i <= n but I think this 
    // shows the intention better - we want to capture the NUL character as well

        t[i] = s[i];
        
    }

    // ... Or we could do this: strcpy(t, s);



    // Make our change to just t
    if (strlen(t) > 0) t[0] = topupper(t[0]);

    printf("%s\n", s);
    printf("%s\n", t);

    // Free up memory
    // Don't need to free s, since get_string() handles this for you
    free(t);
    return 0;

}

#$ make copy
#$ make copy
#$ ./copy
#$ s: hi!
#> hi!
#> Hi! // Correct
```