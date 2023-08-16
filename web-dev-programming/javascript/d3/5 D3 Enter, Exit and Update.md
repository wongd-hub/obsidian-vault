#course_codecademy-d3 #d3

## Enter/Update/Exit

When you see `selectAll()` followed by `.data()` we are:
- Selecting all matching elements
- For each element that exists, we bind an item from the data array to it
- `.data()` then returns an *update selection* containing the existing elements (if any) with data bound to them

If the number of selected elements doesn't match the number of elements in the data array then `.data()` will also return an *enter selection* or an *exit selection* in addition to an *update selection*.

- If we have excess data items, `.data()` provides an *enter selection* with one element for every item we need to add in order to have number of DOM elements = number of data items.

  - In this case, we often want to add more DOM elements to make up the difference and give all data points an element.
  - The *enter selection* contains placeholders for all elements we'd need to add. Calling `.enter()` on the update selection selects these placeholders, then `.append(tagname)` will create these elements in the DOM.
    - If you add attributes to the selection at this stage, you'll only be styling the newly added elements. To ensure that you apply your changes to all elements (old and new), use `.merge()` to merge the *enter* and *update selections*

```js
let circle = svg.selectAll("circle")
  .data([1,2,3,4])

circle.exit().remove()

circle.enter()
  .append("circle") 
    .merge(circle)
    .attr(…)
```

- If we have excess DOM elements, then we get an *exit selection* in addition to the *update selection*.
    - In this case, we often want to delete the excess elements since we don't need them. Calling `.exit()` will return the *exit selection* and calling `.remove()` will remove the excess elements.

## Join

`.join()` exists as a convenience method that:
- Removes the *exit selection*; and,
- Returns a merged *update selection* & *enter selection* that you can continue to style as needed.

The following block is equivalent to the previous block

```javascript
let circle = svg.selectAll("circle")
  .data([1,2,3,4])
  .join("circle")
  .attr(…)
```

## Further sources

- [What is the difference between .append and .join in D3.js - Stack Overflow](https://stackoverflow.com/a/69820794)
- [D3 Enter, Exit and Update](https://www.d3indepth.com/enterexit/)


