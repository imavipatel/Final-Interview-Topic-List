# Memory Allocation & Garbage Collection

**Where does data live?**

- Primitives (numbers, strings, booleans, null, undefined, symbols, bigint):
  → stored as values (usually on the stack or inline).
- Objects/arrays/functions:
  → stored in the heap. Variables hold references to these objects.

# Garbage Collection (GC):

- JS automatically frees memory of unreachable objects.
- "Reachability" = can the object still be reached from the "roots" (like global variables or current function variables)?
- If not → it is garbage.

```js
let obj = { big: new Array(1_000_000).fill("x") };
obj = null; // Now unreachable → GC can reclaim the memory
```

# Types of Garbage Collection & Memory Leaks

# 🔹 Common GC algorithms:

**1. Mark-and-Sweep** → mark reachable objects, sweep the rest.
**2. Generational GC** → most objects die young, so memory is divided into Young/Old generations.
**3. Incremental/Concurrent GC** → do collection in small steps to avoid long pauses.
**4. Compaction** → move survivors together to reduce fragmentation.

# 🔹 Memory Leaks:

When you accidentally keep references alive so GC cannot clean up.

**Examples:**

1. Accidental globals
2. Unstopped timers/intervals
3. Event listeners not removed
4. Detached DOM nodes
5. Large caches (Map/Object) without cleanup
6. Closures capturing large unused data

```js
function leakExample() {
  leaked = "I am global"; // ❌ no var/let/const → becomes global
}
leakExample();
// fix: use 'use strict' + let/const

// ❌ Interval leak
const id = setInterval(() => {
  console.log("leaking...");
}, 1000);
// fix: clearInterval(id);

// ❌ Event listener leak
function attach() {
  const big = new Array(1_000_000).fill("x");
  const handler = () => console.log(big.length);
  window.addEventListener("resize", handler);
  // fix: window.removeEventListener('resize', handler);
}
```

**Practice Question 1:**

```js
console.log("start");
setTimeout(() => console.log("timeout"), 0);
Promise.resolve().then(() => console.log("promise"));
console.log("end");
// Expected: start → end → promise → timeout

/*
Practice Question 2 (Memory Leak):
*/
function leak() {
  const big = new Array(1_000_000).fill("x");
  const handler = () => console.log(big.length);
  window.addEventListener("resize", handler);
}
leak();
// Problem: event listener keeps 'big' in memory.
// Fix: window.removeEventListener('resize', handler);
```
