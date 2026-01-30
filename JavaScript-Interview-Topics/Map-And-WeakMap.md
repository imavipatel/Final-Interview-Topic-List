# 📘 Map & WeakMap in JavaScript

**🔹 THEORY**

**1. Map**

- A `Map` is a collection of **key-value pairs**.
- Unlike objects, **keys can be of any type** (objects, functions, primitives).
- Maintains **insertion order** of elements.
- Provides useful methods like `.set()`, `.get()`, `.has()`, `.delete()`, `.clear()`, `.size`.

**2. WeakMap**

- A `WeakMap` is similar to Map, but **only allows objects as keys**.
- Keys are held **weakly** → if no other reference exists, the object is garbage-collected.
- Cannot be iterated (no `.keys()`, `.values()`, `.entries()`).
- Useful for **private data storage** in objects.

# QUICK SUMMARY

- Map: keys of any type, iterable, size property available.
- # WeakMap: only object keys, no iteration, garbage-collects keys.

**Example 1: Map Basics**

```js
const myMap = new Map();

// Setting values
myMap.set("name", "Alice");
myMap.set(100, "Score");
myMap.set(true, "Boolean Key");

console.log("Map:", myMap);
// Map(3) { 'name' => 'Alice', 100 => 'Score', true => 'Boolean Key' }

// Getting values
console.log("Name:", myMap.get("name")); // Alice
console.log("Has score key?", myMap.has(100)); // true

// Iterating over Map
for (const [key, value] of myMap) {
  console.log(`${key} => ${value}`);
}
// name => Alice
// 100 => Score
// true => Boolean Key

console.log("Map size:", myMap.size); // 3

// Delete and Clear
myMap.delete(100);
console.log("After delete:", myMap);
// Map(2) { 'name' => 'Alice', true => 'Boolean Key' }

myMap.clear();
console.log("After clear:", myMap); // Map(0) {}
```

**🔹 Example 2: WeakMap Basics**

```js
let obj1 = { id: 1 };
let obj2 = { id: 2 };

const myWeakMap = new WeakMap();

// Only objects allowed as keys
myWeakMap.set(obj1, "Object 1 data");
myWeakMap.set(obj2, "Object 2 data");

console.log("Has obj1?", myWeakMap.has(obj1)); // true
console.log("Get obj2:", myWeakMap.get(obj2)); // Object 2 data

// If we set obj1 = null → GC can collect obj1
obj1 = null;
// myWeakMap will automatically remove obj1 reference when GC runs

// WeakMap cannot be iterated
// ❌ for...of not allowed
// ❌ myWeakMap.keys() not available
```

**🔹 Example 3: Private Data with WeakMap**

```js
const privateData = new WeakMap();

class Person {
  constructor(name, age) {
    privateData.set(this, { age });
    this.name = name;
  }

  getAge() {
    return privateData.get(this).age;
  }
}

const p1 = new Person("Bob", 30);
console.log(p1.name); // Bob
console.log(p1.getAge()); // 30
console.log(p1.age); // undefined (truly private)
```

# ❓ Q&A (Interview Style)

**Q1: Difference between Map and Object?**  
👉 Objects have only string/symbol keys, Maps allow any type of key.

**Q2: Why use WeakMap? **
👉 To store data tied to objects without preventing garbage collection.

**Q3: Can WeakMap be iterated?**
👉 No. It's intentionally non-enumerable for security & GC reasons.

**Q4: When to use Map vs WeakMap? **
👉 Use `Map` when you need iteration, use `WeakMap` when you need memory-efficient private storage.
