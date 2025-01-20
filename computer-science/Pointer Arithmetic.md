#course_cs50 

- We've seen that we can use numeric addresses to represent addresses in memory with [[Memory#Pointers|pointers]].
- Since these addresses are numeric, we can do something called pointer arithmetic to manipulate them.

- See the following example, and recall that a string in C is [[Strings#The `string` data type|technically just a pointer to the first character in the string]].

```C
#include <stdio.h>

int main(void) {

    char *s = "HI"!;

    printf("%c",   *s);
    printf("%c",   *(s + 1));
    printf("%c\n", *(s + 2));

}

#$ make addresses
#$ ./addresses
#> HI!
```

- The array access notation using square brackets is essentially syntactic sugar for this pointer arithmetic.

```C
*s 
// Is equivalent to
s[0]

*(s + 1)
// Is equivalent to 
s[1]

// etc.
```