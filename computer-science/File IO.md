- There are functions that let us open and manipulate files:
    - `fopen()` - lets you open a file
    - `fclose()`
    - `fprintf()` - lets you print to a file
    - `fscanf()` - lets you read data not from the keyboard but from a file
    - `fread()`/`fwrite()` - read and write data from a file, generally binary data (like images)
    - `fseek()` - lets you move left to right through a file

# Example functions

## Persistent phonebook data

> [!warning]
> Any time a function returns a pointer, you should check if its `NULL`, because that almost always means something has gone wrong.

```C
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void) {

    // This returns a pointer to a file in memory.
    // fopen() opens the file, and returns the address in the computer's mem
    FILE *file = fopen("phonebook.csv", "a") // "r", read
                                             // "w", write
                                             // "a", append

    // Should check that file is not NULL
    if (file == NULL) return 1;

    char *name = get_string("Name: ");
    char *number = get_string("Number: ");

    // Print the provided name and number into the file
    fprintf(file, "%s,%s\n", name, number);

    // Close the connection
    fclose(file);

}

#$ make phonebook
#$ ./phonebook
#> Name: David
#> Number: +1-617-495-1000

#$ cat phonebook.csv
#> David,+1-617-495-1000
```

## Copy program

```C
#include <stdio.h>
#include <stdint.h>

typedef uint8_t BYTE; // From stdint; give me an unsigned 8 bit integer

// Canonical way to declare main() when you want to get strings from the user
int main(int argc, char *argv[]) {

    // Open the first file specified in the arguments
    FILE *src = fopen(argv[1], "rb"); // Tell fopen() to read binary data
    FILE *dst = fopen(argv[2], "wb"); // Want to write binary to this file

    BYTE b; // Define a single byte


    // fread() reads one or more bytes for you. Just like swap(), you need to 
    //   tell it where to load the bytes into in memory - so we put it in &b.
    // How big is a byte? sizeof(b)
    // How many bytes do we copy at a time? 1
    // Where do we want to read those bytes from? src
    // fread() then returns how many bytes were read in each iteration - it'll be
    //   0 or 1 since we're telling it to read one at a time
    while (fread(&b, sizeof(b), 1, src) != 0) {

        // Then we do the opposite with fwrite(). Where do we write to, how big 
        //   is each unit, how many to write at a time, and where to write it.
        fwrite(&b, sizeof(b), 1, dst);

    }

    fclose(dst);
    fclose(src);

}
```