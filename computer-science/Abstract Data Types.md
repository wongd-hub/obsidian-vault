#course_cs50 

- These are higher level implementations of various data structures, which contrast with the lower level implementations of these structures.
- Abstract data types offer certain properties and characteristics - but its up to the programmer to implement them and there are many ways to do so.

# Queueing

- Queue items can be processed *First In, First Out* (FIFO)
- Queues offer some operations: _enqueueing_ (adding to the queue) and _dequeueing_ (removing from the queue)
- We could define queues as an [[Arrays|array]] in C

# Stacks

- Stacks are processed _Last In, First Out_ (FILO)
    - Not necessarily the system you'd want if you want equitable servicing of all queue items.
    - However, there are contexts where LIFO makes sense - for example, when dealing with your email, the last email received is the first displayed
- Stacks offer the operations: _push_ (push a new item onto the stack) and _pop_ (remove the top item off the stack)
- Could also be implemented with an array in C