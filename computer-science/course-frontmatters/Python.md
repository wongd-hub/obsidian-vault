#course_cs50 

- We step into the realm of high-level languages now, that *abstract* away all of the low-level implementation details that we had to think about when working with [[C]].
- Python's popularity derives from relatively easy to read it is; it also has a large number of libraries, allowing you to get real work done faster.

# Differences to C

- We will now run Python scripts with the following commands. 
    - We no longer need to compile the language before running anymore. This is because Python is an *interpreted* language

```shell
python hello.py
```

- We also do away with the need to load libraries for functions that are included in base Python; as well as the `main()` function.
- We can also use single and double quotes interchangeably. 
- Python also manages your memory for you. No more need for `malloc()` and pointers.

# So why use C?

- C is seen as faster to run than Python. 
- C is also more memory efficient - Python is managing memory for you and won't know _a priori_ how much memory you will need overall.

