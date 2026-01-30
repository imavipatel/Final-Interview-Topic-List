# Curring

- It is advance technique to working with function.
- Currying used to transform function from complex to simple form.
- Currying doesn't call function just transform it.

# What is Currying?

Currying = transforming a function with multiple arguments into a sequence of functions, each taking ONE argument.

```js
// Example (non-curried):
f(a, b, c);

// Curried version:
f(a)(b)(c);
```

**Why? → Helps with code reusability, partial application, functional style.**

# Basic Currying Example

```js
function add(a, b) {
  return a + b;
}
console.log(add(2, 3)); // 5

// Curried version
function curriedAdd(a) {
  return function (b) {
    return a + b;
  };
}
console.log(curriedAdd(2)(3)); // 5

// Partial application possible:
const add5 = curriedAdd(5);
console.log(add5(10)); // 15
```

# Advanced Currying implementation (Generic Currying Function)

```js
// Helper to curry any function with fixed number of args
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args); // enough args → call original
    } else {
      return (...next) => curried(...args, ...next);
    }
  };
}

// With this

function curry(fn) {
  return function curried(...args) {
    // If enough arguments are collected
    if (args.length >= fn.length) {
      // Call original function with correct `this`
      return fn.apply(this, args);
    }

    // Otherwise return a function to collect more arguments
    return function (...nextArgs) {
      return curried.apply(this, args.concat(nextArgs));
    };
  };
}

//With simple undestandable function

function curry(originalFunction) {
  // This function keeps collecting arguments
  function collectArgs(...receivedArgs) {
    // If we already have enough arguments
    if (receivedArgs.length >= originalFunction.length) {
      // Call the original function with all arguments
      return originalFunction(...receivedArgs);
    }

    // Otherwise, return a new function to collect more arguments
    return function getMoreArgs(...newArgs) {
      // Combine old arguments + new arguments
      return collectArgs(...receivedArgs, ...newArgs);
    };
  }

  // Start collecting arguments
  return collectArgs;
}

function sum(a, b, c) {
  return a + b + c;
}

const curriedSum3 = curry(sum);

console.log(curriedSum(1)(2)(3)); // 6
console.log(curriedSum(1, 2)(3)); // 6
console.log(curriedSum(1)(2, 3)); // 6
```

# Infinite Currying

- Infinite currying allows chaining calls endlessly until a "terminator" condition is met. (commonly when no argument is passed)

```js
function infiniteSum(a) {
  return function (b) {
    if (b !== undefined) {
      return infiniteSum(a + b); // keep chaining
    }
    return a; // stop when called with no arg
  };
}

console.log(infiniteSum(1)(2)(3)(4)()); // 10
console.log(infiniteSum(5)(10)(15)()); // 30
```

# Practical Uses of Currying

```js
// 1) Function Reuse (specialization)
function multiply(a, b) {
  return a * b;
}
const curriedMultiply = curry(multiply);
const double2 = curriedMultiply(2);
const triple2 = curriedMultiply(3);

console.log(double2(10)); // 20
console.log(triple2(10)); // 30

// 2) Event Handling (browser)
function handleEvent(eventType) {
  return function (elementId) {
    return function (callback) {
      // In browser only:
      // document.getElementById(elementId).addEventListener(eventType, callback);
      console.log(`Event '${eventType}' bound to #${elementId}`);
    };
  };
}

handleEvent("click")("btnSubmit")(() => console.log("Submitted!"));
// Output: Event 'click' bound to #btnSubmit
```

# ❓ Interview Q&A

**Q1) What is currying?**
→ Converting a function with multiple arguments into a series of functions that each take one argument.

((Q2) Why is currying useful?))
→ Code reuse, partial application, functional composition, readability.

**Q3) Difference between currying and partial application?**
→ Currying transforms function to unary chain (f(a)(b)(c)). Partial application fixes some arguments but keeps others open (f(1, \_, 3)).

**Q4) What is infinite currying?**
→ Currying where function keeps returning another function until a stopping condition (like empty call or coercion).

**Q5) Real-world use cases of currying?**
→ Event binding, logging utilities, mathematical function reuse, API call builders (e.g., createRequest("GET")("users")).
