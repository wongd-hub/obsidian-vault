#course_cs50 

- This is how websites are able to remember that you logged in already or your preferences persistently. In other words, cookies allow the website to be stateful - and remember something about you.
    - Their hidden nature allows advertisers and other websites to track your browsing as well.

> [!note]
> Even if you delete your cookies, it's very likely a server can still identify you with high likelihood based on your IP address, the fonts and extensions you have installed, etc.

# Logging in

- When you navigate to Google, your browser sends an envelope to the server:

```
GET / HTTP/2
Host: accounts.google.com
...
```

- If you logged in successfully, you'll get a response back from the server:
    - The first two lines are what we've seen before and are just the contents of the webpage.
    - The last line, `Set-Cookie`, specifies a key-value pair where the value is some unique identifier.
        - This is essentially the server stamping your hand and saying you've already logged in. This stores the value in the browser's memory for a defined amount of time.
        - *Session* is a word that describes the maintenance of state between a client and a server. So if the server is remembering something about you, you have a session with that server. e.g. a shopping cart.

```
HTTP/2 200
Content-Type: text/html
Set-Cookie: session=value
...
```

- Now, if you have a valid cookie, your browser will send the cookie back to the server whenever you revisit the website:

```
GET / HTTP/2
Host: accounts.google.com
Cookie: session=value
...
```

# Examples: Using cookies in Flask

- [[Flask]] allows you to do this relatively easily.
- `session`, imported from `flask`, is essentially an empty dictionary ready for key-value pairs to be placed into it.
    - This is distinct from the `REGISTRANTS` dictionary defined in the [[Flask#Examples Frosh IMs|Flask Frosh IMs example]]. That dictionary would not persist when the user closes the tab, and it is also global - any user can access it.
    - Information is stored in `./flask_session/`.

## app.py

```python
from flask import Flask, redirect, render_template, request, session
from flask_session import Session

app = Flask(__name__)

# Configure cookies - So the cookie is deleted when you quit the browser
app.config["SESSION_PERMANENT"] = False 

# Configure cookies - So the cookie is stored in the server's files, not in
# the cookie itself for privacy's sake
app.config["SESSION_TYPE"] = "filesystem"
Session(app)

app.config

@app.route("/")
def index():
    return render_template("index.html", name=session.get("name"))

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        session["name"] = request.form.get("name")
        return redirect("/")
    return render_template("login.html")

@app.route("/logout")
def logout():
    session.clear()
    return redirect("/")
```

## layout.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <meta name="viewport" content="initial-scale=1, width=device-width">
        <title>froshims</title>
    </head>
    
    <body>
        {% block body %}{% endblock %}
    </body>
</html>
```

## index.html

```html
{% extends "layout.html" %}

{% block body %}

    {% if name %}

        You are logged in as {{ name }}. <a href="/logout">Log out</a>.

    {% else %}

        You are not logged in. <a href="/login">Log in</a>.

    {% endif %}

{% endblock %}
```

## login.html

```html
{% extends "layout.html" %}

{% block body %}

    <form action="/login" method="post">
        <input 
            autocomplete="off" 
            autofocus 
            name="name" 
            placeholder="Name" 
            type = "text"
        >
        <button type="Submit">Log In</button>
    </form>

{% endblock %}
```

# Examples: Shopping cart

- This uses a similar concept to [[Flask#Examples If we want to use a SQL database to store our registrants|the last Flask example we saw]] in that it gives each item in the shop a unique ID, so every 'Add to Cart' button sends that unique ID to the form. This allows the cookie to keep track of exactly which item is in the cart.

## SQL Lite shop items table

```
sqlite> SELECT * FROM books;
+----+-----------------------------------------------+
| id | title                                         |
+----+-----------------------------------------------+
|  1 | The Hitchhiker's Guide to the Galaxy          |
|  2 | The Restaurant at the End of the Universe     |
|  3 | Life, the Universe and Everything             |
|  4 | So Long, and Thanks for All the Fish          |
|  5 | Mostly Harmless                               |
+----+-----------------------------------------------+
```

## app.py

```python
from cs50 import SQL
from flask import Flask, redirect, render_template, request, session
from flask_session import Session

# Configur eapp
app = Flask(__name__)

# Connect to DB
db = SQL("sqlite:///store.db")

# Cofigure session
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)

@app.route("/")
def index():
    # Returns a list of dictionaries
    books = db.execute("SELECT * FROM books")
    return render_template("bookts.html", books=books)

@app.route("/cart", methods=["GET", "POST"])
def cart():

    # Ensure cart exists
    if "cart" not in session:
        session["cart"] = []

    # POST
    if request.method == "POST":
        book_id = request.form.get("id")
        if book_id:
            session["cart"].append(book_id)
        # Sends us back to this route, but with a GET
        return redirect("/cart")

    # GET
    #  `(?)` lets you plug a list into the query, and commas will be generated
    #  for you.
    books = db.execute("SELECT * FROM books WHERE id IN (?)", session["cart"])
    return render_template("cart.html", books=books)
```
## books.html

```html
{% extends "layout.html" %}

{% block body %}

    <h1>Books</h1>
    {% for book in books %}
        <h2>{{ book["title"] }}</h2>
        <form action="/cart" method="post">
            <input 
                name="id" 
                type="hidden"
                value="{{ book['id'] }}"
            >                
            <button type="Submit">Add to Cart</button>
        </form>
    {% endfor %}

{% endblock %}
```

## carts.html

```html
{% extends "layout.html" %}

{% block body %}

    <h1>Cart</h1>
    <ol>
        {% for book in books %}
            <li>{{ book["title"] }}</li>
        {% endfor %}
    </ol>

{% endblock %}
```

# Examples: Scaling up the data

## app.py



## index.html