#course_cs50

- Used to describe a code algorithm succinctly and accurately.
    - We can break up the example pseudocode below into _functions_ (verbs), *control flow* (if-else), *boolean expressions*, and *loops*.

e.g. 

```
# Implementing binary search for an address book
1. Pick up phone book
2. Open to middle of phone book
3. Look at page
4. If person is on page
5.     Call person
6. Else if person is earlier in book
7.     Open to middle of left half of book
8.     Go back to line 3
9. Else if person is later in book
10.     Open to middle of right half of book
11.     Go back to line 3
12. Else
13.     Quit
```

> [!tip]
> Often software crashes are due to the developers not accounting for a specific situation in their logic, leading the program to continue without resolution. e.g. If we had forgotten that the person we're searching for may not even be in the phone book.

