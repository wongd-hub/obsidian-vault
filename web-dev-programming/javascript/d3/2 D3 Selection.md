#codecademy-d3 

D3 works by 'injecting' data visualisations onto an object's [[Document Object Model]], and associates data with a set of DOM elements.

To do this, D3 first needs to 'select' the elements you want to inject data into. These selections are created using the `.selectAll()` or `.select()` methods.
* Both methods take a CSS3 selector string as a parameter and will return an array-like structure of all elements that match that selector (in the case of `.selectAll()`) or the first element that matches that selector (in the case of `.select()`).

Note that the elements that are being selected don't necessarily need to be present in the DOM. You can select items prior to them being added to the web-page as a way of [['setting the stage']].

## Examples

```html
<html>
	<head>
		<link rel="stylesheet" href="style.css">
		<!-- Loading D3 -->
		<script src="https://d3js.org/d3.v5.min.js"> </script>
		<script src="main.js" defer></script>
	</head>
	...
</html>
```

```js
// Injects the text 'These are divs' into all div elements
d3.selectAll('div').text('These are divs')
```