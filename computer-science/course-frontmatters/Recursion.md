#course_cs50 

- A *recursive* function is a function that calls itself. [[Recursion]] is a powerful problem-solving technique:
    - It allows us to tighten up our code and reduce the use of loops
    - It also allows us to utilise the computer's memory in an interesting way
- An example is the [[Search Algorithms#Binary search|binary search algorithm]]
    - We can see that we use the keyword *search* in the last two conditions
    - All the while, the inputs are getting cut in half until there is only one left

```pseudocode
If no doors left
    Return false
If 50 is behind doors[middle]
    Return true
Else if 50 < doors[middle]
    Search doors[0] through doors[middle - 1]
Else if 50 > doors[middle]
    Search doors[middle + 1] through doors[n - 1]
```

# First example

- Writing code to draw a pyramid of $n$ height:

```C
#include <cs50.h>
#include <stdio.h>

void draw(int n);

int main (void) {

    int height = get_int("Height: ");
    draw(height);

    

}

void draw(int n) {

    // What is a pyramid of height 4? It's a pyramid of height 3 + one more row

    // If nothing to draw, finish
    if (n <= 0) {
        return;
    }
    
    // Print pyramid of height n - 1
    draw(n - 1);

    // Print one more row
    for (int i = 0; i < n; i++) {

        printf("#");

    }
    printf("\n")l;

}
```

- Without the exit condition (the `if n <= 0`), `clang` will return a recursion error upon attempting to compile this.
    - How this runs is once it sees `n = 4`, it will call `draw(3)` which will call `draw(2)`, then `draw(1)`. `draw(1)` will then run the `printf()` for-loop, then will complete and pass to `draw(2)`, then onwards until our entire pyramid is printed.