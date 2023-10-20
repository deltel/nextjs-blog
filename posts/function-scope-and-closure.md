---
title: "JavaScript Functions - Scope and Closure"
date: "2023-10-19"
---

## Scope

A function generally has access to two scopes. The one in which it was defined _(outer)_ and the one which it defines _(inner)_.

Let us focus on the inner function defined below.

```js
function outer() {
  // the scope in which it was defined
  var x = "x";
  function sampleFunction() {}

  function inner() {
    // the scope it defines
    var y = "y";
    function sampleInnerFunction() {}
  }
}
```

The inner function will have access to all variables and other functions defined in its outer scope as well as all the variables and functions defined within its scope.  
So the inner function has access to the variable "x" and the function "sampleFunction"; it can all them within its body.
Of course, it can also do whatever it desires with the variable "y" and the function "sampleInnerFunction".

```js
test("functions have access to the outer scope", () => {
  var x = "x";
  function name() {
    return "Jane";
  }

  function scope() {
    return x;
  }

  function secondScope() {
    return name();
  }

  expect(scope()).toBe("x");
  expect(secondScope()).toBe("Jane");
});
```

It is worth noting that the function "outer" does not have access to the variables and functions defined in "inner".

```js
test("functions do not have access to an inner function's scope", () => {
  function outer() {
    function secondScope() {
      var person = "Jane";
    }

    return person;
  }

  expect(() => outer()).toThrow(ReferenceError);
});
```

## Closure

Functions also form closures. A closure can be thought of as the inner and outer scope of a function. In other words, the closure formed by **inner** has access to **y**, **sampleInnerFunction**, **x** and **sampleFunction**.

```js
function outer() {
  // the scope in which it was defined
  var x = "x";
  function sampleFunction() {}

  function inner() {
    // the scope it defines
    var y = "y";
    function sampleInnerFunction() {}
  }
}
```

Here's another example:

```js
test("the inner and outer scope form the closure", () => {
  function outer(x) {
    function inner(y) {
      return x + y;
    }

    return inner;
  }

  expect(outer(5)(7)).toBe(12);
  expect(outer(7)(14)).toBe(21);
});
```

Notice how **inner** remembers the value of **x** that was received by **outer** in each instance, this is because a closure preserves the arguments and variables in all scopes it has access to.

## Scope Chain

The scope chain refers to the scopes a function has access to starting with the inner most function and then moving outwards. The inner functions have access to the outer functions' scopes because they form closures.

```js
function outer(a) {
  // accessible by innerMost
  // accessible by inner
  // accessible by outer
  function inner(b) {
    // accessible by innerMost
    // accessible my inner
    function innerMost(c) {
      // only accessible by innerMost
    }
  }
}
```

The variables in the inner most function take precedence to any declared in an outer scope. This means that any naming conflicts will always choose the value stored in the inner most scope.

```js
test("naming conflicts are resolved recursively starting with the inner most scope", () => {
  function outer() {
    var x = "x";
    function inner(x) {
      return x;
    }

    return inner;
  }

  expect(outer()("y")).toBe("y");
});
```

There will be no way to access the variable **x** defined in **outer** from within **inner**.
