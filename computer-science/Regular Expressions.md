#course_cs50 

- This allows us to test strings for certain patterns.
- e.g. `".+@.+\.edu"` attempts to match email addresses that have an `edu` suffix.

# Cheatsheet

| Symbol         | Meaning                                        |
| -------------- | ---------------------------------------------- |
| `.`            | Any single character (except line terminators) |
| `*`            | Zero or more times                             |
| `+`            | One or more times                              |
| `?`            | 0 or 1 time                                    |
| `{n}`          | n occurrences                                  |
| `{n,m}`        | At least n occurrences, at most m occurrences  |
| `[0123456789]` | Match any one of the enclosed characters       |
| `[0-9]`        | Match any one in the range of characters       |
| `\d`           | Any digit                                      |
| `\D`           | Any character that is not a digit              |
- Things get way more complicated than this though, here is the standard regex used to validate email addresses:

```regex
^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$
```

