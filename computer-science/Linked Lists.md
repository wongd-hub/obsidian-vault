#course_cs50 

- We can stitch things together in memory to solve more interesting problems than just [[Arrays|arrays]].
- All we really need for this is:
    - [[Structs|`struct`]]
    - `.` to access and edit elements of a `struct`
    - `*` to declare [[Memory#Pointers | pointers]] and de-reference pointers
- We can combine the simultaneous use of `.` and `*` into `->` as we'll see later.

- A linked list is composed of multiple pieces of allocated memory that are not necessarily contiguous. As long as they are somehow connected/linked then we can treat this like an array.
    - We can use pointers to point to the allocated/relevant pieces of memory.
    - The trade-off with this is that we need to store a pointer for each item - using more memory.

![[Pasted image 20250127173822.png]]

- At each location, we also store the address of the *next* value as metadata. At the end of the list, we place a `NULL` in instead of another pointer. At the start of the list, we store one extra pointer to find the first value in the list.

# The naive approach: pre-pending to the list

- Here's how we'd do this in C - step by step.

```C
#include <stdio.h>

typedef struct node { // Need to add 'node' here so we can use it again in the struct
    int number;
    struct node *next; // Pointer to the next node
} node;

// Begin a linked list of size 0, and replace the potential garbage value there
node *list = NULL;

// Returns the address of a chunk of memory for 1 node and puts it in n
node *n = malloc(sizeof(node));

// How do we put a number into this first node? 
//  We could do this - dereference n, then place a number in the slot
(*n).number = 1;

//  However, this syntax is messy; we can equivalently do the following
n->number = 1;
n->next = NULL; // Since this is just a list of size 1 for now, there is no extra pointer

// We then put the pointer to the first element in the list into list
list = n; 


// Allocate space for another node, replace pointer in n
node *n = malloc(sizeof(node));
n->number = 2;
n->next = list; // Lets put this new item into the start of the list first, this is easier.
list = n; // Re-point list to the new n
```

- A more practical implementation would look like this:

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int number;
    struct node *next;
} node;

int main(int argc, char *argv[]) {

    node *list = NULL;
    
    for (int i = 1; i < argc; i++) {

        // Convert from character to integer
        int number = atoi(argv[i]);

        node *n = malloc(sizeof(node));
        if (n == NULL) {

            // Free memory thus far
            return 1;
        
        }

        n->number = number;
        n->next = list; // In the first iteration, this is NULL
        list = n;

    }

    // Print whole list
    node *ptr = list;
    while (ptr != NULL) {
    
        printf("%i\n", ptr->number);
        ptr = ptr->next;

    }

}

#$ make list
#$ ./list 1 2 3
#> 3
#> 2
#> 1
```

- Note that the run-time of pre-pending to a linked list is $O(1)$, because it doesn't matter how large the list is, you're always just pre-pending to the start.
    - The flip-side is that the search time for a linked list is $O(n)$; because we must navigate through each of the elements of the list one at a time.
    - [[Search Algorithms]] such as [[Search Algorithms#Binary search|binary search]] cannot be used on linked lists since they are not stored in contiguous chunks of memory.

# Appending to the list

```C
#include <cs50.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int number;
    struct node *next;
} node;

int main(int argc, char *argv[]) {

    // Memory for numbers
    node *list = NULL;

    // For each command-line argument
    for (int i = 1; i < argc; i++) {
    
        // Convert argument to int
        int number = atoi(argv[i]);

        // Allocate node for number
        node *n = malloc(sizeof(node));
        if (n == NULL) {
            // Free up memory
            return 1;
        }

        // Insert number data, initialise pointer
        n->number = number;
        n->next = NULL;

        // If list is empty at this point
        if (list == NULL) {
        
            // Then this node is the whole list
            list = n;
            
        } else { // If list has numbers already
        
            // Iterate over nodes in list
            //   Initialise ptr to look at start of list
            //   Keep traversing list until we find a node with next=NULL
            //   This is the end of the list, so update the last node's next 
            //     to point to n that we just created
            for (node *ptr = list; ptr != NULL; ptr = ptr->next) {
            
                // If at the end of the list
                if (ptr->next == NULL) {
                
                    // Append node
                    ptr->next = n;
                    break;
                
                }
            
            }
        
        }
    

    }



}
```

- What happens to time efficiency of insertion here? Since we're looking through the whole list to find the last element, which we then point to the inserted item, this is $O(n)$.