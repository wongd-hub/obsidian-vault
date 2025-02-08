#course_cs50 

- A *trie* (short for retrieval) is a [[Trees|tree]] of [[Arrays|arrays]].
    - This gives us $O(1)$ running time, but with a downside.

- In a trie, every node is an array. Each location in the array represents a letter of the alphabet.
    - So the root node is an array of 26 locations.

- If we want to insert a name, we repeatedly hash the letters in the name - and then each letter goes into the tree.
    - Terminating letters for names are marked specially and contain the dictionary value (so to speak).
    - An example of a trie that contains the names: Toad, Toadette, Tom

![[Pasted image 20250208210548.png]]

# Code implementation

```C
typedef struct node {

    // Each node contains a full array of letters
    struct node *children[26];

    // If there is a non-NULL number, consider this to be a terminating node
    char *number;

} node;
```

# Running time

- To navigate this data structure, all we need to keep track of is one pointer called trie that is a pointer to the first of those nodes.
- The running time of a trie will not depend on how many names are stored in the structure. Instead, it depends on the length of the name.
    - For example, it takes us four steps to get to the number of Toad (given they have 4 letters in their name).
- If the length of the name is $k$, which we known to be finite, then the running time is $O(k)$ but that is the same as $O(1)$.

# Drawbacks

- In practice, most systems would use hash tables to implement dictionaries instead of tries.
- A trie takes up a lot of space. See the number of empty spaces used up in the image above.