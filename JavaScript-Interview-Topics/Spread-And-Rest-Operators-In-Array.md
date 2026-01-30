# 📘 Spread & Rest Operators in Arrays

**🔹 THEORY (Easy Language)**

- Both use the same `...` syntax but behave differently depending on context.
- **Spread Operator (`...`)** → "expands" or spreads array elements into individual items.
- **Rest Operator (`...`)** → "collects" multiple values into a single array.

Think:  
👉 Spread = "Expand"  
👉 Rest = "Collect"

# SPREAD OPERATOR (Expand Array)

```js
let numbers = [1, 2, 3];
console.log(...numbers); // 1 2 3  (spread each item)

let newArr = [0, ...numbers, 4];
console.log(newArr); // [0,1,2,3,4]
```

# 2️⃣ COPY ARRAYS WITH SPREAD

```js
let arr1 = [10, 20, 30];
let arr2 = [...arr1]; // copy
arr2.push(40);

console.log(arr1); // [10,20,30] (unchanged)
console.log(arr2); // [10,20,30,40]
```

# 3️⃣ MERGE ARRAYS

```js
let a = [1, 2];
let b = [3, 4];
let merged = [...a, ...b];
console.log(merged); // [1,2,3,4]
```

# 4️⃣ REST OPERATOR (Collect Items)

```js
let [first, ...rest] = [10, 20, 30, 40];
console.log(first); // 10
console.log(rest); // [20,30,40]
```

# 5️⃣ REST IN FUNCTIONS (Variable Arguments)

```js
function sum(...nums) {
  return nums.reduce((total, n) => total + n, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

# 6️⃣ SPREAD IN FUNCTIONS

```js
function multiply(x, y, z) {
  return x * y * z;
}
let values = [2, 3, 4];
console.log(multiply(...values)); // 24
```

# 7️⃣ DIFFERENCE BETWEEN SPREAD & REST

- Spread is used when "passing" or "expanding".
- Rest is used when "receiving" or "collecting".
- Both look the same `...` but depend on context.

```js
// Example:
function f(a, ...rest) {} // Rest (collects args)
f(...arr); // Spread (expands array)
```

# Q&A (Interview Style)

**Q1: What is the difference between spread and rest?**
👉 Spread expands items, Rest collects items.

**Q2: Can spread be used for copying arrays?**  
👉 Yes: `let copy = [...arr]`.

**Q3: How to merge arrays using spread?**  
👉 `[...arr1, ...arr2]`.

**Q4: Can rest operator be used in destructuring?**  
👉 Yes: `[a, ...rest] = [1,2,3]`.

**Q5: Where is rest commonly used?**  
👉 In function parameters to collect unlimited arguments.

# 📊 QUICK REVISION

- Spread → expands: `[...arr]`, `fn(...arr)`.
- Rest → collects: `function(...args)`, `[a,...rest]`.
- Spread: copying, merging, passing arrays.
- # Rest: variable args, destructuring.
