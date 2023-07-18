The Document Object Model (DOM) is the data representation of the objects that comprise the structure and content of a document on the web. This allows programmatic access to the elements on a webpage - as well as attaching event handlers to objects.

The DOM represents the content of a web document as a logical tree where each branch of the tree ends in a node and each node contains objects.

The DOM can be access using an API (which is a collection of multiple APIs) in languages such as JavaScript. e.g.:

```js
const paragraphs = document.querySelectorAll("p");
```

Note that the DOM API is not inherently a part of JavaScript - and can be access via other languages such as Python as well.