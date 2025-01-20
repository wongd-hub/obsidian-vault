#course_cs50

- Recall that there is always a [[Arrays#Character array|terminating `NUL` character]]  at the end of each string, so each string takes $n + 1$ bytes of memory.
- As strings are just arrays of characters, it stands to reason that each character in the string has its own memory address.

# The `string` data type

- Strings have been a [[Arrays#^5a0906|white lie]] for the first part of this course, because technically the strings used are [[Memory#Pointers|pointers]].
    - This means that a string in this case is a pointer to the first character in the string in memory
    - We don't need to know when the string terminates, because the `NUL` character in the string gives us this information

> [!info]
> - We've seen we can use [[Structs]] to create our own data types or aliases of them
> - The CS50 `string` data type is really just an alias for `char *` - [[Structs#^5e27cc|see here]]
> ```C
> // This means that CS50's abstraction of string…
> string s = "HI!";
> 
> // … is equivalent to:
> char *s = "HI!";
> ```

```C
#include <cs50.h>
#include <stdio.h>

int main(void) {

    string s = "HI!";
    printf("%p\n", s);
    printf("%p\n", &s[0]);
    printf("%p\n", &s[1]);
    printf("%p\n", &s[2]);
    printf("%p\n", &s[3]); // The NUL character

}

#$ make addresses
#$ ./addresses
#> 0x5586554ce004
#> 0x5586554ce004
#> 0x5586554ce005
#> 0x5586554ce006
#> 0x5586554ce007
```

- This is equivalent to the following
    - Note that we don't use the `&` here; the C compiler sees the double quotes and puts the address of the first char in the variable for you

```C
#include <stdio.h>

int main(void) {

    char *s = "HI!";
    printf("%s\n", s);

}
```

# String comparison

- The following code returns `Different` no matter what strings are provided to `s` and `t`. This is because `s` and `t` are technically pointers to the first character of each string in memory. When compared, these addresses are different.

```C
#include <cs50.h>
#include <stdio.h>

int main(void) {

    char *s = get_string("s: ");
    char *t = get_string("t: ");

    if (s == t) {

        printf("Same\n");
    
    } else {

        printf("Different\n");
    
    }

}
```

- So how do we actually compare strings?
    - We used `strcompare()` from `string.h` before.
    - However, from first principles, we'd likely need to pull the actual character at each address in the string and compare them ourselves