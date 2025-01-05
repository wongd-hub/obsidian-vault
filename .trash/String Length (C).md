#course_cs50 

- We could create a function definition of our own...

```C
#include <cs50.h>
#include <stdio.h>

int string_length(string s);

int main(void) {

    string name = get_string("Name: ");

    int length = string_length(name);
    printf("%i\n", length);
    

}

int string_length(string s) {

    int n = 0; 
    while (s[n] != '\0') {
        n++;
    }

    return n;

}
```

- But there are functions out there that other people have already created. For example, in the library `string.h` there exists a string length function.

```C
#include <cs50.h>
#include <stdio.h>

```