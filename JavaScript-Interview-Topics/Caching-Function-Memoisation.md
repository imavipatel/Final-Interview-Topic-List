# 10) Caching Functions (Memoization)

---

## 📌 Part 1: What is Memoization?

👉 **Memoization** is an optimization technique used to speed up functions by  
**caching (remembering)** their previously computed results.

### 💡 Idea

- Expensive function calls (recursion, heavy calculations, API calls)
- Avoid recomputation for the **same input**
- Store result in a **cache** (`Object`, `Map`, `WeakMap`)
- Same input again → **return cached result**

### ✅ Pros

- Improves performance

### ❌ Cons

- Increases memory usage (cache storage)

---

## 🐢 Part 2: Example Without Memoization

```js
function slowSquare(n) {
  console.log("Computing...");
  return n * n;
}

console.log(slowSquare(5)); // Computing... 25
console.log(slowSquare(5)); // Computing... 25 (recomputed every time!)
```

# Part 3: Memoization Function

```js
function memoize(fn) {
  let cache = {}; // store results
  return function (n) {
    if (cache[n] !== undefined) {
      console.log("Returning from cache...");
      return cache[n];
    }
    console.log("Computing and caching...");
    cache[n] = fn(n);
    return cache[n];
  };
}

// Wrap slow function with memoize
let fastSquare = memoize(slowSquare);

console.log(fastSquare(5)); // Computing and caching... 25
console.log(fastSquare(5)); // Returning from cache... 25
```

# Part 4: Practical Example – Fibonacci (Huge Performance Boost)

```js
// ❌ Normal recursive Fibonacci (slow for large n)
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
console.log(fib(10)); // 55 (many repeated calls!)

// ✅ Optimized with memoization
function memoizeFib(fn) {
  let cache = {};
  return function f(n) {
    if (n in cache) return cache[n];
    if (n <= 1) return n;
    cache[n] = f(n - 1) + f(n - 2);
    return cache[n];
  };
}

let fastFib = memoizeFib(fib);
console.log(fastFib(40)); // much faster than normal recursion
```

# Part 5: Advanced Notes

**1) Cache Keys**

- In above example, cache works only for single number `n`.
- For multiple arguments, we can use JSON.stringify(args) as cache key.

**2) Cache Storage**

- Object → simple, works fine.
- Map → better for complex keys.
- WeakMap → avoids memory leaks (garbage collected if object keys not used).

**3) Tradeoff**

- Memory usage increases if too many results stored.
- Need cache eviction strategies (LRU, TTL) in real-world.

# Part 6: Real-World Uses

- API calls (avoid making same request multiple times).
- Recursion heavy tasks (like Fibonacci, DP problems).
- CPU-heavy math calculations.
- UI rendering optimizations (React memoization).

# ❓ Interview Q&A

**Q1) What is memoization?**
👉 Technique to cache function results to avoid recomputation.

**Q2) Difference between caching and memoization?**
👉 Caching = generic storage for data.
👉 Memoization = caching specific to function calls.

**Q3) Where is memoization useful?**
👉 Recursion (Fibonacci, factorial), expensive calculations, repeated function calls.

**Q4) What are limitations?**
👉 High memory usage, not useful if inputs are always unique.

**Q5) Difference between Object cache vs Map vs WeakMap?**
👉 Object/Map: store results manually.
👉 WeakMap: allows garbage collection of unused keys (prevents memory leaks).
