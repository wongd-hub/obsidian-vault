#course_cs50 

- Python isn't the most pleasant way to deal with data at scale. Structured Query Language (SQL) is a database-centric language used to query databases.
    - In CS50, we'll be using a light version of SQL, `sqlite3`, which contains the core building blocks of SQL.
    - Note that there's a lot of different SQL vendors and so different platforms such as Oracle and Teradata may have slightly different syntaxes and extra functionality.

- The commands used in SQL correspond closely to the concept of [[Relational Databases#CRUD|CRUD]]. e.g.:

```SQL
"Create"  : CREATE, INSERT
"Retrieve": SELECT
"Update"  : UPDATE
"Delete"  : DELETE, DROP /* (a whole table) */
```

# How do you run `sqlite3`?

- Let's load a [[Relational Databases#^b38c8a|flat file]] into this database for analysis.

```shell
# Create a new database - this will be a binary file
sqlite3 favourites.db

# We now want to import a CSV using the sqlite interpreter we've been put in
#sqlite> .mode csv
#sqlite> .import favourites.csv favourites # <filename> <tablename>
#sqlite> .quit
#$ ls
# favourites.csv favourites.db

# Let's get back into sqlite3
#$ sqlite3 favourites.db
#sqlite> .schema # Shows the command used to create the database
# CREATE TABLE IF NOT EXISTS "favourites"(
# "Timestamp" TEXT, "language" TEXT, "problem" TEXT);
#sqlite> SELECT * FROM favourites;
# ...
#sqlite> SELECT language FROM favourites;


```

# Retrieving
## `SELECT`

- Use `SELECT` to select certain or all columns from a table

```sql
SELECT * FROM table;                 /* Select all columns */
SELECT COLUMNX, COLUMNY FROM table;  /* Select these two columns from the table */
SELECT DISTINCT(COLUMNX) from table; /* Get all distinct values from COLUMNX */
SELECT COUNT(*) FROM table;          /* Count rows in table */
```

# Creation

## `CREATE TABLE`

- When creating a table, you have discretion over the name, and the data types in each column of the table.

```SQL
CREATE TABLE table (column type, ...);
```

## `INSERT INTO`

- We can insert rows into a table with the following

```sql
INSERT INTO favourites (language, problem) 
VALUES      ('SQL', 'Fiftyville');
```

# Predicates

- These are statements we can layer on to our base query to modify how it behaves.

```sql
WHERE    /* Filtering */
LIKE     /* Filtering */
ORDER BY /* Arranging */
LIMIT    /* Limiting rows */
GROUP BY /* Grouping for grouped operations */
```

e.g. 

```sql
SELECT COUNT(*) FROM favourites 
WHERE language = 'C' AND problem = 'Hello, World';
```

- To replicate the functionality in the [[Python#Opening and analysing CSVs|Python example]] of counting how many language responses there were and sorting by descending order:

```sql
SELECT   language, 
         COUNT(*) AS n
FROM     favourites 
GROUP BY language 
ORDER BY n DESC;
```
# Other functions
## Mathematical operations

```sql
AVG
COUNT
MAX
MIN
...
```

## String operations

```sql
UPPER
LOWER
NCHAR
```