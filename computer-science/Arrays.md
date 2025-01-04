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

