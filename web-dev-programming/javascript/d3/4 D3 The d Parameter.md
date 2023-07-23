After we've [[2 D3 Selection|selected]] our elements and [[3 D3 Data-Element Relationship|bound]] our dataset to them, we'll need to find a way to properly display that data in those elements.

This comes in the form of multiple methods, each taking an anonymous function with argument `d` - where `d` represents each individual datum in your dataset. e.g.:

```js
let dataset = [55,34,23,22,59];  
d3.selectAll("p").data(dataset).text((d) => d);
```

The `.text()` method simply says to take each element of `dataset` and apply them to the selected elements.

> [!info]
> You can also include an `i` parameter in the anonymous function to access the index of the element.

There are multiple possible methods that can be used in this manner:

| Method        | Usage                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------- |
| `.attr()`     | Update selected element attribute (can be used for class changes, etc)                            |     
| `.classed()`  | Assigns or unassigns the specified CSS class names on the selected elements                       |     
| `.style()`    | Updates the style property                                                                        |     
| `.property()` | Used to set an element property                                                                   |     
| `.text()`     | Updates selected element text content                                                             |     
| `.html()`     | Sets the inner HTML to the specified value on all selected elements                               |     
| `.append()`   | Appends a new element as the last child of the selected element                                   |     
| `.insert()`   | Works the same as the `.append()` method, except you can specify another element to insert before |     
| `.remove()`   | Removes selected element from the DOM |

`.attr()` and `.style()` take two parameters; the attribute/style rule you want to change, and what you want to change it to. .e.g:

```js
let dataset = [55,34,23,22,59];  
  
let svg = d3
  .select("body")  
  .selectAll("div")  
  .data(dataset)  
  .attr("id", function(d,i) { return "element-" + i; })  
  .style("width", function(d) { return d + "px"; });
```
