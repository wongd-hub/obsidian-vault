#course_cs50 

- We don't want to have to track long lists of numbers in separate variables.
- An array is a chunk of memory storing values back to back; no gaps or fragmentation between the bytes.
- Note that C does not provide methods to check the length of an array like other languages.

# Integer array

- To define a length-3 array of integers in C, do the following:

```C
int scores[3];

// Assigning values into the array
for (int i = 0; i < 3; i++) {
    scores[i] = get_int("Score: ");
}

// Or
int array = {13, 42, 50};
```

# Character array

- In C, a `string` is a sequence of characters, where each character takes 1 byte. Since `string`s are of variable lengths - their total memory usage will vary.

> [!warning]
> Note that `string`s don't seem to be available in vanilla C, `cs50.h` seems to need to be loaded in order to define `string` variables.

^5a0906

- A `string` could also be thought of as an ordered array of character strings - of variable length. 
    - To signify that the string has ended, an extra byte (the `NUL` value) is added to the end of the string that contains all 0 bits (`00000000` - stylised as a `\0`) which represents the end of the string.
    - So every string is actually `n + 1` bytes to contain the 0-value

# String array

- We can also store strings in arrays:

```C
string words[2];

words[0] = "Hello world";
```

- To access single characters within each string, we can do things like `words[0][2]`.

# Resizing arrays

- We can run into issues if we want to exceed the amount of values we originally assigned to an array.
    - We don't know if we'll overwrite something important if we add more data to the next few bytes naively
    - We also can't add the new values anywhere in memory since we need the array memory to remain contiguous

- A solution to the problem might be to move the entire array to a different location with more space.
    - This technically works, but suppose you want to add more - you'd need to repeat the operation
    - Copying takes time, and you're also iterating over the array to copy it over (so at least $O(n)$ - [[Running Time - O(n)|see here for Running Time]])
    - For a brief moment, we're using twice as much space as the original array (due to the nature of copying)
        - We need to have memory allocated for 2 of the original array for a moment in time

- To demonstrate how inefficient this is - we'll do it in C:

```C
#include <stdio.h>
#include <stdlib.h>

int main(void) {

    // We can't do the following since it creates a list, forever of size 3.
    // Can't free this memory (we can only free memory from malloc)
    // int list[3];


    // We instead use malloc to allocate memory for 3 integers
    int *list = malloc(3 * sizeof(int));
    if (list == NULL) return 1;

    // C knows `list` was initialised as an array of integers, so pointer 
    // arithmetic kicks in and we don't need to do things like 
    // list[0], list[4], etc given a list is of size 4
    list[0] = 1;
    list[1] = 2; // Instead of list[4], C is smart enough 
                 // to know where the next int starts
    list[2] = 3;


    // Say we want to dynamically allocate more memory and free up the old
    int *tmp = malloc(4 * sizeof(int));
    if (tmp == NULL) {
    
        free(list);
        return 1;
    
    }

    // Next step is to iteratively copy the old numbers into the new space
    for (int i = 0; i < 3; i++) {

        tmp[i] = list[i];
    
    }
    tmp[3] = 4;
    free(list);

    // We can also keep using the `list` variable by doing
    list = tmp;

    free(list);
    return 0;

}
```

- A more efficient alternative may be the [[Linked Lists|linked list]] since it can point to multiple non-contiguous places in memory.
