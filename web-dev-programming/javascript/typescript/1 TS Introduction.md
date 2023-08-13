#course_codecademy-typescript #typescript

## Introduction

- JavaScript was designed to be flexible and easy to use for small applications. However, this makes the language less ideal for building larger applications.
- Stricter programming languages will inform the developer whenever a change in one area of code will break other areas. This doesn't happen in JavaScript, leading to confusing issues at runtime.

- Microsoft developed TypeScript in 2012 as a way of blending the flexibility of JavaScript with stricter languages (i.e. it is strongly typed).
  - TypeScript adds types to JavaScript allowing us to clarify the structure of and help refactor our code if need be.
  - In addition, TypeScript adds arrow functions and classes to the language years before they were added to JavaScript.

## What is TypeScript?

- TypeScript code is a *superset* of JavaScript code, so it contains everything that JavaScript has, but adds more functionality to it.
- We write TypeScript code in files with the `.ts` extension. This is then run through the TypeScript *transpiler* which checks the codes for errors and code that doesn't adhere to TypeScript's standards.
  - Assuming everything goes to plan, the transpiler will output a JavaScript version of the file (`.js`).
  - You can run the transpiler with the `tsc` bash command on a `.ts` script. Run the resulting `.js` file with the `node filehere.js` command.

For example, this gets compiled to…

```ts
let firstName = 'Anders';
```

… this (which is exactly the same since there's no TypeScript-specific code here)

```js
let firstName = 'Anders';
```

