#course_cs50 

- All about aesthetics and mocking up the structure of web pages. It is a markup language (Hypertext Markup Language).
    - This gives your browser the information on what to display to you.
- There are two main concepts: tags and attributes.

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <title>
            hello, title
        </title>
    </head>
    <body>
        hello, body
    </body>
</html>
```

# Tags




# Attributes



# Serving HTML webpages when on a headless server

- If we had access to Chrome or another browser, we could simply double click the HTML file and it would render the webpage for us.
- However, if we're on a server like the CS50 VSCode fork, we'll need to use the `http-server` package to serve HTML files for you.

```shell
http-server

#> This gives us a link to go to that takes you to part of your file system
#>  This link also contains a port number
```