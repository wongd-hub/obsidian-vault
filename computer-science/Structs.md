#course_cs50 

- Create a *struct* in C using the following syntax:

```C
typedef struct {

    // The contents of this data structure
    string name;
    string number;

} person; // The name of the struct
```

- We can then do things like this:

```C
// Define an array of person structs, pre-allocating 3 items
person people[3];

// We then use `.` to access the attributes of each struct
people[0].name = "Carter";
people[0].number = "1";

people[1].name = "David";
people[1].number = "2";

people[2].name = "John";
people[2].number = "3";
```

# Defining data types

- We can use this concept to declare our own data types, or to create aliases of them.
- For example, to create an alias of `int` called `integer`:

```C
typedef int integer;
```

- C has no built-in data type for a byte, so commonly we see this:

```C
typedef uint8_t BYTE;
```

- The CS50 `string` construct is really just this: ^5e27cc

```C
typedef char *string;

// Easier to understand if its this:
typedef char* string;
```