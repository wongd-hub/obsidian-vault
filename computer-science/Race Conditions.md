#course_cs50 

# In the context of SQL

- Consider the example of the most-liked Instagram post in the world. It's conceivable that Meta is using code similar in spirit to the following to update the number of likes when someone likes the post.

```python
rows = db.execute("SELECT likes FROM posts WHERE id = ?", id);
likes = rows[0]["likes"]
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, id)
```

- However, what happens if you get multiple concurrent likes? Further, large companies like Meta are executing this code on thousands of servers concurrently. 
    - The number of `likes` will not yet be updated with the latest number.
    - What we run into here is called a *race condition* where the servers are racing to handle user requests at the same time.
    - The consequence here is that a number of likes will be lost, since their increment calculations were based off the variable's old value.

> [!example]
> Say you have a refrigerator and find that you're out of milk. You open the fridge to check, then close it to go to get more milk. Then your roommate checks the fridge, finds there's no milk, and leaves to get more milk.
> 
> You end up in a situation where you now have 2 cartons of milk since you both set off to get more milk without the other being aware of it.
> 
> In other words, you both based your decision on the value of a variable - but that variable was in the process of getting updated when your roommate looked at it.

- SQL implements solutions to this. Wrapping the code above in `BEGIN TRANSACTION` and `COMMIT` means that all of these transactions will either happen *together* or *not at all*.
    - There is also the `ROLLBACK` keyword. When used, it reverts all the changes since the last `COMMIT` or `ROLLBACK`.
    - This is likely one of the building blocks towards being [ACID compliant](https://www.mongodb.com/resources/products/capabilities/acid-compliance).

```python
db.execute("BEGIN TRANSACTION")
rows = db.execute("SELECT likes FROM posts WHERE id = ?", id)
likes = rows[0]["likes"]
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, id)
db.execute("COMMIT")
```
