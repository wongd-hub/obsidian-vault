#course_cs50 

- We can set up a C script to take command line arguments by doing the following:

```C
#include <cs50.h>
#include <stdio.h>

// These arguments to main are what we can fill with command line arguments.
// - argc: arg-count (how many words did the user type)
// - argv: arg-vector (list of values/args)
int main(int argc, string argv[]) {

    // Do something
    

}
```

- An example of this is:

```C
#include <cs50.h>
#include <stdio.h>

// Take an integer of the number of args typed, and a vector of strings
int main(int argc, string argv[]) {

    printf("Hello, ");

    // Print all arguments past the name of the program
    for (int i = 1; i < argc; i++) {
        printf("%s\n", argv[i])
    }

}

#$ make greet
#$ ./greet David Malan
#> Hello, David Malan
```

- Note that the first item in `argv` (`argv[0]`) is the name of the program; in this case, `./greet`. This is why we need to start at the 1st place to access the first argument.

- We can also get programs to take flags - this isn't discussed in the lectures (yet).