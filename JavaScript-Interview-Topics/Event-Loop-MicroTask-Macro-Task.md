# Event Loop, Microtasks, Macrotasks

**JavaScript is single-threaded (only 1 call stack).**
But it can handle async tasks (timers, promises, fetch, etc.)
using:

- Call Stack
- Web APIs (Browser / Node APIs)
- Callback Queue (Macrotask Queue)
- Microtask Queue (higher priority)
- Event Loop

# Part 1: Call Stack & Event Loop

- Call Stack executes all synchronous code first.
- Async tasks (setTimeout, promises, etc.) are sent to Web APIs.
- When finished, their callbacks go into either:

1.  Microtask Queue (Promises, MutationObserver, queueMicrotask)
2.  Macrotask Queue (setTimeout, setInterval, setImmediate, I/O)

- Event Loop continuously checks:

1.  If Call Stack is empty → run Microtasks first.
2.  Then run one Macrotask.
3.  Repeat.

# Part 2: Microtasks vs Macrotasks

**Microtasks (higher priority):**

- Promise callbacks (.then, .catch, .finally)
- MutationObserver
- queueMicrotask()

**Macrotasks:**

- setTimeout
- setInterval
- setImmediate (Node.js)
- DOM events (click, input, etc.)
- I/O operations

Rule: After each Macrotask → all Microtasks are drained before next Macrotask.

# Part 3: Examples

```js
console.log("Start");

// setTimeout → Macrotask
setTimeout(() => {
  console.log("setTimeout callback (Macrotask)");
}, 0);

// Promise → Microtask
Promise.resolve().then(() => {
  console.log("Promise.then callback (Microtask)");
});

// queueMicrotask → Microtask
queueMicrotask(() => {
  console.log("queueMicrotask callback (Microtask)");
});

console.log("End");

// Output order:
// Start
// End
// Promise.then callback (Microtask)
// queueMicrotask callback (Microtask)
// setTimeout callback (Macrotask)
```

# Part 4: Why Microtasks have priority?

- Ensures that promises and critical operations finish ASAP.
- Prevents starving important async tasks (e.g., fetch result processing).

```js
// Example:
setTimeout(() => console.log("Macrotask 1"), 0);

Promise.resolve().then(() => {
  console.log("Microtask 1");
  Promise.resolve().then(() => console.log("Microtask 2"));
});

setTimeout(() => console.log("Macrotask 2"), 0);

// Output order:
// Microtask 1
// Microtask 2
// Macrotask 1
// Macrotask 2
```

# Part 5: Practical Example

```js
// Simulating fetching data + updating UI:
console.log("Fetching user...");

setTimeout(() => {
  console.log("⏳ API call finished (Macrotask)");

  Promise.resolve().then(() => {
    console.log("✅ Processing data (Microtask)");
  });
}, 1000);

console.log("Rendering UI...");

// Possible Output:
// Fetching user...
// Rendering UI...
// ⏳ API call finished (Macrotask)
// ✅ Processing data (Microtask)
```

# ❓ Interview Q&A

**Q1) What is Event Loop?**
👉 Mechanism that coordinates Call Stack, Microtasks, Macrotasks execution.

**Q2) Microtask vs Macrotask?**
👉 Microtasks have higher priority → executed before Macrotasks.

**Q3) Examples of Microtasks?**
👉 Promises, queueMicrotask, MutationObserver.

**Q4) Examples of Macrotasks?**
👉 setTimeout, setInterval, DOM events, I/O.

**Q5) Execution order if both Promise and setTimeout are scheduled?**
👉 Promise (Microtask) runs first, then setTimeout (Macrotask).

**Q6) Can Microtasks starve Macrotasks?**
👉 Yes, if you keep scheduling new Microtasks in a loop, Macrotasks get delayed.
