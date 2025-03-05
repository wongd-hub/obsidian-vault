#course_cs50

- We can use JavaScript to dynamically change the [[HTML]] on a webpage in the computer's memory/RAM.
- The are several ways to implement JavaScript, one way is to put it in a `<script>` element in the `<head>`.

# Events

- These are all events that you can add event listeners for in JS

```
blur
change
click
drag
focus
keyup
load
mousedown
mouseover
mouseup
submit
touchmove
unload
...
```

# Examples: Modifying the behaviour of a form

```html
<!DOCTYPE html>

<!-- Demonstrates onsubmit -->
<html lang="en">
    <head>
        <script>
            function greet() {
                alert('hello, ' + document.querySelector('#name').value);
            }
        </script>
        <title>hello</title>
    </head>
    <body>
        <!-- Call greet(), then don't return the result to the server -->
        <form onsubmit="greet(); return false;">
            <input autocomplete="off" autofocus id="name" type="text">
            <input type="submit">
        </form>
    </body>
</html>
```

- A brief explanation of the JavaScript above:
    - We define functions by writing `function function_name() {}`
    - JavaScript has an `alert()` function built-in that creates a browser popup
    - There also exists syntactic sugar in the form of `+` that can concatenate strings
    - `document.querySelector()` allows you to select any element on the page using CSS selectors, then taking the `value` attribute of that object gives you the text
        - In this case, once Submit is hit, the input box will have the `value` of whatever you put in there. So this code takes that value at this point and returns it in a popup.

- We could instead add an on-submit event listener in the form of an *anonymous function*. We add another layer of complexity by only adding this event listener once the [[Document Object Model]] has fully loaded.
    - This is best practice, to ensure that everything is loaded before allowing JavaScript to execute.

```html
<head>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            document.querySelector('form').addEventListener('submit', function(e) {
                alert('hello, ' + document.querySelector('#name').value);
                // Prevent default behaviour of a form and don't submit to server
                //  and also don't refresh webpage
                e.preventDefault();
            });
        });
    </script>
    <title>hello</title>
</head>
```

# Examples: Changing CSS

```html
<body>
    <button id="red">R</button>
    <button id="green">G</button>
    <button id="blue">B</button>

    <script>
        let body = document.querySelector('body');

        document.querySelector('#red').addEventListener('click', function() {
            body.style.backgroundColor = 'red';
        });

        document.querySelector('#green').addEventListener('click', function() {
            body.style.backgroundColor = 'green';
        });

        document.querySelector('#blue').addEventListener('click', function() {
            body.style.backgroundColor = 'blue';
        });
    </script>
</body>
```

- Note that we use `let` to define a variable in JavaScript.
- The code above adds on-click event listeners to the buttons with IDs `#red`, `#green`, `#blue` that sets the `body` [[CSS]] background colour to red, green, or blue.

# Examples: Toggle visibility of element

```html
<head>
    <script>
        // Toggles visibility of greeting
        function blink()
        {
            let body = document.querySelector('body');
            if (body.style.visibility == 'hidden')
            {
                body.style.visibility = 'visible';
            }
            else
            {
                body.style.visibility = 'hidden';
            }
        }

        // Blink every 500ms
        // This function is associated with the actual viewport/window
        window.setInterval(blink, 500);
    </script>
</head>
```

# Examples: Implementing autocomplete

```js
let WORDS = [
    'a',
    'aardvark',
    'aargh'
]
```

```html
<body>
    <input type="text" placeholder="Type to search…">
    <ul></ul>

    <!-- Load the WORDS JS array -->
    <script src="words.js"></script>
    
    <script>
        let input = document.querySelector('input');

        input.addEventListener('keyup', function(event) {
            let html = '';
            if (input.value) {
                for (let word of WORDS) {
                    if (word.startsWith(input.value)) {
                        html += `<li>${word}</li>`;
                    }
                }
            }
            document.querySelector('ul').innerHTML = html;
        });
    </script>
</body>
```

- In this example, there are a few JavaScript entities defined - the `WORDS` array and a pointer to the `input` element.
- Then there is a on-key-up event listener that:
    - Defines an empty string
    - If the value of the input is a non-empty string (empty strings are *falsy*), then it checks to see if any of the words in `WORDS` starts with the value of input.
        - It then appends the found words as list items to the empty unordered list by editing the `innerHTML` of the unordered list.

# Examples: Geolocation of user

- You can figure out where a user is in the world with nearly 3 lines of code assuming they have Location Services turned on:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Geolocation</title>
    </head>
    <body>
        <script>
            navigator.geolocation.getCurrentPosition(function(position) {
                document.write(
                    position.coords.latitude + ", " + position.coords.longitude
                );
            });
        </script>
    </body>
</html>
```