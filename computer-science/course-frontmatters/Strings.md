#course_cs50

- Recall that there is always a [[Arrays#Character array|terminating `NUL` character]]  at the end of each string, so each string takes $n + 1$ bytes of memory.
- As strings are just arrays of characters, it stands to reason that each character in the string has its own memory address.

- Strings have been a [[Arrays#^5a0906|white lie]] for the first part of this course, because technically the strings used are [[Memory#Pointers|pointers]].
    - This means that a string in this case is an integer - i.e. a pointer to the first character in the string
    - We don't need to know when the string terminates, because the `NUL` character in the string gives us this information