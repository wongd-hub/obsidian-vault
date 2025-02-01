#course_cs50 

- [[Arrays]] are problematic since they are a fixed size and can get us into trouble if we want to insert new numbers or items.
- [[Linked Lists]] solve this problem by allowing list items to exist in disparate chunks of memory. We only add as much memory as we need per list item, but we spend more memory per list item since we need to store pointers. Further, insertion at the end or in sorted order has a $O(n)$ time cost, and sorting algorithms don't work on these constructs.

- Trees are kept in sorted order, allow us to divide and conquer, but still give us the dynamism to grow or shrink the data structure. 
    - Trees are mashups of the preceding data structures that have an extra dimension to them (instead of being just left-right).

# Binary search trees (BSTs)

- Take a 2-dimensional array. We can split it into a 'middle' item, then a middle of the middle etc. until we end up breaking the array into nodes of at most length 1.
- If we then stretch this array *vertically* with the first middle being the top, the second middles being the next row, etc, we end up with a tree structure with the strict property that::
    - Each node has at most 2 children
    - The left child contains numbers that are strictly less than the parent node
    - The right child contains numbers that are strictly greater than the parent node

![[Pasted image 20250201115840.png]]

- The high level implementation details are that each node now has *2* pointers instead of just 1 (a la [[Linked Lists]]); a pointer to the left node, and a pointer to the right node.
    - The root node is where the tree starts, so it is the first node in memory.
    - The binary structure allows us to search for numbers (and therefore also insert numbers in sorted order) with runtime $O(logn)$.

```C
typedef struct node {

    int number;
    struct node *left;
    struct node *right;

} node;
```

- Due to their recursive structure, trees lend themselves to recursive algorithms to perform operations with them.

# Searching BSTs


```C
bool search (node *tree, int number) {

    // Where *tree is a pointer to the root node of the tree

    if (tree == NULL) {

        // If there is no tree, return false
        return false;
        
    } else if (number < tree->number) {

        // If the number is less than the number in the current 
        //  tree node, we know that we need to search the left side.
        return search(tree->left, number);

    } else if (number > tree->number) {

        // If the number is greater, then we know we need to 
        //  search the right side.
        return search(tree->left, number);
    
    } else {
    
        // If the number is equal, return true
        return true;

    }

}
```

# Balancing trees

- The tree shown above is a neat tree; but what if the user first inputs a number that is smaller than all numbers that come after it? What if the number of elements is not odd?
- We can end up in scenarios where the tree is nothing more than a linked list if we just started with the first input number and worked from there with no reshuffling occurring.

![[Pasted image 20250201192116.png]]

- This is not an efficient tree, and by themselves, BSTs don't guarantee any balance. In future courses we go through what extra steps we can add to make sure the tree is re-balanced (e.g. AVL, Red Black Trees).