#course_cs50 

- A dictionary is a data structure that stores *keys* and associated *values*.
- There are different ways that we can implement storage of these keys and values.
    - We could use arrays, but we run into the same issues when we try to add more items
    - We could use a linked list, but we then run into issues when trying to search and read them with speed

- In the example of the Contacts app, names are the keys, phones are the values.
    - We don't want running time to become linear, logarithmic will be better, but let's aspire for $O(1)$ - i.e. constant time.