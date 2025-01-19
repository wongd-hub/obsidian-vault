#course_cs50 

- We've seen that we can use `malloc()` to [[Memory#Deep copy|assign memory]] before we create objects.
- For example, let's create a deliberately buggy function.

```C
#include <stdio.h>
#include <stdlib.h>

int main(void) {

    // Let's assign the memory for an array of integers ourselves
    // We can use sizeof() to get the size of a data type

    // This returns a pointer, x, to the first byte allocated for 
    //   an integer array of size 3
    int *x = malloc(3 * sizeof(int));

    x[1] = 72; // Note we should technically start from 0
    x[2] = 73;
    x[3] = 33;

    // Note we didn't call free()

}

#$ make buggy
#$ ./buggy
#> No errors
```

- This function ran fine, but we can use a tool called [Valgrind](https://valgrind.org/) to find memory issues for us.
    - We'll break this output down into actionable pieces of information.

```bash
valgrind ./buggy

#> ==74697== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
#> ==74697== Command: ./memory
#> ==74697== 
#> ==74697== Invalid write of size 4
#> ==74697==    at 0x109170: main (memory.c:9)
#> ==74697==  Address 0x4bb404c is 0 bytes after a block of size 12 alloc'd
#> ==74697==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
#> ==74697==    by 0x109151: main (memory.c:6)
#> ==74697== 
#> ==74697== 
#> ==74697== HEAP SUMMARY:
#> ==74697==     in use at exit: 12 bytes in 1 blocks
#> ==74697==   total heap usage: 1 allocs, 0 frees, 12 bytes allocated
#> ==74697== 
#> ==74697== 12 bytes in 1 blocks are definitely lost in loss record 1 of 1
#> ==74697==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
#> ==74697==    by 0x109151: main (memory.c:6)
#> ==74697== 
#> ==74697== LEAK SUMMARY:
#> ==74697==    definitely lost: 12 bytes in 1 blocks
#> ==74697==    indirectly lost: 0 bytes in 0 blocks
#> ==74697==      possibly lost: 0 bytes in 0 blocks
#> ==74697==    still reachable: 0 bytes in 0 blocks
#> ==74697==         suppressed: 0 bytes in 0 blocks
#> ==74697== 
#> ==74697== For lists of detected and suppressed errors, rerun with: -s
#> ==74697== ERROR SUMMARY: 2 errors from 2 contexts (suppressed: 0 from 0)
```

# Valgrind output

## Writing outside of memory

```bash
#> ==74697== Invalid write of size 4
#> ==74697==    at 0x109170: main (memory.c:9)
#> ==74697==  Address 0x4bb404c is 0 bytes after a block of size 12 alloc'd
#> ==74697==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
#> ==74697==    by 0x109151: main (memory.c:6)
```

- This segment highlights that there was an invalid setting or changing of a value in memory on line 9 of the script. We know an integer is of size 4 (bytes), so we're likely doing something wrong with an integer here.
    - This block indeed refers to the following line, where we've overstepped our boundary of 3 chunks of 4 bytes:

```C
x[3] = 33;
```

## Memory leaks

```bash
#> ==74697== 12 bytes in 1 blocks are definitely lost in loss record 1 of 1
#> ==74697==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
#> ==74697==    by 0x109151: main (memory.c:6)
```

- Losing some bytes like this is referred to as a memory leak; we did not free up the memory we allocated with `malloc()`. Line 6 refers to when `malloc()` was called in the function.
    - To fix this, we should run `free(x)` at the end of the function.