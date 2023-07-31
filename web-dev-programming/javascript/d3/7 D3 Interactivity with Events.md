#course_codecademy-d3 #d3 

Another powerful aspect of D3 is that it has access to [[Document Object Model|DOM]] events.

The `.on()` method takes the event as a string and a function to run upon that event and binds this function to the elements in the selection. e.g.:

```js
selection  
  .on("mouseover", function(d,i) {  
    d3.select(this).text(d);  
  });
```

The following assigns poem verses to each `p` tag and starts them all with 'Click me!' text. The `.on()` method then allows each element to switch over to their bound data element upon click.

```js
let poemVerses = [
  "Always", "in the middle", 
  "of our bloodiest battles", 
  "you lay down your arms",
  "like flowering mines",
  "to conquer me home."
];

d3.select("#viz")
    .selectAll('p')
    .data(poemVerses)
    .enter()
  .append('p')
    .text('Click Me!')
    .on('click', function(d, i) {
      d3.select(this).text(d);
    })
```