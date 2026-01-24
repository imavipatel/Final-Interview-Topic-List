## Pipe and Compose function

Functional programming often shines in JavaScript when you need to handle complex data flows with cleaner and more readable code. Two essential tools in this paradigm are the compose() and pipe() functions, commonly used to combine multiple functions into a single, seamless operation.

**Compose Function**

- compose() is a higher-order function that takes multiple functions as arguments and returns a new function.
- compose() executes functions **right-to-left**.
- Result of one function becomes input for the previous.
- Useful for **functional programming** to combine multiple operations.

**Compose Polyfills**

To build a **compose() polyfill**, we chain functions in reverse order, where the result of one function serves as the input for the next.

```js
const multiplyBy2 = (x) => x * 2;
const add3 = (x) => x + 3;
const square = (x) => x * x;

// Compose utility Using Arrow Function
const compose =
  (...fns) =>
  (value) =>
    fns.reduceRight((acc, fn) => fn(acc), value);

const composedFn = compose(square, add3, multiplyBy2);
console.log(composedFn(5));

// Compose utility Using Normal Function

function compose(...funcs) {
  return function (initialValue) {
    return funcs.reduceRight(
      (accumulator, func) => func(accumulator),
      initialValue,
    );
  };
}

const composedFn = compose(square, add3, multiplyBy2);
console.log(composedFn(5));
```

**Explanation:**
**...funcs:** Collects all arguments into an array of functions.

**reduceRight():** Processes functions in reverse order, starting from the rightmost and moving to the left.

**func(accumulator):** Applies each function to the accumulated value.

**Pipe Function**

- pipe() executes functions **left-to-right**.
- Makes code easier to read in sequential steps.

**Pipe Polyfills**

For pipe(), the logic is almost identical to compose(), but we process functions in the opposite order(**Left to right**).

```js
// Compose utility Using Arrow Function

const pipe =
  (...fns) =>
  (value) =>
    fns.reduce((acc, fn) => fn(acc), value);

const pipedFn = pipe(multiplyBy2, add3, square);
console.log(pipedFn(5));

// Compose utility Using Normal Function

function pipe(...funcs) {
  return function (initialValue) {
    return funcs.reduce((accumulator, func) => func(accumulator), initialValue);
  };
}

const pipedFn = pipe(multiplyBy2, add3, square);
console.log(pipedFn(5));

// Step by step:
// multiplyBy2(5) => 10
// add3(10) => 13
// square(13) => 169
// ✅ Output: 169
```

**Explanation:**

**reduce():** Processes functions from left to right.

## Real-World Examples

**Example 1: Data Transformation**

Suppose you need to transform user data by capitalizing names and appending a greeting.

```js
const capitalize = (name) => name.toUpperCase();
const greet = (name) => `Hello, ${name}!`;

const composedGreeting = compose(greet, capitalize);

console.log(composedGreeting("john")); // Output: "Hello, JOHN!"
```

**Example 2: Chaining API Data**

For API responses, you might need to extract data, transform it, and calculate totals.

```js
const parseResponse = (response) => response.data;
const calculateTotal = (data) =>
  data.reduce((sum, item) => sum + item.price, 0);

const processApiData = pipe(parseResponse, calculateTotal);
const response = { data: [{ price: 10 }, { price: 20 }, { price: 30 }] };
console.log(processApiData(response)); // Output: 60
```

**🔑 Key Points Summary**

1. compose → right-to-left execution of functions
2. pipe → left-to-right execution
3. Both are used in **functional programming** to avoid nested calls
4. Helps **write cleaner and reusable code**
5. Functions must be **pure** for consistent results

## Q & A

**Q1) Difference between pipe and compose?**
compose: right-to-left, pipe: left-to-right execution

**Q2) Can we use compose/pipe with async functions?**
Yes, but need extra handling (e.g., promises or async/await)

**Q3) Why use pipe/compose instead of nested function calls?**
For readability, maintainability, and cleaner code.

**Q4) Can pipe/compose accept any number of functions?**
Yes, you can pass any number of functions using rest parameters.

**Q5) Do functions in compose/pipe modify the original input?**
Ideally not. They should be pure functions to avoid side-effects.
