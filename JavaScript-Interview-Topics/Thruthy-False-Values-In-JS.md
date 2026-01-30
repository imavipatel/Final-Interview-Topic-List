# Truthy & Falsy values

**🔹 Falsy** → false, 0, "", null, undefined, NaN
**🔹 Everything else** → truthy

```js
if ("hello") console.log("Truthy"); // ✅ runs
if (0) console.log("Falsy"); // ❌ doesn’t run

console.log(Boolean("hi")); // true
console.log(Boolean("")); // false
```
