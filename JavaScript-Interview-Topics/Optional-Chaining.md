# 📘 Optional Chaining (?.) in JavaScript

**🔹 THEORY**

- Optional Chaining (`?.`) is a **safe way to access nested properties** in objects.
- If the property doesn’t exist, instead of throwing an error (`Cannot read property ...`), it simply returns `undefined`.
- Works for:
  1. Accessing **object properties** → `obj?.prop`
  2. Accessing **deeply nested properties** → `obj?.prop?.subprop`
  3. Calling **functions/methods** → `obj?.method?.()`
  4. Accessing **array elements** → `arr?.[index]`

✅ Why use it?

- Prevents runtime errors.
- Makes code cleaner (no long `if` or `&&` checks).
- Especially useful in **APIs** or **optional data structures**.

# ⭐ QUICK SUMMARY

- `?.` stops evaluation and returns `undefined` if the value is nullish (`null` or `undefined`).
- # Can be combined with **nullish coalescing (`??`)** for defaults.

**🔹 Example 1: Without optional chaining (error prone)**

```js
const user = {
  name: "Alice",
  address: {
    city: "London",
  },
};

// Accessing nested property
console.log(user.address.city); // "London"

// Problem: property doesn't exist
// console.log(user.contact.phone); // ❌ Error: Cannot read property 'phone'
```

**🔹 Example 2: With optional chaining**

```js
console.log(user.contact?.phone); // ✅ undefined (no error)
console.log(user.address?.city); // "London"

// Deep nesting
console.log(user.profile?.social?.twitter); // undefined safely
```

**🔹 Example 3: Optional chaining with functions**

```js
const person = {
  sayHello: () => "Hello!",
};

console.log(person.sayHello?.()); // "Hello!"
console.log(person.sayBye?.()); // undefined (no error)
```

**🔹 Example 4: Optional chaining with arrays**

```js
const users = [{ name: "Alice" }, null, { name: "Bob" }];

console.log(users?.[0]?.name); // "Alice"
console.log(users?.[1]?.name); // undefined (safe)
```

**🔹 Example 5: Combining with Nullish Coalescing (??)**

```js
const settings = {
  theme: {
    color: null,
  },
};

// If theme.color is null, fallback to "blue"
const color = settings.theme?.color ?? "blue";
console.log(color); // "blue"
```

# ❓ Q&A (Interview Style)

**Q1: What is optional chaining (?.) used for?**
👉 To safely access nested object properties, methods, or arrays without throwing errors if the property doesn’t exist.

**Q2: What does optional chaining return if property doesn’t exist?**
👉 It returns `undefined`.

**Q3: Difference between `?.` and `&&` (logical AND)?**
👉 - `obj && obj.prop` checks if `obj` exists but is longer.

- `obj?.prop` is shorter, safer, and only checks for `null`/`undefined`.

**Q4: Can optional chaining be used for method calls?**
👉 Yes → `obj?.method?.()`.
If `method` doesn’t exist, it safely returns `undefined`.

**Q5: Can you use optional chaining with arrays?**
👉 Yes → `arr?.[index]`. If array is null/undefined, returns `undefined`.

Q6: Can optional chaining be used for assignment?
👉 No, it can only be used for **reading values**, not writing. Example:

```js
obj?.prop = 5; // ❌ SyntaxError
```
