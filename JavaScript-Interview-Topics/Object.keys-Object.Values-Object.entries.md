# 📘 Object Utility Methods in JavaScript

🔹 THEORY:
JavaScript provides built-in methods to work with objects:

- `Object.keys(obj)` → returns an array of object’s keys.
- `Object.values(obj)` → returns an array of object’s values.
- `Object.entries(obj)` → returns an array of [key, value] pairs.
- `Object.hasOwnProperty(key)` → checks if the object has a property (not inherited).

These methods are very useful for iteration, manipulation, and safe property checks.

# ❓ Q&A

**Q1: Difference between Object.keys() and Object.entries()?**
👉 `Object.keys()` returns only keys.  
👉 `Object.entries()` returns both keys & values as pairs.

**Q2: Can Object.values() preserve order?**  
👉 Yes, the order is the same as the property definition order.

**Q3: Why use hasOwnProperty()?**  
👉 To avoid accessing inherited properties from prototype chain.

**⭐ QUICK SUMMARY**

- `Object.keys(obj)` → Array of keys
- `Object.values(obj)` → Array of values
- `Object.entries(obj)` → Array of [key, value] pairs
- # `obj.hasOwnProperty("key")` → true/false (checks direct property)

# 🔹 Example Object

```js
const user = { id: 1, name: "Alice", age: 25 };

// 🔹 Object.keys()

console.log("Object.keys():", Object.keys(user));
// ["id", "name", "age"]

Object.keys(user).forEach((key) => {
  console.log(key, ":", user[key]);
});
// id : 1
// name : Alice
// age : 25

// 🔹 Object.values()

console.log("Object.values():", Object.values(user));
// [1, "Alice", 25]

const scores = { math: 90, science: 85, english: 88 };
const total = Object.values(scores).reduce((a, b) => a + b, 0);
console.log("Total Score:", total);
// 263

// 🔹 Object.entries()

console.log("Object.entries():", Object.entries(user));
// [["id", 1], ["name", "Alice"], ["age", 25]]

for (const [key, value] of Object.entries(user)) {
  console.log(`${key}: ${value}`);
}
// id: 1
// name: Alice
// age: 25

// 🔹 Object.hasOwnProperty()

console.log("Has property 'name'?", user.hasOwnProperty("name"));
// true

console.log("Has property 'toString'?", user.hasOwnProperty("toString"));
// false (comes from Object prototype)

if (user.hasOwnProperty("age")) {
  console.log("Age is present");
}
```
