# this keyword (global / function / arrow / class)

- The `this` keyword in JavaScript behaves differently depending on
  where and how it is used.
- It does NOT refer to the function itself, but to the object that
  "owns" the function execution context.

**Rules change between:**

- Global context
- Normal function calls
- Arrow functions
- Object methods
- Classes / constructors

**Let's break each case step by step with examples & theory.**

# Global Context

**In global scope, `this` refers to the global object.**

- In Browser → `window`
- In Node.js → `{}` (module.exports) instead of `global`

```js
console.log(this); // Browser: window | Node: {}
```

# Normal Function

Rule: In a normal function call, `this` depends on **strict mode**.

- Strict mode (`'use strict'`) → `this = undefined`
- Non-strict mode → `this = global object`

```js
function normalFn() {
  console.log(this);
}
normalFn(); // undefined (strict) OR window (non-strict)
```

# Arrow Function

**Rule: Arrow functions do NOT have their own `this`.**

- Instead, they use `this` from their **lexical scope** (the place where they were created).

```js
const arrowFn = () => {
  console.log(this);
};
arrowFn();
// In browser: window
// In Node: {} (same as global `this` in this file)
```

# Inside Object

**Case 1: Normal method** → `this` refers to the object that calls it.
**Case 2: Arrow method** → does not bind `this`, so it takes from outer (global) scope.

```js
let obj = {
  name: "JS",
  sayName: function () {
    console.log(this.name); // ✅ "JS" → this = obj
  },
  arrowSay: () => {
    console.log(this.name); // ❌ undefined → this = global scope
  },
};
obj.sayName();
obj.arrowSay();

//
// 👉 Why undefined in arrowSay?
// Because arrow functions don't get their own `this`.
// They inherit from the surrounding scope, which here is the file/global,
// not the object.
```

# Inside Class

In classes (or constructor functions), `this` refers to the object being created from the class.

```js
class Person {
  constructor(name) {
    this.name = name; // "this" = the new object
  }
  getName() {
    console.log(this.name);
  }
}

let p = new Person("Alice");
p.getName(); // Alice

// 👉 When using `new`, a brand-new object is created.
//    - `this` inside constructor points to that new object.
//    - The methods inside class also use that same `this`.
//
// Caution: If you pass methods as callbacks, you may lose `this` binding.
// Example:
let callback = p.getName;
callback(); // ❌ undefined (lost this reference)
```

# Easy Theory Summary

**- Global scope** → `this = window` (browser) OR `{}` (Node).
**- Normal function** → strict → undefined, non-strict → window.
**- Arrow function** → no own `this`, takes from outer lexical scope.
**- Object method (normal)** → `this = object` that calls it.
**- Object method (arrow)** → no binding, inherits from outer scope.
**- Class/Constructor** → `this` = new instance created.

# Common Interview Q&A

**Q1: Why does arrow function not bind `this`?**
Because arrow functions were designed to avoid the “losing this”
problem. They use lexical scoping for `this`.

```js
// Example:
let obj2 = {
  name: "Lexical",
  arrow: () => console.log(this.name), // this = outer scope
};
obj2.arrow(); // ❌ undefined
```

**Q2: How to fix lost `this` in class methods?**
Solution: Use .bind(), or arrow functions in class methods.

```js
class User {
  constructor(name) {
    this.name = name;
    this.getName = this.getName.bind(this); // ✅ Fix
  }
  getName() {
    console.log(this.name);
  }
}
let u = new User("Bob");
setTimeout(u.getName, 1000); // Bob
```
