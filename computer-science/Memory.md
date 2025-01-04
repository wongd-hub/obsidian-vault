#course_cs50

| Type | Size    |
| ---- | ------- |
| bool | 1 byte  |
| int  | 4 bytes |
|      |         |
- Consider the memory in your RAM as a canvas of bytes on which your computer can store variables and data. Each item will have an *address* in memory - i.e. where in memory it is stored.
    - For example, an [[Arrays]] of integers is a series of neighbouring addresses in memory - one for each array item.
    - In an array of strings, it's possible to access single characters by first accessing the word you want to look at, then accessing the position of the character. e.g. `words[1][2]`. You can also look at the characters of *other* strings in the array by accessing positions that are higher than the number of characters in the string.

```C
string words = {"HI!", "BYE!"};

words[0][5]; // "B" (note it's the 5th location due to the NUL character)
```