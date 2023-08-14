#course_codecademy-typescript #typescript 

JavaScript lets us assign any value to any variable which makes it flexible and easy to use in the first instance. However, assigning different value *types* to a variable throughout a script can lead to buggy and difficult to understand code.

TypeScript makes it so that a value's type can never be changed once initially set. This is an example of *type inference* - TypeScript expects the type of the variable to match that of the value it was initially assigned. TS recognises JavaScript's primitive types:

- boolean
- number
- null
- string
- undefined

```ts
let order = 'first';  
  
order = 1;

// Type 'number' is not assignable to type 'string'
//  - This is because TS expects `order` to be a string, not a numeric
```

