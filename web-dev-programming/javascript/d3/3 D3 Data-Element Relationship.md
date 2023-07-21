#codecademy-d3 

With elements [[2 D3 Selection|selected]], we can now associate data per element. You do this with `.data()` which takes an array of any type and binds its elements to the selected objects returned by `.select/selectAll()`. e.g.:

```js
let dataset = [55,34,23,22,59];  
d3.selectAll("p").data(dataset);
```

`.data()` does not recycle the items in the `dataset` by default. If you provide a dataset that has less elements than the selected items, then the elements that don't have a matching element are left blank. On the other hand, if `dataset` is longer than the selected items then the surplus items will simply be left out.

If you want to address the mismatch between dataset length and selected elements, use the `.join()` method; e.g.:

```js
let fruits = ['Apple', 'Orange', 'Mango'];

d3.select(".d3_fruit")
    .selectAll("p")
    .data(fruits)
    .join("p") // the join method
      .attr("class", "d3_fruit")
      .text((d) => d);

// html

<div class="d3_fruit"></div>
```