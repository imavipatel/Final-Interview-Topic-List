# Arrow Functions

# What are Arrow Functions?

- Arrow functions were introduced in ES6 (2015).
- They provide a shorter and cleaner syntax for writing functions.
- They are always **expressions** (not declarations).

# Syntax Examples:

```js
// Normal function expression:
const add1 = function (a, b) {
  return a + b;
};
console.log(add1(2, 3)); // 5

// Arrow function (shorter):
const add2 = (a, b) => a + b;
console.log(add2(2, 3)); // 5
```

# Part 2: Syntax Variations

```js
// 1. With one parameter → parentheses optional
const square = (x) => x * x;
console.log(square(5)); // 25

// 2. With zero or multiple parameters → parentheses required
const greet = () => console.log("Hello!");
greet(); // "Hello!"

const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // 12

// 3. With multiple statements → need curly braces + explicit return
const divide = (a, b) => {
  console.log("Dividing numbers...");
  return a / b;
};
console.log(divide(10, 2)); // 5
```

# Lexical `this` (main difference)

**Normal functions** → `this` depends on how function is called.
**Arrow functions** → `this` is taken (lexically) from surrounding scope.

```js
let obj = {
  name: "JavaScript",
  normal: function () {
    console.log("Normal:", this.name);
  },
  arrow: () => {
    console.log("Arrow:", this.name);
  },
};
obj.normal(); // "Normal: JavaScript"
obj.arrow(); // "Arrow: undefined" (or global `this` in browsers)
```

# Arrow Functions vs Normal Functions

**Differences:**

1. Syntax → Arrow is shorter.
2. `this` → Arrow inherits from surrounding scope, Normal creates its own.
3. `arguments` → Normal has `arguments` object, Arrow does not.
4. Constructor → Normal functions can be used with `new`, Arrow cannot.
5. Methods → Avoid arrow for object methods (they don’t bind `this`).
6. Prototype → Normal functions have a prototype, Arrow functions don’t.

```js
// Example: No `arguments` in arrow
function normalFn(a, b) {
  console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
}
normalFn(1, 2);

const arrowFn = (a, b) => {
  // console.log(arguments); // ❌ Error: arguments is not defined
};
arrowFn(1, 2);

// Example: Cannot use as constructor
function Person(name) {
  this.name = name;
}
const p1 = new Person("Alice"); // Works

const PersonArrow = (name) => {
  this.name = name;
};
// const p2 = new PersonArrow("Bob"); // ❌ TypeError: PersonArrow is not a constructor
```

# When NOT to use Arrow Functions

- ❌ As object methods (because they don’t have their own `this`)
- ❌ As constructors
- ❌ When we need `arguments` object

**✅ Great for:**

- Callbacks (setTimeout, map, filter, etc.)
- Short inline functions
- Preserving `this` in nested functions

```js
// Example: setTimeout with arrow
function Timer() {
  this.seconds = 0;
  setInterval(() => {
    this.seconds++;
    console.log(this.seconds);
  }, 1000);
}
const t = new Timer();
// (works because arrow inherits `this` from Timer)
```

# ❓ Interview Q&A

**Q1) What is the main difference between normal and arrow functions?**
👉 Normal functions have their own `this`; arrow functions inherit from surrounding scope.

**Q2) Do arrow functions have `arguments`?**
👉 No, they don’t. You can use rest parameters `(...args)` instead.

**Q3) Can arrow functions be used as constructors?**
👉 No, they cannot. Only normal functions or classes can be used with `new`.

**Q4) Why were arrow functions introduced?**
👉 To provide shorter syntax and fix common issues with `this` in callbacks.

**Q5) When should you avoid using arrow functions?**
👉 As object methods, constructors, or when you need `arguments` object.

**Q6) Why are arrow functions good for callbacks?**
👉 Because they automatically capture `this` from the outer scope, avoiding the need for `.bind(this)`.
