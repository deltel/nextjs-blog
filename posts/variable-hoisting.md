---
title: "Variable Hoisting in JavaScript"
date: "2023-10-15"
---

## Hoisting

This is the process where by the interpreter seemingly moves the declaration of variables, functions, classes or imports to the top of their scope before executing the code.  
[- MDN web docs](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

Our focus is on variables.

Declaring a variable with the **var** keyword will result in the variable being hoisted. This means that calling the variable before its declaration will not result in a reference error, but will instead give the value as **undefined**.  
Since the value is **undefined**, only the declaration is hoisted and not the initialization.

```js
test("var declarations are hoisted", () => {
  expect(x).toBeUndefined();
  var x = "x";
});
```

As for variables declared with the **let** and **const** keywords, we can consider them to not be hoisted for all practical purposes.  
There is some nuance to the behaviour, but I will not explore it in this article.

```js
test('hoisting is immaterial for "let" and "const"', () => {
  let x = 1;
  const y = 2;

  (function () {
    expect(() => x).toThrow(ReferenceError); //notice that x is not 1
    expect(() => y).toThrow(ReferenceError); //and y is not 2
    let x = "x";
    const y = "y";
  })();
});
```
