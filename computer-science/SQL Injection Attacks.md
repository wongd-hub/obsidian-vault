#course_cs50 

- Taking user input without first sanitising it can be highly dangerous, and will leave you open to SQL injection attacks.
- The core concept is that the user inputs something that cancels the current query, then injects their own potentially malicious statements into the query.

```python
# username input: malan@harvard.edu.au'--

db.execute(
    f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
)
```

- This evaluates to the following where the username string is terminated, and the remaining query is commented out because of the `--`. Replace the `--` with your own code to delete tables or pull sensitive information and you've got an SQL injection attack.

```sql
SELECT * FROM users WHERE username = 'malan@harvard.edu.au'--' AND password = '{password}'
```

- The solution is to not use f-strings, and instead use libraries that properly escape user input before running them in your queries.
    - They would do something like escape the user input with a single quote, thereby rendering the injection attack inert.

```sql
SELECT * FROM users WHERE username = 'malan@harvard.edu.au''--' AND password = '{password}'
```