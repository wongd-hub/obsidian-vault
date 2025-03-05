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

# File structure of project

```
📁 flask-project/
├─ 📄 app.py
├─ 📁 templates/
├─ 📄 app.py
```

- We'll have an app.py Python program
- We'll have a `templates` folder with any of the HTML, CSS, and JavaScript we write
- 