# Generator functions

# What are Generator Functions?

- Generator functions are **special functions** that can be paused and resumed.
- Declared with: `function*` (note the `*`).
- They return a **Generator Object** (iterator).

# ✅ Key features:

- Use `yield` keyword to pause execution.
- Calling `.next()` resumes execution from the last yield.
- Each `.next()` returns an object: { value: X, done: boolean }.
- `done: true` → generator is finished.

# Basic Example

```js
function* genNumbers() {
  yield 1; // pause here
  yield 2; // pause here
  yield 3; // pause here
}

let g = genNumbers();

console.log(g.next()); // { value: 1, done: false }
console.log(g.next()); // { value: 2, done: false }
console.log(g.next()); // { value: 3, done: false }
console.log(g.next()); // { value: undefined, done: true }
```

# Iterating Generators

```js
// Generator objects are iterable → can use for...of
for (let num of genNumbers()) {
  console.log(num); // 1, 2, 3
}
```

# Infinite Generators

```js
// Generators don’t need to finish → useful for infinite sequences
function* naturalNumbers() {
  let n = 1;
  while (true) {
    yield n++;
  }
}

let numbers2 = naturalNumbers();
console.log(numbers2.next().value); // 1
console.log(numbers2.next().value); // 2
console.log(numbers2.next().value); // 3
// … continues infinitely
```

# Passing Values to Generators

```js
// 👉 We can send data back into generator using next(arg)
function* greeter() {
  let name = yield "What is your name?";
  yield `Hello, ${name}!`;
}

let greet = greeter();
console.log(greet.next()); // { value: "What is your name?", done: false }
console.log(greet.next("Alice")); // { value: "Hello, Alice!", done: false }
console.log(greet.next()); // { value: undefined, done: true }
```

# Generators vs Normal Functions

- ✅ Normal Function → runs completely, cannot be paused.
- ✅ Generator Function → runs step by step, can pause/resume with yield.

```js
// Example:
function normalFn() {
  return 42;
}
console.log(normalFn()); // 42 (no pause possible)

function* generatorFn() {
  yield 10;
  yield 20;
}
let it1 = generatorFn();
console.log(it1.next()); // { value: 10, done: false }
console.log(it1.next()); // { value: 20, done: false }
```

# Real-World Uses of Generators

1. Lazy Iteration (generate values on demand).
2. Infinite sequences (like streams, numbers).
3. Asynchronous control flow (before async/await, generators + promises used).
4. Data pipelines (step-by-step processing of data).

```js
// Example: Generating Fibonacci sequence lazily
function* fibonacci() {
  let a = 0,
    b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

let fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
console.log(fib.next().value); // 3
console.log(fib.next().value); // 5
```

# ❓ Interview Q&A

**Q1) What is a generator function?**
A function declared with `function*` that can pause (yield) and resume.

**Q2) What does yield do?**
Pauses execution, returns a value, resumes when `.next()` is called.

**Q3) Difference between normal functions and generators?**
Normal functions run fully; generators run step-by-step and can pause.

**Q4) What are real-world use cases?**
Lazy evaluation, infinite sequences, async flow control, data pipelines.

**Q5) Can generators be iterated with for...of?**
Yes, generators are iterable and support `for...of`, spread, etc.
