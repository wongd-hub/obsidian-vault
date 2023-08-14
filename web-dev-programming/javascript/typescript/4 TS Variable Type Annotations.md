#course_codecademy-typescript #typescript 

We can assert the type of a variable upon initialisation by using a *type annotation*. We do this with the `:` symbol.

```ts
let mustBeAString: string;  
mustBeAString = 'Catdog';  
  
mustBeAString = 1337;
// Error: Type 'number' is not assignable to type 'string'

var age: number = 32; // number variable
var name: string = "John";// string variable
var isUpdated: boolean = true;// Boolean variable
```

These are automatically removed when TS transpiles this to JavaScript.

## Any

There are some situations when TS won't try to infer a type, generally when a variable is initialised without assigning a value to it. In cases where TS isn't able to infer a type, it will give the variable the type `any`.

These variables can be re-assigned any type later on with no errors.

```ts
let onOrOff;  
  
onOrOff = 1;  
onOrOff = false;
```

