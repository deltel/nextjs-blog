---
title: "JavaScript Functions - Parameters and Hoisting"
date: "2023-10-16"
---

## Parameters

Parameters are passed to functions by value.  
If a new value is assigned to a parameter in a function, the change will not be reflected globally.

```js
test("parameters reassigned in a function do not affect the global scope", () => {
  function func1(x) {
    x = "not x";
  }

  let x = "x";
  func1(x);

  expect(x).toBe("x");
});
```

This is also for parameters that are objects and arrays.

You can however change the property of an object passed to a function and expect the change to be reflected globally.

```js
test("the change in the property of an object is reflected globally", () => {
  function changeName(person) {
    person.name = "Jane";
  }

  const person = {
    name: "John",
    age: 20,
  };

  changeName(person);

  expect(person.name).toBe("Jane");
});
```

Similarly for arrays, changing a value stored in an array will result in the change being reflected globally.

```js
test("the change in the element of an array is reflected globally", () => {
  function changeName(person) {
    person[0] = "Jane";
  }

  const person = ["John"];

  changeName(person);

  expect(person[0]).toBe("Jane");
});
```

## Hoisting

There are two ways to create a function:

- function declaration
- function expression

### Function Declaration

```js
function square(n) {
  return n * n;
}
```

Function declarations are hoisted.

```js
test("function declarations are hoisted", () => {
  expect(square(5)).toBe(25);

  function square(n) {
    return n * n;
  }
});
```

### Function Expression

```js
const square = function (n) {
  return n * n;
};
```

Function expressions are not.

```js
test("function expressions (anonymous functions) are not hoisted", () => {
  expect(square).toBeUndefined();
  expect(() => square(5)).toThrow(TypeError);

  var square = function (n) {
    return n * n;
  };

  expect(() => square1(5)).toThrow(ReferenceError);

  const square1 = function (n) {
    return n * n;
  };
});
```
