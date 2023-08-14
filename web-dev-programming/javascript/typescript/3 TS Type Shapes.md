#course_codecademy-typescript #typescript 

Since TS knows what *type* our objects are, it also knows what *shape* they take. The *shape* of an object describes (amongst other things) what methods and properties it may contain. For example:

- All strings are known to have `.length` property and a `.toLowerCase()` method

TS's transpiler will let you know if you're trying to access a property or method that doesn't exist on that object's type.

```ts
"MY".toLowercase();  
// Property 'toLowercase' does not exist on type '"MY"'.  
// Did you mean 'toLowerCase'?
```

