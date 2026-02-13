# Symbol datatype

🔹 Introduced in ES6
🔹 Used to create unique identifiers
🔹 Even if symbols have the same description, they are unique

```js
const id = Symbol();
console.log(typeof id); // "symbol"

let sym1 = Symbol("id");
let sym2 = Symbol("id");
console.log(sym1 === sym2); // false (always unique)

let user = {
  name: "Alice",
  [Symbol("id")]: 123,
};
console.log(user); // { name: 'Alice', [Symbol(id)]: 123 }
```

# Use Case of Symbol Data Type

**Avoid Property Name Collision (Very Important)**

🔥 Problem:

In large applications (like React Native projects), multiple developers may use same property name in an object.

```js
// ❌ Without Symbol

const user = {
  name: "Avi",
  id: 101,
};

user.id = 202; // Accidentally overwritten
console.log(user.id); // 202 ❌

//✅ With Symbol (Safe)

const id = Symbol("id");

const user = {
  name: "Avi",
  [id]: 101,
};

user[id] = 202; // Only accessible using symbol reference

console.log(user[id]); // 202
console.log(user.id); // undefined

//No collision possible because Symbol is unique.
```

**2️⃣ Hidden / Private Object Properties**

Symbols are not enumerable in normal loops.

```js
const secret = Symbol("secretKey");

const obj = {
  name: "React",
  [secret]: "Hidden Value",
};

console.log(Object.keys(obj));
// ["name"] ✅ Symbol key hidden

console.log(obj[secret]);
// "Hidden Value"
```

👉 Useful when you want to store internal metadata.

📌 In large React Native apps, libraries use this pattern to avoid exposing internal properties.

**Creating Constants (Safe Enum Alternative)**

```js
const STATUS = {
  SUCCESS: Symbol("success"),
  ERROR: Symbol("error"),
};

function getStatus(status) {
  if (status === STATUS.SUCCESS) {
    console.log("Success");
  }
}

getStatus(STATUS.SUCCESS);
```

Since symbols are unique, no accidental match possible.

**Well-Known Symbols (Advanced Interview Topic)**

- JavaScript provides built-in symbols.

Example:

**🔹 Symbol.iterator**

Makes object iterable.

```js
const obj = {
  data: [1, 2, 3],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.data.length) {
          return { value: this.data[index++], done: false };
        }
        return { done: true };
      },
    };
  },
};

for (let value of obj) {
  console.log(value);
}
```

👉 Used internally by arrays, maps, sets etc.

**Since you are preparing for React Native interview, they may ask:**

Why not use string as key?

How to avoid object property collision?

What are well-known symbols?

Why are symbols not enumerable?

**🎯 When Should You Use Symbol?**

✔ When creating unique object keys
✔ When building libraries
✔ When avoiding name collision
✔ When implementing meta-programming
