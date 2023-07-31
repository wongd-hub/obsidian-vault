#course_codecademy-d3 #d3 

Conventional chaining styling for D3 will use a single-indented line to indicate when the selection is changing, and a double-indented line for operations on the current selection, e.g.:

```js
let dataset = [55,34,23,22,59];  
  
d3.select("body")  
  .selectAll("div") // Selection has changed
    .data(dataset)  
    .enter()  
  .append("div")    // Selection has changed to the enter selection
    .text("Some text");
```