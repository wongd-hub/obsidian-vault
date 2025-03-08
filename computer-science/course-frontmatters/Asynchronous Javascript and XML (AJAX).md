#course_cs50 

- This enables us to create a single page app.

# Flask example

- In this example, on the Python side, you'd only need to implement `/search` to send a list of `<li>` elements that match the queried string.
    - It'd be cleaner to pass back something in [[JavaScript Object Notation (JSON)]] format then dynamically render them as `<li>`s.
## index.html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <meta name="viewport" content="initial-scale=1, width=device-width">
        <title>shows</title>
    </head>
    <body>

        <input autocomplete="off" autofocus placeholder="Query" type="search">

        <!-- Note blank ul here -->
        <ul></ul>

        <script>

            let input = document.querySelector('input');
            input.addEventListener('input', async function() {

                // fetch() lets you make an HTTP request
                let response = await fetch('/search?1=' + input.value);
                let shows = await response.text();
                document.querySelector('ul').innerHTML = shows;
            
            })

        </script>
    </body>
</html>
```