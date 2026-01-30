# 📘 JavaScript Notes: Loops & Iteration

**Topics covered:**

1.  for / while / do-while
2.  for…of / for…in
3.  for-await-of (async iterables)
4.  Difference between forEach & map
5.  Iterables & Iterators Protocol
6.  Nested loops & performance considerations
7.  break / continue usage
8.  Using labels with loops
9.  Generator functions with iteration (yield)
10. Q&A Section

**1) for / while / do-while**

**- for loop:** Best when you know how many iterations are needed.
**- while loop:** Runs until condition is false, used when iterations unknown.
**- do-while:** Executes at least once before checking condition.

```js
for (let i = 0; i < 3; i++) {
  console.log("for:", i); // 0,1,2
}

let j = 0;
while (j < 3) {
  console.log("while:", j);
  j++;
}

let k = 0;
do {
  console.log("do-while:", k);
  k++;
} while (k < 3);
```

# 2) for…of / for…in

- for…of → iterates over values of iterable (Array, String, Set, Map).
- for…in → iterates over enumerable property keys of an object.

```js
let arr = [10, 20, 30];
for (let val of arr) {
  console.log("for...of value:", val);
}

let obj = { a: 1, b: 2, c: 3 };
for (let key in obj) {
  console.log("for...in key:", key, "value:", obj[key]);
}
```

# 3) for-await-of (async iterables)

- Used to loop over async iterables (Promises, async generators).
- Waits for each Promise to resolve before next iteration.

```js
async function* asyncGenerator() {
  yield Promise.resolve(1);
  yield Promise.resolve(2);
  yield Promise.resolve(3);
}

(async () => {
  for await (let val of asyncGenerator()) {
    console.log("for-await-of:", val);
  }
})();
```

# 4. Difference between forEach & map

- forEach → executes callback for each item but does not return anything.
- map → creates a new array with transformed values.

```js
let numbers = [1, 2, 3];

numbers.forEach((num) => console.log("forEach:", num _ 2)); // Logs only
let doubled = numbers.map((num) => num _ 2);
console.log("map:", doubled); // [2,4,6]

```

# 5) Iterables & Iterators Protocol

- Iterable: An object that implements @@iterator method (Symbol.iterator).
- Iterator: Has next() method returning { value, done }.

```js
let iterable = [10, 20, 30];
let iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 10, done: false }
console.log(iterator.next()); // { value: 20, done: false }
console.log(iterator.next()); // { value: 30, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

# 6) Nested loops & performance considerations

- Nested loops multiply complexity (O(n²)).
- Use cautiously for large data sets.

```js
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 2; j++) {
    console.log(`i=${i}, j=${j}`);
  }
}
```

# 7) break / continue usage

- break → exits loop immediately.
- continue → skips current iteration and moves to next.

```js
for (let i = 0; i < 5; i++) {
  if (i === 2) continue; // skip 2
  if (i === 4) break; // stop loop
  console.log(i); // 0,1,3
}
```

# 8) Using labels with loops

- Labels allow breaking/continuing outer loops directly.

```js
// outer:
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (j === 1) break outer; // breaks outer loop
    console.log(`i=${i}, j=${j}`);
  }
}
```

# 9) Generator functions with iteration (yield)

- Generator: Special function that can pause execution and resume.
- Uses function\* syntax and yield keyword.

```js
function* generatorExample() {
  yield 1;
  yield 2;
  yield 3;
}

let gen = generatorExample();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

# 10) Q&A SECTION

**Q1) Difference between for, while, do-while?**

- for: use when count known.
- while: use when condition-driven.
- do-while: ensures at least one run.

**Q2) Difference between for…of and for…in?**

- for…of: iterates values of iterable.
- for…in: iterates keys of object (also array indexes, but not recommended).

**Q3) When should I use for-await-of?**

- When looping async data sources (e.g., reading from API stream).

**Q4) forEach vs map?**

- forEach: side effects only (logs, DB updates).
- map: transforms and returns new array.

**Q5) What is the iterator protocol?**

- Object with next() returning { value, done }.
- Enables for…of to work with it.

**Q6) Performance of nested loops?**

- Grows as O(n²). Bad for large datasets, prefer better algorithms or hash maps.

**Q7) break vs continue?**

- break: stops entire loop.
- continue: skips iteration but loop continues.

**Q8) Why use labels?**

- Needed when breaking outer loop inside nested loops.

**Q9) What are Generators used for?**

- Pause/resume functions, async control flows, iterators creation.
- Common in state machines, data streams, lazy evaluation.

11. COMMON MISTAKES & GOTCHAS

```js
// ❌ Mistake 1: Using for...in with arrays

let nums = [100, 200, 300];
for (let i in nums) {
  console.log("for...in:", i); // "0", "1", "2" → keys as strings
}
for (let v of nums) {
  console.log("for...of:", v); // 100, 200, 300 → values
}

// Always use for...of for arrays.

// ❌ Mistake 2: Forgetting 'await' in for-await-of

async function badLoop() {
  let promises = [Promise.resolve(1), Promise.resolve(2)];
  for (let p of promises) {
    console.log(await p); // must await here, or use for-await-of
  }
}

// ❌ Mistake 3: Thinking forEach respects async/await

nums.forEach(async (n) => {
  await new Promise((res) => setTimeout(res, 1000));
  console.log("forEach async:", n);
});

// Logs out of order since forEach doesn't await promises.
// ✅ Use for...of with await instead:

(async () => {
  for (let n of nums) {
    await new Promise((res) => setTimeout(res, 1000));
    console.log("for...of async:", n);
  }
})();

// ❌ Mistake 4: Infinite loop risk
// while(true) { ... } without break condition → memory & CPU crash.
// Always ensure exit conditions in loops.

// ❌ Mistake 5: break only exits inner loop

for (let x = 0; x < 3; x++) {
  for (let y = 0; y < 3; y++) {
    if (y === 1) break;
    console.log(`x=${x}, y=${y}`);
  }
}
// If you wanted to break outer loop too → need labels.
```
