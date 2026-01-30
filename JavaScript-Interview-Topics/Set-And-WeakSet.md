# 📘 Set & WeakSet in JavaScript

**🔹 THEORY**

# 1. Set

- A `Set` is a **collection of unique values**.
- It removes duplicates automatically.
- Values can be of any type (primitive or object).
- Insertion order is preserved.

**✅ Use cases:**

- Removing duplicates from arrays.
- Checking if an item exists (faster than arrays).
- Performing set operations (union, intersection, difference).

# 2. WeakSet

- A `WeakSet` is similar to `Set`, but:
  - Can only store **objects** (not primitives).
  - Holds **weak references** → objects can be garbage collected if no other reference exists.
  - Does **not support iteration** (no `for...of`, no `size`, no `keys()`).
- Useful for memory-sensitive cases, like tracking objects without preventing garbage collection.

**✅ Use cases:**

- Keeping track of object states (e.g., whether an object has been processed).
- Avoiding memory leaks when working with temporary objects.

# ⭐ QUICK SUMMARY

- `Set` = Unique collection, iterable, works with all types.
- # `WeakSet` = Unique collection, only objects, no iteration, memory-efficient.

**🔹 Example 1: Basic Set usage**

```js
const mySet = new Set([1, 2, 2, 3, 4, 4, 5]);
console.log(mySet); // Set { 1, 2, 3, 4, 5 }

// Add values
mySet.add(6);
mySet.add(3); // Duplicate ignored
console.log(mySet); // Set { 1, 2, 3, 4, 5, 6 }

// Check existence
console.log(mySet.has(3)); // true
console.log(mySet.has(10)); // false

// Delete values
mySet.delete(2);
console.log(mySet); // Set { 1, 3, 4, 5, 6 }

// Iterate
for (const value of mySet) {
  console.log(value);
}

// Size
console.log(mySet.size); // 5

// Convert back to array
const arr = [...mySet];
console.log(arr); // [1, 3, 4, 5, 6]
```

**Example 2: Removing duplicates from an array**

```js
const numbers = [1, 2, 2, 3, 4, 4, 5];
const uniqueNumbers = [...new Set(numbers)];
console.log(uniqueNumbers); // [1, 2, 3, 4, 5]
```

**Example 3: Set operations (Union, Intersection, Difference)**

```js
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);

// Union
const union = new Set([...setA, ...setB]);
console.log(union); // Set { 1, 2, 3, 4, 5 }

// Intersection
const intersection = new Set([...setA].filter((x) => setB.has(x)));
console.log(intersection); // Set { 3 }

// Difference
const difference = new Set([...setA].filter((x) => !setB.has(x)));
console.log(difference); // Set { 1, 2 }
```

# 🔹 Example 4: WeakSet basics

```js
const weakSet = new WeakSet();

let obj1 = { name: "Alice" };
let obj2 = { name: "Bob" };

weakSet.add(obj1);
weakSet.add(obj2);
console.log(weakSet.has(obj1)); // true

weakSet.delete(obj2);
console.log(weakSet.has(obj2)); // false

// obj1 = null; // Now obj1 is eligible for garbage collection
// weakSet will automatically remove it internally

// ❌ NOT allowed:
// weakSet.add(1); // Error: Invalid value used in weak set
// console.log(weakSet.size); // undefined
// for (let item of weakSet) {} // TypeError: weakSet is not iterable
```

# ❓ Q&A (Interview Style)

**Q1: Difference between Set and WeakSet?**
👉 - `Set`: Works with any values, iterable, has size, supports keys/values.

- `WeakSet`: Only objects, no iteration, no size, garbage collection-friendly.

**Q2: Why use Set over Array?**
👉 - Faster lookup (`O(1)` vs `O(n)`).

- Automatically removes duplicates.

**Q3: Why use WeakSet?**
👉 - When you want to store object references **without preventing garbage collection**.

- Good for caching or tracking objects temporarily.

**Q4: Can WeakSet store primitives?**
👉 No, it only stores objects.

**Q5: Common real-world use cases of Set?**
👉 - Deduplication of arrays.

- Performing union/intersection/difference in sets.
- Tracking unique user IDs in a system.

**Q6: Common real-world use cases of WeakSet?**
👉 - Keeping track of DOM nodes.

- Storing metadata about objects without modifying them.
- Preventing memory leaks in cache-like structures.

---

## 📊 Comparison Table (Quick Revision)

| Feature         | Set                              | WeakSet                               |
| --------------- | -------------------------------- | ------------------------------------- |
| Data Type       | Any value (primitives + objects) | Only objects                          |
| Iteration       | Yes (`for...of`, `forEach`)      | No                                    |
| Size Property   | Yes (`size`)                     | No                                    |
| Garbage Collec. | No                               | Yes (weak references auto cleaned)    |
| Use Cases       | Deduplication, set operations    | Tracking objects, memory-safe caching |

```

```
