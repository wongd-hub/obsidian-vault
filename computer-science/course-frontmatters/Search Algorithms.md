#course_cs50 

- A computer at the lowest level doesn't have a birds eye view on all the elements in an array. So how do we check for the existence of a certain number?
# Linear search

- The naive approach is to search all array items from left to right or right to left, however this is slow and unpredictable:
    - We could get lucky and the target value is at the start of our search
    - Or we could get unlucky, and its at the end
    - If the item does not exist in the array, we'll need to step through the entire array to figure this out

> [!example]
> The example used in the lecture was that of a row of closed lockers. Volunteers were asked to find a certain number (opening one locker at a time) and compared linear searching an unsorted array to binary searching a sorted array.

- Pseudocode algorithm:

```pseudocode
For i from 0 to n-1
    If 50 is behind doors[i]
        Return true
Return false
```

# Binary search

- The faster method works only on sorted arrays, where we split the array into two pieces in each iteration - drastically cutting down processing time

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
