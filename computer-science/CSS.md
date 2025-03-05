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

# Adding styling directly to HTML tags

- You can add a `style` attribute directly to HTML tags 

```html
<body>
    <p style="font-size: large; text-align: center;">
        John Harvard
    </p>
    <p style="font-size: medium; text-align: center;">
        Welcome to my home page!
    </p>
    <p style="font-size: small; text-align: center;">
        Copyright (c) John Harvard
    </p>
</body>

```