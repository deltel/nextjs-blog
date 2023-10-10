---
title: "Variable Scope in JavaScript"
date: "2023-10-09"
---

## Scope

For our concern, the scope is simply the location in which a variable can be referenced based upon the location of its declaration.  
A variable may belong to one of 3 main scopes:

- global scope
- module scope
- function scope

## Global Scope

This is the default scope for all code running in script mode. This is the scope for any variable declared outside of a function.

## Module Scope

Similar to global scope but this is specific to code running in module mode.

## Function Scope

This is the scope created when a function is declared. It resides inside the function. It is also known as the local scope.

The code snippet below outlines the different scopes.

```js
var x; //global scope / module scope

function yScope() {
  var y; //function scope
}
```

## Block Scope

This is an additional scope that variables declared with **let** or **const** can belong to. A block scope resides in a pair of curly braces _(the block)_.

The test below runs using **jest** and demonstrates the behaviour for **let** and **const**.

```js
test("block scope applies to let and const", () => {
  if (true) {
    let x = "x";
    const y = "y";
  }

  expect(() => x).toThrow(ReferenceError);
  expect(() => y).toThrow(ReferenceError);
});
```

If a variable is declared with the **var** keyword inside a block, it is still scoped to the function if the block resides in a function or globally if the block does not reside in a function.  
Essentially, the **var** keyword does not believe it should be restricted by the block scope; it ignores it.

```js
test("block scope does not apply to var", () => {
  if (true) {
    var x = "x";
  }

  expect(x).toBe("x");
});
```
