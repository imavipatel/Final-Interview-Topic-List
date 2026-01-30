# Shallow Copy vs Deep Copy & Deep Cloning Techniques

## **📝 THEORY**

- Copying an object/array can be done in **two ways**:
  1. **Shallow Copy**
     - Creates a new object/array.
     - But **nested objects/arrays are still references** to the original.
     - Changes in nested values affect both copies.

  2. **Deep Copy**
     - Creates a new object/array with completely new copies of all nested objects.
     - No references to the original object remain.
     - Fully independent.

⚠️ JavaScript's default methods like spread (`...`) or `Object.assign()` create **shallow copies**.
To achieve deep copies, extra techniques are required.

---

```js
/*
-----------------------------------------------------
🔹 PART 1:  Shallow Copy
-----------------------------------------------------
*/

const original = {
  name: "Avi",
  details: { city: "Ahmedabad" },
};

// Using spread operator (shallow copy)
const shallowCopy = { ...original };

// Modify nested object
shallowCopy.details.city = "Mumbai";

console.log(original.details.city); // "Mumbai" ❌ original affected
console.log(shallowCopy.details.city); // "Mumbai"

// 👉 Problem: Nested objects are shared references.

/*
-----------------------------------------------------
🔹 PART 2: Deep Copy
-----------------------------------------------------
*/

// ✅ Technique 1: JSON.parse(JSON.stringify())

const deepCopy1 = JSON.parse(JSON.stringify(original));

deepCopy1.details.city = "Delhi";

console.log(original.details.city); // "Mumbai" ✅ not affected
console.log(deepCopy1.details.city); // "Delhi"

/*

⚠️ Limitations of JSON method:

- Loses functions, Date objects, undefined, Symbol.
- Not suitable for complex objects with circular references.
*/

// ✅ Technique 2: Structured Clone (modern browsers / Node.js 17+)

const deepCopy2 = structuredClone(original);
deepCopy2.details.city = "Kolkata";

console.log(original.details.city); // "Mumbai" ✅ not affected
console.log(deepCopy2.details.city); // "Kolkata"

// ✅ Technique 3: Recursion (Custom Deep Clone Function)

function deepClone(obj) {
  if (obj === null || typeof obj !== "object") {
    return obj; // primitive values
  }
  if (Array.isArray(obj)) {
    return obj.map((item) => deepClone(item));
  }

  const clonedObj = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      clonedObj[key] = deepClone(obj[key]); // recursive
    }
  }
  return clonedObj;
}

const deepCopy3 = deepClone(original);
deepCopy3.details.city = "Pune";

console.log(original.details.city); // "Mumbai" ✅ not affected
console.log(deepCopy3.details.city); // "Pune"
/*

✅ Technique 4: Using Libraries
// Lodash: \_.cloneDeep(object)
// Immutable.js: Provides immutable data structures

*/
```

# Q&A (Interview Style)

**Q1: What is the difference between Shallow Copy and Deep Copy?**
👉 - Shallow Copy → Only first level is copied, nested objects are references.

- Deep Copy → Entire object is cloned, including nested objects.

**Q2: Which methods create Shallow Copy in JS?**
👉 - Object.assign(), Spread operator `{ ...obj }`, Array methods like slice(), concat().

**Q3: How do you Deep Copy in JavaScript?**
👉 - JSON.parse(JSON.stringify(obj)) → simple deep copy.

- structuredClone(obj) → modern safe deep copy.
- Custom recursion function.
- Libraries like Lodash’s `_.cloneDeep`.

**Q4: Why not always use JSON.stringify for Deep Copy?**
👉 - Fails with Date, RegExp, undefined, functions, Symbols, circular references.

**Q5: Which method is best for Deep Copy?**
👉 - structuredClone() (modern environments).

- Lodash `_.cloneDeep` (cross-browser, safe).

# ⭐ QUICK SUMMARY:

- Shallow Copy: One level copy, nested refs remain shared.
- Deep Copy: Full independent clone.
- # Techniques: JSON, structuredClone, recursion, libraries.

## 🔹 PART 4: Shallow vs Deep Copy in Arrays

```js
// ✅ Shallow Copy Examples
const arr = [1, 2, [3, 4]];

// Using slice() → Shallow Copy
const shallowArr1 = arr.slice();
shallowArr1[2][0] = 99;

console.log(arr[2][0]); // 99 ❌ original affected
console.log(shallowArr1[2][0]); // 99

// Using spread operator → Shallow Copy
const shallowArr2 = [...arr];
shallowArr2[2][1] = 88;

console.log(arr[2][1]); // 88 ❌ original affected
console.log(shallowArr2[2][1]); // 88

// ✅ Deep Copy Examples
// Method 1: JSON Method
const deepArr1 = JSON.parse(JSON.stringify(arr));
deepArr1[2][0] = 77;

console.log(arr[2][0]); // 99 ✅ original not affected
console.log(deepArr1[2][0]); // 77

// Method 2: Recursive Deep Clone Function
function deepCloneArray(array) {
  return array.map((item) =>
    Array.isArray(item) ? deepCloneArray(item) : item,
  );
}

const deepArr2 = deepCloneArray(arr);
deepArr2[2][1] = 66;

console.log(arr[2][1]); // 88 ✅ original not affected
console.log(deepArr2[2][1]); // 66
```

# ❓ Q&A (Arrays Specific)

**Q1: Does `slice()` or `concat()` create a deep copy of arrays?**
👉 No, they create a shallow copy. Nested arrays/objects remain referenced.

**Q2: How can you create a deep copy of an array?**
👉 Use JSON.parse(JSON.stringify(array)), recursion, or structuredClone().

**Q3: What happens if you use spread (`[...arr]`) on nested arrays?**
👉 Only first level is copied. Nested arrays are still referenced.

# ⭐ QUICK SUMMARY (Arrays):

- Shallow Copy: slice(), concat(), spread `[...arr]`
- Deep Copy: JSON, structuredClone, recursion
- # Be careful with nested arrays!
