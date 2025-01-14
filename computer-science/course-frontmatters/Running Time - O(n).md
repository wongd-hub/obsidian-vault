#course_cs50 

- Computer scientists tend to talk about the order of magnitude of the running time of an algorithm, as a function of the number of inputs
    - We do away with most constants since we're more interested in how much the runtime changes as the number of inputs increases
    - This is called *Big O Notation*
        - For example, an algorithm with a $O(n)$ runtime will increase in runtime linearly with the number of inputs provided to it. [[Search Algorithms#Linear search|Linear search]] has an $O(n)$ runtime.
        - $O(n^2)$ means $n$ things doing $n$ things; e.g. if all people in a room had to shake hands with everyone else in the room (technically $n(n-1)$ but we remove the constant)

![[Pasted image 20250112155800.png]]

> [!tip]
> $log_{2}n$ just means what happens to a value when you take n and divide it by 2 again and again. This describes the running time of [[Search Algorithms#Binary search|binary search]].

# Best and worst case

- To get more precise, we represent the running time *upper bound* as $O(n)$.
- In contrast, we represent the running time *lower bound* as $\Omega(n)$.
    - The best case for linear search is $O(1)$ since the target could be the first number you see. Therefore it has a best-case runtime of $\Omega(1)$.
    - The same is the case for binary search.

- If the $O(n)$ and $\Omega(n)$ are equal for a certain algorithm, we refer to this as $\Theta(n)$. ^c6c454

