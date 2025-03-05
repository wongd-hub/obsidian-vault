#course_cs50

- Stands for Cascading Style Sheets and is a way to specify styling of the webpage.

- We're again going to have key-value pairs here, referred to as *properties*.
- We'll be able to specify different selectors which allow us to target [[HTML]] website elements for styling.

- You can either place a `<style>` element in the `<head>` of your HTML or `<link>` to a CSS stylesheet, i.e.:

```html
<head>
    <style></style>
</head>

<head>
    <link href="styles.css" rel="stylesheet">
</head>
```

- CSS is specified with the following structure

```css
<css-selector-here> {
    property: value;
    ...
}
```

- There are various third-party frameworks that can provide CSS classes pre-styled for you such as [[Bootstrap]].
# Common properties

| Property          | What it does                             |
| ----------------- | ---------------------------------------- |
| `font-size`       | Font size                                |
| `text-align`      | Justification of text                    |
| `color`           | Font colour                              |
| `text-decoration` | Modifies things such as text underlining |
# Common selectors

| Selector                           | Meaning                                                  |
| ---------------------------------- | -------------------------------------------------------- |
| `<tag>`, e.g. `head`               | Selects all elements of the given tag type               |
| `.<class>`, e.g. `.centered`       | Selects all elements with a given class                  |
| `#<id>`, e.g. `#harvard`           | Selects the element with a given ID                      |
| `selector:<state>`, e.g. `a:hover` | Allows you to modify on-hover and other on-event styling |

# Adding styling directly to HTML tags

- You can add a `style` attribute directly to HTML tags 

```html
<body>
    <div style="font-size: large; text-align: center;">
        John Harvard
    </div>
    <div style="font-size: medium; text-align: center;">
        Welcome to my home page!
    </div>
    <div style="font-size: small; text-align: center;">
        Copyright (c) John Harvard
    </div>
</body>
```

- However, this is not ideal since we need to center all 3 divs separately. We could instead apply `text-align` once to the `body` and all children will inherit this styling.
- It would also be nice to be able to re-use these styles so that we can work across multiple HTML pages without needing so much copy-paste.

# Styling in the `<style>` tag

```html
<!DOCTYPE html>

<html lang='en'>
    <head>
        <style>
            body { /* Select all body tags */
                text-align: center;
            }

            header {
                font-size: large;
            }

            .medium { /* Select all tags with the medium "class" */
                font-size: medium;
            }

            .small {
                font-size: small;
            }
        </style>
        <title>home</title>
    </head>
    <body>
        <header class="large">John Harvard</header>
        <main class="medium">Welcome!</main>
        <footer class="small">Copyright</footer>
    </body>
</html>
```

# Styling in a separate stylesheet

```css
body { /* Select all body tags */
    text-align: center;
}

header {
    font-size: large;
}

.medium { /* Select all tags with the medium "class" */
    font-size: medium;
}

.small {
    font-size: small;
}
```

```html
<!DOCTYPE html>

<html lang='en'>
    <head>
        <link href="home.css" rel="stylesheet">
        <title>home</title>
    </head>
    <body>
        <header class="large">John Harvard</header>
        <main class="medium">Welcome!</main>
        <footer class="small">Copyright</footer>
    </body>
</html>
```

# Devtools

- You can use browser devtools to view the CSS styling on a webpage as well.
    - Using the 'Computed' tab you can figure out the size of things
    - You can also fine where each style applied to a particular element originates from

