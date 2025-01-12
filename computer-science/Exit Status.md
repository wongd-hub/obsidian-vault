#course_cs50 

- Every C program has an exit status, a special return value from the program
    - By default, this is 0 if the process completed successfully
    - Every other integer probably means something bad

- So far we've been writing `main()` with an `int` return value. This means we can return error status values from our programs too.
    - This can be diagnostically useful to you and others working on your program

```C
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[]) {

    if (argc != 2) {

        printf("Missing command-line argument\n");
        return 1; // An arbitrary error status code
    
    }
    
    printf("hello, %s\n", argv[1]);
    return 0; // Success status code

}
```

- If you want to see the exit code of the last program you ran in your CLI, use:

```bash
echo $?
```

