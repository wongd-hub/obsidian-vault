#course_cs50 

- So far, we've been using `http-server` to serve our static [[HTML]]. This also allows us to run this on non-standard ports.
    - Static websites refer to the fact that the HTML, CSS, and JavaScript are pre-written and will only change once you go in and manually update the code. In other words, content is not dynamically generated and the server does not handle any backend processing.
- We'll move the focus from websites to web applications where we can start taking input and producing output.

- We've talked about URLs until now as essentially ways to navigate a file system. However, with web applications, we refer to these paths more as *routes* since a dynamically generated website can be much more flexible.

- Flask is a (micro)framework that makes it easier to implement web applications in Python.
- We use this in the CLI and start apps with:

```shell
flask run
```

# File structure of Flask project

```
📁 flask-project/
├─ 📄 app.py
├─ 📄 requirements.txt
├─ 📁 static/
├─ 📁 templates/
```

- We'll have an `app.py` Python program, as well as a text file that sets out the package dependencies and their versions
- We'll have a `static/` folder that contains all of our static assets such as images, CSS files
- We'll have a `templates/` folder with any of the HTML, CSS, and JavaScript we write

# Examples: Hello, world

```shell
mkdir templates
code templates/index.html
code app.py
```

## index.html

- These template placeholders are a feature of [[Jinja]], which standardises the syntax you can use for these placeholders.

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <meta name="viewport" content="initial-scale=1, width=device-width">
        <title>hello</title>
    </head>
    <body>
        <!-- Template to fill in, using double curly braces -->
        hello, {{ name }}
    </body>
</html>
```

## app.py

- Some notes:
    - We use `flask.render_template()` to take a HTML file, render it, then serve it to the user's browser
    - We use `request.args` to get a dictionary of the key-value pairs the user has provided in the URL

```python
from flask import Flask, render_template, request

# Tell framework to treat this file as a web app - pass the name of this file to Flask
app = Flask(__name__)

# Implement root route to call `index()`
@app.route("/")
def index():

    if "name" in request.args:
        # Grab name from user input in URL
        name = request.args["name]
    else:
        name = "world"

    # A better way to do the above is with request.args.get()
    
    # Render template reads and renders HTML/template files
    return render_template("index.html", name = name)
```

## Resulting HTML from devtools

- We can see that the server sent the HTML with the placeholder filled in. 
    - This is the distinction between a static and a dynamic application. The HTML file was written statically, but this file + the interpolation of the placeholder was served dynamically.

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta name="viewport" content="initial-scale=1, width=device-width">
        <title>hello</title>
    </head>
    <body>
        hello, Darren
    </body>
</html>
```

# Examples: Greeter

- Here we demo the use of flask layouts to reduce the amount of boilerplate code we need to write and maintain.
    - This allows us to copy snippets of HTML into a pre-written template using `{% block <block-name> %}{% endblock %}`
## layout.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <meta name="viewport" content="initial-scale=1, width=device-width">
        <title>hello</title>
    </head>
    
    <body>
        {% block body %}{% endblock %}
    </body>
</html>
```

## greet.html

- We are *inheriting* everything from `layout.html`, but are also plugging in extra things.

```html
{% extends "layout.html" %}

{% block body %}

    hello, {{ name }}

{% endblock %}
```

## index.html

```html
{% extends "layout.html" %}

{% block body %}

    <form action="/greet" method="get">
        <input autocomplete="off" autofocus name="name" placeholder="Name" type="text">
        <button type="submit">Greet</button>
    </form>

{% endblock %}
```

## app.py

- The logic to interpolate the blocks is abstracted away inside of `render_template()`.

```python
from flask import Flask, render_template, request

# Tell framework to treat this file as a web app - pass the name of this file to Flask
app = Flask(__name__)

# Implement root route to call `index()`
@app.route("/")
def index():
    # Render template reads and renders HTML/template files
    return render_template("form.html")

@app.route("/greet")
def greet():
    name = request.args.get("name", "world")
    return render_template("greet.html", name = name)
```

# Examples: Using POST

- What if we want the user input to not show up in the URL?
- We'll need to tell the server how to handle `POST` methods.

## index.html

```html
{% extends "layout.html" %}

{% block body %}

    <!-- Note the change to the method here -->
    <form action="/greet" method="post">
        <input autocomplete="off" autofocus name="name" placeholder="Name" type="text">
        <button type="submit">Greet</button>
    </form>

{% endblock %}
```

## app.py

```python
from flask import Flask, render_template, request

# Tell framework to treat this file as a web app - pass the name of this file to Flask
app = Flask(__name__)

# Implement root route to call `index()`
@app.route("/")
def index():
    # Render template reads and renders HTML/template files
    return render_template("form.html")

# EDITS HERE - adding the methods argument to the route
@app.route("/greet", methods=["POST"])
def greet():
    # We can no longer use request.args here
    name = request.form.get("name", "world")
    return render_template("greet.html", name = name)
```

# Examples: Putting everything under one route

## app.py

- When the user hits the root route for the first time, they are technically using `GET`. This means they'll get `index.html` rendered to them - containing the form.
- Once they use the form, that implements a `POST` method, which takes them back to the root route but this time `greet.html` will be interpolated and rendered for them.

```python
from flask import Flask, render_template, request

# Tell framework to treat this file as a web app - pass the name of this file to Flask
app = Flask(__name__)

# Implement root route to call `index()`
@app.route("/", methods=["GET", "POST"])
def index():

    if request.method == "POST":
        # Fix a bug where if the user inputs a blank name, "hello, " is printed.
        name = request.form.get("name", "world")
        return render_template("greet.html", name=name)

    return render_template("index.html")
```

## index.html

```html
{% extends "layout.html" %}

{% block body %}

    <form action="/" method="post">
        <input autocomplete="off" autofocus name="name" placeholder="Name" type="text">
        <button type="submit">Greet</button>
    </form>

{% endblock %}
```

## greet.html

- Here we demo conditionals inside a Jinja template

```html
{% extends "layout.html" %}

{% block body %}

    hello, {% if name %}{{ name }}{% else %}world{% endif %}

{% endblock %}
```