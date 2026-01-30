# Function Declaration vs Function Expression

# Function Declaration

- Function Declaration = defining a function with the `function` keyword
- It must have a **name**.
- It is **hoisted** → we can call the function **before** its definition.

```js
// Syntax:
function greet() {
  console.log("Hello from a function declaration!");
}

// Usage:
greet(); // "Hello from a function declaration!"

// Hoisting Example:
sayHi(); // ✅ Works, even though defined later
function sayHi() {
  console.log("Hi (Declaration is hoisted)");
}
```

# Function Expression

- Function Expression = assigning a function to a variable
- Can be **named** or **anonymous**
- NOT hoisted → must be defined before calling

```js
// Syntax (Anonymous):
const greetExpr = function () {
  console.log("Hello from a function expression!");
};
greetExpr(); // "Hello from a function expression!"

// Syntax (Named Expression):
const greetNamed = function myFunc() {
  console.log("Hello from a NAMED function expression!");
};
greetNamed(); // Works
// myFunc(); // ❌ Error (myFunc is only visible inside its own body)
```

# Part 3: Differences

1. Declaration is hoisted → usable before definition
2. Expression is not hoisted → usable only after definition
3. Expression can be anonymous; Declaration must be named
4. Expression allows functions as values (useful for callbacks, etc.)

# Part 4: Examples in Action

```js
// Example 1: Callback function using Function Expression
setTimeout(function () {
console.log("I run after 1 second (function expression as callback)");
}, 1000);

// Example 2: Passing function as value
function runOperation(fn) {
fn();
}
runOperation(function () {
console.log("Running operation passed as function expression");
});

// Example 3: Named Expression in recursion
const factorial = function fact(n) {
if (n === 0) return 1;
return n \* fact(n - 1); // fact is accessible inside function body
};
console.log(factorial(5)); // 120
```

# ❓ Interview Q&A

**Q1) What is the difference between Function Declaration and Function Expression?**
Declaration is hoisted and must have a name.
Expression is not hoisted, can be anonymous, often used as values.

**Q2) Can Function Declarations be used before their definition?**
Yes, because they are hoisted.

**Q3) Why are Function Expressions useful?**
Useful for callbacks, passing as arguments, or returning from functions.

**Q4) What happens if you name a Function Expression?**
The name is only available inside the function body (good for recursion).

**Q5) Which one is better for recursion – Declaration or Expression?**
Both can be used, but named expressions are useful in recursion without polluting the global scope.
