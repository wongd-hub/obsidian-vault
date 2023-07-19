#codecademy-d3 

With elements [[2 D3 Selection|selected]], we can now associate data per element. You do this with `.data()` which takes an array of any type and binds its elements to the selected objects returned by `.select/selectAll()`. e.g.:

```js
let dataset = [55,34,23,22,59];  
d3.selectAll("p").data(dataset);
```

